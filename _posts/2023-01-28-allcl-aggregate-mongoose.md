---
title: How to make complex Aggregation from ref's fields with mongoose?
categories: 
  - ALLCL
tags:
  - Nodejs
  - Typescript
  - Mongoose
relative: true
comments: true
---

# Background

During project, I faced complex problem. With LogSchema as below, I have to make groups by `try_at` and `branch` field of `problem` reference field. Response should be shaped like this.

```ts
// Parameters
{
  user: ObjectId,
  branch?: ObjectId,
  fromdate?: Date,
  todate?: Date
}

// Expected Result
{
  date: 'yyyy-mm-dd',
  branchName: 'foo',
  logs: [
    {
      _id: 'xxx',
      levelColor: 'bar'
    }
  ]
}
```

```ts
/* Schemas */
const LogSchema = new Schema<ILog, LogModel>({
	user: {
		type: Types.ObjectId,
		ref: 'User',
		required: true,
	},
	problem: {
		type: Types.ObjectId,
		ref: 'Problem',
		required: true,
	},
	try_at: {
		type: Date,
		default: Date.now,
		required: true,
	},
  ...
});

const ProblemSchema = new Schema<IProblem, ProblemModel>({
	branch: {
		type: Types.ObjectId,
		ref: 'Branch',
		required: true,
	},
	color: {
		type: String,
		enum: Color,
		require: true,
		default: Color.RED,
	},
  ...
});

const BranchSchema = new Schema<IBranch, BranchModel>({
	name: {
		type: String,
		required: true,
	},
  ...
});
```

# Methods

Let's organize our progress.

1. filter out by `user` and `fromdate` & `todate`
2. join `problem` collection and `branch` collection
3. if branch parameter exists, then filter out by `branch`
4. group by `try_at` and `branch`
5. projection

## Filter out by `user` and `fromdate` & `todate`

[MongoDB/Mongoose querying at a specific date?](https://stackoverflow.com/questions/11973304/mongodb-mongoose-querying-at-a-specific-date)

[date-fns library](https://date-fns.org/)

```ts
logs = this.aggregate([
			{
				$match: {
					$and: [
						{
							user: new mongoose.Types.ObjectId(user),
						},
						{
							try_at: {
                $gte: startOfDay(new Date(syear, smonth, sday)),
								$lt: endOfDay(new Date(eyear, emonth, eday)),
							},
						},
					],
				},
			},
		]);
```

## Join `problem` collection and `branch` collection

using [`$lookup`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/)

[Querying after populate in Mongoose](https://stackoverflow.com/questions/11303294/querying-after-populate-in-mongoose)

```ts
logs.append({
		$lookup: {
			from: Problem.collection.name,
			localField: 'problem',
			foreignField: '_id',
			pipeline: [
				{
					$lookup: {
						from: Branch.collection.name,
						localField: 'branch',
						foreignField: '_id',
						as: 'branch_info',
					},
				},
			],
			as: 'problem_info',
		},
	});
```

## If branch parameter exists, then filter out by `branch`

```ts
if (branchName !== undefined) {
  logs.append({
    $match: {
      'problem_info.branch_info.name': branchName,
    },
  });
}
```

## Group by `try_at` and `branch`

[MongoDB - group composite key with nested fields](https://stackoverflow.com/questions/29879825/mongodb-group-composite-key-with-nested-fields)

Without [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/), `$problem_info.branch_info.name` is resulted as a nested string as below.

```ts
"branch" : [
  [
    "branch name"
  ]
]
```

```ts
logs.append(
  { $unwind: '$problem_info' },
  { $unwind: '$problem_info.color' },
  { $unwind: '$problem_info.branch_info' },
  { $unwind: '$problem_info.branch_info.name' },
  {
    $group: {
      _id: {
        date: {
          $dateToString: {
            format: '%Y-%m-%d',
            date: '$try_at',
          },
        },
        branch: '$problem_info.branch_info.name',
      },
      logs: {
        $push: {
          id: '$_id',
          color: '$problem_info.color',
        },
      },
    },
  },
)
```

## Projection

[`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project)

```ts
logs.append(
  {
    $project: {
      _id: 0,
      date: '$_id.date',
      branch: '$_id.branch',
      logs: 1,
    },
  }
)
```

