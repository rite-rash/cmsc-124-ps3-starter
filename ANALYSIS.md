# Environments and the Toy Evaluator

The expression `(let ((factor 2)) (let ((f (lambda (n) (* n factor)))) (let ((factor 10)) (f 3))))` demonstrates how lexical scoping works in the toy evaluator.

First, `eval-expr` at line 127 encounters the outer `let` and passes it to `eval-let` (line 67). The initial value `2` is evaluated in an empty environment, then `extend-env` (line 65) adds `(factor . 2)` to it.

Next, the second `let` creates the lambda assigned to `f`. The evaluator recognizes the lambda at line 131 and calls `make-closure` at line 132, passing the current environment. At line 90, the closure stores the environment where the lambda was defined: `(:closure (n) (* n factor) ((factor . 2)))`. The closure therefore remembers its defining environment, not the environment where it is eventually called.

The third `let` introduces another `factor`, this time with a value of `10`, creating the environment `((factor . 10) (f . closure) (factor . 2))`. When `(f 3)` is evaluated, lines 136-138 look up `f` in the current environment and evaluate the argument, resulting in `(3)`.

When `apply-closure` (line 104) executes, it extends the environment stored inside the closure instead of using the caller's environment. This produces `((n . 3) (factor . 2))`. Evaluating `(* n factor)` therefore finds `n = 3` and `factor = 2`, returning **6**.

If the evaluator used the caller's environment instead, it would find the innermost `factor`, which has a value of `10`, and return `30`. That would be dynamic scoping rather than lexical scoping, causing the E4 lexical-scope tests, which expect `3`, `101`, and `102`, to fail.

The host code needs `funcall` because CLISP follows a Lisp-2 model: variables and functions have separate namespaces. Since the closure is stored as the value of `f`, writing `(f 3)` would make CLISP look for a function named `f`; `(funcall f 3)` invokes the function stored in the variable instead. The toy evaluator follows a Lisp-1 model. At line 137, it evaluates the operator using the same `lookup` function used for variables, so `f` directly evaluates to the closure and `funcall` or `#'` is unnecessary.

For example, tracing `(+ (* a b) c)` in `((a . 6) (b . 7) (c . 6))`: `eval-expr` sees the `+` head and calls `eval-operands`, which maps `eval-expr` over the operands. The inner `(* a b)` is evaluated the same way, and `lookup` resolves `a`, `b`, and `c` to `6`, `7`, and `6`. `apply-op` then multiplies `6` and `7` to get `42`, and adds `6` to get `48`. Evaluation proceeds inner to outer, so the result is **48**.
