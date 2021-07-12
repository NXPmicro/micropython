.. _storage_zephyr:

Filesystem
==========

Storage modules support virtual filesystem with FAT and littlefs formats, backed by either
Zephyr DiskAccess or FlashArea (flash map) APIs.

See `uos Filesystem Mounting <https://docs.micropython.org/en/latest/library/uos.html?highlight=os#filesystem-mounting>`_.

Disk Access
-----------

Uses `Zephyr Disk Access API <https://docs.zephyrproject.org/latest/reference/storage/disk/access.html>`_ and
implements the `uos.AbstractBlockDev` protocol.

This module allows access to storage devices on the board, such as support for
SD card controllers and interfacing with SD cards via SPI. SD cards must be present 
at boot & not removed; they will be auto detected and initialized by filesystem at boot.
Use the disk driver interface and a file system to acceess SD cards via disk access (see below).

Example usage of FatFS with an SD card on the mimxrt1050_evk board::
    
    import os
    from zephyr import DiskAccess
    bdev = zephyr.DiskAccess('SDHC')        # create block device object using DiskAccess
    os.VfsFat.mkfs(bdev)                    # create FAT filesystem object using the disk storage block
    os.mount(bdev, '/sd')                   # mount the filesystem at the SD card subdirectory
    with open('/sd/hello.txt','w') as f:    # open a new file in the directory
        f.write('Hello world')              # write to the file
    print(open('/sd/hello.txt').read())     # print contents of the file


Flash Area
----------

Uses `Zephyr Flash map API <https://docs.zephyrproject.org/latest/reference/storage/flash_map/flash_map.html#>`_ and
implements the `uos.AbstractBlockDev` protocol.

This module allows access to device flash partition information.
Flash areas consist of a globally unique ID number, the name of the flash device the partition is in, 
and the start offset (expressed in relation to the flash memory beginning address per partition) 
and size of the partition that the device represents. For fixed flash partitions, data from the device 
tree is used. 

Flash storage usage ...

Example usage with the internal flash on the reel_board or the rv32m1_vega_ri5cy board::

    import os
    from zephyr import FlashArea
    bdev = FlashArea(FlashArea.STORAGE, 4096)
    os.VfsLfs2.mkfs(bdev)
    os.mount(bdev, '/flash')
    with open('/flash/hello.txt','w') as f:
        f.write('Hello world')
    print(open('/flash/hello.txt').read())

Things get a little trickier with the frdm_k64f due to the micropython
application spilling into the default flash storage partition defined
for this board. The zephyr build system doesn't enforce the flash
partitioning when mcuboot is not enabled (which it is not for
micropython). For now we can demonstrate that the littlefs filesystem
works on frdm_k64f by constructing the FlashArea block device on the
mcuboot scratch partition instead of the storage partition. Do this by
replacing the FlashArea.STORAGE constant above with the value 4.
