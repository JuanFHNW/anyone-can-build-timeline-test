# Plan: like a post

Status: reviewed and agreed.

*No real person's name or data in a plan or a prompt.*

## 1. The idea

- **The problem:** a person reads a post they agree with, and the only way to say so is to write a
  reply post of their own.
- **What it costs:** the timeline fills with short posts that only say *same* or *yes*, and the
  person who wrote the first post cannot tell how many people agreed.
- **The change:** under each post, a **♥ Like** button and a count: *♥ 4*. A person can like a
  post once, so the count is the number of people who liked it.
- **What gets better:** fewer *same* posts, and a writer can see which posts people liked.
- **Checked with a real person:** not yet.

## 2. Files it touches

| File | Function or section | Add or change |
|---|---|---|
| `with-backend/server.py` | model: `create_tables` | add: a `likes` table, `post_id` and `user_id`, unique on the pair; and a `like_count` column on `posts`, starting at 0, so the timeline can show the count without counting rows |
| `with-backend/server.py` | model: `like_post(db_path, post_id, name)` | add: validates name, checks post existence (raises KeyError if missing), finds user with `user_id_for`, checks for duplicate like (raises `RuleBroken`), inserts into `likes`, increments `like_count` on `posts`, and returns the updated post row |
| `with-backend/server.py` | controller: `do_POST` | change: a new address, `POST /posts/<id>/like`, with `{"name": "Aiko"}`. Parses JSON, calls `like_post`, and returns 200 with the updated post, 400 on `RuleBroken`, or 404 on `KeyError` |
| `with-backend/server.py` | view: `post_to_json` | change: adds `"likes"`, from `like_count` |
| `with-backend/app.js` | `showPost` | change: the button and the count; pressing it sends the name in the *Your name* box |
| `with-backend/style.css` | `.post-like` | add |
| `with-backend/test_server.py` | `ModelTests`, `RealServerTest` | add: three tests |

## 3. Done when

- When a person likes a post, its count should go up by one.
- When the same person likes the same post again, the server should answer 400 and the count
  should not change.
- When a post does not exist, the server should answer 404.
- Test count before: 12. Test count when done: 15.

## 4. Not in this plan

- Taking a like back.
- Showing who liked a post.
- Likes in the page-only version.
- Real-time updates of like counts on already-loaded posts across other windows without reload.

## 5. Decisions for me

1. **Can you like your own post?** Most sites allow it. *Decision: accept recommendation; allow it.*
2. **Show the count when it is 0?** *♥ 0* under every new post is noise. *Decision: accept recommendation; show only the heart until the first like.*

After the review, each objection, *accept* or *reject*, and *why*:

- **Objection 1 (serious):** Controller in row 2 was planned to look up posts and check for duplicate likes in the database, violating `AGENTS.md` separation of concerns.
  *Decision:* Accept.
  *Why:* Move all database queries, post lookup, and rule checking into `like_post` in the model. The controller only handles HTTP parsing and status mapping.
- **Objection 2 (serious):** Name validation for the liker was not specified, allowing blank or oversized names.
  *Decision:* Accept.
  *Why:* `like_post` will validate that `name` is non-empty and within `MAX_AUTHOR` characters, raising `RuleBroken` if invalid.
- **Objection 3 (minor):** Storing `like_count` on `posts` creates redundant state alongside the `likes` table.
  *Decision:* Reject.
  *Why:* Denormalizing `like_count` allows polling every second without running an expensive `COUNT(*)` / join on every query. The increment and insert will happen in the same database transaction.
- **Objection 4 (minor):** `app.js` polls only posts with `id > lastId`, so other open windows will not see like count changes on older posts until reload.
  *Decision:* Accept.
  *Why:* Documented in section 4 (*Not in this plan*) to keep scope focused on the simplest working implementation.
