# Goal
The field of Machine Learning internatomic potentials has witnessed significant advancements with the evolution of various models and networks architectures to train the interatomic potential for various materails. The state-of-the-art techniques and models have reached a level of precision where the accuracy of Machine Learning Force Fields (MLFFs) closely mirrors the quality of the data used for training. This is a notable achievement, as it indicates that these models have become highly proficient in capturing and reproducing the intricate details present in the input data. However, this also highlights a current limitation: the precision of MLFFs is inherently tied to the accuracy of their training datasets.

# Machine Learning Force Field

### Classical Force Field
For a given configuration, the forces on individual atoms are calculated and newtonian equation of motion is numerically integrated at each time-step to advance the dynamics. 
How are these forces obtained- solving the SE equation? Use post-HF methods like CCSD is too expensive. Instead, force fields are used. 
Force on each atom is mostly sum of bonded (bonds, angles, dihedrals) plus non-bonded interactions (pairwise: LJ potential, vdW forces, electrostatics etc.).
Classical FFs give a reasonable description of chemical interactions, even though the quality of these simulations is ultimately limited. 

### Machine Learning Force Fields
In MLFF, the key conecpt is to come up with an analytical expression that relates the atomic configuration to its potential energy. 
MLFF has no preconceived notion about the chemical bonds or the atomic interactions.
Narrows the gap between the accuracy of ab initio methods and the eﬃciency of classical FFs.


# Application

The accuracy of DFT with the efficiency of classical force field. 

The photoexcitated charge decay in such systems happen over the timescale of a few nanoseconds, and to perform the charge dynamics within classical path approximation, trajectories over a few nanoseconds are required, which would be impossible to obtain in large systems using abinitio methods. 


