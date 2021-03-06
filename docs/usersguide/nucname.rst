.. currentmodule:: pyne.nucname

.. _usersguide_nucname:

==========================
Nuclide Naming Conventions
==========================
One of the most frustrating aspects of nuclear data software is the large number
of different ways that people choose to nuclide names.  Functionally, there are 
three pieces of information that *should* be included in a radionuclide's name

1. **Z Number**: The number of protons.
2. **A Number**: The number of nucleons (neutrons + protons).
3. **Metastable Level**: The metastable state of the nucleus as defined by ENSDF.

Some common naming conventions exist.  The following are the ones currently 
supported by PyNE.  Functions to convert between forms may be seen in :ref:`name_cast`.

 #. **zzaaam**: This type places the charge of the nucleus out front, then has three
    digits for the atomic mass number, and ends with a metastable flag (0 = ground,
    1 = first excited state, 2 = second excited state, etc).  Uranium-235 here would
    be expressed as '922350'.
 #. **zzllaaam**: The ZZLLAAAM naming convention is similar to name form.  However, it is preceded by
    the nuclides two AA numbers, followed by the two LL characters.  Of the two LL characters, only 
    the first letter in the chemical symbol is uppercase, the dash is always present, and the the
    meta-stable flag is lowercase.  For instance, '95-Am-242m' is the valid serpent notation for 
    this nuclide.
 #. **name**: This is the more common, human readable notation.  The chemical symbol
    (one or two characters long) is first, followed by the atomic weight.  Lastly if
    the nuclide is metastable, the letter *M* is concatenated to the end.  For example,
    'H-1' and 'Am242M' are both valid.  Note that nucname will always return name form with
    the dash removed and all letters uppercase.
 #. **MCNP**: The MCNP format for entering nuclides is unfortunately non-standard.  In most
    ways it is similar to zzaaam form, except that it lacks the metastable flag.  For information
    on how metastable isotopes are named, please consult the MCNPX documentation for more information.
 #. **Serpent**: The serpent naming convention is similar to name form.  However, only the first
    letter in the chemical symbol is uppercase, the dash is always present, and the the meta-stable
    flag is lowercase.  For instance, 'Am-242m' is the valid serpent notation for this nuclide.
 #. **NIST**: The NIST naming convention is also similar to the Serpent form.  However, this
    convention contains no metastable information.  Moreover, the A-number comes before the
    element symbol.  For example, '242Am' is the valid NIST notation.
 #. **CINDER**: The CINDER format is similar to zzaaam form except that the placement of the Z- and
    A-numbers are swapped. Therefore, this format is effectively aaazzzm.  For example, '2420951' is
    the valid cinder notation for 'AM242M'.
 #. **ALARA**: In ALARA format, elements are denoted by the lower case atomic symbol. Isotopes are
    specified by appending a semicolon and A-number. For example, "fe" and "fe:56" represent
    elemental iron and iron-56 respectively. No metastable flag exists.
 #. **state_id**: The state id naming convention uses the form zzzaaassss. It is different from the
    canonical zzzaaammmm form in that ssss refers to a list of states by ordered by energy. This is
    derived from the levels listed in the ENSDF files for a given nuclide. Using this form is
    dangerous as it may change with new releases of ENSDF data.
 #. **Groundstate**:  In Groundstate format, the nuclide is stored in a form similar to the standard
    id form, but the last four digits are zero to eliminate the information about the nuclides state.  

If there are more conventions that you would like to see supported, please contact the :ref:`dev_team`.


--------------
Canonical Form
--------------
The ``zzzaaammmm`` integer form of nuclide names is the fundamental form of nuclide naming because
it accurately captures all of the needed information in the smallest amount of space.  Given that the 
Z-number may be up to three digits, A-numbers are always three digits, and the excitation level is
one digit, all possible nuclides are represented on the range ``0 <= zzzaaammmm < 10000000000``.  This 
falls well within 32 bit integers (but sadly outside of the smaller 16 bit ints).

On the other hand, ``name`` string representations may be anywhere from two characters (16 bits)
up to six characters (48 bits).  So in general, ``zzzaaammmm`` is smaller by 50%.  Other forms do 
not necessarily contain all of the required information (``MCNP``) or require additional storage 
space (``Serpent``).  It may seem pedantic to quibble over the number of bits per nuclide name, 
but these identifiers are used everywhere throughout nuclear code, so it behooves us to be as small
and fast as possible.

The other distinct advantage that integer forms have is that you can natively perform arithmetic
on them.  For example::

    # Am-242m
    nuc = 942420001

    # Z-number
    zz = nuc/10000

    # A-number
    aaa = (nuc/10)%1000

    # Meta-stable state
    m = nuc%10

Code internal to PyNE use ``zzzaaammmm``, and except for human readability, you should too!  
Natural elements are specified in this form by having zero A-number and excitation flags
(``zzzaaammmm('U') == 920000``).


---------------
Examples of Use
---------------

.. code-block:: ipython

    In [1]: from pyne import nucname

    In [2]: nucname.zzaaam('U235')
    Out[2]: 922350

    In [3]: nucname.zzaaam(10010)
    Out[3]: 10010

    In [4]: nucname.zzllaaam(942390)
    Out[4]: '95-Am-242m'

    In [5]: nucname.name(10010)
    Out[5]: 'H1'

    In [6]: nucname.serpent('AM242M')
    Out[6]: 'Am-242m'

    In [7]: nucname.name_zz['SR']
    Out[7]: 38

    In [8]: nucname.zz_name[57]
    Out[8]: 'LA'

    In [9]: nucname.alara('FE56')
    Out[9]: 'fe:56'



Further information on the naming module may be seen in the library reference :ref:`pyne_nucname`.
