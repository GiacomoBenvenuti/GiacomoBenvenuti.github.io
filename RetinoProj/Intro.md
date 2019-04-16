
# Retinotopy Model

## Background
test

<p align="center">
<img src="./figures/retino3D.jpg" width="70%">
</p>

<p style="font-size:80%;" title="legend">
 <b>Fig1 Retinotopic maps in V1 of different species.</b> <br>
 Each panel shows medial (left) and posterior (center) views of partially ‘inflated’ right hemispheres of the brains of the three species, based on MRI reconstructions of the middle layers of the cortex. The location of area V1 is indicated in red. On the right are two-dimensional models of the retinotopic organization of V1, fitted to high-resolution data. The grey region indicates the part of V1 that is buried within the calcarine sulcus, and the yellow region indicates the part that is exposed on the surface of the occipital lobe (white indicates regions that are exposed on the mesial surface of the brain). Corresponding landmarks are shown for macaque and marmoset monkey. Scale bars in all panels = 5 mm. <a href="https://doi.org/10.1016/j.cub.2012.11.003"> (https://doi.org/10.1016/j.cub.2012.11.003 </a>)
</p>

<p align="center">
<img src="./figures/retinoWoman.png" width="70%">
</p>

<p style="font-size:80%;" title="legend">
 <b>Fig2 Example of retinotopic projection to primary visual cortex (V1).</b> <br>
 </p>

## Simulation of retinotopic responses in V1

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
