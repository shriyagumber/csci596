# Goal
The field of Machine Learning internatomic potentials has witnessed significant advancements with the evolution of various models and network architectures to train the interatomic potential for various materials. The state-of-the-art techniques and models have reached a level of precision where the accuracy of Machine Learning Force Fields (MLFFs) closely mirrors the quality of the data used for training. This is a notable achievement, as it indicates that these models have become highly proficient in capturing and reproducing the intricate details present in the input data. However, this also highlights a current limitation: the precision of MLFFs is inherently tied to the accuracy of their training datasets.

Since the majority of MLFFs are trained using data derived from Density Functional Theory (DFT), the limitations of DFT are consequently reflected in the MLFFs. DFT, while powerful and widely used for quantum mechanical calculations, is not without its shortcomings. It often struggles with accurately modeling certain types of interactions, such as van der Waals forces or accurately describing the electronic states of transition metals. As a result, MLFFs trained on DFT data inherit these limitations.

To surpass these constraints and enhance the accuracy of MLFFs beyond the current threshold set by DFT, further developments are required. Here, a proposal on improvement of MLIP using experimental data is presented. Specifically, the experimental data for properties such as radial distribution function is readily available and can be utilized to improve the force field. 

# Specific objectives:
The test data presented for the Machine Learning Interatomic Potential (MLIP) in this project, specifically applied to carbon-defect modified graphitic carbon nitride (g-C3N4), exhibits a root mean square error (RMSE) of 0.7 milli-electronvolts per atom. This level of accuracy is generally considered satisfactory within the standards of the machine learning community for interatomic potentials, indicating that the model performs well for the dataset it was trained on.

However, it's noted that the RMSE value escalates when the model is used to predict the dynamics of a larger system. This increase in error with system size points to a limitation in the MLIP's transferability. Essentially, it suggests that while the MLIP is adept at handling the specific type of system on which it was trained, its ability to generalize and accurately predict the properties of systems with significantly different sizes or compositions is constrained. This limitation is crucial for further development, as it indicates a need for the model to be trained on a more diverse set of structures or to incorporate mechanisms that allow it to scale more effectively to larger systems.

Here, I propose to increase the quality of this trained force field using the experimental values of radial-distribution function for this specific system. Here is a detailed outline of the implementation: 

#### Work done:
### 1. Training Data: 
Ab initio molecular dynamics (AIMD) using Density Functional Theory (DFT) with the Generalized Gradient Approximation (GGA) and the Perdew-Burke-Ernzerhof (PBE) exchange-correlation functional is used to obtain the training data. ~30,000 data points at temperatures varying from 100K to 1000K is obtained and fed into the deepMD architecture for training.

### 2. Obtaining the Trajectory
The trajectory was acquired for both the supercell upon which the model was initially trained and larger models. This analysis aimed to assess the transferability of the force field and explore the extent of extrapolation capabilities. LAMMPS was employed to apply the trained MLIP and generate a trajectory, starting from an optimized initial structure. 

#### Future directions:
### 3. Obtain structural properties 
Calculate the structural properties of g-C_{3}N_{4} using MLIP and compare it with the experimental data in literature. 

### 4. Tranfer Learning
Apply the DiffTRe method (discussed below) to tune the parameters of the MLIP and further compare it with the experimental results 

# Background

There are two broad categories in which MLIP training can be divided: 

### Bottom-Up Approach:
The bottom-up approach in MLFF attempts to build up the force field from a detailed understanding of atomic and molecular interactions. This approach often involves using high-level quantum mechanical calculations as a reference. The machine learning model is trained on data obtained from these ab initio methods, learning to predict the potential energy and forces with quantum accuracy. The primary advantage of this approach is its accuracy. Since the model is trained on highly accurate quantum mechanical data, it can predict the behavior of molecules with a high degree of precision. The main limitation is the computational cost associated with generating the training data, as ab initio methods are resource-intensive. This approach may also struggle with generalizability to systems or states not represented in the training data.

