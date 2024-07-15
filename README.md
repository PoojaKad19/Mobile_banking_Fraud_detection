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

Functional Model :

![image](https://github.com/user-attachments/assets/aea7b81d-ad8f-4099-8144-8ac3f78ec990)       
                                                                                                                  
Sequential Model:

 ![image](https://github.com/user-attachments/assets/64c8d48f-7c94-45dd-9c2b-466be2911c54)

Ensemble learning of Sequential Model :

![image](https://github.com/user-attachments/assets/18578739-03f3-4c96-b49f-cad40320310e)              ![image](https://github.com/user-attachments/assets/531d73a2-ed6f-4ca5-8210-7c7756a7cb4f)                ![image](https://github.com/user-attachments/assets/61072e83-06a8-470b-b540-d415f675123c)


### Training and Validation Loss

Functional Model: 

![image](https://github.com/user-attachments/assets/c8426c9a-14fc-4d18-a40c-c8438e765a0a)


Sequential Model:

![image](https://github.com/user-attachments/assets/5211c24a-2a4a-490d-b044-0d9790660910)


Ensemble learning of Sequential Model:

![image](https://github.com/user-attachments/assets/cbae2436-ecfc-48c5-a60d-3c944f6d1e1a)              ![image](https://github.com/user-attachments/assets/96d2ccbd-3948-450f-873e-e94dc9ed4655)                 ![image](https://github.com/user-attachments/assets/ca5a938f-e43f-4a33-8964-c113ef6f99bb)


### Confusion Matrix

Functional Model: 

![image](https://github.com/user-attachments/assets/64f08b9f-3643-471c-935d-06370d88c273)


Sequential Model:

![image](https://github.com/user-attachments/assets/da16d938-c5b0-4cc1-b3af-14784ca44acf)


Ensemble learning of Sequential Model:

![image](https://github.com/user-attachments/assets/d5880ca7-8a58-42b2-b3e4-8769a0903492)


## Visualizing Predictions

![image](https://github.com/user-attachments/assets/93289c23-204b-42a6-9d91-eba48f67e7a4)             ![image](https://github.com/user-attachments/assets/0e4cf601-471e-49d6-8747-c774781b49dd)               ![image](https://github.com/user-attachments/assets/8152d12f-7401-4479-a5a0-a4366dc2aad6)

![image](https://github.com/user-attachments/assets/fadeb066-89fa-48bd-bbae-f30ccf675e95)              ![image](https://github.com/user-attachments/assets/05f2da09-5bbb-4a9e-a975-6e2ac4e4c1de)


- Visualize the predictions and compare them with the ground truth to gain insights into the model's performance.
- The blue line represents the values of the input features (CategoricalAmount, hour, transaction_type) for a specific example (index) from your test data.
- Ground Truth (Ground Truth line in red dashed):The red dashed line represents the ground truth value for the corresponding example (index). This is the actual target or label for the given input features.
- Ensemble Prediction (Ensemble Prediction line in blue dashed):The blue dashed line represents the prediction made by your ensemble model for the same example (index). This is the predicted value based on the ensemble of multiple models.
- By visualizing the input features, ground truth, and ensemble prediction on the same plot, you can get an intuitive sense of how well your ensemble model is performing for these specific examples. If the blue dashed line (ensemble prediction) aligns well with the red dashed line (ground truth), it indicates that your ensemble model is making accurate predictions for these particular examples. If there are discrepancies, you may want to inspect and understand why the model predictions differ from the ground truth.



