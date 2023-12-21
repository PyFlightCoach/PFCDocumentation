
.. _geometry:

geometry
--------


Tools for handling 3D geometry, mostly just adds a nice interface to various geometric enterties. Each geometric 
entity can also be a vector of geometric entities. Each entity wraps a numpy array with the relevant number of 
columns labelled according to the cols class property and rows equal to the number of elements in the vector. 
Attribute access to each column is available and returns a numpy array.

Where operations are supported between geometric types the size of the output is inferred based on the length of
the inputs. Where the two vectors of entities are of the same length, elementwise operations are performed. 
Where one vector is length one and the other is greater than one then the operation will be performed on every
element of the longer vector.

Magic methods are used extensively and the function of operators are logical for each type. If unsure what 
the logical option is then check the code where it should be pretty clear.

Many convenience methods and constructors are available. Documentation is limited but if you need something 
it has probably already been written so check the code first.

now available on pypi:

.. code-block:: console

    $ pip install pfc-geometry

.. code-block:: python

    >>> import geometry as g
    >>> import numpy as np

Basic Constructors:

.. code-block:: python

    >>> g.PX(5)
    Point(x_=5.0 y_=0.0 z_=0.0, len=1)

    >>> g.PX(5).x
    array([5])

    >>> g.P0()
    Point(x_=0.0 y_=0.0 z_=0.0, len=1)

    >>> g.Point(np.random.random((5,3))*-5 -5)
    Point(x_=-7.5 y_=-7.41 z_=-7.59, len=100)

    >>> g.Point([[1,1,1], [2,2,2]]).data
    array([[1, 1, 1],
        [2, 2, 2]])

    >>> g.Coord.zero()
    Coord(ox_=0.0 oy_=0.0 oz_=0.0 x1_=1.0 y1_=0.0 z1_=0.0 x2_=0.0 y2_=1.0 z2_=0.0 x3_=0.0 y3_=0.0 z3_=1.0, len=1)

    >>>g.Coord.zero().origin
    Point(x_=0.0 y_=0.0 z_=0.0, len=1)


Create a Quaternion from Euler angles:

.. code-block:: python

    >>> g.Euler(np.pi, 0, 0)
    Quaternion(w_=0.0 x_=1.0 y_=0.0 z_=0.0, len=1)

    >>> g.Euldeg(180, 0, 0)
    Quaternion(w_=0.0 x_=1.0 y_=0.0 z_=0.0, len=1)

Basic Geometric Operations:

.. code-block:: python

    >>> g.Point(1,1,0) * np.linspace(0,10,10)
    Point(x_=5.0 y_=5.0 z_=0.0, len=10)

    >>> g.PX(1) * np.linspace(0,5,4) * 2
    Point(x_=5.0 y_=0.0 z_=0.0, len=4)

    >>> g.PX(-1,10).cumsum()
    Point(x_=-5.5 y_=0.0 z_=0.0, len=10)


