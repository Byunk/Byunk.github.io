---
title: 'Assignment 1: Threads'
categories: 
  - pintOS
relative: true
comments: true
---

# Project1: Threads

Main goal of project1 is not only implement given assignments, but also look around whole pintOS project structures and figure out how each function really operating. As below, actual codes you have to write to pass all cases in project 1 is comparably small. It a good point to conquer how pintOS work.

```sh
devices/timer.c               |  29 +++++++++++++++++++++++++++++
include/threads/fixed-point.h |  10 ++++++++++
include/threads/synch.h       |   4 ++++
include/threads/thread.h      |  21 +++++++++++++++++++++
threads/synch.c               | 143 +++++++++++++++++++++++++++++++++++++++++++-
threads/thread.c              | 135 ++++++++++++++++++++++++++++++++++++++++----
6 files changed, 330 insertions(+), 12 deletions(-)
```

## Thread Structure

`thread.h` file is one of the most important files in pintOS. In the whole process, you have to look for given functions that works the way you want, or define new functions if it needed. Please read carefully whole sentences in `thread.h`.

```c
// thread.h
 *
 * Each thread structure is stored in its own 4 kB page.  The
 * thread structure itself sits at the very bottom of the page
 * (at offset 0).  The rest of the page is reserved for the
 * thread's kernel stack, which grows downward from the top of
 * the page (at offset 4 kB).  Here's an illustration:
 *
 *      4 kB +---------------------------------+
 *           |          kernel stack           |
 *           |                |                |
 *           |                |                |
 *           |                V                |
 *           |         grows downward          |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           |                                 |
 *           +---------------------------------+
 *           |              magic              |
 *           |            intr_frame           |
 *           |                :                |
 *           |                :                |
 *           |               name              |
 *           |              status             |
 *      0 kB +---------------------------------+
 *
 * The upshot of this is twofold:
 *
 *    1. First, `struct thread' must not be allowed to grow too
 *       big.  If it does, then there will not be enough room for
 *       the kernel stack.  Our base `struct thread' is only a
 *       few bytes in size.  It probably should stay well under 1
 *       kB.
 *
 *    2. Second, kernel stacks must not be allowed to grow too
 *       large.  If a stack overflows, it will corrupt the thread
 *       state.  Thus, kernel functions should not allocate large
 *       structures or arrays as non-static local variables.  Use
 *       dynamic allocation with malloc() or palloc_get_page()
 *       instead.
 *
 * The first symptom of either of these problems will probably be
 * an assertion failure in thread_current(), which checks that
 * the `magic' member of the running thread's `struct thread' is
 * set to THREAD_MAGIC.  Stack overflow will normally change this
 * value, triggering the assertion. */
/* The `elem' member has a dual purpose.  It can be an element in
 * the run queue (thread.c), or it can be an element in a
 * semaphore wait list (synch.c).  It can be used these two ways
 * only because they are mutually exclusive: only a thread in the
 * ready state is on the run queue, whereas only a thread in the
 * blocked state is on a semaphore wait list. */
struct thread {
	/* Owned by thread.c. */
	tid_t tid;                          /* Thread identifier. */
	enum thread_status status;          /* Thread state. */
	char name[16];                      /* Name (for debugging purposes). */
	int priority;                       /* Priority. */

	/* Shared between thread.c and synch.c. */
	struct list_elem elem;              /* List element. */

#ifdef USERPROG
	/* Owned by userprog/process.c. */
	uint64_t *pml4;                     /* Page map level 4 */
#endif
#ifdef VM
	/* Table for whole virtual memory owned by thread. */
	struct supplemental_page_table spt;
#endif

	/* Owned by thread.c. */
	struct intr_frame tf;               /* Information for switching */
	unsigned magic;                     /* Detects stack overflow. */
};

void thread_init (void);
void thread_start (void);

void thread_tick (void);
void thread_print_stats (void);

typedef void thread_func (void *aux);
tid_t thread_create (const char *name, int priority, thread_func *, void *);

void thread_block (void);
void thread_unblock (struct thread *);

struct thread *thread_current (void);
tid_t thread_tid (void);
const char *thread_name (void);

void thread_exit (void) NO_RETURN;
void thread_yield (void);

int thread_get_priority (void);
void thread_set_priority (int);

int thread_get_nice (void);
void thread_set_nice (int);
int thread_get_recent_cpu (void);
int thread_get_load_avg (void);

