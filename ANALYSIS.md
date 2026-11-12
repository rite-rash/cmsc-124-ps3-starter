# Environments and the Toy Evaluator

The expression `(let ((factor 2)) (let ((f (lambda (n) (* n factor)))) (let ((factor 10)) (f 3))))` demonstrates how lexical scoping works in the toy evaluator.

First, `eval-expr` at line 127 encounters the outer `let` and passes it to `eval-let` (line 67). The initial value `2` is evaluated in an empty environment, then `extend-env` (line 65) adds `(factor . 2)` to the environment. The body is then evaluated using this updated environment.

Next, the second `let` creates the lambda assigned to `f`. The evaluator recognizes the lambda expression at line 131 and calls `make-closure` at line 132, passing the current environment. At line 90, the closure stores the parameter, body, and environment where it was defined: `(:closure (n) (* n factor) ((factor . 2)))`. This is important because a closure remembers the environment where the lambda was created, rather than the environment where it is eventually called.

The third `let` introduces another `factor`, this time with a value of `10`. This creates a new environment: `((factor . 10) (f . closure) (factor . 2))`. When `(f 3)` is evaluated, the application branch at lines 136–138 looks up `f` in the current environment and evaluates the argument, resulting in `(3)`.

When `apply-closure` (line 104) executes, it extends the environment stored inside the closure instead of using the current caller environment. This produces `((n . 3) (factor . 2))`. As a result, evaluating `(* n factor)` finds `n = 3` and `factor = 2`, giving us **6**.

If the evaluator instead used the caller's environment when applying the closure, it would find the innermost `factor`, which has a value of `10`. The expression would then return `30` instead. This would be dynamic scoping rather than lexical scoping, causing the E4 lexical-scope tests, which expect `3`, `101`, and `102`, to fail.

Lastly, the host code needs `funcall` because CLISP follows a Lisp-2 model, meaning variables and functions have separate namespaces. Since the closure is stored as the value of the variable `f`, writing `(f 3)` would make CLISP look for a function named `f` instead. Using `(funcall f 3)` tells it to invoke the function stored in the variable.

The toy evaluator, on the other hand, follows a Lisp-1 model. At line 137, it evaluates the operator expression using the same `lookup` function used for regular variables. This means `f` directly evaluates to the closure, so there is no need for `funcall` or `#'`.


The form I'll trace is the expression (+(*a b) c) with the environment ((a.6) (b.7) (c.6)).

First, eval-expr gets a list whose head is + triggering third condition which calls eval-operands, whose lambda uses mapcar to run eval-expr on each operand. The inner (*a b) goes through the same steps, and for each symbol the second condition calls lookup, which finds a as 6, b as 7 and c as 6. Then apply-op is handed the operator and the list of evaluated values. We assign the values to x and y accordingly then we apply the operation to them depending on operator given. Since this is recursion, it is always the case that we evaluate inner to outer. Hence, we evaluate 6 * 7 first giving 42 then 42+6 next giving 48.

The need for funcall is tied around the concept of namespaces. Toy language only uses a single namespace for both variable and function names, while the host has two separate namespaces: one for functions and another for variables. Specifically, we need funcall if a function is held by a variable, which is also the times where defun can't be used like in local scopes. For instance, evaluating (f 1) in host is okay if defined with defun since it automatically checks the function namespace, but if f is held by a variable, we need to use (funcall f1) to explicitly invoke the function. Conversely, in eval-expr, lookup evaluates f like any standard variable since the environment is shared and function bindings live in the same table, allowing (f 1) to work in the toy language without funcall.
