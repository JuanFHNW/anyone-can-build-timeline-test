# Plan: delete your own post

Status: draft, not yet reviewed.

*No real person's name or data in a plan or a prompt.*

## 1. What it does

Under each of your own posts, a **Delete** button. Only the person who wrote a post can delete it,
and once it is deleted it is gone from every window.

## 2. Files it touches

| File | Function or section | Add or change |
|---|---|---|
| `with-backend/server.py` | model: `delete_post(db_path, post_id)` | add: deletes the post's row; raises `RuleBroken` if there is no such post |
| `with-backend/server.py` | controller: `do_POST` | change: a new address, `POST /posts/<id>/delete`, with no body; calls `delete_post`, answers 204, or 404 if there was no such post |
| `with-backend/app.js` | `showPost` | change: draws **Delete** only on posts whose author is the name in the *Your name* box, so nobody sees a Delete button on someone else's post |
| `with-backend/app.js` | `checkForNewPosts` | no change needed: the other windows drop a deleted post on their next check, within a second |
| `with-backend/style.css` | `.post-delete` | add |
| `with-backend/test_server.py` | `ModelTests` | add: two tests |

## 3. Done when

- When a person deletes their post, it should be gone from the database.
- When a post does not exist, the server should answer 404.
- When a post is deleted in one window, every other window should lose it within a second.
- Test count before: 12. Test count when done: 15.

## 4. Not in this plan

- Undo.
- Deleting by staff.
- Deleting in the page-only version.

## 5. Decisions for me

1. **Ask first?** A *Delete this post?* question stops a slip of the finger. *Recommendation: ask,
   with the browser's own `confirm()`.*
2. **What about the user?** When a person deletes their only post, their row in `users` stays.
   *Recommendation: leave it; it is one short row.*

After the review, each objection, *accept* or *reject*, and *why*:

-
