# CSCI596
For the final project of CSCI596- 
1. Visualizing the molecular dynamics of C-defect modified 2D- graphitic Carbon Nitride simulated using machine learning force field. 
2. Improvement of machine learning force field (MLFF) using experimental data- A proposal

# Classical v/s Machine Learning Force Fields (MLFF)

### Classical Force Field
In MD simulations, the dynamics of a molecular system are studied by calculating the forces acting on each atom for a given configuration. These forces are used to integrate the Newtonian equations of motion numerically. This integration, performed at each timestep, advances the simulation, allowing us to observe how the positions and velocities of atoms evolve over time. This method enables the study of molecular behavior under various conditions, like changes in temperature or pressure.
Ideally, the most accurate way to determine the forces on atoms in a molecular system is by solving the Schrödinger equation (SE), which is the fundamental equation of quantum mechanics. The SE provides a description of how the quantum state of a physical system changes over time.
However, the SE cannot be solved exactly for systems larger than a few atoms due to its computational complexity. To overcome this, approximate methods are employed. These include techniques like Hartree-Fock and coupled cluster equations of motion (EOM). While these methods can provide a high level of accuracy, they are computationally expensive, especially for large systems, making them impractical for routine simulations of large biomolecules or materials.
As a result, most MD simulations rely on force fields. A force field is a set of equations and parameters that describe the potential energy of a system as a function of the atomic positions. In a force field, the force on each atom is typically expressed as the sum of bonded interactions (involving bonds, angles, and dihedrals) and non-bonded interactions. Non-bonded interactions include pairwise potentials like the Lennard-Jones (LJ) potential for modeling van der Waals (vdW) forces and Coulomb's law for electrostatic interactions. 
Classical force fields provide a reasonable description of chemical interactions in a wide range of systems. They are particularly effective for studying large systems over long time scales, where quantum mechanical methods are not feasible. However, the quality of simulations using classical force fields is ultimately limited. They may not capture certain quantum mechanical effects and can be less accurate in systems where electronic effects are significant. 

### Machine Learning Force Fields
In the domain of Machine Learning Force Fields (MLFF), the fundamental concept revolves around devising an analytical expression that directly correlates the atomic configuration of a system with its potential energy. This approach is distinctively characterized by its lack of inherent assumptions or predefined notions regarding the nature of chemical bonds or atomic interactions. Unlike classical force fields, which rely on fixed functional forms and parameters to describe interactions based on empirical data or simpler quantum mechanical calculations, MLFFs adopt a more flexible and data-driven methodology. They learn from extensive datasets, often derived from high-level ab initio (first principles) calculations, to accurately predict potential energy landscapes. This methodology allows MLFFs to capture complex, non-linear interactions that are typically challenging for classical force fields to model accurately. As a result, MLFFs significantly narrow the gap between the high accuracy typical of ab initio methods and the computational efficiency characteristic of classical force fields. By doing so, they offer a powerful alternative that combines the best of both worlds: the precision of quantum mechanics-based simulations and the scalability and speed of classical approaches. This advancement holds immense promise for the study of complex molecular systems where traditional methods may fall short.

# Training
Dataset Information: ~30,000 data points collected from ab initio moleclar dynamics (AIMD) within the framework of density functional theory (DFT), at varied temperature: from 100K to 1200K. 
The DFT calculations are done using the GGA-PBE approximation as implemented in the VASP software. 

Trained on a deep learning network from DeepMD package. 
https://github.com/deepmodeling/deepmd-kit

GPU-accelerated training

<img src="https://github.com/shriyagumber/csci596/assets/84539282/330452cf-7dac-44d0-8284-d4c09d55bec5" width="550" height="450">

# Visualization

Trajectory at every 10 picoseconds, over the total dynamics of 1 nanosecond is visualized using ovito software:

https://github.com/shriyagumber/csci596/assets/84539282/ea8202f1-8f9e-4f0e-bc3a-fd2dd164d675

https://github.com/shriyagumber/csci596/assets/84539282/b530a264-8ea5-4bec-9c32-1762fc705ff6

https://github.com/shriyagumber/csci596/assets/84539282/409a51d9-7ff9-4f0f-99c4-bb24f5cf095d

# Machine Learning Hamiltonian

$$
d_{ij}\left(t+\frac{\Delta t}{2}\right) = \left\langle \psi_i\\left(t+\frac{\Delta t}{2}\right)
\Bigg| \frac{\partial}{\partial t}
\Bigg| \psi_j\\left(t+\frac{\Delta t}{2}\right)
\right\rangle
\approx \left\langle \frac{\psi_i(t+\Delta t)+\psi_i(t)}{2}
\Bigg| \frac{\psi_j(t+\Delta t)-\psi_j(t)}{\Delta t} \right\rangle
$$

$$
=
\frac{1}{2\,\Delta t}
\left(
\langle \psi_i(t+\Delta t)\,|\,\psi_j(t+\Delta t)\rangle
-
\langle \psi_i(t+\Delta t)\,|\,\psi_j(t)\rangle
+
\langle \psi_i(t)\,|\,\psi_j(t+\Delta t)\rangle
-
\langle \psi_i(t)\,|\,\psi_j(t)\rangle
\right)
$$

The time–overlap is calculated as:

$$
S_{\mathrm{MO}}(t_1,t_2) =
C(t_1)\,
S_{\mathrm{AO}}(t_1,t_2)\,
C^{*}(t_2)
\tag{6}
$$

# Application
The accuracy of DFT with the efficiency of classical force field. 

The photoexcitated charge decay in such systems happen over the timescale of a few nanoseconds, and to perform the charge dynamics within classical path approximation, trajectories over a few nanoseconds are required, which would be impossible to obtain in large systems using abinitio methods. 

# Future Directions

The force field trained from DFT data is as accurate as the data fed into it and still far from the experimental results. A proposal to improve this force field using radial distribution function is provided https://github.com/shriyagumber/csci596/tree/main/Proposal


