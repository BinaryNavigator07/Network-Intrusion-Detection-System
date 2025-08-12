# 📊 Network Intrusion Detection System

## Overview

This project delivers a robust Network Intrusion Detection System (NIDS) designed to help organizations safeguard their digital infrastructure against evolving cyber threats. Leveraging multiple CICIDS datasets from 2017 and 2018, the system learns from a wide range of real-world attack scenarios — including DDoS, port scans, brute-force attempts, web attacks, and infiltration — to provide a more comprehensive and resilient detection capability. By combining traditional machine learning models with LSTM-based deep learning architectures, the solution balances interpretability, speed, and accuracy, ensuring both quick response in operational environments and adaptability to new threat patterns. This hybrid approach enables businesses to proactively detect and classify malicious traffic with high precision, reducing the risk of data breaches, financial loss, and operational downtime while supporting compliance with security standards and regulations.

---

## Additional Documentation

please refer to [NIDS-Info-README.md](NIDS-Info-README.md).

Consulting this file will provide further context and technical insights beyond the main project overview.

---

## 📁 Project Structure

```
Network-Intrusion-Detection-System/
│
├── CICIDS 2017/                                 
│   ├── 2017 LSTM with feature extraction.ipynb   # Traditional ML models with feature engineering
│   ├── 2017_LSTM_with_feature_extraction.ipynb               # LSTM model with feature extraction
│   └── 2017_LSTM_without_feature_extraction.ipynb            # LSTM model without feature engineering
├── CICIDS 2018/                                    
│   ├── 2018 Feature extraction and LSTM.ipynb   # models with feature engineering
│   ├── 2018 LSTM without Feature Extraction.ipynb # LSTM model without feature extraction
|
├── Data-Set-README.md                                       # DataSet README
├── README.md                                       # Project description and usage guide (this file)
└── .gitignore                                      # Git ignore file for version control

```

For detailed information about the datasets used in this project, please refer to [Data-Set-README.md](Data-Set-README.md).

---

## 🔧 Setup and Installation Instructions

### Option 1: Run on Google Colab (Recommended)

> **Note:** Due to the large size of the 2018 dataset (~12 GB RAM required), running this notebook on Google Colab may cause the runtime to crash. It is recommended to run the 2018 dataset notebook locally on a machine with sufficient memory.

#### 1. Open the notebooks in Google Colab
Each notebook contains a "Open in Colab" button at the top. Click on it to be redirected to Google Colab.

#### 2. Set up runtime
- Go to **Runtime** → **Change runtime type**
- Select **GPU** or **TPU** for faster training (recommended for LSTM models)
- Choose **Python 3** as the runtime

#### 3. Grant permissions
- When prompted, allow the notebook to access your Google Drive
- This is required for dataset download and model saving
- Click "Allow" when the permission dialog appears

#### 4. Run the notebooks
- Execute cells sequentially using **Shift + Enter** or **Runtime** → **Run all**
- The notebooks will automatically download the dataset and install required packages

### Option 2: Run Locally

#### 1. Create a virtual environment and activate it

```bash
python3.10 -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
```

#### 2. Install dependencies

```bash
pip install -r requirements.txt
```

---


## 🛠️ Step-by-Step Guide

> **Note:** For demonstration and resource efficiency, only **10% of the dataset** is selected for training and testing in the provided code. If you wish to use the **entire dataset** (approximately 2.8 million records), simply **delete or comment out the cell** that performs this data reduction. This cell is present in all the notebooks and typically looks like:
>
> ```python
> # Selecting 10% of random rows for better running time
> X_train = X_train.sample(frac=0.1, replace=True, random_state=1)
> y_train = y_train.sample(frac=0.1, replace=True, random_state=1)
> X_test = X_test.sample(frac=0.1, replace=True, random_state=1)
> y_test = y_test.sample(frac=0.1, replace=True, random_state=1)
> print(X_train.shape, y_train.shape)
> print(X_test.shape, y_test.shape)
> ```
>
> Removing this cell will allow the code to process the full dataset, but ensure your machine has sufficient memory and compute resources.

> **Note:** The following steps are independent and can be executed in parallel. You may choose any step based on your preferred approach or experiment with multiple methods simultaneously.

### 1: Traditional Machine Learning Analysis with Feature Engineering
**File**: `CICIDS 2017/2017_feature_extraction_with_machine_learning.ipynb`


The code starts by installing and importing all required libraries for data handling, visualization, and machine learning, then downloads the dataset using kagglehub, loads all CSV files, and merges them into one DataFrame. It cleans the data by stripping column names, replacing negative values with zero, removing zero-variance and duplicate columns, handling infinities, and dropping duplicates to ensure only useful features remain. It then explores the class distribution with a plot, normalizes all numeric features to a 0–1 range using MinMaxScaler, and one-hot encodes the labels so each attack type becomes its own binary column. Here, the X (features) are all numeric columns describing network traffic statistics—such as Flow Duration, Total Fwd Packets, Total Backward Packets, Fwd Packet Length Mean, Bwd Packet Length Std, Flow IAT Mean, SYN Flag Count, and many more—while the Y (target) is the Label column, which contains the class of each record, either "BENIGN" (normal traffic) or specific attack types like "DoS Hulk", "PortScan", "DDoS", "FTP-Patator", and so on. The dataset is split into training (80%) and testing (20%) sets, with both reduced to 10% of their size for faster execution. Feature selection is performed using a Random Forest model to keep only the most important columns. Four machine learning models—Random Forest, Decision Tree, XGBoost, and CatBoost—are trained on this reduced feature set, each evaluated using accuracy, precision, recall, and F1-score. Finally, the results are visualized in bar charts to compare performance, providing a clear view of which algorithm works best for detecting network intrusions.

