# CSCI596
For the final project of CSCI596- 
Visualizing the molecular dynamics of C-defect modified 2D- graphitic Carbon Nitride simulated using machine learning force field

# Classical v/s Machine Learning Force Fields (MLFF)

## Classical Force Field
For a given configuration, the forces on individual atoms are calculated and newtonian equation of motion is numerically integrated at each time-step to advance the dynamics. 
How are these forces obtained- solving the SE equation? Use post-HF methods like CCSD is too expensive. Instead, force fields are used. 
Force on each atom is mostly sum of bonded (bonds, angles, dihedrals) plus non-bonded interactions (pairwise: LJ potential, vdW forces, electrostatics etc.).
Classical FFs give a reasonable description of chemical interactions, even though the quality of these simulations is ultimately limited. 

## Machine Learning Force Fields
In MLFF, the key conecpt is to come up with an analytical expression that relates the atomic configuration to its potential energy. 
MLFF has no preconceived notion about the chemical bonds or the atomic interactions.
Narrows the gap between the accuracy of ab initio methods and the eﬃciency of classical FFs.

# Training
Dataset Information: ~30,000 data points collected from ab initio moleclar dynamics (AIMD) within the framework of density functional theory (DFT), at varied temperature: from 100K to 1200K. 

Trained on a deep learning network from DeepMD package. 

GPU-accelerated training

<img src="https://github.com/shriyagumber/csci596/assets/84539282/330452cf-7dac-44d0-8284-d4c09d55bec5" width="550" height="450">

# Visualization

Trajectory at every 10 ps, over the total dynamics of 1 ns:


https://github.com/shriyagumber/csci596/assets/84539282/ea8202f1-8f9e-4f0e-bc3a-fd2dd164d675

https://github.com/shriyagumber/csci596/assets/84539282/b530a264-8ea5-4bec-9c32-1762fc705ff6

https://github.com/shriyagumber/csci596/assets/84539282/409a51d9-7ff9-4f0f-99c4-bb24f5cf095d

# Application

The accuracy of DFT with the efficiency of classical force field. 

The photoexcitated charge decay in such systems happen over the timescale of a few nanoseconds, which would be impossible to obtain in large systems using abinitio methods. 


