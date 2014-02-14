=========================
Scattering Kernel Project
=========================


Brief explanation:
------------------

This work will pursue the development of one or more implementations of 
scattering kernel functions for use within the PyNE toolkit. It will also focus
heavily on the verification and validation processes necessary for implementing
scientific theory in software. 

Neutronic calculations require detailed knowledge of cross sections
(interaction probabilities) for the materials and neutron energies in the
system of interest. Many neutron systems involve moderating materials intended
to slow down incident neutrons. These materials typically acheive neutron
moderation via their high scattering probabilities and are very important to
nuclear reactor physics, among other applications. The quantity used to
describe this scattering interaction between an arbitrary neutron and a
specific material is the Scattering Kernel, S(E, p), which is dependent upon
both the energy and momentum of the incident neutron. 

The methods by which the scattering kernel are calculated range from the simple
to the complex. A range of levels of detail are desired for the PyNE library of
kernels. Initial simple efforts have been begun within Pyne as the
pyne.xs.models.sigma_s() and pyne.xs.channels.sigma_s_gh() functions. These are
a good starting place. Verification and validation procedures need to be
conducted for these and all new scattering kernel implementations in PyNE.


Expected results:
------------------

This work will result in verification and validation of the current scattering
kernel implementation in PyNE. Additionally, this work will result in one or
more additional implementations of scattering kernel functions for use within
the PyNE toolkit. 

All new code will be accompanied by appropriate tests, verifcation, validation,
and documentation according to our project guidelines.  


Knowledge Prerequisites:
------------------------

Required:

* An undergraduate knowledge of physics.
* Familiarity with C++
* Familiarity with Python


Desired:

* A familiarity with the rest of our stack git, github, python, C++

Mentor:
-------

* Dr. Anthony Scopatz, Research Scientist, University of Wisconsin, Madison
* Dr. Kathryn Huff, Postdoctoral Scholar, University of California, Berkeley


More Resources:
---------------

* https://pynesim.org
* https://github.com/pyne/pyne
* https://github.com/pyne/pyne/issues/21


