Notes on how I added a Z endstop to my Neptune 4 Pro. This should be compatible with the non-pro, however, due to the nonexistent off-bed range of the X axis on the Plus and Max a different solution will need to be designed.
## Just a brain dump for now

## Parts
- [Cable Endstop Mechanical Limit Optical Switch Connection Wire](https://www.amazon.com/gp/product/B0B6FHZLLF)
- [X/Y/Z Axis End Stop Limit Switch](https://www.amazon.com/gp/product/B098PXX6Q7)
- [2.6x16mm Dowel Pins](https://www.amazon.com/gp/product/B0BCFN4RHN0)
- [0406 Sleeve Bearings](https://www.amazon.com/gp/product/B0CXXGHT1P)
- some screws... 

## Code
When reading this configs keep in mind that the Pro has a bed size of 235x235 so the center would be 117.5,117.5
```
[stepper_z]
...
endstop_pin: ^!PC3
homing_speed: 8
homing_retract_dist: 5
second_homing_speed: 3
```
```
[gcode_macro Z_ENDSTOP_CALIBRATE]
rename_existing: Z_ENDSTOP_CALIBRATE_RAW
gcode: 
  BED_MESH_CLEAR
  G28
  G90 # Abs positioning
  G1 X117.5 Y117.5 F6000  
  Z_ENDSTOP_CALIBRATE_RAW {rawparams}
```
```
[safe_z_home]
...
home_xy_position: -5,117.5
```
```
[bed_mesh]
...
zero_reference_position: 117.5,117.5
```



## References
- https://www.youtube.com/watch?v=Q2uBK5XzX4I Note: this video fails to mention what the zero_reference_position does.
- https://www.klipper3d.org/G-Codes.html?h=z_offset_app#z_endstop_calibrate
- https://www.klipper3d.org/G-Codes.html?h=z_offset_app#z_offset_apply_endstop
- https://www.klipper3d.org/Endstop_Phase.html
- https://www.klipper3d.org/Bed_Mesh.html?h=zero_reference_position#configuring-the-zero-reference-position
