# Design doc — <your product>

*Every name and example row here is made up.*

Timeline's own `DESIGN.md` is the worked example. Its sections 3, 4 and 6 have older names:
Screens, The three parts, and How I will know it works; they are the same sections. Its section 8,
Build or borrow, is left out here.

---

*The product spec*

## 1. The problem

Who has the problem, what happens to them, how often, and what they do now.

## 2. Not in this version

What this version will not do, so nobody builds it by accident.

## 3. User flow

Each screen, what is on it, and its one job. What a person does to move from one screen to the
next.

---

*The engineering design*

## 4. Architecture

The frontend, the backend and the database: what each one is here, and why.

## 5. The data model

The tables, their columns, how they point at each other, and example rows. (`docs/maker.md` can
propose this; you decide.)

## 6. Acceptance criteria

Sentences a machine can settle: *when ___, it should ___*. At least one edge case.

---

## 7. Adversarial review

| Objection | About | Decision | Why | What changed |
|---|---|---|---|---|
| | product or design | accept or reject | | |

(`docs/checker.md` finds the objections; you decide each one.)
