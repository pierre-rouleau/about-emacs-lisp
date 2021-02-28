==============================
Instrumentation of macro calls
==============================


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

As described in the Emacs Lisp manual::

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



sexp
~~~~

Description:
  A single un-evaluated Lisp object, which is not instrumented.

Details:
  Describes a macro argument that is passed unquoted and used unquoted.


Example:

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

..  LocalWords:  Edebug
