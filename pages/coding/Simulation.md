# Simulation of retinotopic responses
In this short tutorial, I show how to simulate retinotopic responses to specific visual stimuli (e.g. a gabor) over a specific cortical area (e.g. to build a retinotopic decoder, see [Benvenuti et al. 2018]((https://www.ncbi.nlm.nih.gov/pubmed/30392796))).

Let's assume that you have measured the retinotopy of a large cortical area by using wide-field optical imaging, for example by using the method described by [Yang et al., 2007](https://journals.physiology.org/doi/full/10.1152/jn.00417.2007). Now, you want to compare responses recorded by zooming with the camera to different locations, at different angles, to simulations of the retinotopic layout of those responses.

The first thing you want to do, it is to fit the retinotopic maps you have measured with a [retininotopic model](pages/coding/RetinoModel.md) in order to get rid of noise and estimate the coordinates of the map at not measured locations. Then, you need to estimate which portion of the map you were imaging in each experiment. To do that you can use the configuaration of the cortical vasculature as a reference. Once you have the exact retinotopic map corresponding to your imaging area, you can project the visual stimulus from the visual field to the retinotopic map.


## 1. Fit the Model
Calibrate the retinotopic model with the retinotopic coordinates you have estimated with your measurements.

```Matlab
% RX, RY, : retinotopic cartesian coordinates
param = FitRetino(RX,RY)
```

<p align="center">
<img src="./figures/FitRetino.png" width="70%">
</p>

## 2. Match vasculature
 Find the correspondence between the retinotopy's experiment vasculature and the vasculature of the experiment you want to simulate. To do that, use my [vasculature registration software](https://github.com/giacomox/BruteForceRegistration/blob/master/README.md).

```Matlab
% I,J : vasculature images
tform = SelectFeatures(I,J);
```

<p align="center">
<img src="./figures/selection.png" width="90%">
</p>


## 3. Generate Retinotopic map
Generate the full map by inverting the retinotopic model.

```Matlab
[Xq, Yq ]= meshgrid( 1:size(RX,1) , 1:size(RX,2) );
[Uq Vq] = RetinoModel_INV(Xq,Yq,param)
```
<p align="center">
<img src="./figures/Interp2.png" width="70%">
</p>



## 4. Get ROI retinotopy
 Use the transformation coordinates (tform) you have estimated in point 2, to generate the retinotopy of the experiment you want to simulate (i.e. crop the retinotopic map in the same way you cropped the vasculature image to match the two experiments)

```Matlab
UU = imwarp(Uq,tform,'OutputView', imref2d(size(I)));
VV = imwarp(Vq,tform,'OutputView', imref2d(size(I)));
```

<p align="center">
<img src="./figures/M15D20141209R1_Retino.png" width="70%">
</p>

## 5. Simulate response
 Project visual stimulus to retinotopic map and compare with real response

 You can use the same function you used to generate the visual stimulus in the visual space to generate the projection to  the cortical ROI<sup>4</sup> .

```Matalb
[x,y] = Gabor2D(UU,VV,param_stimulus)
```

<p align="center">
<img src="./figures/M15D20141209R1.png" width="70%">
</p>

In some cases (e.g. if using another software to display the stimuli and set their position on screen) you have to take in account the (i) stimulus position (the Matlab fun generates the stimulus image but the position is changed through the stimulation software) and (ii) the fact that the Y axis of the stimulus is inverted.

```Matlab
%% Set stimulus position
% Cx and Cy are the cartesian coordinates of the stimulus' center
Cx = 1; Cy = -1;
% UU and VV are the visual coordinates corresponding to the cortical
% coordinates we want to use to generate the retinotopic response
% simulation (the ROI).
UU = UU - Cx ;
VV = VV - Cy ;
```

## 6. Cortical point image
Retinotopic projections must be "blurred" by convolving with a 2D gaussian kernel with adequate parameters, to simulate the effect of the CPI.

Since the receptive fields of sensory neurons extend over space (e.g. the average full-width-at-half-height (FWHH) of the receptive field of V1 neurons at 2-5 dva of eccentricity is ~0.6 dva)<sup>1,2</sup>, the receptive fields of laterally-displaced neurons largely overlap, and each point in the visual field is represented by a widespread population of neurons. This area, which is referred to as the *cortical point image (CPI)*, in V1 spans a region with full-width-at-half-height of ~2 mm for spiking activity and ~4 mm for sub-threshold activity<sup> 3</sup>. The predicted retinotopic response to a visual stimulus can therefore be obtained by projecting the stimulus to the retinotopic map and then convolving the result with the sub-threshold CPI<sup>4</sup> (e.g. 2D Gaussian filter, corresponding to a "blurring" effect). The effect of the CPI on the amplitude of the predicted response is negligible at low SFs, but it becomes dominant as the spatial frequency of the stimulus increases.

```
TO DO
```


## References
1. Van Essen, D. C. & Newsome, W. T. The visual field representation in the striate cortex of the macaque monkey: Asymmetries, anisptropies, and indiviual variability. Vision Res. 24, 429–448 (1984).

2. Hubel, D. H. & Wiesel, T. N. Uniformity of monkey striate cortex: a parallel relationship between field size, scatter, and magnification factor. J. Comp. Neurol. 158, 295–305 (1974).

3. Yang, Z., Heeger, D.J., and Seidemann, E. (2007). Rapid and precise retino- topic mapping of the visual cortex obtained by voltage-sensitive dye imaging in the behaving monkey. J. Neurophysiol. 98, 1002–1014

4. Palmer, C. R., Chen, Y. & Seidemann, E. [Uniform spatial spread of population activity in primate parafoveal V1.](https://journals.physiology.org/doi/full/10.1152/jn.00417.2007) J. Neurophysiol. 107, 1857–67 (2012).

5. Benvenuti, G. et al. [Scale-Invariant Visual Capabilities Explained by Topographic Representations of Luminance and Texture in Primate V1.](https://www.ncbi.nlm.nih.gov/pubmed/30392796) Neuron 1–9 (2018).
