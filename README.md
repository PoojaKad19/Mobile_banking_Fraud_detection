# Mobile_banking_Fraud_detection

Fraud detection is essential for companies to safeguard their customers' transactions and accounts by detecting fraud before or as it happens. 
The FBI reports that in 2022, elder fraud victims in the US lost an average of $35,101 each, resulting in a total loss of over $3 billion.
Fraud detection using machine learning requires less manual labour. As the amount of data and experience increases, the results of machine learning become more accurate.


### Goal:

The goal of this project is to predict if a mobile banking transaction was fraudulent or not.

### About Dataset:
Financial transactions are intrinsically private in nature and hence this leads to no publicly available datasets, specially in the emerging mobile money transactions domain.
Used dataset from Kaggle. It is synthetic dataset generated using the simulator called PaySim. 
This work is part of the research project ”Scalable resource-efficient systems for big data analytics” fundedby the Knowledge Foundation (grant: 20140032) in Sweden.
E. A. Lopez-Rojas , A. Elmir, and S. Axelsson. "PaySim: A financial mobile money simulator for fraud detection". In: The 28th European Modeling and Simulation Symposium-EMSS, Larnaca, Cyprus. 2016

## EDA & Data Pre-processing

- Checked for missing values, and class imbalance in target feature.
- The data was imbalanced. Performed random under-sampling
- No missing values were mentioned as NaN but there are in fact missing details in account balances. 


![image](https://github.com/user-attachments/assets/f6f6550b-4136-4a0d-87d3-ca24666c6d47)


![image](https://github.com/user-attachments/assets/93422a63-c49c-4838-b65a-c3ed5dd701d7)


- 'oldbalanceOrg' in fraud cases is much higher, indicating those who are targeted are generally those with a lot of money. 'newbalanceOrig' in fraud cases are much less in fraud cases. 'newbalanceDest' in fraud cases is very less, indicating the person who does have much money in account is more likely to steal. 

## Feature Engineering

- Mapped the Amount column into different ranges using transformer

- Defined transaction type ,i.e, who the transaction is between. This could be found from the customer name. Defined is the transaction is between:
          0)Customer to Customer   1)Customer to Merchant 
          2) Merchant to Customer  3) Merchant to Merchant

- It was found that majority of transaction type was Customer to customer and few customer to Merchant

- Step column was converted into hours of the day 

![image](https://github.com/user-attachments/assets/af721d87-1990-494c-acca-30e577dd38db)

![image](https://github.com/user-attachments/assets/cb3f3f1d-f85b-4941-99ef-73d714e3df2a)

![image](https://github.com/user-attachments/assets/ddc6bae9-3763-4c82-8840-9b8614d877cc)


## Training and Validation

- Dropped unnecessary columns and then best features were selected. 

![image](https://github.com/user-attachments/assets/37be82a0-c8a7-4109-a21b-290634417d2b)

These features were re-scaled. The features  – CategoricalAmount, hour, transaction_type

- First, the training data was trained for  functional neural network data model. 
- Then, training and validation was done using Sequential Neural Network.
- Also checked whether performance increased or not using ensemble of sequential Neural Network model.

### 1. Functional API of Keras

- The functional API can handle models with non-linear topology, shared layers, and even multiple inputs or outputs. The Functional API is a very flexible and powerful way to define models.

- The code defines a neural network model for binary classification with four hidden layers, each followed by a dropout layer for regularization. 

- The output layer uses a sigmoid activation function, and the model is compiled with binary cross-entropy loss and the Adam optimizer. 


![image](https://github.com/user-attachments/assets/fb135798-6ce9-42ee-a7e0-5ec09a7b90bc)          ![image](https://github.com/user-attachments/assets/e838ebda-d045-4684-87d6-1e0dc6d867aa)


### 2. Sequential Model

- Sequential model supports linear stacks of layers.

- Below code defines a neural network for binary classification using the Sequential API. 

- It consists of four hidden layers with ReLU activation, dropout layers for regularization, and an output layer with a sigmoid activation function. 


![image](https://github.com/user-attachments/assets/c4a44e8a-68a2-4db0-9918-96729cc019d1)          ![image](https://github.com/user-attachments/assets/a862bfa0-a110-4f8e-8915-5851159eff1e)


### 3. Ensemble Learning of Sequential Model

- The provided code creates an ensemble of neural network models for binary classification. The number of models in the ensemble is set to 3 (num_models = 3).

1. Initialization: Two empty lists (models and history_enseq_list) are initialized to store individual models and their training histories, respectively.

2. Model Training Loop: The code iterates num_models times, each time splitting the training data into training and validation sets using different random seeds.

3. The ModelCheckpoint callback is used to save the weights of the best-performing model based on validation loss during training.

- Training and Saving:
1. Each model is trained on its respective split of the data for 150 epochs with a batch size of 64.
2. The weights of the best-performing model are loaded back into the model after training.

After the loop completes, the models list contains three trained neural network models, and history_enseq_list contains their corresponding training histories. This ensemble of models provides diversity in training data splits and initialization conditions, contributing to potentially improved generalization performance.

![image](https://github.com/user-attachments/assets/005dfa26-d039-4906-b176-0ae5f8d3c3dc)


## Results: Training and Validation Accuracy

![image](https://github.com/user-attachments/assets/aea7b81d-ad8f-4099-8144-8ac3f78ec990)        ![image](https://github.com/user-attachments/assets/64c8d48f-7c94-45dd-9c2b-466be2911c54)
                            Functional Model                                                                                       Sequential Model






