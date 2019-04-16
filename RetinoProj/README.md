# Simulation of retinotopic responses in V1

Calibrate retinotopic model with real retinotopy

```Matlab
% RX, RY, : retinotopic coordinates
param = FitRetino(RX,RY)
```
2. Find correspondence btw retinotopy's experiment vasculature and the vasculature of the experiment you want to simulate.

```Matlab
% I,J : vasculature images
tform = SelectFeatures(I,J);
```
3. Use the vasculature coordinates to generate the retinotopy of the experiment you want to simulate

```Matlab
[Xq, Yq ]= meshgrid( 1:size(RX,1) , 1:size(RX,2) );
[Uq Vq] = RetinoModel_INV(Xq,Yq,param)
```
4.

```Matlab
UU = imwarp(Uq,tform,'OutputView', imref2d(size(I)));
VV = imwarp(Vq,tform,'OutputView', imref2d(size(I)));
```
5. Project visual stimulus to retinotopic map and compare with real response

```Matalb
[x,y] = Stimulus(UU,VV,param_stimulus)
```
