========
Formless
========

.. todo:: Intro


A Note About Formless TypedInterfaces
=====================================

One of the original goals for Formless was that as well as web forms, it should
be flexible enough to handle forms for other user interfaces e.g. curses, gtk.
The idea was that the developer should create a TypedInterface (an enhanced
Zope Interface) describing the signature of a method that might be invoked in
response to a form submission. By implementing this interface in a
Nevow.rend.Page, the developer could present a web UI. Elsewhere the same
TypedInterface could be implemented to provide a gtk UI for example. In
practice though, only the web form rendering code was maintained.

* see the Formless section of `this 2004 paper by Donovan Preston, the original
  author of Nevow and Formless
  <http://www.python.org/pycon/dc2004/papers/60/context>`_

Recently *TypedInterface has been deprecated in favour of the newer and simpler
`bind_*` syntax*. (see `Nevow Changelog 2005-07-12
<source:trunk/Nevow/ChangeLog>`_) Confusingly for the beginner, most of the
Nevow examples still use TypedInterfaces, but hopefully these will be updated
in due course.

In the rest of this document we will use only the new bind_* syntax.



More on Standard Form Controls and Input Types
==============================================

.. todo:: Review the input types and widget types available


Custom Form Controls and Input Types
====================================

.. todo:: Describe how to create a simple captcha widget


Customising Form Layout
=======================

TODO