### Top-Down Approach:
In contrast, the top-down approach in MLFF is more phenomenological. It focuses on capturing the overall behavior of molecular systems without delving deeply into the fundamental physics at play. This approach often involves training the machine learning model on empirical data or data from experimental results. The model learns to reproduce the observed macroscopic properties of the system. The top-down approach is generally more computationally efficient in terms of data generation. It can also be more flexible and adaptable to a wide range of systems, as it is not as tightly bound to the specifics of quantum mechanical calculations. The accuracy of a top-down model is generally limited by the quality of the training data. If the training data lacks certain interactions or is not representative of the true diversity of molecular behavior, the model's predictions may be less reliable.

While mostly bottom-up approaches are used to train MLIPs, the parameters, however, can be re-trained using the experimental results by transfer learning approach. 

# Previous Work

While a considerable body of research exists on training Machine Learning Interatomic Potentials (MLIPs) using data from first-principles calculations, there have been relatively few studies on learning directly from experimental results to inform force fields. This is largely attributable to the inherent complexities associated with translating experimental observations into training datasets.

Data from Density Functional Theory (DFT) typically includes discrete configurations, each paired with precise energies and atomic forces, providing a clear and direct foundation for training MLIPs. In contrast, experimental results often yield statistical averages across an ensemble of states or 'snapshots,' obscuring the specific atomic configurations and the exact forces at play. This difference renders the direct application of experimental data to the training of force fields a challenging task.

However, advanced methods such as Differential Trajectory Re-weighting offer a pathway to reconcile these differences. This technique allows for the refinement of parameters in a machine-learning force field—initially trained on first-principles data like that from DFT—to align with experimental observations. By adjusting the force field to reproduce macroscopic properties measured experimentally, such as the radial distribution function, X-ray absorption spectroscopy signatures, or heat capacities, the MLIP can be calibrated to reflect experimental realities, thereby enhancing its predictive power and relevance to real-world systems.

Here is a brief description on the methodology:

### DiffTRe Method

In this method, we begin with a reference potential and an initial state. A trajectory is first generated using the MLIP and K different outputs are obtained over this trajectory. 
A loss function is defined based on the difference between the K ouputs from the MLIP and the corresponding experimental output. 

$$
L(\theta) = \frac{1}{K} \sum_{k=1}^{K} \left[ \left\langle O_k(U_{\theta}) \right\rangle - \tilde{O}_k \right]^2
$$

Each of the K outputs is calculated over an ensemble of N states or snapshots. To consider decorrelated states, the snapshots are considered at every 100th or 1000th time-step from the calculated trajectory. 
The loss is reduced using a gradient descent or other similar algorithm that minimizes the loss and finds a new set of parameters: \theta
Then the K ouputs will be calculated using the 

$$
\langle O_k(U_{\theta}) \rangle \approx \sum_{i=1}^{N} w_i O_k(S_i, U_{\theta})
$$

where weights is calculated using the thermodynamics perturbation theory. In Canonical ensemble, the weight of a given state/snapshot can be calculated using its boltzmann factor. Boltzmann depends on the hamiltonian (Kinetic energy + Potential energy) of a state


$$
Num = \frac{p_{\theta}(S_i)}{\tilde{p}_{\theta}(S_i)}
$$

$$
Denom = \sum_{j=1}^{N} \frac{p_{\theta}(S_j)}{\tilde{p}_{\theta}(S_j)}
$$

$$
w_i = \frac{Num}{Denom}
$$


This approximates the reuse of already calculated trajectpry using reference potential by weighing different states by a factor w. 

# Expected Results
Based on the radial distribution function for g-C3N4, an improvement in the atomic forces calculated using MLIP is expected. 
Improvement in root mean square error (RMSE) for potential energy for systems larger than which the model is trained on 

# References
https://doi.org/10.1038/s41467-021-27241-4
