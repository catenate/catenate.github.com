---
layout: draft
title: Beautiful operators for C-influenced programming languages
categories: draft
---
For beauty, lexers should accept (not mandate) Unicode and alternative  
forms of operators, and translate them to standard forms.  It's a shame  
that modern programming languages can accept Unicode strings and even  
variable names, but cannot accept more pleasant operator forms.  
This post lists corrected, alternative and subjectively-better operator  
forms from the C family and languages it influenced, and methods to use  
them before languages accept them.  

To test equality of two values, `==` is not intuitive: its symbols do  
not innately express test.  `==` is unique to programming languages:  
Unicode has no double-equals sign.  `=?` better encodes both concepts  
(test and equality).  `≟` (questioned equal to) is the corresponding  
single-character option for font sizes large enough to legibly display  
the question mark.  

For Boolean logical-and and logical-or, `&&` and `||` are unique to  
programming languages, so I prefer the forms `⋀` (n-ary logical and) and  
`⋁` (n-ary logical or).  Bitwise operations are treated similarly:  
instead of `&` and `|`, use `∧` (logical and) and `∨` (logical or).  
To take the bitwise-exclusive-or of two values, `^` looks more like `∧`  
than `∨`, and does not encode exclusivity, as `⊻` (xor) does.  

Exponentiaton offers a variety of symbols.  `^` suffers from confusion  
with bitwise-exclusive-or.  `^^` implies multiple operations, so does  
not scale well to actual repeated exponentiation.  `**` also does not  
scale well, but reminds one of multiplication, but only in programming  
languages, since mathematicians do not denote multiplication with `*`  
outside of programming languages.  Several languages offer a named  
function, usually `pow` for power, but since it takes options and might  
need qualified with a library name it is not usually succinct.  I prefer  
`↑` (up arrow), which scales well with Knuth’s up-arrow notation.  

*C*

*Go*

[This post] [unigo] describes the approach: replace the scanner function of the  
formatter `gofmt`, then run the source code through the formatter before  
the compiler.  [My version] [sgo] of the scanner function is always more or less  
out of date.  
[unigo]: http://swtools.wordpress.com/2010/02/10/update-go-scanner-to-accept-non-ascii-operators/ "Update Go scanner to accept non-ASCII operators"
[sgo]: http://dl.dropbox.com/u/502901/scanner.go "scanner.go"

*Haskell*

A single preprocessing step translates all alternative symbols to those  
recognized by the language.  For example, a rule in Plan 9's mk to  
translate a literate-Haskell source file.  

	%.lhs: %.ℓhs
		sed -f toascii.sed $stem.ℓhs > $stem.lhs

The sed script is simple substitution.  The language does not reserve  
any of the non-ASCII symbols or the `=?` combination, so the only  
difficulty would come if strings in the program were meant to have one  
of these symbols.  In that case, one could omit the translation, if the  
symbol is not otherwise used, or complicate the rule by not translating  
within a string (left as an exercise for the reader).  

	s,=\?,==,g
	s,·,*,g
	s,×,*,g
	s,÷,/,g
	s,λ,\\,g
	s,—,--,g
	s,’,',g
	s,′,',g
	s,←,<-,g
	s,↑,^,g
	s,→,->,g
	s,⇐,=,g
	s,⇒,=>,g
	s,−,-,g
	s,⊥,_|_,g
	s,⋀,\&\&,g
	s,⋁,||,g

Compiler output is a minor inconvenience: the compiler will refer to the  
translated file rather than the Unicode file, so methods to jump to  
offending source code will not refer to the file you want to change.  
We can deal with this by routing the error messages to standard input  
(mk uses sh by default for its shell commands) and substituting the  
desired extension.  

	%: %.lhs
		ghc -o $stem $stem.lhs 2>&1 | sed 's,\.lhs,.ℓhs,'

*Ruby*
