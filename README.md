# SFND Radar Target Generation and Detection
This is project 3 of Udacity's Sensor Fusion Nanodegree. For this project I implemented a two dimensional constant false alarm rate (CFAR) filter for use in detecting targets via radar returns. This was simulated using Matlab. I followed the process outlined in the lessons for this project.

## Range FFT Implementation

I configured an FMCW waveforme and simulated radar system based on the project specfications. I simulated a target at a range of 100 m traveling toward the radar at 25 m/s. I then generated a simulated time-domain data stream by looping over a time vector and propagating the target using a simple one dimensional linear constant acceleration propagation. In this same loop I simulated the transmision and recieve signals and their corresponding mix. Using Matlab's built in fft() method, I computed the range of the target using the Fast Fourier Transform.

## 2D CFAR

For this method, I implemented a set of nested for loops to slide the cell under test across the complete matrix. In each loop if followed the method described in the 2D CFAR lesson:

* For every iteration sum the signal level within all the training cells. To sum convert the value from logarithmic to linear using db2pow function.
* Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.
* Further add the offset to it to determine the threshold.
* Next, compare the signal under CUT against this threshold.
* If the CUT level > threshold assign it a value of 1, else equate it to 0.

As mentioned in that lesson, this results in a map that is smaller than the original. I solved this by maniuplating the orignial matrix. At the end of the computation, any cell that wasn't set to either 0 or 1 was a cell that was in the boundary defined by the hyperparameters. These cells could be safely set to zero using Matlab's matrix index searching semantics: `RDM(RDM~=0 & RDM~=1) = 0;`
