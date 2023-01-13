---
title: "Flutter: Refresh when go back to screen"
categories: 
  - Meetup
tags:
  - Flutter
relative: true
comments: true
---

# Motivation

In my project, meetup, I had to call an API when user get back to previous screen. Project structure looks like below.

```
lib
├── main.dart
├── models
├── screens
│   ├── chat_screen.dart
│   └── chatroom_screen.dart
└── services
    └── chatroom_api.dart
```

![First Page](/assets/images/230114-goback1.png){: width="25%" height="25%"}

![Second Page](/assets/images/230114-goback2.png){: width="25%" height="25%"}

The first page call chatroom_api to fetch chatroom list info. Chatroom info contains last messages of each chatroom. So, get back from second page to first page, we have to refresh those last messages informations by calling chatroom api.

# Solution

To navigate from first page to second page, we can write code as below. But with this code, chatrooms info does not refresh when get back from second page.

```dart
ElevatedButton(
    onPressed: () => _joinChat(chatroom),
    style: ElevatedButton.styleFrom(
        minimumSize: Size(size.width, 80),
        backgroundColor: Colors.white,
)

void _joinChat(Chatroom chatroom) {
    Route route = MaterialPageRoute(
      builder: (context) => ChatScreen(user: widget.user, room: chatroom),
    );
    Navigator.push(context, route);
}
```

**To solve this problem, we can register callback function with `then`.** Therfore,`_joinChat` changes into as below.

```dart
void _joinChat(Chatroom chatroom) {
    Route route = MaterialPageRoute(
      builder: (context) => ChatScreen(user: widget.user, room: chatroom),
    );
    Navigator.push(context, route).then((value) async {
      _controller.add(await ChatroomApi.fetch(widget.user.name));
    });
}
```

# Whole code

```dart
class ChatRoomScreen extends StatefulWidget {
  final User user;

  const ChatRoomScreen({super.key, required this.user});

  @override
  State<ChatRoomScreen> createState() => _ChatRoomScreenState();
}

class _ChatRoomScreenState extends State<ChatRoomScreen> {
  final TextEditingController _textEditingController = TextEditingController();
  final ScrollController _scrollController = ScrollController();

  late StreamController<List<dynamic>> _controller;
  List<dynamic> chatrooms = [];

  @override
  void initState() {
    super.initState();
    _controller = StreamController.broadcast();
  }

  @override
  void dispose() {
    _scrollController.dispose();
    _textEditingController.dispose();
    _controller.close();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    Size size = MediaQuery.of(context).size;
    return Scaffold(
      backgroundColor: Colors.grey.withOpacity(1),
      appBar: const MeetupAppbar(),
      floatingActionButton: _createChatroomButton(),
      body: FutureBuilder(
          future: ChatroomApi.fetch(widget.user.name),
          builder: ((context, snapshot) {
            if (!snapshot.hasData) {
              return const CircularProgressIndicator();
            } else {
              chatrooms = snapshot.data!;
              return StreamBuilder(
                initialData: snapshot.data,
                stream: _controller.stream,
                builder: ((context, streamSnapshot) {
                  return ListView.separated(
                    controller: _scrollController,
                    itemCount: streamSnapshot.data!.length,
                    separatorBuilder: ((BuildContext context, int index) {
                      return const Divider(
                        thickness: 0.5,
                        height: 1,
                      );
                    }),
                    itemBuilder: ((BuildContext context, int index) {
                      Chatroom chatroom =
                          Chatroom.fromJson(streamSnapshot.data![index]);
                      return ElevatedButton(
                        onPressed: () => _joinChat(chatroom),
                        style: ElevatedButton.styleFrom(
                          minimumSize: Size(size.width, 80),
                          backgroundColor: Colors.white,
                        ),
                        child: Row(
                          children: [
                            const Icon(
                              Icons.people,
                              color: Colors.black,
                            ),
                            const SizedBox(
                              width: 50,
                            ),
                            Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              mainAxisAlignment: MainAxisAlignment.spaceBetween,
                              children: [
                                Text(
                                  chatroom.title,
                                  style: const TextStyle(
                                    color: Colors.black,
                                  ),
                                ),
                                const SizedBox(
                                  height: 10,
                                ),
                                Text(
                                  chatroom.lastChat != null
                                      ? chatroom.lastChat!.content
                                      : "",
                                  style: const TextStyle(
                                    color: Colors.black,
                                  ),
                                )
                              ],
                            ),
                          ],
                        ),
                      );
                    }),
                  );
                }),
              );
            }
          })),
    );
  }

  FloatingActionButton _createChatroomButton() {
    return FloatingActionButton(
      backgroundColor: Colors.green,
      onPressed: () => showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('Chatroom title'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              TextField(
                controller: _textEditingController,
                autofocus: true,
              ),
              const SizedBox(
                height: 5,
              ),
              TextButton(
                onPressed: () async {
                  Navigator.of(context, rootNavigator: true).pop();
                  _createChat();
                },
                child: const Text(
                  'save',
                  style: TextStyle(fontSize: 20),
                ),
              ),
            ],
          ),
        ),
      ),
      child: const Icon(Icons.add),
    );
  }

  Future<void> _createChat() async {
    var title = _textEditingController.text;
    _textEditingController.text = '';
    if (title != '') {
      chatrooms.add(await ChatroomApi.createChatroom(title, widget.user.name));
      _controller.add(chatrooms);
    }
  }

  void _joinChat(Chatroom chatroom) {
    Route route = MaterialPageRoute(
      builder: (context) => ChatScreen(user: widget.user, room: chatroom),
    );
    Navigator.push(context, route).then((value) async {
      _controller.add(await ChatroomApi.fetch(widget.user.name));
    });
  }
}
```