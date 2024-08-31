# Klipper TMC Autotune Notes

Notes on my experimentation with getting the Klipper autotune working on my Elegoo Neptune 4 Pro

Based on what SilentedFrost has discovered these are some of the attributes of the steppers. I know I am using the BJ42D22-53V04 for the X, assuming the Y will be identical. I will have to find evidence for the others.

![image](https://github.com/user-attachments/assets/75a469f6-d764-46fc-84b8-a697ec7c54df)

Some more specs on the steppers: https://www.3djake.com/elegoo/stepper-motor-4


```ini
[autotune_tmc stepper_x]
motor: bjd42d22-53v02
# motor: bj4d22-53v04

[autotune_tmc stepper_y]
motor: bjd42d22-53v02
# motor: bj4d22-53v04
```

## Fixing packages

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
