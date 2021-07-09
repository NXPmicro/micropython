.. _storage_zephyr:

Filesystem
==========

Storage modules support virtual filesystem with FAT and littlefs formats, backed by either
Zephyr DiskAccess or FlashArea (flash map) APIs.



Disk Access
-----------

Uses `Zephyr Disk Access API <https://docs.zephyrproject.org/latest/reference/storage/disk/access.html>`_ and
implements the `uos.AbstractBlockDev` protocol.

Access to storage devices.
Support for SD card controllers and interfacing with SD cards via SPI. Cards must be present at boot & not removed.
Use disk driver interface and a file system to accee SD cards via disk access.
SD cards will be auto detected and initialized by filesystem at boot.

specific meaning to fs use?

Flash Area
----------

Uses `Zephyr Flash map API <https://docs.zephyrproject.org/latest/reference/storage/flash_map/flash_map.html#>`_ and
implements the `uos.AbstractBlockDev` protocol.

Allows access to device flash partition information.
Flash areas have globally unique ID numbers, name of flash device the partition is in, and start offset and size of the partition
that the device represents.

Uses data from the device tree for fixed flash partitions.

Offsets are expressed in relation to flash memory beginning address per partition

how to use?
