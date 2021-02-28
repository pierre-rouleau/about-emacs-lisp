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


sexp:
     A single unevaluated Lisp object, which is not instrumented.



..  LocalWords:  Edebug
