.. include:: ../references.rst

:tocdepth: 2

Reading and Writing binary data
*******************************

astropy.io.fits
===============

`astropy.io.fits`_  is a Python module developed at STScI to read and write all types of FITS files.

.. admonition:: External resource!

    The `astropy.io.fits`_ tutorial itself is good for our purpose and since this
    is the internet I will not reinvent the wheel. Read the manual
    then come back to this page for an exercise.

.. admonition::  Exercise

    Use the following code to download a FITS file for this exercise::
        
        from astropy.extern.six.moves.urllib import request
        import tarfile
        url = 'http://python4astronomers.github.com/core/core_examples.tar'
        tarfile.open(fileobj=request.urlopen(url), mode='r|').extractall()
        cd py4ast/core
        ls

    Read in the FITS file. Find the time and date of the observation. Then use ``plt.imshow()`` to display the intensity array using some sensible minimum and maximum value so that the spectrum is visible.


.. raw:: html

   <p class="flip6">Click to Show/Hide Solution</p> <div class="panel6">

Here is a possible solution::
    
    from astropy.io import fits
    hdus = fits.open('3c120_stis.fits.gz')
    hdus.info()
    head = hdus[0].header
    head.keys()             # lists all keywords in a dictionary
    head['TDATEOBS']
    head['TTIMEOBS']
    img = hdus[1].data      # Intensity data

    plt.clf()
    plt.imshow(img, origin = 'lower', vmin = -10, vmax = 65)
    plt.colorbar()

You might recognize this piece of code. It was used before in
the :doc:`../core/numpy_scipy` part of the tutorial, but now you should understand the `astropy.io.fits`_ commands in more detail.

.. raw:: html

   </div>



Reading IDL .sav files
======================

IDL is still a very common tool in astronomy. While IDL packages exist to read and write data in simple (ASCII) or standardized file formats (FITS), that users of all platforms can use, IDL also offers a binary file format with an undocumented, proprietary structure. However, acess to this file format (usually called ``.sav``) is very simple and convenient in IDL. Therefore, many IDL users dump their data in this way and you might be forced to read ``.sav`` files a collegue has sent you.

Here is an examplary ``.sav`` :download:`file <../downloads/myidlfile.sav>`. 
If you have trouble downloading the file, then use IPython::

    from astropy.extern.six.moves.urllib import request
    url = 'http://python4astronomers.github.com/_downloads/myidlfile.sav'
    open('myidlfile.sav', 'wb').write(request.urlopen(url).read())
    ls


What can you do?
    1. Convert your collegue to use a different file format.
    2. Read that file in python.

With any relatively recent version ``scipy`` (at least 0.9) then this is a matter of two lines::
    
    from scipy.io.idl import readsav
    data = readsav('myidlfile.sav')

.. admonition::  Exercise: Where is your data?

    ``idlsave`` already prints some information on the screen while reading the file. Inspect the object ``data``, find out how you use it and plot
    the ``x`` and ``y`` data in it.

.. raw:: html

   <p class="flip6">Click to Show/Hide Solution</p> <div class="panel6">

``data`` is a dictionary and all the variables in the ``.sav`` file are
fields in this dictionary. You get a list with ``data.keys()``. Then, this
is easy::
    
    plt.plot(data['x'], data['y'])

.. raw:: html

   </div>

Note that ``idlsave`` cannot write files and that it will fail to read if the ``.sav`` file contains special structures like system variables or compiled IDL code.


.. include:: ../references.rst


