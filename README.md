# **Named Entity Recognition (NER) Pipeline**

This project implements a **Named Entity Recognition (NER)** pipeline using **BERT-based models**. The pipeline includes three models with varying complexity, including models with **BERT + CRF** (Conditional Random Fields), and a more advanced **BERT + BiLSTM + CRF** model. The goal is to extract structured information (like names, organizations, locations, etc.) from unstructured text.

---

## **Table of Contents**

1. [Project Overview](#project-overview)
2. [Model Architecture](#model-architecture)
3. [Training & Evaluation](#training--evaluation)
4. [Performance Comparison](#performance-comparison)
5. [Deployment Plan](#deployment-plan)
6. [Future Directions](#future-directions)
7. [Usage](#usage)
8. [Requirements](#requirements)
9. [License](#license)

---

## **Project Overview**

This **NER pipeline** uses **BERT-based architectures** to classify named entities in a given text. Three distinct models were tested:
1. **Model 1**: BERT + CRF (BERT-base-cased)
2. **Model 2**: BERT + CRF (BERT-base-uncased)
3. **Model 3**: BERT + BiLSTM + CRF (BERT-base-uncased)

These models use **BertTokenizerFast** for tokenization and handle a variety of named entities such as persons (**PER**), organizations (**ORG**), locations (**LOC**), and more.

---

## **Model Architecture**

### **Model 1**:
- **Architecture**: **BERT + CRF**
- **Tokenizer**: **BertTokenizerFast** with **bert-base-cased** (case-sensitive)
  - Case-sensitive, useful for distinguishing proper nouns like **Paris** (City) vs **paris** (common noun).
- **Strengths**: 
  - **CRF** captures sequence dependencies, important for NER.
  - Retains case sensitivity.

### **Model 2**:
- **Architecture**: **BERT + CRF**
- **Tokenizer**: **BertTokenizerFast** with **bert-base-uncased** (case-insensitive, converts text to lowercase)
- **Strengths**:
  - The **CRF** layer is present here too, capturing sequential dependencies like **Model 1**.
  - The **BertTokenizerFast** with **bert-base-uncased** converts all words to **lowercase**, which might be beneficial when case sensitivity is not critical.

### **Model 3**:
- **Architecture**: **BERT + BiLSTM + CRF**
- **Tokenizer**: **BertTokenizerFast** with **bert-base-uncased** (case-insensitive, converts text to lowercase)
- **Strengths**:
  - **BiLSTM** combined with **BERT** and **CRF** allows the model to capture **both long-range** and **short-range dependencies** between tokens in the sequence.
  - **BertTokenizerFast** uses **bert-base-uncased**, ensuring the model works well with **lowercased** text, making it easier to handle case-insensitive tasks.
  - The **BiLSTM** layer captures **bidirectional context** (past and future tokens), enhancing sequence understanding.
  - The **CRF** layer ensures **valid tag sequences** in predictions, improving tagging consistency.

---

## **Training & Evaluation**

### **Training Process**:
- The models are trained using **BERT** (a pre-trained transformer model) as the base model. 
- **CRF** (Conditional Random Field) is used to capture label dependencies in **Model 1** and **Model 2**.
- **BiLSTM** (Bidirectional LSTM) is introduced in **Model 3** for better sequence modeling.

### **Evaluation Metrics**:
The models are evaluated using the following metrics:
- **Micro F1 Score**: Overall performance across all entities.
- **Macro F1 Score**: Measures performance across all classes equally, important for rare classes.
- **Weighted F1 Score**: Accounts for the support (frequency) of each class in the dataset.

### **Performance Comparison**:
| **Model**   | **Micro F1** | **Macro F1** | **Weighted F1** |
|-------------|--------------|--------------|-----------------|
| **Model 1** | 0.79         | 0.50         | 0.79            |
| **Model 2** | 0.80         | 0.56         | 0.81            |
| **Model 3** | 0.81         | 0.51         | 0.80            |

- **Model 3** performs best overall, with the highest **Micro F1** and strong performance on both common and rare tags.

---

## **Deployment Plan**

### **Model Serialization**:
- The trained models can be exported using **TorchScript** or **ONNX** for inference deployment.

### **Inference API**:
- **FastAPI** is used to wrap the model for inference.
- The API takes a list of raw sentences as input and outputs the **BIO-tagged tokens**.

### **Scaling**:
- The API can be containerized using **Docker**.
- **Kubernetes** can be used to auto-scale the service depending on resource demand.

---

## **Future Directions**

1. **Data Augmentation**:
   - Implement **back-translation** and **synonym swapping** to create more examples for rare classes.
  
2. **Domain-Adaptive Pretraining**:
   - Fine-tune the model on domain-specific **unlabeled corpora** for better performance in niche applications.
  
3. **Active Learning**:
   - Prioritize human labeling for **rare entities** where the model is uncertain.

4. **Model Compression**:
   - Apply **DistilBERT** and **quantization** techniques to deploy the model on edge devices with lower resource consumption.

---
