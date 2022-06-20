# node-red-subflows-FFT-dashboard_ui
*node-red-subflows-FFT-dashboard-ui* accepts raw streaming numbers.  An entire *dashboard-ui* tab is devoted to information and control of the display.

This subflow is its own tab in the *dashboard-ui* display. *dashboard-ui* is, of course, required.

I put this tool together for my own purposes, and thought others might make use of it as well.  My application was display and analysis of low sampling rate data.  Run it as fast as you can, but I haven't even tested this at audio frequencies.

![image](https://user-images.githubusercontent.com/105139648/174671595-7c64bdda-1702-4b05-b373-1f5c32be99c5.png)


## Inputs

  The sub-flow is expecting an input that has a *numerical value* in *msg.payload*
  If you include a millisecond timestamp (e.g., from Date.now()) as msg.timestamp, that has upcoming application.

## FFT Engine / required library

  fft-js     https://www.npmjs.com/package/fft-js
  Pure Node.js implementation of the Fast Fourier Transform (Cooley-Tukey Method)

  npm install fft-js
  
  ( or sudo npm install -g fft-js to install in the non-local node-red libraries)

  in settings.js (**very important**):

      functionGlobalContext: {
        fftjs:require('fft-js')
      },

## Automatic crash-stopper

If the time to calculate the FFT is greater than the time between data points, the system
will force you to use a smaller buffer sample size, automatically.  (The sample size selection 
window will change to reflect the new size)  Because this subflows raison d'etre (reason to be) is 
*live streaming FFT data*, the FFT must be calculable within one sample period 
so the software can keep up with the data rate.

There is also a *commented out section* of code that will allow the subflow to impose
more restrictions on the input data rate, but in the current incarnation limiting the 
data rate will lead to [signal aliasing](https://en.wikipedia.org/wiki/Aliasing), which is bad. 

If you want to go faster, use more/faster processors.

# Screencaps

![image](https://user-images.githubusercontent.com/105139648/174211742-bc2c1930-5828-4054-97e7-32734d1f83f6.png)  ![image](https://user-images.githubusercontent.com/105139648/174219573-93fec829-8300-4528-ba9d-27002a74abc2.png)

--------------------------------

![image](https://user-images.githubusercontent.com/105139648/174221099-20035e2b-87c9-498e-a23e-f98889e1f722.png)


# Dashboard UI Features

## FFT Buffer Filled

  The percentage shown is the amount of data in the buffer before the calculations shown are accurate.
  It is feedback for my impatient user (me).

  Until the data buffer (used by the FFT) is filled with data, the rest of the buffer has been
  padded with zeros.

  When the sub-flow is freshly started, none of the calculations shown are accurate because
  of the zero-padding.

## Sample Size

  Select from various Power-of-2 buffer sizes (this is a requirement for the used FFT algorithm).
  The smallest size buffer is 2 (2^1), and the largest buffer currently programmed is 524288 (2^19),
  but there is no reason the buffer cannot be expanded if your CPU power is capable. (see FFT Calculation Time)

## FFT Magnitudes Graph

  A live graph displays the magnitudes from the FFT calculation, starting with a0, followed by
  all the frequency bins.  If a0 is not included in the graph (as per the ui switch), a0 is shown as a zero 
  quantity.  The number of frequncies shown can be selected, below.

  The graphs are **not** accurate until the FFT Buffer is Filled.  If you are into the Fourier
  Series, the look of the magnitude and the phase graphs is the effect of the zero-padding.

## Include a0 switch

  The a0 magnitude might dwarf all the other magnitudes when in linear magnitude mode.
  This switch allows you to include or exclude a0 from the live graph so you can see the
  other magnitudes as well.  (like a switchable DC/AC input coupling on an oscilloscope)

## Display Magnitudes as linear/log (dB<units>)
  
  Choose to see magnitudes in linear format, or logarithmic.
  (the logarithmic units are deciBels of the input data units)
  
  Log(magnitude) mode looks pretty crappy, I gotta say, but is still accurate. 
  I haven't bothered to improve the look of it.

## Pause/Run
  
  Pause/Run button on the left, text of "running" or "paused"
  
## a0 live
 
  The magnitude of a0 for the currently-shown FFT.  
  a0 is literally the average value over your entire buffer size.
  
## FFT Phases Graph
    
  A live graph displays the phase angless from the FFT calculation, starting with a zero for a0, followed by
  all the frequency bins.  The a0 switch does not affect this graph.  
  The number of frequncies shown can be selected, below.
    
  The graphs are **not** accurate until the FFT Buffer is Filled.  If you are into the Fourier
  Series, the look of the magnitude and the phase graphs is the effect of the zero-padding.
  
## Select Number of Frequencies Shown
  
  Can choose from 1-256.   256 pixels is near the useful limit on the 6 unit wide display.
  This does NOT affect the fft calculation, the purpose of this selection is to limit the 
  number of frequencies shown. (e.g., for zooming in on the lower frequency bins)
  
## Clear Graphs
  
  Clicking the clear graphs button clears all the graphs.  The graphs will update with fresh data
  as soon as the next  data arrives.  
  The button is useful for clearing any weird data from re-deployed node-red flows.

## a0(t) graph
  
  This is a live graph of the a0 magnitude from FFT calculations, as it changes with each sample.
  a0 is the mean value over all the data; this graph shows the history of the average value.
 
## FFT Calculation Time
  
  The most recent amount of time it took to run the FFT-js algorithm on your data buffer.  The time required
  for the calculation will follow a square(-ish, it's actually nlog(n)) relationship with the amount of samples.  
  This measurement is pretty useful to compare to Time between Samples, below.  
  The FFT Calculation Time should **never** exceed the time between samples.
  See *Automatic Crash Stopper*, above.
  
## Time between Samples
  
  Units in milliseconds, shows how much time passed between the latest data point and the previous one.  
  See *Automatic Crash Stopper*, above.
  
## Average Sample Frequency
  
  The average sample frequency is based on the last 2000-10000 samples, and does some cheat-y tricks to
  use low memory.  This average sampling frequency value (to a couple sig figs) is used to poopulate 
  the (independent) Frequency axis on the graphs.
  
## log frequency scale
  
  Not implemented.  The switch does nothing.  
  
## last update
  
  Updates the time in H:M:S so you know data is arriving.


  

# But wait, there's more!
  
