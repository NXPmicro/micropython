.. _pins_zephyr:

GPIO Pins
=========

Use :ref:`machine.Pin <machine.Pin>` to control I/O pins.

For Zephyr, pins are initialized using a tuple of port and pin number ``(\"GPIO_x\", pin#)``
for the ``id`` value. For example to initialize a pin for the red LED on a FRDM-k64 board::

        LED = Pin(("GPIO_1", 22), Pin.OUT)
        
Reference your board's datasheet or Zephyr documentation for pin numbers.

Interrupts
----------

The Zephyr port also supports interrupt handling for Pins using `machine.Pin.irq() <machine.Pin.irq>`. 
To respond to Pin change IRQs run::

    from machine import Pin

    SW2 = Pin(("GPIO_2", 6), Pin.IN)
    SW3 = Pin(("GPIO_0", 4), Pin.IN)

    SW2.irq(lambda t: print("SW2 changed"))
    SW3.irq(lambda t: print("SW3 changed"))

    while True:
        pass

pin.irq(handler=None, trigger=IRQ_FALLING|IRQ_RISING, hard=False)
...
