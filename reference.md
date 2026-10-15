# Reference
## Resources
<details><summary><code>client.resources.<a href="/src/api/resources/resources/client/Client.ts">counts</a>({ ...params }) -> Recoms.ResourceCommentCountsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Render comment-count badges for an article or resource list without paginating roots and replies. Supply 1–100 distinct resourceId query values in request order; opaque IDs may contain slashes and must not be encoded as path segments. Returns exactly one entry per requested resource, including zeros. commentCount counts live comments with an APPROVED published version at every depth; rootCommentCount counts the subset with no parent. Deleted and never-published submissions are excluded, while pending/failed/rejected edits preserve counts of the previous published version. Counts reflect current committed state, not a stored counter; replyCount uses the same visibility but counts only a comment's direct replies. Project API key only; no author header. Read-only, available in frozen projects, consumes no write permits; no separate read rate limiter is applied.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.resources.counts({
    resourceIds: ["article:postgresql-cursors", "articles/nextjs"]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.CountsResourcesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ResourcesClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Comments
<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">list</a>({ ...params }) -> core.Page&lt;Recoms.PublicComment, Recoms.CommentListResponse&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Public reads return approved content and deleted structural nodes only; unapproved revisions are never published. Authenticate from your backend with the project's API key, never from an end-user browser. Returns root comments only, not their replies. The required resourceId is your opaque resource identifier. Default sort is NEWEST; limit defaults to 20 (1–100). To continue, copy nextCursor into cursor with the same resourceId and sort. A null nextCursor ends the list. Replies have a separate listDirectReplies operation. Reads do not consume write permits.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const pageableResponse = await client.comments.list({
    resourceId: "article:postgresql-cursors",
    limit: 20
});
for await (const item of pageableResponse) {
    console.log(item);
}

// Or you can manually iterate page-by-page
let page = await client.comments.list({
    resourceId: "article:postgresql-cursors",
    limit: 20
});
while (page.hasNextPage()) {
    page = await page.getNextPage();
}

// You can also access the underlying response
const response = page.response;

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.ListCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">create</a>({ ...params }) -> Recoms.AuthorCommentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authenticate the end user in your backend and pass their stable external author ID. The project key selects the tenant; it is not proof of end-user identity. Send JSON with Content-Type: application/json. Bodies must not exceed 16 KiB. Use Idempotency-Key when retrying an action after network failure. Successful AI moderation failure is not an HTTP error: inspect latestRevision.status. Omit parentId for a root comment, or supply the immediate parent to create a reply. A parent must be in the same project and resource and either have approved content or be a deleted node. Moderation may approve, reject, await review, or fail technically. The 201 response confirms the submission, not guaranteed publication. Read its latestRevision.status; only APPROVED content is public. New activity is blocked in frozen projects and when submission quota is unavailable.

