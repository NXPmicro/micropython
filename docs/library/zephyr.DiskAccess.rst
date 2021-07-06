.. currentmodule:: zephyr
.. _zephyr.DiskAccess:

class DiskAccess -- access to disk storage
===============================================

Uses zephyr disk access API.

Constructors
------------

.. class:: DiskAccess(disk_name)

   Gets an object for accessing disk memory of the specific disk.


Methods
-------

.. method:: DiskAccess.readblocks(block_num, buf)
            DiskAccess.readblocks(block_num, buf, offset)
.. method:: DiskAccess.writeblocks(block_num, buf)
            DiskAccess.writeblocks(block_num, buf, offset)
.. method:: DiskAccess.ioctl(cmd, arg)

    These methods implement the simple and extended
    :ref:`block protocol <block-device-interface>` defined by
    :class:`uos.AbstractBlockDev`.

