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
                the types of arguments of a calling form.
=============== =============================================================
