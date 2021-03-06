===========================
Concurrency and Scalability
===========================

Several questions seem to keep coming relating to these issues; this is an
attempt to summarize the discussion around them.


Why is the API synchronous?
===========================

Generally database access through the SQLite backend is fast enough that
database operations don't block long enough to be of concern.


Are you sure?
=============

Sort of. Current usage seems to indicate that it works out just fine, but you
should conduct your own testing to determine whether this model is suitable for
your particular usage patterns.


Is it thread-safe, though?
==========================

No.  Accessing a Store from different threads will break.  Accessing different
Stores, even backed by the same SQLite database, is fine.


But how do you achieve scalability then?
========================================

Use multiple databases. For example, :doc:`/products/mantissa/index` has
per-user and per-application stores.


What if that isn't good enough?
===============================

If you do need finer-grained concurrency, then running transactions in separate
threads is one way to go about it. Glyph's blog has `more on this subject
<http://www.livejournal.com/users/glyf/2005/09/24/>`_; also see #537 which
covers the DivmodAxiom implementation of this model.
