# **Environment**

The most important python packages are:
- python == 3.6.7
- pytorch == 1.2.0
- torch == 0.4.1
- tensorboard == 1.13.1
- rdkit == 2019.09.3
- scikit-learn == 0.22.2.post1
- hyperopt == 0.2.5
- numpy == 1.18.2

For using our model more conveniently, we provide the environment file *<environment.txt>* to install environment directly.


---
# **Command**

### **1. Train**
Use train.py

Args:
  - data_path : The path of input CSV file. *E.g. input.csv*
  - dataset_type : The type of dataset. *E.g. classification  or  regression*
  - save_path : The path to save output model. *E.g. model_save*
  - log_path : The path to record and save the result of training. *E.g. log*

E.g.

`python train.py  --data_path data/test.csv  --dataset_type classification  --save_path model_save  --log_path log`

### **2. Predict**
Use predict.py

Args:
  - predict_path : The path of input CSV file to predict. *E.g. input.csv*
  - result_path : The path of output CSV file. *E.g. output.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*

E.g.

`python predict.py  --predict_path data/test.csv  --model_path model_save/test.pt  --result_path result.csv`

### **3. Hyperparameters Optimization**
Use hyper_opti.py

Args:
  - data_path : The path of input CSV file. *E.g. input.csv*
  - dataset_type : The type of dataset. *E.g. classification  or  regression*
  - save_path : The path to save output model. *E.g. model_save*
  - log_path : The path to record and save the result of hyperparameters optimization. *E.g. log*

E.g.

`python hyper_opti.py  --data_path data/test.csv  --dataset_type classification  --save_path model_save  --log_path log`

### **4. Interpretation of Fingerprints**
Use interpretation_fp.py

Args:
  - predict_path : The path of input CSV file. *E.g. input.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*
  - result_path : The path of result. *E.g. result.txt*

E.g.

`python interpretation_fp.py  --predict_path test.csv  --model_path model_save/test.pt  --result_path result.txt`

### **5. Interpretation of Graph**
Use interpretation_graph.py

Args:
  - predict_path : The path of input CSV file. *E.g. input.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*
  - figure_path : The path to save figures of graph interpretation. *E.g. figure*

E.g.

`python interpretation_graph.py  --predict_path test.csv  --model_path model_save/test.pt  --figure_path figure`


---
# **Data**

We provide the three public benchmark datasets used in our study: *<Data.rar>*

Or you can use your own dataset:
### 1. For training
The dataset file should be a **CSV** file with a header line and label columns. E.g.
```
SMILES,CYP_1a2
O(C(=O)C(=O)NCC(OC)=O)C,0
FC1=CNC(=O)NC1=O,0
...
```
### 2. For predicting
The dataset file should be a **CSV** file with a header line and without label columns. E.g.
```
SMILES
O(C(=O)C(=O)NCC(OC)=O)C
FC1=CNC(=O)NC1=O
...
```
### 3. For interpreting fingerprints
The dataset file should be a **CSV** file with a header line and label columns. E.g.
```
SMILES,CYP_1a2
O(C(=O)C(=O)NCC(OC)=O)C,0
FC1=CNC(=O)NC1=O,0
...
```
### 4. For interpreting molecular graphs
The dataset file should be a **CSV** file with a header line and without label columns. E.g.
```
SMILES
O(C(=O)C(=O)NCC(OC)=O)C
FC1=CNC(=O)NC1=O
...
```


---
# **CYP**
## 1. Data
CYP dataset comes from article *<[iCYP-MFE: Identifying Human Cytochrome P450 Inhibitors Using Multitask Learning and Molecular Fingerprint-Embedded Encoding](https://pubs.acs.org/doi/10.1021/acs.jcim.1c00628)>*

#### 1.1 Single Task
Data files of single-task are in *<data/CYP_single-task/>*

#### 1.2 Multi Tasks
Data files of multi-tasks are in *<data/CYP_multi-tasks/>*

## 2. Model
#### 2.1 Single Task
The best trained single-task models are in *<model_save/CYP_single-task/>*

#### 2.2 Multi Tasks
The best trained multi-tasks model is in *<model_save/CYP_multi-tasks/>*

## 3. Prediction with trained models
#### 3.1 Single Task
For example, to predict new molecules (in *test.csv*) with trained single-task CYP-1a2 model, use the command:
`python predict.py  --predict_path test.csv  --model_path model_save/CYP_single-task/singletask_cyp1a2.pt  --result_path result_CYP1a2.csv`
The result file only contains the prediction of CYP_1a2. E.g.
Smiles|labels
:-:|:-:|:-:
NC(Cn1ccc(=O)n(Cc2ccccc2C(=O)O)c1=O)C(=O)O|0.0008448198204860091
c1ccc(CCc2cccnc2)nc1|0.4285607933998108
CCOC(=O)Cn1nc(C)n(-c2ccc(C)cc2)c1=O|0.20116816461086273
Cc1cc(O)c2c(c1)C(=O)c1cc(O)cc(O)c1C2=O|0.9857478737831116
CN(C(=O)C(Cl)Cl)c1ccc(OC(=O)c2ccco2)cc1|0.9518516063690186
Cc1n[nH]c(C)c1N=Nc1cc(Cl)ccc1Cl|0.9686630964279175

#### 3.2 Multi Tasks
For example, to predict new molecules (in *test.csv*) with trained multi-tasks model, use the command:
`python predict.py  --predict_path test.csv  --model_path model_save/CYP_multi-tasks/multitask_model.pt  --result_path result_CYP.csv`
The result file contains the prediction of five CYP subtypes. E.g.
Smiles|1a2|2c9|2c19|2d6|3a4
:-:|:-:|:-:|:-:|:-:|:-:
NC(Cn1ccc(=O)n(Cc2ccccc2C(=O)O)c1=O)C(=O)O|0.0029923394322395325|0.012909792363643646|0.010715223848819733|0.014555497094988823|0.0006137789459899068
c1ccc(CCc2cccnc2)nc1|0.10426828265190125|0.03495674952864647|0.19115062057971954|0.055862411856651306|0.014222786761820316
CCOC(=O)Cn1nc(C)n(-c2ccc(C)cc2)c1=O|0.030616765841841698|0.01593831740319729|0.11343128234148026|0.0012947379145771265|0.00494389096274972
Cc1cc(O)c2c(c1)C(=O)c1cc(O)cc(O)c1C2=O|0.9989868998527527|0.9270832538604736|0.849529504776001|0.48023781180381775|0.866969883441925
CN(C(=O)C(Cl)Cl)c1ccc(OC(=O)c2ccco2)cc1|0.9830098748207092|0.09900782257318497|0.7728306651115417|0.00026804395020008087|0.00951392576098442
Cc1n[nH]c(C)c1N=Nc1cc(Cl)ccc1Cl|0.9980672001838684|0.04949260130524635|0.7717867493629456|0.005170329939574003|0.036462053656578064

