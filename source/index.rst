Syntactical Forms
*****************

This is a comparison of various bits of the syntax of the current infix Dylan,
prefix Dylan as it was defined by Apple in 1992 and subsequently amended by
the Dylan Design Notes, Goo and David Moon's PLOT3.

Adding other syntaxes is welcome via pull request, as is fixing any of the
forms listed here or adding additional examples to illustrate various details.

.. contents::
   :local:

Method definitions
==================

Infix Dylan
-----------

.. code-block:: dylan

    define method floor/ (numerator :: <integer>, denominator :: <integer>)
     => (quotient :: <integer>, remainder :: <integer>)
      ...
    end;

    define inline method curry
        (function :: <function>, #rest curried-args) => (result :: <function>)
      ...
    end;

Prefix Dylan
------------

.. code-block:: scheme

    (define-method floor/ ((numerator <small-integer>)
                           (denominator <small-integer>)
                           #values  (quotient <integer>)                 
                                    (remainder <small-integer>))
      ...)

    (define-method curry ((function <function>)
                          #rest curried-args
                          #values (result <function>))
      ...)

* No support for adjectives.


Goo
---

.. code-block:: scheme

    (dm floor/ (real|<num> divisor|<num> => (tup x|<int> rem|<num>))
      ...)

    (dm (curry inline) (f|<fun> curried|... => <fun>)
      ...)

* Adjectives are slightly odd in their placement.

PLOT3
-----

::

    defun floor/(numerator is integer, denominator is integer)
      ...

    defun curry(fun is function, rest: curried-args) is function : inline
      ...

* PLOT3 doesn't directly support multiple return values.
* ``inline`` isn't a predefined annotation (adjective) but annotations are extensible.

Classes
=======

Infix Dylan
-----------

.. code-block:: dylan

    define class <foo> (<bar>)
      constant slot foo-a :: <string>, required-init-keyword: a:;
      slot foo-b = #f,
        init-keyword: b:;
    end;

Prefix Dylan
------------

.. code-block:: scheme

    (define-class <foo> (<bar>)
      (foo-a type: <string>
             required-init-keyword: a:
             setter: #f)
      (foo-b init-value: #f
             init-keyword: b:))

* Again, no support for adjectives.

Goo
---

.. code-block:: scheme

    (dc <foo> (<bar>))
      (dp foo-a (<foo> => <string>))
      (dp! foo-b (<foo> => <any>))

* Note that the ``dp`` and ``dp!`` forms are not contained within the
  ``dc`` form.
* I don't know how to specify the various options that we did in Dylan.


PLOT3
-----

::

    defclass foo
      foo-a is string
      foo-b := #f

* How to specify supertypes?
* How to specify keywords and such?


Variables / Constants
=====================

Infix Dylan
-----------

.. code-block:: dylan

    define variable *foo* :: <string> = "abc";
    define constant $bar = 238;

Prefix Dylan
------------

.. code-block:: scheme

    (define-variable (*foo* <string>) "abc")
    (define-constant $bar 238)

Goo
---

.. code-block:: scheme

    (dv *foo*|<string> "abc")
    (dv $bar 238)

* While docs indicated that ``(d. $bar 238)`` might yield a constant,
  I found no usage of that in the sources.

PLOT3
-----

::

    def *foo* is string := "abc"
    def $bar = 238

* Note that ``:=`` makes it a variable while ``=`` is for constants.

