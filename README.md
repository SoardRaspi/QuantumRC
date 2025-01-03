<h1>QRC - Quantum Reservoir Computing</h1>

The main submission file is the "main submission.ipynb" jupyter notebook and the trained model in the pickle file is "best_xgboost_allcateg_XYZZY.pkl".

The maximum recall for the minority class I could achieve is **0.51**. I tried various things to maximize the recall score for this label. The one which worked was XGBoost post-processing with the observable "XYZZY" used for the quantum reservoir. The value of this observable is used along with the integer-valued features and then used to classify into two classes (1 - majority class, 2 - minority class).

<h3>Data Pre-Processing</h3>
Before training and testing the model, the data pre-processing done is only on the categorical features. No pre-processing such as normalization and regularization was done on the integer-valued features as no minimum or maximum bound was known. For the categorical data, The values were changed so that for each of the categorical features, the minimum values after processing are '0'.

<hl>

Quantum Resrvoir Computing is the implementation of the concept of Reservoir Computing where the resrevoir is a 5-quibt Quantum Computer. In this case, IBM's Qiskit framework is used. Since it was not possible to access the actual QPU because of the number of qubits available for open access (127 qubits) for long periods of time (maximum is 10 minutes per month). As a result Qiskit FakeBelemV2 fake backend was used. Out of the features available in the dataset, 13 variables are categorical and are thus encoded using the Angular Encoding scheme. The whole reservoir process can be seen as the circuit described in the figure below. The figure first contains the circuit of the fake backend. This is the internal circuit of the qubits of the virtual QPU. After this, the value assignments and circuit operations are shown. The letters 'L' and 'M' are used. Here, 'L' represents the values assignment stage and 'M' represents the controlled operations stage. In L1, qubits 0, 2 adn 4 are assignmed the values. In each of these circles, the acronyms represent the rotation gates are used for value assignment. After assigning these qubits, the controlled-NOT (CNOT) gates are used. In 'M1', the CNOT operations are shown. The 'C' represents the control qubit and 'T' represents the target qubit. In 'M1', qubits 0 and 2 are control qubits and qubit 1 is the target qubit. Similarly, qubit 4 is the control qubit and 3 is the target qubit. After this, in the 'L2' stage, qubits 1 and 3 are now assigned two other categorical attribute values. After this in the 'M2' stage, the SWAP operation is used on the qubits 1 and 3, which is basically a combination of 3 CNOT operations, operated in the alternate orientation.

This represents the assignment and operations on 5 categorical attributes. The 'x3' in the bottom right shows that these operations are done three times, or rather, in the following sequence:

L1 -> M1 -> L2 -> M2 -> L1 -> M1 -> L2 -> M2 -> L1

This sequence assigns all the 13 categorical features to the qubits. After the final 'L1', all the qubits are measured using the observables.

![image](https://github.com/user-attachments/assets/965a042c-9a8b-4a00-9c75-ac64c8574358)

**Quantum-Hybrid Mixture-Based Network for Quantum Machine Learning**

In my journey of learning about Quantum Computing and Quantum Machine Learning, I have seen quite a few projects and research papers, using the concept of Quantum-Hybrid networks or circuits with classical structures to enhance the different Machine Learning ideas. These ideas mostly use separate/isolated Quantum and Classical circuits and then combine them in a sequential or parallel configuration. As an example, if we talk about variational quantum circuits for machine learning, I have mostly seen the use of transfer learning, either majorly quantum variational circuits optimized by classical algorithms or majorly classical circuit(s) taking input as the output of some other quantum system .eg. in the case of Quantum Convolutional Networks or Kernels. Recently, there has also been some progress in the field of implementing the concept of spiking-neurons in the Quantum paradigm. Taking inspiration from these recent developments, a slightly unique idea that I had thought of was the implementation of a **real** Quantum-Hybrid network in Neural Networks.

Talking about the field of Quantum Machine Learning (QML), the idea can be seen as a mixture of perceptrons and a few qubits in each of the layers. Between two consecutive layers, the idea proposes entanglement of qubits from one layer to those of the other layer. Graphically, the implementations looks like:

![Quantum RC](https://github.com/SoardRaspi/Quantum-RC-Reservoir-Computing-/blob/main/Bloq%20Crazy.png)

In the above diagram, in each layer, there are two types of 'circles' with a different label. In the black-filled circle, the naming followed is pl_i and in the white-filled circle, the naming followed is ql_i. Here, 'p' represents a simple perceptron which is used in the normal neural networks and 'q' represents the quibt which is being used along-side the perceptrons in the layer. In the subscript, 'l' represents the layer number and 'i' represents the number of perceptron or qubit in that layer.

Between two layers, in the above case layer 1 and 2, the connections between the neurons are not shown as they are well-understood. However, the dotted lines shown, represent the entanglement(s) between the qubits of the different layers. The potential benefit they provide is the all-together different type of embedding and the processing of the qubits. The entanglement is believed to help in changing the weight(s)/embedding(s) in the layers. This would be like back-propagation, only faster. I really believe that this design could help in bringing some new insights in Quantum Machine Learning and could help in faster training of the networks.

To make it more natural or similar to the way the human brain functions, a few connections among the components of a single layer could be made as well. This could mean an increase in the computational needs while training and inference of the model. However, introducing random connections among the components using spiking neurons would be an interesting introduction in the existing architecture. Elaborating on this, the spiking neurons should be connected between the outputs of some neurons to the input of the other neurons in the same layer. This would mean that either within a layer, some components (source of the spiking neurons) should be computed first and then the others (those taking the output of these spiking neurons as their input) later. Or, any layer which contains such spiking connections needs to be computed twice, first with active spiking connections and next with normal computations (keeping the spiking connections inactive).

This proposed method can also be used as a reservoir instead of the layers in the Neural Network. Having a quantum-classic hybrid reservoir with semi-random spiking connections could bring new insights in Reservoir Computing.
