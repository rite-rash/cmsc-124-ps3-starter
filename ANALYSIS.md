# Environments and the Toy Evaluator

Replace this paragraph after your checks pass. Trace one form through your own
`eval-expr`, point to the line that captures the defining environment, say what
would change if application joined the arguments to the caller's environment
instead, and explain why the host CLISP needs `funcall` where the toy language
doesn't.



The form I'll trace is the expression (+(*a b) c) with the environment ((a.6) (b.7) (c.6)).

First, eval-expr gets a list whose head is + triggering third condition which calls eval-operands, whose lambda uses mapcar to run eval-expr on each operand. The inner (*a b) goes through the same steps, and for each symbol the second condition calls lookup, which finds a as 6, b as 7 and c as 6. Then apply-op is handed the operator and the list of evaluated values. We assign the values to x and y accordingly then we apply the operation to them depending on operator given. Since this is recursion, it is always the case that we evaluate inner to outer. Hence, we evaluate 6 * 7 first giving 42 then 42+6 next giving 48.

The need for funcall is tied around the concept of namespaces. Toy language only uses a single namespace for both variable and function names, while the host has two separate namespaces: one for functions and another for variables. Specifically, we need funcall if a function is held by a variable, which is also the times where defun can't be used like in local scopes. For instance, evaluating (f 1) in host is okay if defined with defun since it automatically checks the function namespace, but if f is held by a variable, we need to use (funcall f1) to explicitly invoke the function. Conversely, in eval-expr, lookup evaluates f like any standard variable since the environment is shared and function bindings live in the same table, allowing (f 1) to work in the toy language without funcall.