Required API-key permission: comments:write. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.comments.create({
    idempotencyKey: "0198a6ec-9e00-7000-8000-000000000010",
    resourceId: "article:postgresql-cursors",
    authorUserId: "usr_42",
    content: "This explanation of **cursor pagination** was helpful."
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.CreateCommentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">get</a>({ ...params }) -> Recoms.CommentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Public reads return approved content and deleted structural nodes only; unapproved revisions are never published. Authenticate from your backend with the project's API key, never from an end-user browser. Pending, rejected or failed submissions without a public version return 404. A pending edit does not hide the previous approved text. Deleted comments return DELETED with null content and retain their position in the thread. Use getAuthorComment to inspect an author's unpublished submission.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.comments.get({
    commentId: "0198a6ec-9e00-7000-8000-000000000001"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.GetCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">delete</a>({ ...params }) -> Recoms.DeleteCommentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

The externally authenticated author in X-Author-User-Id must own the comment. Soft deletion hides its source in public and author responses but preserves the structural node, reply relationships and stored revision history for authorized dashboard members. This is not author erasure. Deletion remains available in frozen projects and consumes write permits. A second deletion without replay returns 409 COMMENT_DELETED; use the original Idempotency-Key to retry the same action. No request body is required.

Required API-key permission: comments:write. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.comments.delete({
    authorUserId: "usr_42",
    idempotencyKey: "0198a6ec-9e00-7000-8000-000000000010",
    commentId: "0198a6ec-9e00-7000-8000-000000000001"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.DeleteCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">update</a>({ ...params }) -> Recoms.AuthorCommentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authenticate the end user in your backend and pass their stable external author ID. The project key selects the tenant; it is not proof of end-user identity. Send JSON with Content-Type: application/json. Bodies must not exceed 16 KiB. Use Idempotency-Key when retrying an action after network failure. Successful AI moderation failure is not an HTTP error: inspect latestRevision.status. authorUserId must own the comment. The entire Markdown source is replaced by a new revision; resource and parent identity cannot be changed. Existing published content stays visible while an edit is PENDING, FAILED or REJECTED. An APPROVED edit replaces it. Frozen projects and unavailable submission quota block new edits. Deleted comments cannot be edited. Use getAuthorComment to follow moderation state.

Required API-key permission: comments:write. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.comments.update({
    idempotencyKey: "0198a6ec-9e00-7000-8000-000000000010",
    commentId: "0198a6ec-9e00-7000-8000-000000000001",
    authorUserId: "usr_42",
    content: "Updated explanation of **cursor pagination**."
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.EditCommentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.comments.<a href="/src/api/resources/comments/client/Client.ts">listReplies</a>({ ...params }) -> core.Page&lt;Recoms.PublicComment, Recoms.CommentListResponse&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Public reads return approved content and deleted structural nodes only; unapproved revisions are never published. Authenticate from your backend with the project's API key, never from an end-user browser. Lists immediate children, not all descendants, ordered by createdAt ascending then id ascending. The parent must be publicly accessible, including deleted nodes. To traverse deeper, call this operation for each reply. Limit defaults to 20 (1–100). Copy nextCursor into cursor for this same parent and project; null ends the list. A cursor from root comments or another parent is invalid.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const pageableResponse = await client.comments.listReplies({
    commentId: "0198a6ec-9e00-7000-8000-000000000001",
    limit: 20
});
for await (const item of pageableResponse) {
    console.log(item);
}

// Or you can manually iterate page-by-page
let page = await client.comments.listReplies({
    commentId: "0198a6ec-9e00-7000-8000-000000000001",
    limit: 20
});
while (page.hasNextPage()) {
    page = await page.getNextPage();
}

// You can also access the underlying response
const response = page.response;

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.ListRepliesCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Reactions
<details><summary><code>client.reactions.<a href="/src/api/resources/reactions/client/Client.ts">set</a>({ ...params }) -> Recoms.ReactionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authenticate the end user in your backend and pass their external authorUserId. Only comments with approved public content accept reactions. Each user has at most one LIKE or DISLIKE per comment: setting the same type leaves it unchanged and setting the other replaces it. A pending edit does not disable reactions to existing public content. Frozen projects block new reaction activity. This mutation consumes write permits; optional Idempotency-Key provides replay for network retries. Send JSON; the maximum body is 16 KiB.

Required API-key permission: comments:write. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.reactions.set({
    idempotencyKey: "0198a6ec-9e00-7000-8000-000000000010",
    commentId: "0198a6ec-9e00-7000-8000-000000000001",
    authorUserId: "usr_42",
    type: "LIKE"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.SetReactionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ReactionsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.reactions.<a href="/src/api/resources/reactions/client/Client.ts">remove</a>({ ...params }) -> Recoms.ReactionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Remove the reaction belonging to the external user in X-Author-User-Id. Your backend authenticates this identity. No request body is required. If no reaction exists, return type null and unchanged counters. Deleted comments and comments without approved public content cannot be targeted. Removal remains available in frozen projects and consumes write permits. Use the original Idempotency-Key to retry an uncertain action; other users' reactions are unchanged.

Required API-key permission: comments:write. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.reactions.remove({
    authorUserId: "usr_42",
    idempotencyKey: "0198a6ec-9e00-7000-8000-000000000010",
    commentId: "0198a6ec-9e00-7000-8000-000000000001"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.RemoveReactionsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ReactionsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Project
<details><summary><code>client.project.<a href="/src/api/resources/project/client/Client.ts">get</a>() -> Recoms.GetProjectResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Read project settings, billing state and non-secret provider configuration. Requires project:read. The key selects the project. access returns its keyId and effective permissions in resource:action form, allowing you to choose available operations before sending a request. Permissions can change; the API checks each request independently. Moderation rules and the recommended reset policy are included for management access. Available while frozen; no write permits are consumed.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.get();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `ProjectClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.<a href="/src/api/resources/project/client/Client.ts">update</a>({ ...params }) -> Recoms.UpdateProjectResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Atomically replace the supplied moderation mode, policy or analytics timezone. Requires project:update. Omitted fields remain unchanged; supply at least one. OFF publishes immediately, MANUAL queues review, AI resolves automatically, HYBRID queues suspected violations for review. Rules are trimmed. A timezone changes analytics calendar boundaries, not stored UTC timestamps. Existing decisions are not rerun. Refresh settings before retrying an uncertain outcome; Idempotency-Key replay is not supported. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.update({
    moderationMode: "HYBRID",
    analyticsTimezone: "Europe/Moscow"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.UpdateProjectSettings` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ProjectClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.<a href="/src/api/resources/project/client/Client.ts">analytics</a>({ ...params }) -> Recoms.AnalyticsProjectResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Read discussion and moderation metrics in the project's timezone. Requires analytics:read. Discussion counts first publication rather than edits; moderation summary counts unique comments with approval/rejection decisions. CUSTOM requires inclusive YYYY-MM-DD dates; the returned endAt is exclusive and current-day ranges stop at request time. Free history is limited to 90 days; unavailable segmentation metrics are null. planUsage follows the current billing period independently of the selected range. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.analytics();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.AnalyticsProjectRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ProjectClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.<a href="/src/api/resources/project/client/Client.ts">usage</a>() -> Recoms.UsageProjectResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Read submission usage and allowance for the current project billing period. Requires usage:read. Edits count as submissions; freeze/activation does not reset usage. Returns null without billing state. With billing state but no current usage interval, consumed is zero, included follows the plan and period is null. This read never reserves quota and is available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.usage();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `ProjectClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Me Reactions
<details><summary><code>client.me.reactions.<a href="/src/api/resources/me/resources/reactions/client/Client.ts">list</a>({ ...params }) -> core.Page&lt;Recoms.AuthorReaction, Recoms.AuthorReactionListResponse&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Restore liked/disliked UI state after a reload without writing or persisting reaction state in your own database. Your backend authenticates the user before supplying X-Author-User-Id. Returns only that user's existing reactions on APPROVED, non-deleted comments in the key's project and required resource; pending edits preserve existing public visibility. Optionally repeat commentId (maximum 100) to restrict to rendered comments. Ordered by reaction creation time descending, then reaction ID descending. limit defaults to 20 (1–100). Continue using nextCursor with the same project, author, resource and commentId filter set; null ends the list. Read-only, available in frozen projects, consumes no write permits; no separate read rate limiter is applied.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const pageableResponse = await client.me.reactions.list({
    authorUserId: "usr_42",
    resourceId: "article:postgresql-cursors",
    commentIds: ["0198a6ec-9e00-7000-8000-000000000001"],
    limit: 20
});
for await (const item of pageableResponse) {
    console.log(item);
}

// Or you can manually iterate page-by-page
let page = await client.me.reactions.list({
    authorUserId: "usr_42",
    resourceId: "article:postgresql-cursors",
    commentIds: ["0198a6ec-9e00-7000-8000-000000000001"],
    limit: 20
});
while (page.hasNextPage()) {
    page = await page.getNextPage();
}

// You can also access the underlying response
const response = page.response;

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.me.ListReactionsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ReactionsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Me Comments
<details><summary><code>client.me.comments.<a href="/src/api/resources/me/resources/comments/client/Client.ts">list</a>({ ...params }) -> core.Page&lt;Recoms.AuthorComment, Recoms.AuthorCommentListResponse&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Build a my-comments page or recover submissions whose IDs were lost, including comments awaiting review, without duplicating comment state in your database. Your backend authenticates the author supplied in X-Author-User-Id; the project key and author scope every result. Roots and replies are included, deleted comments are excluded. Optional resourceId narrows the resource; repeat latestRevisionStatus to match the latest submission only, including pending edits of already published comments. Returns the existing AuthorComment representation: published content plus latestRevision. NEWEST (default) orders comment creation time and ID descending; OLDEST orders both ascending. limit defaults to 20 (1–100). Copy nextCursor with unchanged project, author, filters and sort; null ends the list. Filter order is immaterial and limit may change; pagination is not a snapshot across requests. Read-only, available in frozen projects, consumes no write permits; no separate read rate limiter is applied.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const pageableResponse = await client.me.comments.list({
    authorUserId: "usr_42",
    resourceId: "article:postgresql-cursors",
    limit: 20
});
for await (const item of pageableResponse) {
    console.log(item);
}

// Or you can manually iterate page-by-page
let page = await client.me.comments.list({
    authorUserId: "usr_42",
    resourceId: "article:postgresql-cursors",
    limit: 20
});
while (page.hasNextPage()) {
    page = await page.getNextPage();
}

// You can also access the underlying response
const response = page.response;

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.me.ListCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.me.comments.<a href="/src/api/resources/me/resources/comments/client/Client.ts">get</a>({ ...params }) -> Recoms.AuthorCommentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Read a comment owned by the externally authenticated user identified by X-Author-User-Id. Your backend must establish this identity; never trust an arbitrary client-supplied user ID. The response contains published content and the latest submitted revision, including PENDING, FAILED or REJECTED. A published comment can remain APPROVED while its edit awaits review. Deleted source is hidden in both content fields. This read does not consume write permits.

Required API-key permission: comments:read. A valid key without this permission is not sufficient; author identity and ownership requirements still apply where documented.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.me.comments.get({
    authorUserId: "usr_42",
    commentId: "0198a6ec-9e00-7000-8000-000000000001"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.me.GetCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Project Comments
<details><summary><code>client.project.comments.<a href="/src/api/resources/project/resources/comments/client/Client.ts">queues</a>() -> Recoms.QueuesCommentsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Count the latest PENDING and FAILED revisions of live comments, including pending edits of already published comments. Requires comments:read. Deleted comments are excluded. FAILED is a technical check failure, not rejection. These counts and a separate list are not a shared snapshot. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.comments.queues();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.comments.<a href="/src/api/resources/project/resources/comments/client/Client.ts">list</a>({ ...params }) -> Recoms.ProjectComments</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List roots and replies with their latest moderation revision. Requires comments:read. Filter status follows the latest revision, so an approved comment with a pending edit appears under PENDING. Creation time or current reaction/reply counts determine order; ID breaks ties. Continue with nextCursor and identical filters/sort; null ends pagination. Pages are not a snapshot. hasComments ignores filters; queues describes moderation backlog. Available while frozen; no write permits are consumed.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.comments.list();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.project.ListCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.comments.<a href="/src/api/resources/project/resources/comments/client/Client.ts">review</a>({ ...params }) -> Recoms.ReviewCommentsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Approve or reject the specified latest PENDING or FAILED revision. Requires comments:review. Approval publishes that revision; rejection retains an earlier approved version. A key decision has no verified moderator-account attribution. A stale or already decided revision returns REVISION_ALREADY_REVIEWED. Refresh before another action or after an uncertain network outcome; this operation does not replay Idempotency-Key. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.comments.review({
    commentId: "0198a6ec-9e00-7000-8000-000000000001",
    revisionId: "0198a6ec-9e00-7000-8000-000000000003",
    decision: "APPROVED"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.project.ReviewComment` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CommentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Project Moderation
<details><summary><code>client.project.moderation.<a href="/src/api/resources/project/resources/moderation/client/Client.ts">test</a>({ ...params }) -> Recoms.TestModerationResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Test draft rules against example text using the current provider. Requires provider:test. No policy or comment is saved; no submission quota is consumed. The provider attempt is recorded and the separate provider-test rate limit applies. ALLOWED and DISALLOWED are successful test outcomes. Provider absence or technical failure returns PROVIDER_UNAVAILABLE rather than a content rejection. RATE_LIMITED includes Retry-After. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.moderation.test({
    moderationPolicy: "Reject unsolicited advertising; allow relevant discussion.",
    exampleComment: "Thank you for this explanation."
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.project.TestModerationPolicy` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ModerationClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Project Provider
<details><summary><code>client.project.provider.<a href="/src/api/resources/project/resources/provider/client/Client.ts">configure</a>({ ...params }) -> Recoms.ConfigureProviderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Test and save an OpenAI-compatible HTTPS provider. Requires provider:configure. Unsafe outbound destinations are blocked. Settings are saved only after a successful connection test; a failed candidate does not replace the current configuration. preserve retains an existing credential or accepts an initial one; replace requires credential; remove clears authentication. Credentials are encrypted and never returned. Refresh after PROVIDER_CONFIGURATION_CHANGED; wait Retry-After on RATE_LIMITED. Provider tests do not consume submission quota. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.provider.configure({
    baseUrl: "https://api.openai.com/v1",
    model: "gpt-4.1-mini",
    credential: "example-provider-key-not-a-secret",
    credentialIntent: "replace"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.project.ConfigureProvider` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ProviderClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.project.provider.<a href="/src/api/resources/project/resources/provider/client/Client.ts">test</a>({ ...params }) -> Recoms.TestProviderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retry the saved provider connection. Requires provider:test. Availability and last test outcome may change and the attempt is recorded, but settings are not replaced. Missing/unreadable credentials or an unusable provider return PROVIDER_UNAVAILABLE; concurrent changes return PROVIDER_CONFIGURATION_CHANGED. Respect Retry-After on RATE_LIMITED. This diagnostic call changes test state, consumes provider-test capacity and does not consume comment submission quota. Available while frozen.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.project.provider.test();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Recoms.project.EmptyProjectAction` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ProviderClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

