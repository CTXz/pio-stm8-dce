# PlatformIO + STM8-DCE

See project https://github.com/CTXz/STM8-DCE for main information.

This is an example project that integrates the STM8 Dead Code Eliminator into the PlatformIO build process.

Shortly before PlatformIO wants to link the firmware from the generated `.rel` files, the extra script runs all generated assembly files through the dead code optimizer and then regenerates the `.rel` (and `.lst` and `.sym`) files based of them.

In even the simplest [GPIO blink demo](https://github.com/platformio/platform-ststm8/tree/develop/examples/spl-blink), this optimization makes the project go from

```
RAM:   [          ]   0.0% (used 0 bytes from 1024 bytes)
Flash: [=         ]   8.0% (used 655 bytes from 8192 bytes)
```

to

```
RAM:   [          ]   0.0% (used 0 bytes from 1024 bytes)
Flash: [=         ]   6.5% (used 532 bytes from 8192 bytes)
```

it might have an even bigger impact in larger projects that uses more parts of the SPL.

Note: This is only tested against `framework = spl`. The `framework = arduino` framework uses SDCC 3.x instead of 4.x, which the STM8-DCE tool might fail against.

## Warning

Does not seem to work on Linux because it uses the older SDCC 4.1.0 instead of 4.2.0 there. Produces a
```
?ASlink-Warning-Undefined Global '___str_0' referenced by module 'stm8s_gpio'
```
linking error at the end.

PlatformIO team should update the SDCC version accordingly.