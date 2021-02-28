==============================
Instrumentation of macro calls
==============================

:URL: https://github.com/pierre-rouleau/about-emacs-lisp/blob/master/doc/macro-edebug-spec.rst
:Project:  `About Emacs Lisp home page`_
:Modified: 2021-02-28 15:39:29, updated by Pierre Rouleau.
:License:
    Copyright (c) 2021 Pierre Rouleau <prouleau001@gmail.com>


    You can redistribute this document and/or modify it under the terms of the GNU
    General Public License as published by the Free Software Foundation, either
    version 3 of the License, or (at your option) any later version.


    This document is distributed in the hope that it will be useful, but WITHOUT ANY
    WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
    PARTICULAR PURPOSE. See the GNU General Public License for more details.



.. _About Emacs Lisp home page:  https://github.com/pierre-rouleau/about-emacs-lisp


.. contents::  **Table Of Contents**
.. sectnum::


Macro def-edebug-spec macro specifications
==========================================

Inside a declare form, you can identify the way a macro is instrumented for
debugging by using the ``(debug ...)`` form.

The ``(debug ...)`` form accept the following arguments:

=============== =============================================================
Argument        Description
=============== =============================================================
**t**           All macro arguments are instrumented for evaluation.

**0**           None of the macro arguments are instrumented for evaluation.

*a symbol*      The symbol of another macro that has an Edebug specification,
                which is used instead.  Allows you to inherit the
                specifications of another macro.

*a list*        A list of elements, listed in the next section, that describe
                the types of arguments of a calling form.  A list is required
                for Edebug specification if some arguments of a macro call are
                evaluated while others are not.
=============== =============================================================


Specification List
------------------

As described in the `section titled Instrumenting Macro Calls in the Emacs Lisp manual`_::

  A “specification list” is required for an Edebug specification if some
  arguments of a macro call are evaluated while others are not.  Some
  elements in a specification list match one or more arguments, but others
  modify the processing of all following elements.  The latter, called
  “specification keywords”, are symbols beginning with ‘&’ (such as
  ‘&optional’).

     A specification list may contain sublists, which match arguments that
  are themselves lists, or it may contain vectors used for grouping.
  Sublists and groups thus subdivide the specification list into a
  hierarchy of levels.  Specification keywords apply only to the remainder
  of the sublist or group they are contained in.



.. _section titled Instrumenting Macro Calls in the Emacs Lisp manual: https://www.gnu.org/software/emacs/manual/html_node/elisp/Instrumenting-Macro-Calls.html#Instrumenting-Macro-Calls

I expanded the information from the manual to include examples for several of
the possible symbols described in the manual.  Each one has it's own section.



sexp
~~~~

Description:
  A single un-evaluated Lisp object, which is not instrumented.

Details:
  Describes a macro argument that is passed unquoted.
  Inside the macro, it may be used quoted or unquoted.
  It may be a single symbol but also a Lisp object like a list of objects
  that may constitute a parameter list of a lambda.
  See the ``gv-letplace`` macro for an example of this.


**Example**:

.. code:: elisp

    (defmacro first-in (seq)
      "Return first element of sequence SEQ."
      (declare (debug (sexp)))
      `(nth 0 ,seq))

    (defmacro last-in (seq)
      "Return last element of sequence SEQ."
      (declare (debug (sexp)))
      `(nth (1- (length ,seq)) ,seq))

Using these macros:

.. code:: elisp

    ELISP> (setq digits '(0 1 2 3 4 5 6 7 8 9 ))
    (0 1 2 3 4 5 6 7 8 9)

    ELISP> (first-in digits)
    0 (#o0, #x0, ?\C-@)
    ELISP> (last-in digits)
    9 (#o11, #x9, ?\C-i)
    ELISP>

Macro expansion in code:

Given the following code:

.. code:: elisp

    (setq digits '(0 1 2 3 4 5 6 7 8 9))
    (setq first-digit (first-in digits))
    (setq last-digit (last-in digits))

The in-line macro expansion produces the following code:

.. code:: elisp

    (setq digits '(0 1 2 3 4 5 6 7 8 9))
    (setq first-digit (nth 0 digits))
    (setq last-digit (nth
                      (1-
                       (length digits))
                      digits))

Another example: ``flip-args``:

.. code:: elisp

    (defmacro flip-args (fun arg1 arg2)
      "Calls function FUN, passing ARG2 before ARG1."
      (declare (debug (sexp body)))
      `(,fun ,arg2 ,arg1))

where:

.. code:: elisp

    (flip-args / 4 36)

expands to:

.. code:: elisp

    (/ 36 4)


.. ---------------------------------------------------------------------------

..
       Local Variables:
       time-stamp-start: "^:Modified:[ \t]+\\\\?"
       time-stamp-end:   "\\.$"
       End:

..  LocalWords:  Edebug
