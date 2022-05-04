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
# **Data Standard**

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
CYP dataset comes from the article *<[iCYP-MFE: Identifying Human Cytochrome P450 Inhibitors Using Multitask Learning and Molecular Fingerprint-Embedded Encoding](https://pubs.acs.org/doi/10.1021/acs.jcim.1c00628)>*

#### 1.1 Single Task
Data files of single-task are in *data/CYP_single-task/*

#### 1.2 Multi Tasks
Data files of multi-tasks are in *data/CYP_multi-tasks/*

## 2. Model
#### 2.1 Single Task
The best trained single-task models are in *model_save/CYP_single-task/*

#### 2.2 Multi Tasks
The best trained multi-tasks model is in *model_save/CYP_multi-tasks/*

## 3. Prediction with trained models
#### 3.1 Single Task
For example, to predict new molecules (in *test.csv*) with trained single-task CYP-1a2 model, use the command:

`python predict.py  --predict_path test.csv  --model_path model_save/CYP_single-task/singletask_cyp1a2.pt  --result_path result_CYP1a2.csv`

The result file only contains the prediction of CYP_1a2. 
E.g.

Smiles|labels
:-:|:-:
NC(Cn1ccc(=O)n(Cc2ccccc2C(=O)O)c1=O)C(=O)O|0.001
c1ccc(CCc2cccnc2)nc1|0.429
CCOC(=O)Cn1nc(C)n(-c2ccc(C)cc2)c1=O|0.201
Cc1cc(O)c2c(c1)C(=O)c1cc(O)cc(O)c1C2=O|0.986
CN(C(=O)C(Cl)Cl)c1ccc(OC(=O)c2ccco2)cc1|0.952
Cc1n[nH]c(C)c1N=Nc1cc(Cl)ccc1Cl|0.969

#### 3.2 Multi Tasks
For example, to predict new molecules (in *test.csv*) with trained multi-tasks model, use the command:

`python predict.py  --predict_path test.csv  --model_path model_save/CYP_multi-tasks/multitask_model.pt  --result_path result_CYP.csv`

The result file contains the prediction of five CYP subtypes. 
E.g.

Smiles|1a2|2c9|2c19|2d6|3a4
:-:|:-:|:-:|:-:|:-:|:-:
NC(Cn1ccc(=O)n(Cc2ccccc2C(=O)O)c1=O)C(=O)O|0.003|0.013|0.011|0.016|0.001
c1ccc(CCc2cccnc2)nc1|0.104|0.035|0.191|0.056|0.014
CCOC(=O)Cn1nc(C)n(-c2ccc(C)cc2)c1=O|0.031|0.016|0.113|0.001|0.005
Cc1cc(O)c2c(c1)C(=O)c1cc(O)cc(O)c1C2=O|0.999|0.927|0.850|0.480|0.867
CN(C(=O)C(Cl)Cl)c1ccc(OC(=O)c2ccco2)cc1|0.983|0.099|0.773|0.000|0.010
Cc1n[nH]c(C)c1N=Nc1cc(Cl)ccc1Cl|0.998|0.049|0.772|0.005|0.036

