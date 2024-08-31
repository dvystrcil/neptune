# 

Notes on my experimentation with getting the Klipper autotune working on my Elegoo Neptune 4 Pro

Based on what SilentedFrost has discovered these are some of the attributes of the steppers. I know I am using the BJ42D22-53V04 for the X, assuming the Y will be identical. I will have to find evidence for the others.

![image](https://github.com/user-attachments/assets/75a469f6-d764-46fc-84b8-a697ec7c54df)



```ini
[autotune_tmc stepper_x]
motor: bjd42d22-53v02
# motor: bj4d22-53v04

[autotune_tmc stepper_y]
motor: bjd42d22-53v02
# motor: bj4d22-53v04
```
