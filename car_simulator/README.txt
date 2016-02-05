===========================================
 Example Code -- Car Simulator
===========================================

Applies to RTI Connext DDS Toolkit for LabVIEW 1.3.1.92 and above.

Example Description
===================
This example shows how the Readers and Writers can work together simulating a
car control. It shows how DDS is used in a Real-World example. Several Readers
and Writers are created in parallel. Besides, they are reading or writing at 
the same time.
	 
In a general view, we can differentiate it on two different parts, one of them 
which simulates the control of a car, and the second one which simulates its
behavior (calculating the speed and the distance from the start point).

This example contains different topics in order to share (send & receive) the
information over the network between the two systems we have.
So, we create a topic for every different data we need to work with. We need
to keep information about:
    - Gas
    - Brake
    - Speed
    - Position (from the start point)

VIs Description
===============
The Car Simulator example is divided on four different VIs.
   - CarControl.vi: It is the main control of this example because the values
     of Gas & Brake we want to have our simulated car, are set here.
   - CarSim.vi: It receives Gas & Brake values, calculates the simulated 
     speed and position (calling SpeedSimulator.vi), and sending these new 
     values. 
   - SpeedSimulator.vi: it calculates the speed and position from the Gas &
     Brake values, using EulerIntegrator.vi
   - EulerIntegrator.vi: it represents the Euler Integrator.
   
Main Scenario
=============   
The steps which this example follows are:
    1. The Gas & Brake data are set on CarControl.vi. 
    2. Gas & Brake data are sent to CarSim.vi (using their topics).
	3. CarSim.vi receives Gas & Brake values.
	4. CarSim.vi calculates the corresponding speed and distance.
	5. CarSim.vi send the calculated Speed and Distance (using their topics).
	6. CarControl.vi receives the Speed and Distance and print them.
	
Running LabVIEW Example
=======================
In two separate LabVIEW windows for the CarControl.vi and CarSim.vi. Run
both VIs and modify the value of the Gas & Brake from CarControl.vi in order
to simulate the car responses.
