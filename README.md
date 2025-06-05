# Building a Radar Target Generator

## Overview
This repo outlines the following:
1. Implementation steps for the 2D CFAR process.
2. Selection of Training, Guard cells and offset.
3. Steps taken to suppress the non-thresholded cells at the edges.



## Table of Content
- [Radar Target Generation Pipeline](#pipeline)
- [Radar System Requirements](#requirements)
- [Radar System Evaluation](#eval)
- [Acknowledgements](#acknowledgements)



## Radar Target Generation Pipeline <a name="pipeline"></a>
<p align="center"><img src="assets/2025.06.04-00-radar-pipeline.png" width="591" height="335"/></p>

1. Configure the FMCW waveform based on the system requirements.
2. Define the range and velocity of target and simulate its displacement.
3. For the same simulation loop process the transmit and receive signal to determine the beat signal
4. Perform Range FFT on the received signal to determine the Range
5. Towards the end, perform the CFAR processing on the output of 2nd FFT to display the target.



## Radar System Requirements <a name="requirements"></a>
System Requirements defines the design of a Radar. The sensor fusion design for different driving scenarios requires different system configurations from a radar. In this project, we follow the following system requirements to design our radar.
<p align="center"><img src="assets/2025.06.04-01-radar-sysreqs.png" width="450" height="125"/></p>

The sweep bandwidth can be determined according to the range resolution and the sweep slope is calculated using both sweep bandwidth and sweep time.
```matlab
bandwidth(B_sweep) = speed_of_light / (2 * range_resolution)
```

The sweep time can be computed based on the time needed for the signal to travel the unambiguous maximum range. In general, for an FMCW radar system, the sweep time should be at least 5 to 6 times the round trip time. This example uses a factor of 5.5.
```matlab
T_chirp = 5.5 * 2 * R_max / c
slope = bandwidth / T_chirp  % slope of chirp signal
```
For the initial selection of target range and velocity: the range cannot exceed the max value of 200m and velocity can be any value in the range of -70 to + 70 m/s.



## Radar System Evaluation <a name="eval"></a>
In conclusion, we were able to use the given system requirements to design a FMCW waveform. That is, to find its Bandwidth (B), chirp time (Tchirp) and slope of the chirp. Our calculated slope was determined to be ~2e13.

We also got to simulate target movement and calculate the beat or mixed signal for every timestamp. A beat signal was generated such that once range FFT implemented, it gives our expected range--the initial position of target had an error margin of +/- 10 meters.

We implemented the Range FFT on the Beat or Mixed Signal and plot the result.It correctly generated a peak at the correct range, i.e the initial position of target also had an error margin of +/- 10 meters.

We also implemented the 2D CFAR process on the output of 2D FFT operation, i.e the Range Doppler Map. The 2D CFAR processing was able to suppress the noise and separate the target signal. The output then matched the image below.



## Ackowledgements <a name="acknowledgements"></a>
* [Udacity Sensor Fusion Program](https://learn.udacity.com/nanodegrees/nd313/parts/cd0683/lessons/b7907bc7-58e1-430a-ad51-a1f3e554b4dc/concepts/b7907bc7-58e1-430a-ad51-a1f3e554b4dc-project-rubric)
For any questions or feedback, feel free to email moorissa.tjokro@columbia.edu.