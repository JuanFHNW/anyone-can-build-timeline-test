# AGENTS.md — for the AI agent working in this repository

The person asking you may be new to programming, and may read English as a second language.
Explain in short, plain sentences, and define a technical word the first time you use it. When you
change code, say which file changed and why.

## What this is

Timeline is a small app where people post short messages, and everyone's posts appear on one
timeline, newest first. It comes in two versions with the same screen:

- `page-only/` runs only in the browser. It has no server, so nothing is shared between windows.
- `with-backend/` has a small Python server and a database, so every window sees every post.

There are no accounts: a person types a name. `DESIGN.md` says how it works and why;
`FEATURES.md` lists what to build next.

## What each file does

| File | Its one job |
|---|---|
| `page-only/index.html` | The parts of the screen: name, post box, Post button, timeline. |
| `page-only/style.css` | How the screen looks. |
| `page-only/app.js` | Checks the rules and keeps the posts in this window's `sessionStorage`. |
| `with-backend/index.html` | The same screen as `page-only/index.html`. |
| `with-backend/style.css` | The same look as `page-only/style.css`. |
| `with-backend/app.js` | Sends each post to the server, and asks the server for new posts every second. |
| `with-backend/server.py` | The backend, in three labelled parts: **controller**, **model**, **view**. |
| `with-backend/test_server.py` | The tests for `server.py`. |
| `with-backend/timeline.db` | The database. The server creates it when it starts. It is not in git. |
| `docs/plans/` | One plan for each piece of work, from `TEMPLATE.md`. |
| `docs/reviewer.md` | The prompt for an adversarial review of a plan or a diff, in a new session. |
| `docs/design-template.md` | The empty design doc, in the same eight sections as `DESIGN.md`. |
| `docs/maker.md` | The prompt that proposes section 5 of a design doc, the data model. |
| `docs/checker.md` | The prompt for an adversarial review of a design doc, with no context: a subagent, or a new session. |
| `docs/prompts/` | Every prompt typed into Claude Code here, word for word. Written by `.claude/hooks/log_prompt.py`. |
| `Makefile` | Short commands. `make help` lists them. |

The three parts of `server.py`:

- **Controller** (`TimelineHandler`): reads each request and picks what to do.
- **Model** (`check_rules`, `user_id_for`, `save_post`, `posts_after`): the rules a post must
  follow, and the database, in two tables: `users` (each person once) and `posts` (each post
  points at its author by `author_id`).
- **View** (`post_to_json`, `posts_to_json`): turns database rows into the JSON the page reads.

## Commands

Run every Python command through `uv run`, so it uses this project's Python and tools.

- `make setup` — install Python and the tools, turn on the commit checks
- `make run` — start the server at <http://localhost:8009>
- `make test` — every test, and how many ran
- `make lint` / `make format` — Ruff
- `make check` — every commit check on every file, then every test
- `make worktree BRANCH=name` — a branch in its own folder, `.claude/worktrees/name`

See what is saved: `sqlite3 with-backend/timeline.db 'select * from users; select * from posts'`.

## Rules

- **Run `make check` before every commit.**
- **Keep the three parts of `server.py` separate. A new rule goes in the model.** The controller
  does not check rules and does not touch the database. The view does not decide anything.
- **The server checks every rule**, even when the page also checks it, because a user can change
  anything that runs on their own device. Hiding a button is not a check.
- **A name is kept once, in `users`.** Never copy it into another table.
- **The app uses only the Python standard library.** No packages for the app, and no build step
  for the page. The tools around it (Ruff, pre-commit) are added with `uv add --dev`.
- **Never edit or delete anything in `docs/prompts/`.**
- **Add or change a test with every change in behaviour**, and show the person the test failing
  before the fix and passing after.
