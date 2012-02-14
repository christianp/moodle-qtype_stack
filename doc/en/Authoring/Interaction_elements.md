# Interaction elements

Interaction elements are the points at which the student interacts with the question.
For example, it might be a form box into which the student enters their answer.

* Only the [question stem](CASText#Question_stem) may have interaction elements. 
* Interaction elements are not required. Hence it is possible for the teacher to make a
  statement which asks for no response from the student, i.e. a rhetorical question.
* A question may have as many interaction elements as needed.
* Interaction elements can be positioned anywhere within the
  [question stem](CASText#Question_stem).  If JSMath is used for display this includes within equations.  MathJax does not currently support this feature.

The position of an interaction element in the [question stem](CASText#Question_stem) is denoted by

	#stuff#
 
Here `stuff` denotes the name of a [Maxima](../CAS/Maxima) variable to which the student's answer is to be assigned.
This must only be letters (optionally) followed by numbers, e.g.

	#ans1#

No special characters are permitted.

Feedback as to the syntactic validity of a response is by default inserted just after
the interaction element. Feedback is positioned using tags such as

	<IEfeedback>stuff</IEfeedback>
 
where stuff is the name of the variable. This string is automatically generated if it
does not exist and is placed after the interaction element. This feedback must be given.
Interaction elements have a number of options. Specific interaction elements may have extra options.

## Interaction element options ##

Each interaction element may have a number of options.

## Student's Answer Key ##  {#Answer_Key}

The maxima variable to which the student's answer is assigned.
This is set in the Question Stem by enclosing the variable name between hash symbols, e.g.
	
	#ans1#

Internally you can refer to the student's answer using the variable name `ans1`.

### Input Type ### {#Input_Type}

Currently STACK supports the following kinds of interaction elements.   

#### Algebraic input ####

The default: a form box.

#### Algebraic (NUMBAS) ####

A form box with a Javascript overlay to "see as you type".  (Experimental)

#### True/False ####

Simple drop down. A Boolean value is assigned to the variable name.

#### Single Character ####

A single letter can be entered.  This is useful for creating multiple choice question

#### Drop down list ####

Use the Input Type Options field to indicate a comma separated list of possible values.

#### Matrix ####

The size of the matrix is inferred from the teacher's answer.
STACK then adds an appropriate grid of boxes (of size Box Size) for the student to fill in.
This is easier than typing in [Maxima](../CAS/Maxima)'s matrix command, but does give the game away about the size of the required matrix.

#### Text area ####

Enter algebraic expressions on multiple lines.  STACK passes the result to [Maxima](../CAS/Maxima) as a list.

#### Slider ####

(New in STACK 2.2) Draggable slider bar resulting in a numerical value.  Useful for [confidence testing](../Diagnostics/Confidence_testing).

### Teacher's Answer ###  {#Teacher_Ans}

**This field is compulsory.** Every interaction element must have an answer, although this answer is not necessarily the unique correct answer. 
This value will be available as a question variable named `tans`**`n`** (where **`n`** is 1, 2, ...)

### Box Size ### {#Box_Size}

The width of the input box.

### Strict Syntax ### {#Strict_Syntax}

If set to false, the system will automatically insert *'s into a student's answer,
(actually any cas string) and it will not throw an error. Note however, that this is
actually very hard to define robustly, since \(x(x+1)\) means apply the function \(x\) to
the argument \((x+1)\), whereas \(\sin(x)\) would be fine. How does one distinguish between the two?
The system currently assumes a single letter is a variable, and otherwise this is a function to be applied.
So \(tx(x-1)\) will not mean `t*x*(x-1)`

### Insert Stars ### {#Insert_Stars}

If set to true then the system will automatically insert *'s into a student's answer when it is validated.
So, for example \(2(1-4x)\) will be changed to `2*(1-4*x)`.

### Syntax Hint ### {#Syntax_Hint}

A syntax hint allows the teacher to give the student a pro-forma in the input box.
This can include '?' characters (which the CAS treats as the special variable qmchar).
The syntax hint will appear in the answer box, whenever this is left blank by the student.
For example, rather than having to type

	matrix([1,2],[3,4])

the teacher may want to provide an answer box which already contains the string

	matrix([?,?],[?,?])

instead. The student then need only to edit this, to replace ?s with their values.
This helps reduce syntax error problems with more difficult syntax issues.
The ? may also be used to give partial credit. Of course it could also be used for general expressions such as:

	x^2+?*x+1

### Forbidden words ### {#Forbidden_Words}

This is a comma separated list of text strings which are forbidden in a student's answer.
If one of these strings is present then the student's attempt will be considered invalid,
and no penalties will be given.

### Allowed words ### {#Allowed_words}

See Forbidden words above.

### Forbid Floats ### {#Forbid_Floats}

If set to TRUE, then any answer of the student which has a floating point number
will be rejected as invalid. Student's sometimes use floating point numbers when
they should use fractions. This option prevents problems with approximations being used.

### Require lowest terms ### {#Require_lowest_terms}

When this option is set to true, any coefficients or other rational numbers in an
expression, must be written in lowest terms.  Otherwise the answer is rejected as "invalid".
This enables the teacher to reject answers, and not consider them further.

### Check Students answer's type ### {#Check_Type}

If this option is set to true then unless the student's expression is the same
[Maxima](../CAS/Maxima#Types_of_object) as the teacher's correct answer,
then the attempt will be rejected as invalid.

This is very useful for ensuring the student has typed in an "equation", such as \(y=mx+c\)
and not an expression such as \(mx+c\).  Remember, you can't compare an expression with an equation!

Another useful way of avoiding this problem is to put a LaTeX string such as \(y=\) just before the interaction element.  E.g.

	\(y=\)#ans1#.

### Student must verify ### {#Student_must_verify}

(New in 2.2) Specifies whether the student's input is presented back to them before scoring.
Useful for complex algebraic expressions but not needed for constrained input like true/false.

Experience strongly supports the use of verification by "validating" input whenever a student types in an expression.

### Hide feedback ### {#Hide_feedback}

(New in 2.2) Feedback to students is in two forms.  

* feedback tied to interaction elements, in particular if the answer is invalid.
* feedback tied to each potential response tree.

Setting this option does not display any feedback from this input.
Generally, feedback and verification are used in conjunction.

### Options ### {#Options}

Different types of interaction elements have various options.   These are described under the IE type.

### List			{#List}

This allows the following kinds of interactions to be included in STACK questions.

* Radio buttons.
* Dropdown lists.
* Check boxes.

The teacher can choose to construct an interaction which displays a random selection,
in a random order, from a list of potential "distractors".  The top element, named "Correct answer",
is always included, although this isn't really needed for the checkbox type.

You have to enter a content form ([maxima](../CAS/Maxima) format) and displayed form
(i.e. [CASText](CASText)) for each of these.  Both may depend on the question variables.

STACK will automatically add space to ensure you have at least two blank distractors when
you update the question. In the case of the radio button or dropdown list a single expression will be returned.
In the case of the check boxes, we return a list of expressions.  Note, 

* The Teacher's Answer in the Interaction element needs to be a list of objects, even if only one is correct.
* The order of elements in this list is not certain, because we display them in a random order to students.
  It will be necessary to `setify()` this to compare with a set of answers without order becoming a problem.

## Future plans ##

Adding new interaction elements should be a straightforward job for the developers.  We have plans to add interaction elements as follows.

| Package   | Functionality                                                                                                                                                                                                                     
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
| Dragmath  | Adds the [DragMath](http://www.dragmath.bham.ac.uk) applet as an interaction element.  The code is in place, but there are JavaScript bugs, so we have not given authors access to this feature for the time being.  
| GeoGebra  | [GeoGebra](http://www.geogebra.org/) worksheets, for example.                                                                                                                                                        

The only essential requirement is that the result is a valid CAS expression, which includes of course a string data type, or a list.