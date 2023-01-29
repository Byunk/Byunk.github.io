---
title: Nestjs + Mongoose How to start project?
categories: 
  - Meetup
tags:
  - Nestjs
  - Mongoose
relative: true
comments: true
---

In this article, I will introduce how to start Nestjs project with mongoose, and fundamental concepts of mongoose.

*(Expected) Nodejs starter template with mongoose*

# Questions

## Many to Many Relationships with Mongoose

[Many to Many with Mongoose](https://stackoverflow.com/questions/46019149/many-to-many-with-mongoose)

### Sol: Use Virtuals and an Intermediary Collection

```ts
// user.schema.ts
@Schema({ toJSON: { virtuals: true } })
export class User {
  @Prop({ type: String, required: true })
  name: string;
  @Prop({ type: String, required: true })
  userId: string;
  @Prop({ type: String, required: true })
  password: string;
  @Prop({ type: String })
  introduction: string;
  @Prop({ type: String, required: true, enum: Object.values(Gender) })
  gender: string;
}
export const UserSchema = SchemaFactory.createForClass(User);

UserSchema.virtual('chatrooms', {
  ref: UserChatroom.name,
  localField: '_id',
  foreignField: 'userId',
});
```

```ts
// user.chatroom.schema.ts
@Schema({ _id: false })
export class UserChatroom {
  @Prop({
    type: [mongoose.Schema.Types.ObjectId],
    ref: User.name,
    required: true,
  })
  userId: mongoose.Schema.Types.ObjectId;

  @Prop({
    type: [mongoose.Schema.Types.ObjectId],
    ref: Chatroom.name,
    required: true,
  })
  chatroomId: mongoose.Schema.Types.ObjectId;
}

export const UserChatroomSchema = SchemaFactory.createForClass(UserChatroom);
```

```ts
// user.service.ts
async getUser(userId: string): Promise<UserDocument> {
  const existingUser = await this.userModel
    .findOne({ userId: userId })
    .populate({ path: 'chatrooms', populate: { path: 'chatroomId' } })
    .exec();

  if (!existingUser) {
    throw new NotFoundException(`User #${userId} not found`);
  }
  return existingUser;
}
```

## Cascade delete style in Mongoose

[Cascade delete style in Mongoose](https://stackoverflow.com/questions/14348516/cascade-style-delete-in-mongoose)

### Reference

[견고한 Nodejs 프로젝트 아키텍쳐 설계하기](https://velog.io/@hopsprings2/%EA%B2%AC%EA%B3%A0%ED%95%9C-node.js-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90-%EC%84%A4%EA%B3%84%ED%95%98%EA%B8%B0)

[Mongoose의 올바른 사용 가이드(KR)](https://a1p4ca.netlify.app/2018/09/10/mongoose%EC%9D%98-%EC%98%AC%EB%B0%94%EB%A5%B8-%EC%82%AC%EC%9A%A9-%EA%B0%80%EC%9D%B4%EB%93%9C-1-validator-middleware-%EA%B7%B8%EB%A6%AC%EA%B3%A0-query/)

