<!--no-pdf-->
# CMSC 124 Problem Set 3 Starter

This repository holds the CLISP prediction corpus and the toy evaluator for
Problem Set 3. The assignment manual defines the work and the submission rules.

## Members
| Name | GitHub username |
|---|---|
| Trisha Mae A. Hechenagocia | rite-rash |
| Renz Frederick P. Bañas | d3nzus |


## Layout

```text
cases/cases.lisp        the 16 forms, stored unevaluated
predictions.tsv         your value, kind, length, and operator predictions
REASONING.md            the rule behind every case, written before you run
src/evaluator.lisp      the eight functions you implement
tests/expected.tsv      every published expected result
tests/check-all.lisp    the complete public automated checker
scripts/                given runner, notation, and form-check code
run  lint  check.sh     the course run contract
```

## First Run

Fill `predictions.tsv` and `REASONING.md`, check the table's form with `./lint`,
and commit both before you evaluate a case. Then run one case or all automated
checks:

```bash
./lint
./run P01
./check.sh
```

`./lint` reads the table's form only. It never opens `tests/expected.tsv` and
never evaluates a case, so it reveals no answers and is safe to run before the
prediction commit. It catches a padded cell, a space inside a value, a tab an
editor replaced with spaces, and a missing or reordered row, all of which
survive the parse and then fail their comparisons, so without it they read like
wrong predictions. One trailing space per line fails all 16 `operator` checks
that way. `./check.sh` lists the same faults before it scores.

A fresh starter reports `1/91 checks passed` and exits 1. A complete submission
reports `91/91 checks passed` and exits 0. `check.sh` is the complete public
automated check. The expected table and the checker are both in this
repository. The assignment rubric separately assesses the written analysis,
commit history, and workflow runs.

## Reading a First Run

Read `1/91` as the starting state. The single pass is `evaluator_loads`, which asks
only whether `src/evaluator.lisp` reads without a syntax fault. The stubs
satisfy that on the first commit. A stub that signals an error when called
is still valid Lisp. Every prediction, every evaluator check, and both written
files fail. Nothing has been done.

That check earns its place anyway. `load` evaluates one form at a time, so a
truncated form at the end of the file leaves every function above it defined
and working. Without a check on the load itself, a file that stops mid-form
scores 90 of 90 and exits 0.

The workflow run in this repository's Actions tab is red for the same reason
the score is low. It stays red until a pair completes the assignment. That is
the correct state for a starter. Your fork's run goes green when you finish.

## The Four Check Groups

`src/evaluator.lisp` is scored in four groups that stand on their own.

| Group | What works | Functions |
|---|---|---|
| E1 | numbers evaluate to themselves, symbols resolve through the environment | `eval-expr`, `lookup` |
| E2 | `(+ a b)`, `(- a b)`, `(* a b)` over recursively evaluated operands | `eval-operands`, `apply-op` |
| E3 | `let` extends the environment for its body and nothing else | `extend-env`, `eval-let` |
| E4 | `lambda` builds a closure, application joins the arguments to it | `make-closure`, `apply-closure` |

A group you never reach costs you that group only. Deleting `make-closure`
leaves E1, E2, and E3 at full marks and zeroes E4.

## Why `ps3-load` Instead of `load`

`check.sh`, `lint`, and `run` all start CLISP with a small bootstrap that
defines `ps3-load`, and every Lisp file here uses it in place of `load`.

That works around a Windows bug. GNU CLISP 2.49 on Windows answers `Win32 error
267 (ERROR_DIRECTORY)` for `(load "any/path.lisp")` in every directory, which
also breaks passing a script as a command-line argument. Loading from an
already-open stream works, so `ps3-load` opens the file first and hands `load`
the stream. The same bootstrap runs on all three platforms. You learn one
contract.

## When the Grader Stops Early

`predictions.tsv` has to keep its five columns and all 16 ids in order. When it
doesn't, the grader stops before the first check and prints why. A run that
ends without a `== result ==` line never scored anything, so read the message
and repair the table's columns and ids before you look at your answers.

Fields are compared exactly. `integer` passes and ` integer` doesn't, so don't
pad a cell to line the columns up in your editor.

## Exit Codes

The course contract.

| Code | Command | Meaning |
|---|---|---|
| 0 | `./check.sh` | all 91 checks passed |
| 1 | `./check.sh` | at least one check failed |
| 0 | `./lint` | `predictions.tsv` is well formed and complete |
| 1 | `./lint` | the table is malformed or still holds a TODO |
| 0 | `./run <case>` | the case was found and printed |
| 64 | `./run <case>` | called with the wrong number of arguments |
| 65 | `./run <case>` | the case id is unknown |

## Tested Toolchains

Every row below is a run that happened.

| Environment | Version reported by `clisp --version` | Result |
|---|---|---|
| Windows 11 25H2, Git Bash, the 2.49 win32-mingw zip | `GNU CLISP 2.49 (2010-07-07)` | `1/91` on the starter, `91/91` with the instructor solution |
| WSL Ubuntu 24.04, `apt-get install clisp` | `GNU CLISP 2.49.93+ (2018-02-18)` | `1/91` on the starter, `91/91` with the instructor solution |
| GitHub Actions, `ubuntu-latest`, `apt-get install clisp` | `GNU CLISP 2.49.93+ (2018-02-18)` | `1/91` on the starter |
| GitHub Actions, `macos-latest`, `brew install clisp` | `GNU CLISP 2.49.92 (2018-02-18)`, arm64 | `1/91` on the starter |
| GitHub Actions, `windows-latest`, the 2.49 win32-mingw zip | `GNU CLISP 2.49 (2010-07-07)` | `1/91` on the starter |

Three CLISP builds grade this assignment and eight years separate the oldest
from the newest. They agree. The grader re-derives all 64 published
expectations before it scores anything, so a build that printed one value
differently, or classified one head differently, would stop the run and say so.
No runner has. `./run P01` also prints the same four fields on all three.
