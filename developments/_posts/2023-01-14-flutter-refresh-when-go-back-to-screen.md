---
title: "[Flutter] Refresh when go back to screen"
except: Mongoose starting guides with TypeScript
tags:
  - Flutter
comments: true
gallery:
  - url: /assets/images/230114-goback1.png
    image_path: /assets/images/230114-goback1.png
  - url: /assets/images/230114-goback2.png
    image_path: /assets/images/230114-goback2.png
---

# Motivation

In my simple-chatapp project, API should be called whenever user reach to main screen that lists all of chatrooms the user participating. But in Flutter, when user get back to previous screen, the screen does not refresh automatically. Therefore we have to manually configure this issue.

Screenshots looks like following:

The first page (chatroom_screen) call its api to fetch chatroom list, and the response include last messages from each chatroom. Therefore, when we get back to chatroom_screen from chat_screen we expect that each chatroom's last message will be updated.

{% include gallery %}

Project structure looks like below:

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

<br />

# Solution

We can handle this issue with simple `then`, which register callback function to `Navigator.push()` method.

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
# Reference

[https://www.nstack.in/blog/flutter-refresh-on-navigator-pop-or-go-back/](https://www.nstack.in/blog/flutter-refresh-on-navigator-pop-or-go-back/)