%title: zepto
%author: hellerve
%date: 2016-01-12

the zepto module system

----

# Basics

* Based on Scheme
* Largely compatible with a few automatable rewrites
* Superset?

----

# Features

* Full Numeric Tower (1s->1->1.0->1/1->1+0i)
* Module System
* Collections ("strings" [lists] {vectors} #{hash maps})
* Macros
* Continuations
* Interpreter in Native Code and JavaScript

----

# Libraries

* csv
* json
* rsa
* parser combinators
* monads
* transducers
* pattern matching
* html
* datetime
* mustache
* ...

----

# Docs

   **Move along, nothing to see here**

----

# The module system

* Minimal
  * 65 admittedly involved lines of code
* Easy?
  * small, (somewhat) consistent interface
  * hard to debug if something goes wrong => remediable

----

# Defining a module

    ; This defines a module
    (module "advanced-math"
      (export
        (list "adder" adder)
        (list "subtractor" subtractor)
        (list "multiplicator" multiplicator))

      (mather (lambda (op a b) (op a b)))

      (adder (lambda (a b) (mather + a b)))

      (subtractor (lambda (a b) (mather - a b)))

      (multiplicator (lambda (a b) (mather * a b))))

----

# Using a module

    ; let's use this
    (load "advanced-math") ; => returns the namespace as a hashmap already.

    (import "advanced-math:adder") ; => returns the function, does not define it
    (define add (import "advanced-math:adder")) ; => binds it to any name, duh

    (add 10 12) ; => 22
    ((import "advanced-math:adder") 10 12) ; => 22

----

# Using a module II

    ; this is all fairly verbose for big modules
    ; think of (require 'namespace)
    (import-all "advanced-math") ; => get me all my friends!

    (import-all "advanced-math" "am") ; => just rename the namespace

----

# The Implementation

    (define *modules* (make-hash))

    (define (import name)
      (if (not (in? name #\:))
        (if (hash:contains? *modules* name)
          (*modules* name)
          :no)
        (let* ((fullname (string:split name #\:))
               (module (car fullname))
               (function (cadr fullname)))
          (if (hash:contains? *modules* module)
            (if (hash:contains? (*modules* module) function)
                ((*modules* module) function)
                :no)
            :no))))

    (define-syntax import-all
      (syntax-rules ()
        ((import-all name to)
          (if (hash:contains? *modules* name)
            (let ((ns (current-env)))
              (hash:kv-map
                (lambda (kv)
                  (eval `(define ,(string->symbol (++ to ":" (head kv)))
                                 (quote ,(cadr kv)))
                         ns))
                (*modules* name)))
            :no))
        ((import-all name)
          (import-all name name))))

    (define-syntax module
      (syntax-rules ()
        ((module name (export exports ...) x ...)
          (letrec* (x ...)
            (set! *modules*
              (make-hash *modules*
                (make-hash name
                  (make-hash
                  (map
                    (lambda (el) "build the module map"
                      (if (atom? (car el))
                        (cons (string:tail (symbol->string (car el))) (cdr el))
                        el))
                    (list exports ...))))))))))

----

# So what?

* Yes, this is nothing fancy
* Yes, it lacks features
* Yes, this is not better than Clojure's system

----

# What is it?

* Consistent
* Easily extensible
* Easily understandable
* There is almost no magic

----

# What could it be possibly used for?

* Maybe a plugin system?
* Maybe to entertain yourself for a few hours
* And impress your coworkers with a simple idea (that does not hide it is simple)

----

# Moriturus te saluto

        Questions?

        Comments?

        Remarks on how stupid of an idea that is?

        I'll take it all.

