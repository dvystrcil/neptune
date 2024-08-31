# Klipper TMC Autotune Notes

Notes on my experimentation with getting the Klipper autotune working on my Elegoo Neptune 4 Pro

## Installation

Read and install: https://github.com/andrewmcgr/klipper_tmc_autotune

## Configuration

Based on what SilentedFrost has discovered these are some of the attributes of the steppers. I can confirm that I am using the BJ42D22-53V04 for the X, assuming the Y will be identical. I will have to find evidence for the others.

![image](https://github.com/user-attachments/assets/75a469f6-d764-46fc-84b8-a697ec7c54df)

Specs on the steppers: https://www.3djake.com/elegoo/stepper-motor-4


```ini
########################################
# Klipper TMC Autotune configuration
########################################

[motor_constants bj42d22-53v04]             ; 1.2A version of the stepper motor
resistance: 2.1
inductance: 0.0046
holding_torque: 0.4
max_current: 1.20
steps_per_revolution: 200

[autotune_tmc stepper_x]
motor: bj42d22-53v04
tuning_goal: auto

[autotune_tmc stepper_y]
motor: bj42d22-53v04
tuning_goal: auto
```

## Fixing Packages

### Enum error
https://github.com/andrewmcgr/klipper_tmc_autotune/issues/40

run:
```bash
source ~/klippy-env/bin/activate
pip install enum
deactivate
```

### Signature error

run:
```bash
source ~/klippy-env/bin/activate
pip install funcsigs
deactivate
```

Change this line:
https://github.com/andrewmcgr/klipper_tmc_autotune/blob/main/autotune_tmc.py#L3
```python
import math, logging, os
from enum import Enum
from inspect import signature
from . import tmc
```
to
```python
import math, logging, os
from enum import Enum
#from inspect import signature
from funcsigs import signature
from . import tmc
```
Probably a better way to do this so it can try the first and fall back on the second

