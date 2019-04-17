# Simulation of retinotopic responses in V1

## 1. Fit the Model
#### Calibrate retinotopic model with real retinotopy

```Matlab
% RX, RY, : retinotopic coordinates
param = FitRetino(RX,RY)
```

<p align="center">
<img src="./figures/FitRetino.png" width="70%">
</p>

## 2. Match vasculature
 Find correspondence btw retinotopy's experiment vasculature and the vasculature of the experiment you want to simulate.

```Matlab
% I,J : vasculature images
tform = SelectFeatures(I,J);
```

<p align="center">
<img src="./figures/Selection.png" width="70%">
</p>


## 3. Generate Retinotopic map

```Matlab
[Xq, Yq ]= meshgrid( 1:size(RX,1) , 1:size(RX,2) );
[Uq Vq] = RetinoModel_INV(Xq,Yq,param)
```
<p align="center">
<img src="./figures/Interp2.png" width="70%">
</p>



## 4. Get ROI retinotopy
 Use the vasculature coordinates to generate the retinotopy of the experiment you want to simulate

```Matlab
UU = imwarp(Uq,tform,'OutputView', imref2d(size(I)));
VV = imwarp(Vq,tform,'OutputView', imref2d(size(I)));
```

<p align="center">
<img src="./figures/M15D20141209R1_Retino.png" width="70%">
</p>

## 5. Simulate response
 Project visual stimulus to retinotopic map and compare with real response

 You can use the same function you used to generate the visual stimulus in the visual space to generate the projection to  the cortical ROI.

```Matalb
[x,y] = Gabor2D(UU,VV,param_stimulus)
```

<p align="center">
<img src="./figures/M15D20141209R1.png" width="70%">
</p>
