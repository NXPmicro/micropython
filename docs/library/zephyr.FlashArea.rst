.. currentmodule:: zephyr
.. _zephyr.FlashArea:

class FlashArea -- access to built-in flash storage
===================================================

Uses zephyr flash map API.

Constructors
------------

.. class:: FlashArea(id)

   Gets an object for accessing flash memory at partition specified by *id*.


Methods
-------

.. method:: FlashArea.readblocks(block_num, buf)
            FlashArea.readblocks(block_num, buf, offset)
.. method:: FlashArea.writeblocks(block_num, buf)
            FlashArea.writeblocks(block_num, buf, offset)
.. method:: FlashArea.ioctl(cmd, arg)

    These methods implement the simple and extended
    :ref:`block protocol <block-device-interface>` defined by
    :class:`uos.AbstractBlockDev`.

