PyTables seeks volunteer contributors for 2 projects
====================================================

**Add h5py as a backend to PyTables**
——————————————————

The goal here is to define a new way to access I/O that would allow a new version of PyTables (probably v4.x) to use different backends.  As h5py is a great interface for HDF5 the main priority is for interfacing h5py so as to allow HDF5 access through it.  This way PyTables can leverage h5py to access the most advanced features of HDF5 while still delivering features like advanced table management, fast table queries and easy access to advanced Blosc meta-compressors (and with it, to a wide array of codecs, like LZ4, Snappy and Zstandard).  You can see a more detailed blog about our vision here:

https://hdfgroup.org/wp/2015/09/python-hdf5-a-vision/

In fact, work has already started on that front: in August 2016 a handful of PyTables core developers gathered with the goal to start this precise task, and although we certainly did a lot progress on the Table object (the fundamental one in PyTables), there is still quite a bit of work to do.  See the final report that we wrote at the end of our hackfest in Perth:

https://github.com/PyTables/PyTables/blob/pt4/doc/New-Backend-Interface.rst

The idea is to continue the job done till now and release an alpha release with the basic Table, CArray, EArray and VLarray objects working, and hope to get some traction for promptly releasing a stable version unifying the best of PyTables and h5py packages.




**Implement column-wise tables in PyTables**
————————————————————————————————

Both NumPy and PyTables implement tables that are arranged row-wise. This approach works well for performing lookups on top of very large tables with a relatively small record size (typically <= 100 bytes).

However, there are a lot of user cases where a column-wise arrangement would be more beneficial, like for example being able to add or remove columns efficiently, having tables with a much larger record size, columns that could have a variable length elements (varchar, ragged arrays or BLOBs) and better compression performance (based in the fact that entropy varies less in the elements along a column than the ones in a row).

We have a more complete report about this, and the work to be done, here:

https://github.com/PyTables/proposal/blob/master/column-wise-pytables.rst

Many areas would benefit from this, but specially the pandas community because this should allow to represent DataFrames on disk that are closer to its representation in memory, opening the door to a more faithful disk-mapped representation of the DataFrame. Also, column-wise tables can find a good niche in the financial world, where access to columns is many times preferred (because of efficiency reasons) than access to whole rows.

