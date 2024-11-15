Notes on how I added a Z endstop to my Neptune 4 Pro. This should be compatible with the non-pro, however, due to the nonexistent off-bed range of the X axis on the Plus and Max a different solution will need to be designed.
## Just a brain dump for now

## Limitations
- If the nozzle is dirty, your offset is not going to be accurate
- It may be difficult to clean the nozzle without homing first
- you cannot use bed corners on the same side as the offset end stop

## Parts
- [Cable Endstop Mechanical Limit Optical Switch Connection Wire](https://www.amazon.com/gp/product/B0B6FHZLLF) Note: 1M is too long, if you can find even a third of that length it would work.
- [X/Y/Z Axis End Stop Limit Switch](https://www.amazon.com/gp/product/B098PXX6Q7)
- [2.6x16mm Dowel Pins](https://www.amazon.com/gp/product/B0BCFN4RHN0)
- [0406 Sleeve Bearings](https://www.amazon.com/gp/product/B0CXXGHT1P)
- 2 - M3*6 button head screws
- 1 - M4*20 hex head bolt with nut (poor man's t-nut)
- [Printable Parts](https://github.com/dvystrcil/neptune/tree/main/stl/z-endstop)

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


## Installation Pictures
The -Z pin PC3
![PXL_20241104_004744595 MP](https://github.com/user-attachments/assets/6362001b-c09c-4494-87e3-b5266c0ecc98)
I had to remove the top and both of the bottom panels to route the wire through, but there is plenty of clearance.
![PXL_20241104_022942347](https://github.com/user-attachments/assets/e0b8dcf1-3519-4921-a214-b947d757e708)
There is a clearance issue that needs to be resolved. In my case, there is no pressure from the arm so the rubbing is minimal.
![PXL_20241104_023002523](https://github.com/user-attachments/assets/7a22dc30-36ad-408c-98c2-95be230c4d12)

## On and Off the Plate
- 0.2 layer height
- Any filament is OK
- Brim Type: Mouse ear
- I used supports: Normal with Snug
![image](https://github.com/user-attachments/assets/c83d34b6-0ac3-433a-9a41-686fa11b9b04)
![PXL_20241115_000939859](https://github.com/user-attachments/assets/8b79132e-35ab-44f0-8ecc-b3f460978c1d)
![PXL_20241115_003716392](https://github.com/user-attachments/assets/fe577832-1e0b-485d-a59e-7ee3a4109dd3)

## References
- https://www.youtube.com/watch?v=Q2uBK5XzX4I Note: this video fails to mention what the zero_reference_position does.
- https://www.klipper3d.org/G-Codes.html?h=z_offset_app#z_endstop_calibrate
- https://www.klipper3d.org/G-Codes.html?h=z_offset_app#z_offset_apply_endstop
- https://www.klipper3d.org/Endstop_Phase.html
- https://www.klipper3d.org/Bed_Mesh.html?h=zero_reference_position#configuring-the-zero-reference-position
