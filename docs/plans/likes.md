# Plan: like a post

Status: draft, not yet reviewed.

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
| `with-backend/server.py` | model: `like_post(db_path, post_id, name)` | add: finds the user with `user_id_for`, adds a row to `likes`, and adds 1 to the post's `like_count` |
| `with-backend/server.py` | controller: `do_POST` | change: a new address, `POST /posts/<id>/like`, with `{"name": "Aiko"}`. Before calling `like_post`, it looks up the post, answers 404 if there is none, and answers 400 if this name is already in `likes` for this post |
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

## 5. Decisions for me

1. **Can you like your own post?** Most sites allow it. *Recommendation: allow it.*
2. **Show the count when it is 0?** *♥ 0* under every new post is noise. *Recommendation: show
   only the heart until the first like.*

After the review, each objection, *accept* or *reject*, and *why*:

-
