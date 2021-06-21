.. _storage_zephyr:

Filesystem
==========

Storage modules support virtual filesystem with FAT and littlefs formats, backed by either
Zephyr DiskAccess or FlashArea (flash map) APIs.

Disk Access
-----------

Uses `Zephyr Disk Access API <https://docs.zephyrproject.org/latest/reference/storage/disk/access.html>`_ and
implements the `uos.AbstractBlockDev` protocol.


Flash Area
----------

Uses `Zephyr Flash map API <https://docs.zephyrproject.org/latest/reference/storage/flash_map/flash_map.html#>`_ and
implements the `uos.AbstractBlockDev` protocol.


