==============
Divmod Epsilon
==============

A small utility package that depends on tools too recent for Twisted (like
datetime in python2.4) but performs generic enough functions that it can be
used in projects that don't want to share Divmod's other projects' large
footprint.

Currently included:

  * A powerful date/time formatting and import/export class (ExtimeDotTime),
    for exchanging date and time information between all Python's various ways
    to interpret objects as times or time deltas.
  * Tools for managing concurrent asynchronous processes within Twisted.
  * A metaclass which helps you define classes with explicit states.
  * A featureful Version class.
  * A formal system for application of monkey-patches.

Download
========

 * Stable: [http://divmod.org/trac/attachment/wiki/SoftwareReleases/Epsilon-0.6.0.tar.gz?format=raw Get the most recent release - 0.6.0!] ([source:/tags/releases/Epsilon-0.6.0/NEWS.txt Release Notes])
 * Trunk: svn co http://divmod.org/svn/Divmod/trunk/Epsilon Epsilon

See Also
========

 * ''Development'' version of the [http://buildbot.divmod.org/apidocs/epsilon.html Epsilon API docs]