---

### 2: LSTM Deep Learning with Feature Engineering

## 2017 Dataset

**File:** `CICIDS 2017/2017_LSTM_with_feature_extraction.ipynb`

## 2018 Dataset

**File:** `CICIDS 2018/2018 Feature extraction and LSTM.ipynb`

> **Note:** Due to the large size of the 2018 dataset (~12 GB RAM required), running this notebook on Google Colab may cause the runtime to crash. It is recommended to run the 2018 dataset notebook locally on a machine with sufficient memory.

The code begins by installing and importing the necessary libraries for data processing, visualization, and machine learning, including CatBoost, XGBoost, and TensorFlow. It downloads the CICIDS2017 dataset using kagglehub, loads all CSV files in the directory, and merges them into a single DataFrame. Data cleaning removes leading/trailing spaces in column names, replaces negative values with zero, drops zero-variance and duplicate columns, handles infinities, removes NaN rows, and eliminates duplicate records. Attack labels are grouped into broader categories such as Dos, WebAttack, and BruteForce for simpler classification. All numeric columns are normalized to a 0–1 range using MinMaxScaler, while the target Label column is one-hot encoded so each class (e.g., BENIGN, Bot, BruteForce, Dos, Infiltration, PortScan, WebAttack) becomes its own binary column. Here, X (features) contains the normalized network traffic statistics such as flow duration, packet counts, byte counts, and timing metrics, while Y (target) is the one-hot encoded label matrix representing the attack category of each record. The dataset is split into training (80%) and testing (20%) sets, with both subsets further reduced to 10% for faster execution. Feature selection is performed with a Random Forest model to retain only the most important features, and the resulting feature matrices are reshaped into 3D arrays for LSTM compatibility. An LSTM-based deep learning model with multiple stacked LSTM layers, dropout for regularization, and a softmax output layer is defined and compiled using categorical cross-entropy loss and the Adam optimizer. The model is trained on GPU with custom logging of GPU usage at the end of each epoch. Finally, the trained LSTM is evaluated on the test set, reporting both loss and accuracy for network intrusion detection performance.




---

### 3: LSTM Deep Learning without Feature Engineering

## 2017 Dataset

**File**: `CICIDS 2017/2017_LSTM_without_feature_extraction.ipynb`

## 2018 Dataset

**File**: `CICIDS 2018/2018_without_feature_extraction_and_LSTM.ipynb`

> **Note:** Due to the large size of the 2018 dataset (~12 GB RAM required), running this notebook on Google Colab may cause the runtime to crash. It is recommended to run the 2018 dataset notebook locally on a machine with sufficient memory.

The code begins by installing and importing all necessary libraries for data handling, visualization, and machine learning, then downloads the CICIDS2017 dataset using kagglehub, loads all CSV files in the folder, and merges them into a single DataFrame. It cleans the data by stripping extra spaces from column names, replacing negative values with zero, removing zero-variance and duplicate columns, handling infinities, dropping NaN rows and duplicates, and eliminating columns with identical values. Attack labels are grouped into broader categories (Dos, WebAttack, BruteForce) to simplify classification. All numeric features are normalized to a 0–1 range using MinMaxScaler. Here, X (features) contains all numeric traffic statistics such as flow duration, packet counts, byte counts, and timing metrics, while Y (target) is the Label column representing the attack category, one-hot encoded into seven binary columns (BENIGN, Bot, BruteForce, Dos, Infiltration, PortScan, WebAttack). The dataset is split into training (80%) and testing (20%) sets, with both subsets reduced to 10% for faster execution. The features are reshaped into 3D format for LSTM input, and an LSTM-based model with stacked LSTM layers, dropout for regularization, and a softmax output layer is defined and compiled using categorical cross-entropy and the Adam optimizer. Training runs on GPU with a custom callback logging GPU memory usage after each epoch, and the model is finally evaluated on the test set to report loss and accuracy for network intrusion detection performance.

---

## 📊 Results

### Traditional Machine Learning Models Performance

The project achieves exceptional performance with traditional machine learning algorithms:

- **Random Forest**: 99.80% accuracy across all metrics (Precision, Recall, F1-Score)
- **Decision Tree**: 99.71% accuracy across all metrics
- **XGBoost**: 99.75-99.80% accuracy across all metrics
- **CatBoost**: 99.53-99.55% accuracy across all metrics

### Deep Learning Model Performance

The LSTM model without feature extraction achieves:
- **Test Accuracy**: 97.94%
- **Validation Accuracy**: 96.83% (final epoch)

### Key Findings

1. **Feature Engineering Impact**: Traditional ML models with feature engineering achieve higher accuracy (99.5%+) compared to LSTM models without feature engineering (97.94%)

2. **Model Comparison**: Random Forest and XGBoost perform best among traditional ML algorithms, while LSTM shows competitive performance for deep learning approaches

3. **Dataset**: The system analyzes 2,830,743 network flow records with 79 features from the CICIDS 2017 and 2018 dataset, covering various attack types including DDoS, Port Scan, Web Attacks, and Infiltration

4. **Scalability**: The models can effectively classify both normal and malicious network traffic with high precision and recall rates

### Interpretation of Results

- **Accuracy**: Represents the percentage of correctly classified network flows (normal vs. malicious)
- **Precision**: Measures the accuracy of positive predictions (malicious traffic detection)
- **Recall**: Measures the ability to find all malicious traffic instances
- **F1-Score**: Harmonic mean of precision and recall, providing balanced performance metric

> **Note:** The exceptionally high accuracy and metric scores—particularly for traditional machine learning models—may indicate potential overfitting, given the dataset's complexity and large number of features. To improve generalizability and robustness, it is recommended to validate the models on external datasets and consider applying additional regularization techniques.
