---
title: Mongoose + TypeScript Tutorial
tags:
  - Mongoose
comments: true
---

# Mongoose?

Mongoose is a ODB (Object Data Mappig) library for mongoDB and Node.js. It provides various convient methods for JavaScript applications.

## Installation

```sh
npm install mongodb mongoose
```

## Connection

```ts
import mongoose from 'mongoose';

const db = mongoose.connection;
db.on('error', console.error);
db.once('open', function () {
  logger.info('Connected to mongod server');
});

mongoose.connect(YOUR MONGODB URL);
```

## Schema

Mongoose supports schema instead of table. Simply, it similar to concept of interface for mongoDB. Mongoose schema validates types and fields when we create a new document.

With TypeScript, we have to specify interface for schema. In below example, we have `IUser` interface for `UserSchema`. It contains `name` and `created_at` fields and `name` field validate `type: String` and whether empty, undefined or valid, while `created_at` field validate `type: Date` and auto generate current timestamp.

In addition, **we have to declare mongoose model object to work with schema in Node.js.**

```ts
import mongoose, { Model, Schema } from 'mongoose';

interface IUser {
  name: string;
  created_at: Date;
}

// mongoose schema object
const UserSchema = new Schema<IUser>({
  name: {
    type: String,
    required: true,
  },
  created_at: {
    type: Date,
    default: Date.now, // automated timestamp
  },
});

export const User = mongoose.model<IUser>('User', UserSchema);
```

## Middlewares

Mongoose middleware is very useful for various situations. It effectively reduces duplication of codes. For example, we can convert Local timestamp to UTC timestamp whenever create a new document as follows:

```ts
UserSchema.pre('save', function (next) {
  // Custom Function: convert local time to UTC timestamp
  this.created_at = convertLocalTimeToUTC(this.created_at);
  next();
});
```

There are two types of middleware. One is a `pre` and the other is a `post`. The difference is that `post` middleware are executed after the hooked method and all of its `pre` middleware have completed.

## Relationship

One big challenge for me was that how mongoDB and mongoose support relationship. It actually does not support real relationship which always satisfy the integrity constraint. However, mongoDB and mongoose do support **reference**. It is possible to have other schema as a field for another schema. And it is also possible to join two schemas and includes another schema's field with `populate()` method.

```ts
import mongoose, {Schema, Types} from 'mongoose';
import City from './city.schema';

const UserSchema = new Schema<IUser>({
  name: {
    type: String,
    required: true,
  },
  created_at: {
    type: Date,
    default: Date.now, // automated timestamp
  },
  // Reference City schema
  city: {
    type: Types.ObjectId,
    ref: City.name,
    required: true,
  }
});

export const User = mongoose.model<IUser>('User', UserSchema);
```

### Many to Many relationship

Then how can we deal with many to many relationship with mongoose. There are two types of solutions, either define virtual fields and define additional schema and save as a list on both schemas. I prefer to define virtual fields. Let's look at the following examples.

*Assume that a user can be author of many books and also a book has a many users as a authors.*

```ts
const UserSchema = new Schema<IUser>({
  name: {
    type: String,
    required: true,
  },
});

const User = mongoose.model<IUser>('User', UserSchema);

UserSchemas.virtual('books', {
  ref: 'UserBook',
  localField: '_id',
  foreignField: 'userId',
});

const BookSchema = new Schema<IBook>({
  name: {
    type: String,
    required: true,
  },
})

const Book = mongoose.model<IBook>('Book', BookSchema);

BookSchema.virtual('users', {
  ref: 'UserBook',
  localField: '_id',
  foreignField: 'bookId',
});

const UserBookSchema = new Schema<IUserBook>({
  userId: {
    type: Types.ObjectId,
    ref: 'User',
    required: true,
  }, 
  bookId: {
    type: Types.ObjectId,
    ref: 'Book',
    required: true,
  },
});
```

Update the `UserBookSchema`.

```ts
const newUser = new User(...);
const newBook = new Book(...);

// Update each relations between user and book
await UserBookSchema.update(
  userId: newUser._id,
  bookId: newBook._id,
)
```

### Cascade delete

How about cascade delete? MongoDB and mongoose also do not support cascade delete. We have to manually implement cascade delete if we need it. This can be simply implemented with mongoose middleware.

```ts
UserSchema.post('remove', async function (doc, next) {
  // Delete all documents in UserBookSchema whoose userId is equal to doc._id
  await UserBook.deleteMany({
    userId: doc._id,
  });
  next();
});
```

## Mongoose + TypeScript Boilerplate

You can use Mongoose + TypeScript Boilerplate to create your application much more easily.

[node-express-mongoose-with-typescript boilerplate](https://github.com/Byunk/node-express-mongoose-with-typescript){: .btn .btn--info}

### Reference

[Many to Many with Mongoose](https://stackoverflow.com/questions/46019149/many-to-many-with-mongoose)

[Cascade delete style in Mongoose](https://stackoverflow.com/questions/14348516/cascade-style-delete-in-mongoose)
