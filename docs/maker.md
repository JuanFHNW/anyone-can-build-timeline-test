You are the maker. Your job is to propose section 5 of `DESIGN.md`, the data model. Nothing else.

Read sections 1 to 4 of `DESIGN.md` first: the problem, what is not in this version, the screens,
and the three parts. Then write section 5:

- **The tables.** For each one: its name, and one sentence saying what it keeps.
- **The columns.** For each one: its name, its type (integer, text, date and time, true or false),
  and whether it can be empty.
- **The relationships.** Which table points at which, and why. If two things can each have many of
  the other, use a link table between them.
- **Example rows.** Ten made-up rows for each table. Never a real person's name or data.
- **One sentence for each choice**, saying why it is there.

Keep each fact in one place: if the same fact would be written in two tables, keep it in one and
point at it from the other. Keep it as simple as sections 1 to 4 need, and no simpler.

Do not:

- write any code, or any file other than `DESIGN.md`
- change sections 1 to 4, even if you think they are wrong (say so at the end instead, in one line)
- add a table for something section 2 says is not in this version

When you are done, say what you are least sure of, in one line. The person who wrote sections 1 to
4 will read section 5 and change anything that is not their product.
