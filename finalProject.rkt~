#lang eopl

;Especificación Léxica

(define lexemes
  '((white-sp (whitespace) skip)
    (comment​ (​"#"​ (​arbno​ (​ not​ ​ #\newline​ ))) skip)
    (identifier ((​arbno ​ "@"​) letter (​ arbno​ (​ or​ letter digit ​ "_" "?" "="​ ))) symbol)
    (number​ (digit (​arbno​ digit)) number)
    (​number (​"-"​ digit (​arbno​ digit)) number)
    (text (​"\""​ (​or​ letter whitespace)(arbno​ (or letter digit whitespace ​ ":"​ ​ "?"​ ​ "=" "'"​ )) ​ "\""​ ) string)
  ))

;Especificación Gramatical

(define grammar
  '((ruby-program ("ruby" exp-batch "end") a-program)
    (exp-batch (expression (arbno expression)) a-batch)
    (expression (simple-exp) a-simple-exp)
    (expression ("declare" identifier (arbno "," identifier) ";") declare-exp)
    (expression ("puts" comp-value (arbno "," comp-value) ";") puts-exp)
    (expression ("if" comp-value (arbno "then") exp-batch
                      (arbno "elseif" comp-value (arbno "then") exp-batch)
                      (arbno "else" exp-batch)
                      "end"
                      )if-exp)
    (expression ("unless" comp-value (arbno "then") exp-batch (arbno "else" exp-batch) "end" ) unless-exp)
    (expression ("while" comp-value (arbno "do") exp-batch "end") while-exp)
    (expression ("until" comp-value (arbno "do") exp-batch "end") until-exp)
    (expression ("for" identifier "in" comp-value (arbno "do") exp-batch "end") for-exp)
    (expression ("def" identifier "(" (arbno (separated-list identifier ",")) ")" exp-batch "end") function-exp)
    (expression ("super" identifier arguments ";") super-exp)
    (class-decl ("class" identifier (arbno "<" identifier)
                         "attr" (arbno (separated-list ":" identifier ",")) ";"
                         (arbno method-decl) "end") class-exp)
    (method-decl ("def" identifier "(" (arbno (separated-list identifier ",")) ")"
                        exp-batch "end") a-method-decl)
    (simple-exp (simple-value complement ";") val-exp)
    (complement ("=" comp-value calls) assign)
    (complement (assign-op comp-value calls) assign-end)
    (complement (calls) comp-calls)
    (calls ("{"(arbno call)"}") some-calls)
    (call ("." identifier arguments) method-call)
    (call (arguments) arguments-call)
    (arguments ("(" (arbno (separated-list comp-value ",")) ")") m-arguments)
    (arguments ("[" comp-value (arbno (separated-list comp-value ",")) "]") arr-arguments)
    (comp-value (value) op-value)
    (comp-value (un-op comp-value) unop-value)
    (value (simple-value) simple-val)
    (value ("(" value val-compl ")") call-cal)
    (val-compl (calls) val-call)
    (val-compl (bin-op value) binop-val)
    (simple-value (identifier) id-val)
    ;(simple-value (integer) num-val)
    ;(simple-value (float) float-val)
    ;(simple-value (text) str-val)
    (simple-value ("true") true-val)
    (simple-value ("false") false-val)
    (simple-value ("nil") nil-val)
    (simple-value ("[" (arbno (separated-list comp-value ",")) "]") arr-val)
    (bin-op ("+") add)
    (bin-op ("-") diff)
    (bin-op ("*") mult)
    (bin-op ("/") div)
    (bin-op ("%") mod)
    (bin-op ("**") pow)
    (bin-op (">") great)
    (bin-op (">=") great-eq)
    (bin-op ("<") less)
    (bin-op ("<=") less-eq)
    (bin-op ("==") is-equal)
    (bin-op ("!=") not-equal)
    (bin-op ("and") and-op)
    (bin-op ("&&") and-op)
    (bin-op ("or") or-op)
    (bin-op ("||") or-op)
    (bin-op ("..") in-range)
    (bin-op ("...") ex-range)
    (bin-op ("step") st-range)
    (assign-op ("+=") add-eq)
    (assign-op ("-=") diff-eq)
    (assign-op ("*=") mult-eq)
    (assign-op ("/=") div-eq)
    (assign-op ("**=") pow-eq)
    (un-op ("not") not-op)
    (un-op ("!") not-op)
    ))


(sllgen:make-define-datatypes lexemes grammar)

(define show-the-datatypes
  (lambda () (sllgen:list-define-datatypes lexemes grammar)))

;*******************************************************************************************
;Parser, Scanner, Interfaz

;El FrontEnd (Análisis léxico (scanner) y sintáctico (parser) integrados)

(define scan&parse
  (sllgen:make-string-parser lexemes grammar))

;El Analizador Léxico (Scanner)

(define just-scan
  (sllgen:make-string-scanner lexemes grammar))
