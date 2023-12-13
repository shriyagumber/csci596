# Goal
The field of Machine Learning internatomic potentials has witnessed significant advancements with the evolution of various models and network architectures to train the interatomic potential for various materials. The state-of-the-art techniques and models have reached a level of precision where the accuracy of Machine Learning Force Fields (MLFFs) closely mirrors the quality of the data used for training. This is a notable achievement, as it indicates that these models have become highly proficient in capturing and reproducing the intricate details present in the input data. However, this also highlights a current limitation: the precision of MLFFs is inherently tied to the accuracy of their training datasets.

Since the majority of MLFFs are trained using data derived from Density Functional Theory (DFT), the limitations of DFT are consequently reflected in the MLFFs. DFT, while powerful and widely used for quantum mechanical calculations, is not without its shortcomings. It often struggles with accurately modeling certain types of interactions, such as van der Waals forces or accurately describing the electronic states of transition metals. As a result, MLFFs trained on DFT data inherit these limitations.

To surpass these constraints and enhance the accuracy of MLFFs beyond the current threshold set by DFT, further developments are required. Here, a proposal on improvement of MLIP using experimental data is presented. Specifically, the experimental data for properties such as radial distribution function is readily available and can be utilized to improve the force field. 

# Introduction

There are two broad categories in which MLIP training can be divided: 

### Bottom-Up Approach:
The bottom-up approach in MLFF attempts to build up the force field from a detailed understanding of atomic and molecular interactions. This approach often involves using high-level quantum mechanical calculations as a reference. The machine learning model is trained on data obtained from these ab initio methods, learning to predict the potential energy and forces with quantum accuracy. The primary advantage of this approach is its accuracy. Since the model is trained on highly accurate quantum mechanical data, it can predict the behavior of molecules with a high degree of precision. The main limitation is the computational cost associated with generating the training data, as ab initio methods are resource-intensive. This approach may also struggle with generalizability to systems or states not represented in the training data.

### Top-Down Approach:
In contrast, the top-down approach in MLFF is more phenomenological. It focuses on capturing the overall behavior of molecular systems without delving deeply into the fundamental physics at play. This approach often involves training the machine learning model on empirical data or data from experimental results. The model learns to reproduce the observed macroscopic properties of the system. The top-down approach is generally more computationally efficient in terms of data generation. It can also be more flexible and adaptable to a wide range of systems, as it is not as tightly bound to the specifics of quantum mechanical calculations. The accuracy of a top-down model is generally limited by the quality of the training data. If the training data lacks certain interactions or is not representative of the true diversity of molecular behavior, the model's predictions may be less reliable.

While mostly bottom-up approaches are used to train MLIPs, the parameters, however, can be re-trained using the experimental results by transfer learning approach. 

# Literature

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

Each of the K outputs is calculated over an ensemble of N states or snapshots. To consider decorrelated states, the snapshots are considered at every 100th or 1000th time-step from the trajectory. 



# Expected Results
Based on the radial distribution function, 
