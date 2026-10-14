# Features to add

Timeline starts small. These are features it could have next. Each one is small enough for one
plan, one branch and one pull request.

Pick any of them, in any order, or add your own. **Write a plan for each one first**, in
`docs/plans/<feature>.md`, from the template in `docs/plans/TEMPLATE.md`. The two plans already in
`docs/plans/` are for features that are not on this list; read them, and have them reviewed,
before you build them.

## How to read the table

- **Parts** are the parts of the app the feature changes: the **page** (`app.js`, `index.html`,
  `style.css`), and the **controller**, **model** and **view** of `server.py`.
- **New table** says whether the feature changes the database. Two branches that both change
  `create_tables` will clash when the second one merges: build those one after the other.
- **Size**: **S** is a few lines and a test or two. **M** changes the server and the page. **L**
  needs a decision about the product before it can be built, and the plan should say what you
  decided and why.

| # | Feature | Parts | New table | Size |
|---|---|---|---|---|
| 1 | **Reply to a post.** A reply shows under the post it answers. | page, controller, model, view | yes | M |
| 2 | **Edit your own post.** It then shows *edited*. Decide who counts as "you" in an app with no accounts. | page, controller, model, view | yes | L |
| 3 | **A person's page.** Click a name to see only that person's posts. | page, controller, model | no | S |
| 4 | **Follow someone,** and a *following* timeline. Decide who is following, with no accounts. | page, controller, model, view | yes | L |
| 5 | **The date, not only the time,** for posts older than today. | model, view, page | yes | S |
| 6 | **Search** the posts for a word. | page, controller, model | no | S |
| 7 | **`#tags`.** A word starting with `#` becomes a link to every post with that tag. | page, controller, model | no | M |
| 8 | **Warn before the limit.** The count turns amber at 260 characters, before it turns red. | page | no | S |
