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
Replace this line with the rule behind this case's four fields.

## P03
The value is nil as it is. The kind is null since nil evaluates to itself. The length is 0 since
nil is an empty list and operator is none since there is no head.

## P04
Replace this line with the rule behind this case's four fields.

## P05
The value is t since symbolp returns true if the statement is a symbol and nil is a
 symbol. The kind is a symbol since according to the manual t reads symbol. Length is
none since this isn't a list and operator is function since head is symbolp.

## P06
Replace this line with the rule behind this case's four fields.

## P07
The head of the list is the function cons so quoter gives (2,3) untouched then put the element
1 in front of the list giving (1,2,3). Kind is cons since, result is a nonempty list with length
of 3 since there are 3 elements in the list..

## P08
Replace this line with the rule behind this case's four fields.

## P09
The head is an and which is special-operator and it evaluates to nil if even one of its subforms 
is nil which is the case in here given the second subform thus the value is nill. Kind is null 
since nil reads null while the length here is 0 since nil is an empty sequence.

## P10
Replace this line with the rule behind this case's four fields.

## P11
The head is defun which is a macro and what it returns is the symbol or the name of the function it 
defined which in here is ps3-twice. Since it's not a list, length is none.

## P12
Replace this line with the rule behind this case's four fields.

## P13
mapcar is a function, so its arguments are evaluated first; it applies length to each element ((1 2) is 2,
() is 0, (3) is 1) and collects the results into

## P14
Replace this line with the rule behind this case's four fields.

## P15
As stated in the manual, lambda is a macro. It's value is a function object, 
written #<function> since it is an anonymous function meaning it creates a 
function without a name. A function object has a kind function and length is
none since it is not a sequence.

## P16
Replace this line with the rule behind this case's four fields.