void do_iret (struct intr_frame *tf);
```

## Alarm Clock

One of the most important concepts in OS is *interrupt*. Programs which implemented with [*busy waits*](https://en.wikipedia.org/wiki/Busy_waiting), and if has to wait for a long time, there are a lot of unnecessary overhead. To solve this issue, engineers introduced *interrupt*. And this assignment is all about this issue.

In our pintOS, `timer_sleep()` busy waits until timer exceeds certain ticks.

```c
/* Suspends execution for approximately TICKS timer ticks. */
void
timer_sleep (int64_t ticks) {
	int64_t start = timer_ticks ();

	ASSERT (intr_get_level () == INTR_ON);
	while (timer_elapsed (start) < ticks)
		thread_yield ();
}
```

*To solve this problem, we have to define two functions*

1. make thread sleep, and block. If needed put thread in list.
2. make thread awake, and unblock.

In my case, I defined additional field on `struct thread` and save awake time.

```c
struct thread {
	...
	int64_t alarm_tick;
	...
}

void 
thread_sleep (int64_t ticks) {
	...
	struct thread *t = thread_current ();
	t->alarm_tick = ticks;
	
	// put thread into list

	thread_block();
	...
}

void 
thread_awake (int64_t ticks) {
	...
	// Search threads whose alarm_tick lower or equal to current ticks
	// remove it
	// unblock it
	...
}
```

## Priority Scheduling

When it comes to cpu scheduling, priority inversion is a big problem when we adapt priority system on threads.

Our priority system satistify these constraints.

1. range from `PRI_MAIN (0)` to `PRI_MAX (63)`
2. if there's no reason to choose another priority, use `PRI_DEFAULT (31)`
3. if new thread's priority is higher than current thread, preemption is occured.

### Priority Donation

To deal with priority inversion, our document introduces *priority donation*.

>Implement priority donation. You will need to account for all different situations in which priority donation is required. Be sure to handle multiple donations, in which multiple priorities are donated to a single thread. You must also handle nested donation: if H is waiting on a lock that M holds and M is waiting on a lock that L holds, then both M and L should be boosted to H's priority. If necessary, you may impose a reasonable limit on depth of nested priority donation, such as 8 levels.

To implement priority donation, we need to look at the moment when a lock is acquired and released. In `synch.c`, `lock_acquire` and `lock_release` are defined as below. It checks some contraints and just down & up semaphore. We have to implements donation logics in these functions.

In my case, I introduces new fields in `struct thread` and `struct lock` to memorize its dependency and its donated priority. There are many ways to implement this logic.

```c
void
lock_acquire (struct lock *lock) {
	ASSERT (lock != NULL);
	ASSERT (!intr_context ());
	ASSERT (!lock_held_by_current_thread (lock));

	sema_down (&lock->semaphore);
	lock->holder = thread_current ();
}

void
lock_release (struct lock *lock) {
	ASSERT (lock != NULL);
	ASSERT (lock_held_by_current_thread (lock));

	lock->holder = NULL;
	sema_up (&lock->semaphore);
}
```

### When Preemption occurs?

This is the most confusing point in this assignment. First case is when thread is created. When thread is unblocked, it preempt the running thread? No. How about when calling `thread_set_priority()`. Yes. It should preempt running thread.

>thread_unblock : This function does not preempt the running thread.  This can
   be important: if the caller had disabled interrupts itself,
   it may expect that it can atomically unblock a thread and
   update other data.

To sum up, we have to look at these situations.

1. `thread_create()` => preempt
2. `thread_set_priority()` => preempt
3. `thread_unblock()` or `thread_yield()` => not preempt, but put thread in order of priority.

## Advanced Scheduler

In this assignment, I recommend you to read carefully, and just follow document one sentence by one. All you need is on document.

The order can be described as follows.

1. define fixed point operations
2. implement provided logics

These are not that difficult, but the problem is test codes are very time-consuming and challenging. Escpecially, tests located in mlfqs directories are vulnerable to TLE (Time Limit Exceed). If test does not work as expected, look your preemption & donation logic carefully.

Also, all test codes except `mlfqs/` will be checked at every assignment again. It means, if there is a problem with the logic of assignment 1, deductions will be made for all subsequent questions. 
{: .notice--warning}

I hope you have successfully completed assignment 1 : Thread.