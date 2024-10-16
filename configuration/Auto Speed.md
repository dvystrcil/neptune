# Testing Auto Speed 

## Where to find it:
https://github.com/Anonoei/klipper_auto_speed

## Some explanation on how to use it:
https://www.youtube.com/watch?v=8d49_QUmP9k

## Running AUTO_SPEED with Autotune set to `silent`

```
// AUTO SPEED found maximum acceleration after 114.36s
// | X max: 8089
// | Y max: 12601
// 
// Recommended values:
// | X max: 6471
// | Y max: 10081
```
```
// AUTO SPEED found maximum velocity after 134.67s
// | X max: 92
// | Y max: 352
// 
// Recommended values
// | X max: 74
// | Y max: 282
// Recommended velocity: 74
```
```
// AUTO SPEED found recommended acceleration and velocity after 249.04s
// | X max: a6471 v74
// | Y max: a10081 v282
// Recommended accel: 6471
// Recommended velocity: 74
```

![AUTO_SPEED_GRAPH_2024-09-14_22_30_47_x](https://github.com/user-attachments/assets/1ba282d8-4deb-4506-b64b-6077a71301b1)

![AUTO_SPEED_GRAPH_2024-09-14_22_34_53_y](https://github.com/user-attachments/assets/4ef7f4f3-85fb-4095-a2bc-999e75f474e5)

## Running AUTO_SPEED with Autotune set to `performance`

```
// AUTO SPEED found maximum acceleration after 76.15s
// | X max: 97937
// | Y max: 97937
// 
// Recommended values:
// | X max: 78350
// | Y max: 78350
```
```
// AUTO SPEED found maximum velocity after 97.30s
// | X max: 1261
// | Y max: 965
// 
// Recommended values
// | X max: 1009
// | Y max: 772
// Recommended velocity: 772
```
```
// AUTO SPEED found recommended acceleration and velocity after 173.47s
// | X max: a78350 v1009
// | Y max: a78350 v772
// Recommended accel: 78350
// Recommended velocity: 772
```
![image](https://github.com/user-attachments/assets/ef777066-b3a8-4b2d-ac7b-08c165bf6a24)

![image](https://github.com/user-attachments/assets/9231bd24-601d-456b-98a0-65afe2eda006)

## Running with Autotune set to `performance` and some different defaults

After talking with SilencedFrost he said the accel wasn't making sense. My numbers were the same as the video, which was odd. I did some poking around and found this issue https://github.com/Anonoei/klipper_auto_speed/issues/25

So I used the same numbers as the person in the issue:
```config
[auto_speed]
margin: 2.5 ; How far away from your axes to perform movements
accel_min: 1000.0 ; Minimum acceleration test may try
accel_max: 30000.0 ; Maximum acceleration test may try

velocity_min: 50.0 ; Minimum velocity test may try
velocity_max: 700.0 ; Maximum velocity test may try
```

```
// AUTO SPEED found maximum acceleration after 76.51s
// | X max: 29395
// | Y max: 29395
// 
// Recommended values:
// | X max: 23516
// | Y max: 23516
```
```
// AUTO SPEED found maximum velocity after 77.16s
// | X max: 686
// | Y max: 604
// 
// Recommended values
// | X max: 549
// | Y max: 483
// Recommended velocity: 483
```
```
// AUTO SPEED found recommended acceleration and velocity after 153.68s
// | X max: a23516 v549
// | Y max: a23516 v483
// Recommended accel: 23516
// Recommended velocity: 483
```

The numbers still sound a little suspicious. Shouldn't the velocity be hitting 700, since the uncapped run was at 772?

