# orbit


Originally for Lunar Numbat.

This program implements a simple-minded model of Numbat motion,
in the vicinity of the Earth and Moon.

Included are the effects of gravity from both the earth, moon and sun.
An on-board engine can be fired to accelerate the craft.

The position of the moon can be approximated using internal functions,
or positional data of DE405-level accuracy can be used by contacting
JPL's Horizons ephemeris service (the program will do this automatically
if requested via the appropriate command-line option).

The state vector of the Numbat is converted to orbital elements if the
appropriate command-line option is given.

The program now also includes selenographic position and velocity
information, which may prove useful as the Numbat approaches the
moon.


--------------------------------------------------------------------------


SOME DETAILS

All this chaos takes place in something approximating a geocentric
inertial frame, using equinox J2000.0.  We use two coordinate
systems herein.  First, Ecliptic coordinates (longitude, latitude
and distance).  Second, a right-handed, cartesian system defined
such that the "positive x" axis is in the direction of Ecliptic
longitude = 0, latitude = 0, and the "positive y" axis points
towards longitude = pi / 2 rad, latitude 0.  The "positive z" axis
is oriented such that positive Ecliptic latitudes correspond to z
values > 0.

Included are the effects of gravity from both the earth, moon and sun.

Both the moon and sun appear in the code as point-like masses, so
we're effectively including these bodies as perfect spheres of
uniform mass density.  Exactly how good an approximation this is
when we get close to the moon is unclear to me at the moment.  The
Earth is modelled as an oblate spheroid; that is, a point-like
mass (i.e., gravitational potential = -GM/r) but also including
the zonal harmonic associatend with the "n = 2" Legendre
polynomial.  As the magnitude of this n=2 term is approximately
1000 times that of the next largest term, only n=2 has been
included here.

I have chosen the classic, second-order integrator affectionately
known as "leapfrog".  Google or read any book on numerical methods
for more info on this.  The moon_internal(), sun() and anomaly()
functions were adapted from the book "Astronomy with your Personal
Computer" by P. Duffett-Smith Cambridge University Press.  The
function libration() was adapted from "Practical astronomy with
your calculator", also by Duffett-Smith.  The position of the Moon
is accurate to maybe 50 km using moon_internal().  You can do
better by specifying the "-horizons" option, as this will obtain
data from the JPL Horizons web interface.  This gives the program
access to moon position and libration data at a level of accuracy
comparable to JPL's DE405.

When requesting data from JPL's Horizons service, data are
obtained at five-minute intervals, and cubic splines are used
within this program to interpolate values between these points.
Data from Horizons are saved in a disk cache file, so they can be
reused in subsequent runs without having to contact Horizons
again.
