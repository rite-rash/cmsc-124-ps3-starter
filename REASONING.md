# Reasoning for the Predictions Table

Write one entry under every heading below, then commit this file together with
`predictions.tsv`, before you run a case or open `tests/expected.tsv`.

Each entry names the rule that produces the four predicted fields for that
case. One to three sentences is enough. The grader checks structure only. It
does not judge quality, and it sets no word minimum.

After you run the cases, add a short correction under each case you predicted
wrongly. Say what you predicted, what CLISP printed, and which rule you
misread. Those entries belong in the correction commits.

## P01
The head of the list is a function +. By the rule, the arguments are evaluated inside-out.
So evalluating the inner which is (* 2 3) gives 6 then 1 + 6 which gives 7. The output value 
is a whole number thus it's an integer. The length is none since it isn't a sequence.

## P02
The ' is a special operator called a quote. It essentially says to keep the input as is, skipping evaluation. That is why it returns the cons and maintains its length of 3.

## P03
The value is nil as it is. The kind is null since nil evaluates to itself. The length is 0 since
nil is an empty list and operator is none since there is no head.

## P04
The t symbol is a symbol that denotes the true boolean value. In the same way that inputting just true in a condition parameter in other languages always returns true, its the same here. Boolean values dont have lengths and there is no operation performed.

## P05
The value is t since symbolp returns true if the statement is a symbol and nil is a
 symbol. The kind is a symbol since according to the manual t reads symbol. Length is
none since this isn't a list and operator is function since head is symbolp.

## P06
The car keyword takes the first item of a list but the input is an empty list with no first item so it returns null, in this case nil which doesnt have a length.

## P07
The head of the list is the function cons so quoter gives (2,3) untouched then put the element
1 in front of the list giving (1,2,3). Kind is cons since, result is a nonempty list with length
of 3 since there are 3 elements in the list..

## P08
The (null '()) condition of the if statement yeilds true, so it picks the true branch which has a quote operator thus it's a symbol which has no length. The if keyword is a special operator.

## P09
The head is an and which is special-operator and it evaluates to nil if even one of its subforms 
is nil which is the case in here given the second subform thus the value is nill. Kind is null 
since nil reads null while the length here is 0 since nil is an empty sequence.

## P10
The defvar keyword makes ps3-rate a special variable, so it is looked up when the function runs. The let changes it to 10 while ps3-scale is called inside it. Its then multiplied by 3 yielding 30, an integer with no length.

## P11
The head is defun which is a macro and what it returns is the symbol or the name of the function it 
defined which in here is ps3-twice. Since it's not a list, length is none.

## P12
First 3 is consed into (down 2), then 2 onto (down 1), then 1 onto (down 0) which finally gives '(). The special outer operator is labels that defines the down operator allowing us to build the list starting with the inner operators.

## P13
mapcar is a function, so its arguments are evaluated first; it applies length to each element ((1 2) is 2,
() is 0, (3) is 1) and collects the results into

## P14
The mapcar keyword runs the function on each item and collects the results into a new list. Each of the elements is then squared. The value is a cons, with unchanged length still remaining 3.

## P15
As stated in the manual, lambda is a macro. It's value is a function object, 
written #<function> since it is an anonymous function meaning it creates a 
function without a name. A function object has a kind function and length is
none since it is not a sequence.

## P16
The f in the 2nd to outer let in the nest captures factor = 2 when it is made, that the inner let makes a separate variable, and that the value is 3 × 2, yeilding 6 which is an integer with no length. 