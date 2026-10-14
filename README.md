# RECOMS TypeScript SDK

Create and manage comments, replies and reactions from your backend.

## Install

```sh
bun add @recoms/sdk
```

Or use `npm install @recoms/sdk`.

## Create a comment

```typescript
import { RecomsClient } from "@recoms/sdk";

const recoms = new RecomsClient({
  token: process.env.RECOMS_API_KEY,
});

const comment = await recoms.comments.create({
  resourceId: "article:welcome",
  authorUserId: "usr_42",
  content: "Thanks for the helpful article!",
});
```

Create a project API key in Dashboard and keep it on your server. Use your application's user ID as `authorUserId`.

## Read comments

```typescript
const page = await recoms.comments.list({
  resourceId: "article:welcome",
});

for (const comment of page.data) {
  console.log(comment.content);
}
```

See the [SDK reference](./reference.md) for all methods and the [API documentation](https://recoms.tech/docs) for integration guides.
