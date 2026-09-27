# david-pwa-blog-comment

Comment storage for [seunghun-website.vercel.app](https://seunghun-website.vercel.app).

This repository holds **no code**. Every issue here is one blog post's comment thread,
created automatically by [utterances](https://utteranc.es/).

## Why it is separate

utterances stores comments as GitHub issues, and it can only do that in a **public**
repository — visitors need to be able to read and open issues.

The blog's source is private. Keeping comments in their own public repository lets both
be true at once:

| | Source repo | This repo |
|---|---|---|
| Visibility | private | **public** (required) |
| Contains | Next.js app, 246 markdown posts, drafts | only issues |
| Issues used for | project tracking | one per blog post |

## How a thread is created

1. A visitor opens `https://seunghun-website.vercel.app/blog/<slug>`
2. The utterances script loads and searches this repository for an issue whose title
   matches the page's **pathname** — `blog/<slug>`
3. If none exists, the first person to comment causes utterances to open one

Configured in the blog source at `lib/site.ts`:

```ts
export const comments = {
  repo: "a1603169/david-pwa-blog-comment",
  issueTerm: "pathname",
} as const;
```

## Requirements — do not change these

- **Keep this repository public.** Making it private silently breaks every comment thread;
  the widget renders an error instead of comments.
- **Keep Issues enabled** (Settings → Features → Issues).
- **Keep the utterances app installed** on this repository:
  <https://github.com/apps/utterances>
- **Do not rename issues.** The title *is* the lookup key. Renaming an issue detaches it
  from its post.
- **Do not rename post files in the blog.** `issueTerm: "pathname"` means the URL is the
  key, so changing `/blog/<slug>` orphans that post's thread just as surely as renaming
  the issue would.

## Moderation

- Delete a comment: open the issue here and delete the comment
- Close a thread: lock the issue (Lock conversation) — the post then shows existing
  comments but accepts no new ones
- Block someone: GitHub account settings → Blocked users

## History

Comments used to live in the blog's own repository. When that repository went private,
threads moved here. Issues created before the move stayed behind, so a handful of older
posts show no comments even though replies exist on the original issues.
