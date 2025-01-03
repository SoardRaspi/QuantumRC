<h1>QRC - Quantum Reservoir Computing</h1>

The main file is the "main submission.ipynb" jupyter notebook and the trained model in the pickle file is "best_xgboost_allcateg_XYZZY.pkl".

The maximum recall for the minority class I could achieve is **0.51**. I tried various things to maximize the recall score for this label. The one which worked was XGBoost post-processing with the observable "XYZZY" used for the quantum reservoir. The value of this observable is used along with the integer-valued features and then used to classify into two classes (1 - majority class, 2 - minority class).

<h3>Data Pre-Processing</h3>
Before training and testing the model, the data pre-processing done is only on the categorical features. No pre-processing such as normalization and regularization was done on the integer-valued features as no minimum or maximum bound was known. For the categorical data, The values were changed so that for each of the categorical features, the minimum values after processing are '0'.

<hl>

Quantum Resrvoir Computing is the implementation of the concept of Reservoir Computing where the resrevoir is a 5-quibt Quantum Computer. In this case, IBM's Qiskit framework is used. Since it was not possible to access the actual QPU because of the number of qubits available for open access (127 qubits) for long periods of time (maximum is 10 minutes per month). As a result Qiskit FakeBelemV2 fake backend was used. Out of the features available in the dataset, 13 variables are categorical and are thus encoded using the Angular Encoding scheme. The whole reservoir process can be seen as the circuit described in the figure below. The figure first contains the circuit of the fake backend. This is the internal circuit of the qubits of the virtual QPU. After this, the value assignments and circuit operations are shown. The letters 'L' and 'M' are used. Here, 'L' represents the values assignment stage and 'M' represents the controlled operations stage. In L1, qubits 0, 2 adn 4 are assignmed the values. In each of these circles, the acronyms represent the rotation gates are used for value assignment. After assigning these qubits, the controlled-NOT (CNOT) gates are used. In 'M1', the CNOT operations are shown. The 'C' represents the control qubit and 'T' represents the target qubit. In 'M1', qubits 0 and 2 are control qubits and qubit 1 is the target qubit. Similarly, qubit 4 is the control qubit and 3 is the target qubit. After this, in the 'L2' stage, qubits 1 and 3 are now assigned two other categorical attribute values. After this in the 'M2' stage, the SWAP operation is used on the qubits 1 and 3, which is basically a combination of 3 CNOT operations, operated in the alternate orientation.

This represents the assignment and operations on 5 categorical attributes. The 'x3' in the bottom right shows that these operations are done three times, or rather, in the following sequence:

L1 -> M1 -> L2 -> M2 -> L1 -> M1 -> L2 -> M2 -> L1

This sequence assigns all the 13 categorical features to the qubits. After the final 'L1', all the qubits are measured using the observables.

![image](https://github.com/user-attachments/assets/965a042c-9a8b-4a00-9c75-ac64c8574358)
