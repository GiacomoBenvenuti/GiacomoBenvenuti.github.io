
## RF area estimation from the response to a sparse-noise stimulus

* As a first step toward estimating the significant RF area, we calculated a PSTH for each position stimulated using the sparse-noise stimulus (e.g. 10 * 10 squares grid; squares side = 0.6deg) by reverse correlation. PSTHs were estimated from 300ms before the stimulus was presented to 300ms after (time bin = 10ms).
<br>
* Next, we estimated a map of the evoked response over the SN grid by averaging each PSTH in the time window for which the response was considered significant. This integration window was estimated independently for each cell, based on the PSTH of the SN position producing the stronger evoked response.
  - First, we found the SN position with the stronger response in the 150ms time window just after stimulus presentation (Pevoked).
  - Second, we found the the SN position producing the stronger response in a time window before stimulus presentation (Pcontrol).
  - We used the first 150ms before stimulus presentation in the PSTH corresponding to Pcontrol to estimate a significance threshold, following equation 1
  $$Thr^{Time} = <PSTH_{pos(x,y)}(1:150)>+(C*<std(PSTH_{pos(x,y)}(1:150))>)$$
  where C is the Z-score corresponding to one-tail p-value of 0.25 with Bonferroni correction (Matlab: icdf('normal',1-Alph/N,0,1), where N is the time-bins number).
  - Then, we detected the interval for which responses were crossing this threshold in the 150ms after stimulus presentation, in the PSTH of Pevoked. This interval was used as integration window.
  <br>
* After estimating the evoked response map we calculated the area presenting a significant response.
  - To do that, we first estimated a pre-stimulus response map by averaging the PSTHs responses in an interval with the same duration of the integration window but before the stimulus presentation.
  - Then, we estimated  a confidence threshold based on the responses at different position of the pre-stimulus map in a  similar way as we did above in the time domain.
$$Thr^{Space} = <PSTH_{t}(x,y)>+(C*<std(PSTH_{t}(x,y))>)$$
  - Eventually, we fitted the responses passing the threshold on the map with an ellipse.
