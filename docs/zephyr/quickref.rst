.. _zephyr_quickref:

Quick reference for the Zephyr port
===================================

Below is a quick reference for the Zephyr port. If it is your first time working with this port please consider reading the following sections first:

.. toctree::
   :maxdepth: 1

   general.rst
   tutorial/index.rst

Running Micropython
-------------------

See the corresponding section of the tutorial: :ref:`intro`.

Delay and timing
----------------

Use the :mod:`time <utime>` module::

    import time

    time.sleep(1)               # sleep for 1 second
    time.sleep_ms(500)          # sleep for 500 milliseconds
    time.sleep_us(10)           # sleep for 10 microseconds
    start = time.ticks_ms()     # get millisecond counter
    delta = time.ticks_diff(time.ticks_ms(), start) # compute time difference

Pins and GPIO
-------------

Use the :ref:`machine.Pin <machine.Pin>` class::

    from machine import Pin

    pin = Pin(("GPIO_1", 21), Pin.IN)   # create input pin on GPIO1
    print(pin)                          # print pin port and number

    pin.init(Pin.OUT, Pin.PULL_UP, value=1)     # reinitialize pin

    pin.value(1)                        # set pin to high
    pin.value(0)                        # set pin to low
    
    pin.on()                            # set pin to high    
    pin.off()                           # set pin to low

    pin = Pin(("GPIO_1", 21), Pin.IN)   # create input pin on GPIO1

    pin = Pin(("GPIO_1", 21), Pin.OUT, value=1)         # set pin high on creation

    pin = Pin(("GPIO_1", 21), Pin.IN, Pin.PULL_UP)      # enable internal pull-up resistor


    switch = Pin(("GPIO_2", 6), Pin.IN)
    SW3 = Pin(("GPIO_0", 4), Pin.IN)

    switch.irq(lambda t: print("SW2 changed"))
    SW3.irq(lambda t: print("SW3 changed"))

    ...

Hardware I2C bus
----------------

Hardware I2C is accessed via the :ref:`machine.I2C <machine.I2C>` class::

    from machine import I2C

    i2c = I2C("I2C_0")          # construct an i2c bus
    print(i2c)                  # print device name

    i2c.scan()                  # scan the device for available I2C slaves

    i2c.readfrom(0x1D, 4)                # read 4 bytes from slave 0x1D
    i2c.readfrom_mem(0x1D, 0x0D, 1)      # read 1 byte from slave 0x1D at slave memory 0x0D

    i2c.writeto(0x1D, b'abcd')           # write to slave with address 0x1D
    i2c.writeto_mem(0x1D, 0x0D, b'ab')   # write to slave 0x1D at slave memory 0x0D

    buf = bytearray(8)                  # create buffer of size 8
    i2c.writeto(0x1D, b'abcd')          # write buf to slave 0x1D

Hardware SPI bus
----------------

Hardware SPI is accessed via the :ref:`machine.SPI <machine.SPI>` class::

    from machine import SPI

    spi = SPI("SPI_0")          # construct a spi bus with default configuration
    spi.init(baudrate=100000, polarity=0, phase=0, bits=8, firstbit=SPI.MSB) # set configuration

    # equivalently, construct spi bus and set configuration at the same time
    spi = SPI("SPI_0", baudrate=100000, polarity=0, phase=0, bits=8, firstbit=SPI.MSB)
    print(spi)                  # print device name and bus configuration

    spi.read(4)                 # read 4 bytes on MISO
    spi.read(4, write=0xF)      # read 4 bytes while writing 0xF on MOSI

    buf = bytearray(8)          # create a buffer of size 8
    spi.readinto(buf)           # read into the buffer (reads number of bytes equal to the buffer size)
    spi.readinto(buf, 0xF)      # read into the buffer while writing 0xF on MOSI

    spi.write(b'abcd')          # write 4 bytes on MOSI

    buf = bytearray(4)                  # create buffer of size 8
    spi.write_readinto(b'abcd', buf)    # write to MOSI and read from MISO into the buffer
    spi.write_readinto(buf, buf)        # write buf to MOSI and read back into the buf

USocket
-------

module for networking (IPv4/IPv6)::

    ...

Disk Access
-----------

Use the :ref:`zephyr.DiskAccess <zephyr.DiskAccess>` class::

    from zephyr import DiskAccess

    disk = DiskAccess(*disk_name*)      # create disk object
    print(disk)                         # print disk name

    disk.readblocks(*block num*, *buffer*, *offset*)
    disk.writeblocks(*block num*, *buffer*, *offset*)

    disk.ioctl(*cmd*, *arg*)


Flash Area
----------

Use the :ref:`zephyr.FlashArea <zephyr.FlashArea>` class::

    from zephyr import FlashArea

    flash = FlashArea(1, 8)             # create flash map object with block size 8
    print(flash)                        # print flash area id

    buf = bytearray(8)
    flash.readblocks(2, buf, 0)         # read into buffer from block 2 offset 0
    flash.writeblocks(2, b'abcd', 0)    # write buffer to block 2 offset 0

    flash.ioctl(6, 2)                   # erase block number 2

