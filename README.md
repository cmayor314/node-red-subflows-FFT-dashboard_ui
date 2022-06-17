# node-red-subflows-FFT-dashboard_ui
node-red-subflows-FFT-dashboard-ui accepts raw streaming numbers.  An entire dashboard-ui tab is devoted to information and control of the display.

This subflow is its own tab in the dashboard-ui display.

I put this tool together for my own purposes, and thought others might make use of it as well.  My application was display and analysis of low sampling rates (shown as "Average Sample Frequency" in the screencaps, below.

  
# Screencaps

![image](https://user-images.githubusercontent.com/105139648/174211742-bc2c1930-5828-4054-97e7-32734d1f83f6.png)  ![image](https://user-images.githubusercontent.com/105139648/174219573-93fec829-8300-4528-ba9d-27002a74abc2.png)

--------------------------------

![image](https://user-images.githubusercontent.com/105139648/174221099-20035e2b-87c9-498e-a23e-f98889e1f722.png)


# Dashboard UI Features

## FFT Buffer Filled

  The percentage shown is the amount of data in the buffer before calculations are accurate.
  It is feedback for my impatient user (me).

  Until the data buffer on which the FFT is filled, the rest of the buffer has been
  padded with zeros.

  When the sub-flow is freshly started, none of the calculations shown are accurate because
  of the zero-padding.

## Sample Size

  Select from various Power-of-2 buffer sizes (this is a requirement for the used FFT algorithm).
  The smallest size buffer is 2 (2^0), and the largest buffer currently programmed is 524288 (2^19),
  but there is no reason it cannot be expanded if your CPU power is capable. (see FFT Calculation Time)

## FFT Magnitudes Graph

  Live graph displays the magnitudes from the FFT calculation, starting with a0, followed by
  all the frequency bins.  If a0 is not included in the graph (as per the switch), a0 is shown as a zero 
  quantity.  The number of frequncies shown can be selected, below.

  The graph is **not** accurate until the FFT Buffer is Filled.  If you are into the Fourier
  Series, the look of the magnitude and the phase graphs is due to the zero-padding.

## Include a0 switch

  The a0 value might dwarf all the other values when in linear magnitude mode.
  This switch allows you to include or exclude a0 from the live graph.

## Display Magnitudes as linear/log (dB<units>)
  
  Choose to see magnitudes in linear format, or logarithmic.
  (the logarithmic units are deciBels of the input data units)
  Log(magnitude) mode looks pretty crappy, I gotta say.

## Pause/Run
  
  Pause/Run button on the left, text of "running" or "paused"
  
## a0 live
 
  The value of a0 for the currently-shown FFT.  
  a0 is literally the average value over your entire buffer size.
  
## FFT Phases Graph
  
  Live graph displays the phase angles from the FFT calculation, starting with zero for a0, followed by
  all the frequency bins.  The a0 switch does not affect this graph. 
  The number of frequncies shown can be selected, below.
  
  The graph is **not** accurate until the FFT Buffer is Filled.  If you are into the Fourier
  Series, the look of the magnitude and the phase graphs is due to the zero-padding.
  
## Select Number of Frequencies Shown
  
  Can choose from 1-256.   256 pixels is near the useful limit on the 6 unit wide display.
  This does NOT affect the fft calculation, the purpose is to limit the number of frequencies
  shown at any update (e.g., for zooming in)
  
## Clear Graphs
  
  Clicking the buttons clears all the graphs.  The graphs will update as soon as the next
  data arrives.  The button is useful for clearing any weird data from re-deployed flows.

## a0(t) graph
  
  Graph of the a0 from FFT calculations, as it changes with each sample.
 
## FFT Calculation Time
  
  The amount of time it just took to run the FFT-js algorithm on your data buffer.  The time required
  for the calculation will follow square relationship with the amount of samples.  This is pretty
  useful to compare to Time between Samples, below.  The FFT Calculation Time should **never** exceed the
  time between samples.
  
## Time between Samples
  
  Units in milliseconds, shows how much time passed between the latest data point and the previous one.  
  
## Average Sample Frequency
  
  The average sample frequency is based on the last 200-10000 samples, and does some cheat-y tricks to
  use low memory.  This value (to a couple sig figs) is used to poopulate the (independent) Frequency axis.
  
## log frequency scale
  
  Not implemented.  The switch does nothing.
  
## last update
  
  Updates the time in H:M:S so you know data is arriving.


  
## FFT Engine

fft-js    https://www.npmjs.com/package/fft-js

# But wait, there's more!
  
