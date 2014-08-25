# Notes on Learning Lisp

### global variables

	;; enclose the labels with 'earmuffs', they are a convention rule
	;; will overwrite previously set values
	(defparameter *asdf* 99)

	;; will not overwrite previously set values
	(defvar *asdf* 616)

### global functions

    ;; define global function
	(defun fun-name (args)
		function-body)

    ;; change global vars
    (setf varname value)

    ;; bla-bla-if-not 
    ;; such functions are considered deprecated, and to be avoided

### local variables

    ;; define local variable
    (let (variable declarations)
        ... body... )
    
### local functions

    ;; define local functions
    (flet ((function_name (arguments)
            ... function body...))
        ... body...)

    ;; for recursion
    ;; labels is same as flet, but lets it call itself
    (labels (function_name (arguments)
                (... function_body...))
           (function_name (arguments)
                (function))
           (... body...))

### symbols

    ;; are case insensitive
    ;; use eq to compare
    ;; are variable/function names
    ;; are regular objects!!!
    ;; '|this is a symbol with spaces|
    ;; with pipes, case-sensitive symbols are possible!
    ;; are interned into current package upon creation
    ;; but do not have to be interned
    ;; 'name is a keyword denoting a symbol "name", interned in the keyword packaga
    ;; vars and functions are stored in different namespaces
    ;; thus common lisp is a lisp-2
    ;; scheme stores both in the same, so it's a lisp-1
    ;; that's why functions symbols are called like `#'this-name`
    ;; keyword symbols have ':' prepended
    ;; they always stand for themselves and cannot be assigned a value
    ;; example:  :cigar --> :CIGAR

    
### booleans

    ;; symbols T and F represent these
    ;; only empty list is false
    ;; everything else is true
    ;; and
    ;; or
    ;; functions returning booleans or nil ("predicates"), get '-p' appended to their name by convention

### numbers

    ;; - integers are built-in
    ;; - floating-points are built in

### strings

    ;; " ... "
    ;; \" for literal '"'
    ;;

### characters

    ;; #\a
    ;; #\A
    ;; #\newline
    ;; #\tab
    ;; #\space
    
### code vs. data mode

    ;; always in code mode (evaluate things)
    ;; '(...) -> data mode, treat contents as list
    ;; `(... ,statment ...) -> data mode, too
    ;; but comma's jump back to code mode temporary
    ;; that's called quasiquoting
    ;; the comma 'unquotes' the backquote and flops back into code mode
    
### cons cells

    ;; (bla . blubb)
    ;; group two objects together via pointers

### lists

    ;; '() empty list
    ;; 'nil symbol also represents empty list
    ;; lists are 'cons cells' under the hood
    ;; (list 'a 'b) === '(a b) === (cons 'a (cons 'b '() ))
    ;; can be nested
    ;; member = contains
    ;; null = check for nil
    ;; (setf *print-circle* t) for circular lists!
    ;; use car to get the head
    ;; use cdr to get the tail
    ;; if both are mixed together, read from right to left
    
### branching

    (if (condition)
        (true)
        (false))

    ;; if with implicit progn, nil if false
    (when (condition)
        ((statement)
         (statement))

    ;; if with implicit progn, nil if true
    (unless (condition)
        ((statement)
         (statement))

    ;; rock n roll, cond can do all you want
    (cond ((condition1) ((statement1-true)
                         (statement1-true))
                        ((statement1-false)
                         (statement1-false))
           (condition2) (statement2-true)
                        (statement2-false))
    ;; case matches parameter via eq
    ;; use inside a function
    ;; string breaks it
    (case (bla)
        ((bla1) (statement1-true)
                (statement1-false))
        ((bla2) (statement2-true)
                (statement2-false)))

    ;; IO
    ;; read
    ;; read-line
    ;; read-from-strin
    ;; eval
    ;;
    ;; do not use eval or read unless you know what you are doing

### circular lists

    ;; example
    (defparameter foo '(1 2 3))
    (setf (cdddr foo) foo)

### alists / association lists

    ;; list of key-value pairs / conses
    ;; get elements by keynames via `assoc`
    

### graphs

    ;; todo

### keyword parameters

    ;; functions which let you choose which parameters you want, let you specify these via ':keys'
    ;; see keyword symbols

### generic setters

    ;; in most cases, getters can be used for setting variables, too
    ;; use with prepended 'setf'

### objects

    ;; created with defparameter
    ;; names in 'plain
    ;; when instatiated, reference properties as keywords
    ;; getters/setters are created via objectname-slotname / objectname-propertyname
    ;; 'sequencing objects': lists, arrays, strings

### predicate functions

    ;; arrayp
    ;; characterp
    ;; consp
    ;; functionp
    ;; hash-table-p
    ;; listp
    ;; stringp
    ;; symbolp

### loop

    (loop for i from 2 to 4 sum i) ;; 9
    (loop for i in '(2 4 6) sum i) ;; 12
    (loop for i below 4 do (sum i)) ;; 6
    (loop for i below 4 when (oddp i) sum i) ;; 4
(loop for i from 0 sum i) ;; error infinity
    (loop for x from 1 to 2 for y below 5 collect (* x y)) ;; (0 2)

### format

    (format t "hello world") ;; t = console output
    (format nil "hello world") ;; nil = just create string
    (format stream "hello world") ;; stream = output stream output
    ;; control sequences:
    ;; ~$ monetary floating point valuee
    ;; ~a absolute (no "")
    ;; ~s string (with "")
    ;; ~10a 10-char-wide, leftbound
    ;; ~12@a 12-char-wide, rightbound
    ;; ~5,3a 5-char-wide, leftbound, extend by size of 3
    ;; ~,,4a padding of 4 spaces to the right
    ;; ~,,5,'?a padding of 5 '?' to the right
    ;; ~,,2,':@a padding of 2 ':' to the left
    ;; ~x base-16 / hex
    ;; ~b base-2 / binary
    ;; ~d base-10 / decimal
    ;; ~:d base-10, with ',' each 1000er group
    ;; ~f floating-point
    ;; ~3f numbers plus comma altogether = 3
    ;; ~,4f four decimals
    ;; ~,,3f scale value by 10^3
    ;; ~% hard newline
    ;; ~& soft newline, if needed
    ;; ~t for creating tables
    ;; ~< and ~> make spaces evenly distributed in between
    ;; can be used for centering, too
    ;; repeat for each column in conjunction with ':@' for having all cells centered
    ;; ~{ and ~} make a string repeat for the amount of elements in a passed list
    ;;
