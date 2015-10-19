LHCO_reader 
***********

Introduction
============

:mod:`LHCO_reader` is a Python module for reading `LHCO files <http://madgraph.phys.ucl.ac.be/Manual/lhco.html>`_ from detector simulators such as `PGS <http://www.physics.ucdavis.edu/~conway/research/software/pgs/pgs4-general.htm>`_ into a Python class, with useful functions for implementing an analysis. It can also read ROOT files from `Delphes <https://cp3.irmp.ucl.ac.be/projects/delphes>`_, by immediately converting them to LHCO files.

Getting started
===============

The module does not require installation; simply clone the module::

    git clone https://github.com/innisfree/LHCO_reader.git

or `download it via a web browser <https://github.com/innisfree/LHCO_reader/archive/master.zip>`_.

To load the module :mod:`LHCO_reader` and look at an LHCO file, simply::

    import LHCO_reader
    events = LHCO_reader.Events(f_name="example.lhco")
    print events

  
To test the module::

    python LHCO_reader.py -v


The module requires some common modules that you might need to install separately, the most obscure of which is :mod:`prettytable`, see  `here for installation <https://code.google.com/p/prettytable/wiki/Installation>`_.

The :class:`Events` object in the above code is a list-like object. Cuts can be implemented with lambda-functions, e.g. to cut events with one tau-lepton::

    tau = lambda event: event.number()["tau"] == 1
    events.cut(tau)
   
The :class:`Events` object is structured as follows:

- :class:`Events` - A list of all events in the LHCO file

- :class:`Events[0]` - The zeroth event in the LHCO file. The :class:`Events` can be looped with e.g.:

.. code-block:: python

    for event in events:
      ... scrutinize an event ...
 
but beware that altering list-type objects in a loop can be problematic. The best way to cut :class:`Events` is with the :func:`Events.cut` function.
    
- :class:`Events[0]["electron"]` - A list of all electrons in the zeroth event in the LHCO file. For ordinary LHCO files, the possible keys are :literal:`electron`, :literal:`muon`, :literal:`tau`, :literal:`jet`, :literal:`MET` and :literal:`photon`.

- :class:`Events[0]["electron"][0]` - The zeroth electron in the zeroth event in the LHCO file.
  
- :class:`Events[0]["electron"][0]["PT"]` - The transverse momentum of the zeroth electron in the zeroth event in the LHCO file. The other possible keys are :literal:`event,` :literal:`type`, :literal:`eta`, :literal:`phi`, :literal:`PT`, :literal:`jmass`, :literal:`ntrk`, :literal:`btag` and :literal:`hadem`.
 
There are many useful functions, including printing in LHCO format (:func:`LHCO`), plotting (:func:`plot`), sorting (:func:`order`) and cutting events (:func:`cut`), manipulating four-momenta with boosts (:func:`vector`), counting the numbers of types of object in an event (:func:`number`), angular separation (:func:`delta_R`), that should make implementing an analysis easy.

ROOT
====

ROOT files can be converted into LHCO files with :mod:`root2lhco` in `Delphes <https://cp3.irmp.ucl.ac.be/projects/delphes>`_, which can be linked with and called from within :mod:`LHCO_reader` via :mod:`LHCO_converter`, i.e. you can load a ROOT file, which will be immediately converted into an LHCO file and parsed. If you wish to use ROOT files::

    export DELPHES=MY/PATH/TO/DELPHES