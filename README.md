# Timeline

A small app where people post short messages. Everyone's posts appear on one timeline, newest
first. It comes in two versions with the same screen:

- **`page-only/`**: everything runs in the browser. There is no server. Each window keeps its own
  posts, so nothing is shared.
- **`with-backend/`**: the page sends each post to a small Python server, which saves it in a
  database. Every window asks the server for new posts once a second, so every window sees every
  post.

The difference between the two is the reason a backend exists. [`FEATURES.md`](FEATURES.md) lists
what to build next, and [`DESIGN.md`](DESIGN.md) says how it works and why.

## Get your own copy

Your copy is a **private** repository on your own GitHub account. You make it in four steps:
clone this one, make an empty repository of your own, push to it, and add your teacher.

**1. Clone this repository.** In a terminal, in the folder where you keep your projects:

```bash
git clone https://github.com/kreativitea/anyone-can-build-timeline.git timeline-rehearsal
cd timeline-rehearsal
```

It goes into a folder called `timeline-rehearsal`, so it does not clash with a copy of Timeline you
may already have.

**2. Make an empty repository on GitHub.** On <https://github.com>, press **+** at the top right,
then **New repository**.

- **Repository name:** `timeline-rehearsal`
- Choose **Private**.
- Leave **Add a README**, **.gitignore** and **license** turned off. The repository must be
  empty, or the push in step 3 is refused.

Press **Create repository**.

**3. Push your copy to it.** Put your GitHub name where it says `YOUR-NAME`:

```bash
git remote rename origin upstream
git remote add origin https://github.com/YOUR-NAME/timeline-rehearsal.git
git push -u origin main
```

The first line keeps the original repository under the name `upstream`. The second makes your new
repository `origin`, the one `git push` sends to from now on. You never push to `upstream`.

If git asks you to log in, run `gh auth login` and try the push again.

**4. Add your teacher as a collaborator.** In your new repository on GitHub, open **Settings**,
then **Collaborators**, then **Add people**, and add
[**kreativitea**](https://github.com/kreativitea) (Michael Omoto).

**Check:** reload your repository's page on GitHub. It says **Private** next to its name, it has
all the files, and **Settings → Collaborators** shows kreativitea, as invited or added.

Your pull requests now open in your own repository, with your own `main` as the base.

## Your prompts are written down

Every prompt you type into Claude Code in this repository is saved, word for word, in
`docs/prompts/`, by a hook in `.claude/settings.json`. **Commit those files with your work**, and
never change what is already in them. Only your words are saved, not the AI's answers.
[`docs/prompts/README.md`](docs/prompts/README.md) has the details, and what to do if you use a
different AI tool.

The first time you open Claude Code in this folder, it asks whether you trust it. Say yes, or the
hook does not run.

## Run it on your laptop

**1. Install uv, once.** uv installs Python and this project's tools for you.

Mac:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows (PowerShell):

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Close the terminal and open a new one, then check with `uv --version`.

**2. Set it up.** This installs Python and the tools, and turns on the commit checks:

```bash
make setup
```

Without `make` (on Windows), the same is:

```bash
uv sync
uv run pre-commit install
```

**3. Start the server.**

```bash
make run
```

Open <http://localhost:8009>. Each new post prints one line in the terminal: the time, who wrote
it, and what it says. Press `Ctrl+C` in the terminal to stop the server. Without `make`:
`cd with-backend`, then `uv run python server.py`.

The page-only version needs no server: open `page-only/index.html` in a browser.

**4. Run the tests.**

```bash
make test
```

You should see `Ran 12 tests` and `OK`. **Write down the count**: every feature you add should
make it go up. Without `make`: `cd with-backend`, then `uv run python -m unittest -v`.

`make help` lists every command.

## See the difference

Open the app in two windows side by side, one of them a **private window** (Chrome: Incognito,
Safari: Private Window), and post from each. With the backend, a post from one window appears in the
other within a second. Page-only, it never does: each window keeps only its own posts. Open a new
window rather than duplicating a tab, because a duplicated tab copies the first tab's
`sessionStorage`.

Stop the server with **Ctrl+C**, and both windows say *Cannot reach the server*. Start it again with
`make run`, and they recover by themselves.

## Open the store

The backend keeps everything in one file, `with-backend/timeline.db`, in two tables: `users`,
with each person once, and `posts`, where each post points at its author by number. To see what is
inside:

```bash
sqlite3 with-backend/timeline.db 'select * from users; select * from posts'
```

A user line is `id|name`. A post line is `id|author_id|text|posted_at`: the `author_id` is the
`id` of a user.

To start again with an empty timeline, stop the server and run `make reset`.

## Build a feature

1. Pick a feature from [`FEATURES.md`](FEATURES.md).
2. Write its plan in `docs/plans/<feature>.md`, from `docs/plans/TEMPLATE.md`. Two plans are
   already there, for features not on the list. They have not been reviewed: review them before
   you build either of them.
3. Have the plan reviewed (`docs/reviewer.md`, in a new session), build it on its own branch
   (`make worktree BRANCH=<feature>`), have the result reviewed, and check it in the browser.
4. Open a pull request in your repository, in your own words, with the plan and the prompts in it.
   Merge it when `make check` passes and you can explain every change.

## Write a design doc

`DESIGN.md` is Timeline's own design doc: read it as the example. To write one for your own product,
copy the files in `docs/design-doc/` into your product's repository. If your idea is already a slide
deck (a press release and FAQ), save it as a PDF in the same folder, and start with
**`from-your-deck.md`**: your agent gives feedback on the idea, you decide what to keep, and then it
drafts sections 1 to 4 for you to edit. Otherwise:

1. **`design-template.md`**, as your `DESIGN.md`. Write sections 1 to 4 yourself, in your own words.
2. **`maker.md`**. Ask your agent to read it and follow it: it proposes section 5, the data model.
   Change anything that is not your product.
3. **`checker.md`**. Start a reviewer with no context, a subagent or a new session, and have it
   read `checker.md` and follow it. Write each objection in section 7, with *accept* or *reject*,
   and *why*.

Then commit, and add one line to your `AGENTS.md`: *Read `DESIGN.md` before any change.*

## Checks

| Command | What it does |
|---|---|
| `make lint` | Finds mistakes and bad habits in the Python code (Ruff) |
| `make format` | Rewrites the Python code in the standard style (Ruff) |
| `make check` | Every commit check on every file, then every test |

The commit checks run by themselves on every `git commit`. If a check fails because it fixed a
file for you, look at the change, `git add` the file, and commit again.

Keep every name and every example row made up. Never a real person's data, in a post, a test, a
plan or a prompt.
