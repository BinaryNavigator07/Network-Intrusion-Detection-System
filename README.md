# 📊 Network Intrusion Detection System

## Overview

This project implements a comprehensive Network Intrusion Detection System (NIDS) using machine learning and deep learning approaches to identify and classify various types of network attacks. The system analyzes network traffic patterns from the CICIDS 2017 dataset to detect malicious activities including DDoS attacks, port scans, web attacks, and infiltration attempts. The project compares traditional machine learning algorithms with LSTM-based deep learning models to achieve high accuracy in intrusion detection.

---

## 📁 Project Structure

```
Network-Intrusion-Detection-System/
│
├── CICIDS 2017/                                    # Main project directory containing all analysis notebooks
│   ├── 2017_feature_extraction_with_machine_learning.ipynb    # Traditional ML models with feature engineering
│   ├── 2017_LSTM_with_feature_extraction.ipynb               # LSTM model with feature extraction
│   └── 2017_LSTM_without_feature_extraction.ipynb            # LSTM model without feature engineering
│
├── README.md                                       # Project description and usage guide (this file)
└── .gitignore                                      # Git ignore file for version control

```

---

## Understanding the Dataset — CICIDS 2017 Feature Breakdown

**Dataset**: CICIDS 2017 (Canadian Institute for Cybersecurity Intrusion Detection System Dataset)
**Records**: \~2.8M flows (rows)
**Columns**: 79 numerical features + 1 target label

**What each column represents:**

### **1. Flow Identification & Duration**

* **Flow Duration** — Total time (in microseconds) between the first and last packet of the flow.
* **Protocol, Source Port, Destination Port** — Identify the network connection endpoints.
* **Why it matters:** Certain attacks have very short or very long flows compared to normal traffic.

---

### **2. Packet Counts and Sizes**

* **Total Fwd Packets / Total Bwd Packets** — Number of packets sent forward (source → destination) and backward.
* **Total Length of Fwd/Bwd Packets** — Sum of bytes sent in each direction.
* **Min/Max/Mean/Std of Packet Lengths** — Packet size statistics.
* **Why it matters:** Attacks often have repetitive, uniform packet sizes or unusual size patterns.

---

### **3. Timing (Inter-Arrival Times)**

* **Flow IAT Mean / Max / Min / Std** — Time gaps between packets in the whole flow.
* **Fwd IAT / Bwd IAT** — Same but calculated separately for each direction.
* **Why it matters:** Some attacks (e.g., flooding) send packets at high speed with very low IAT.

---

### **4. TCP Flags & Header Information**

* **SYN, ACK, PSH, URG, FIN, RST, ECE, CWE Flag Counts** — Count of specific TCP control flags.
* **Fwd Header Length / Bwd Header Length** — Size of headers in packets.
* **Why it matters:** Abnormal flag patterns (e.g., SYN flood) signal scanning or intrusion.

---

### **5. Bulk Data Transfer Metrics**

* **Avg Bytes/Bulk, Avg Packets/Bulk, Avg Bulk Rate** — Bulk transfer behavior in forward/backward directions.
* **Why it matters:** File transfers or data exfiltration often show large bulk metrics.

---

### **6. Subflow Statistics**

* **Subflow Fwd Packets/Bytes, Subflow Bwd Packets/Bytes** — Packets and bytes within smaller segments of the flow.
* **Why it matters:** Detects bursts of traffic or chunked data transfers.

---

### **7. Active and Idle Times**

* **Active Mean / Min / Max / Std** — Periods when packets are actively being sent.
* **Idle Mean / Min / Max / Std** — Periods with no traffic.
* **Why it matters:** Attacks like brute force might have short active bursts and long idle waits.

---

### **8. Target Column**

* **Label** — Category of the traffic:

  * **BENIGN** — Normal traffic
  * **Attack types** — e.g., DDoS, PortScan, Botnet, Brute Force, Web Attack, Infiltration, Heartbleed.
* **Why it matters:** This is the ground truth for model training.

## 🔧 Setup and Installation Instructions

### Option 1: Run on Google Colab (Recommended)

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

#### 3. Set up Kaggle API (for dataset download)

```bash
pip install kaggle
# Configure your Kaggle API credentials
# Download kaggle.json from your Kaggle account and place it in ~/.kaggle/
```


---

## 🛠️ Step-by-Step Guide

### Step 1: Traditional Machine Learning Analysis with Feature Engineering
**File**: `CICIDS 2017/2017_feature_extraction_with_machine_learning.ipynb`


The code starts by installing and importing all required libraries for data handling, visualization, and machine learning, then downloads the dataset using kagglehub, loads all CSV files, and merges them into one DataFrame. It cleans the data by stripping column names, replacing negative values with zero, removing zero-variance and duplicate columns, handling infinities, and dropping duplicates to ensure only useful features remain. It then explores the class distribution with a plot, normalizes all numeric features to a 0–1 range using MinMaxScaler, and one-hot encodes the labels so each attack type becomes its own binary column. Here, the X (features) are all numeric columns describing network traffic statistics—such as Flow Duration, Total Fwd Packets, Total Backward Packets, Fwd Packet Length Mean, Bwd Packet Length Std, Flow IAT Mean, SYN Flag Count, and many more—while the Y (target) is the Label column, which contains the class of each record, either "BENIGN" (normal traffic) or specific attack types like "DoS Hulk", "PortScan", "DDoS", "FTP-Patator", and so on. The dataset is split into training (80%) and testing (20%) sets, with both reduced to 10% of their size for faster execution. Feature selection is performed using a Random Forest model to keep only the most important columns. Four machine learning models—Random Forest, Decision Tree, XGBoost, and CatBoost—are trained on this reduced feature set, each evaluated using accuracy, precision, recall, and F1-score. Finally, the results are visualized in bar charts to compare performance, providing a clear view of which algorithm works best for detecting network intrusions.

---

### Step 2: LSTM Deep Learning with Feature Engineering
**File**: `CICIDS 2017/2017_LSTM_with_feature_extraction.ipynb`

**What this notebook does:**
- **Data Preprocessing**: Similar data cleaning and feature engineering as Step 1
- **Feature Selection**: Uses the same RFE and Random Forest feature selection methods
- **LSTM Architecture**: Implements a deep learning model with:
  - LSTM layers for sequence learning from network flow patterns
  - Dense layers for classification
  - Dropout layers for regularization
  - MaxPooling for feature aggregation
- **Data Reshaping**: Converts tabular data into 3D sequences suitable for LSTM processing
- **Model Training**: 
  - Uses categorical cross-entropy loss function
  - Adam optimizer for gradient descent
  - Includes validation split for monitoring overfitting
- **Performance Analysis**: Evaluates LSTM performance against traditional ML models
- **Comparative Study**: Shows how feature engineering affects deep learning performance

**Expected Output**: LSTM model performance metrics and comparison with traditional ML approaches

---

### Step 3: LSTM Deep Learning without Feature Engineering
**File**: `CICIDS 2017/2017_LSTM_without_feature_extraction.ipynb`

**What this notebook does:**
- **Raw Data Processing**: Uses the original dataset without feature selection or engineering
- **Direct LSTM Implementation**: 
  - Processes all 79 features directly through LSTM layers
  - No feature reduction or selection applied
  - Tests LSTM's ability to learn from raw network traffic data
- **Model Architecture**: Similar LSTM structure but with:
  - Higher input dimensionality (all features)
  - Potentially different layer configurations
  - Same regularization techniques (dropout, pooling)
- **Training Process**: 
  - Longer training time due to higher feature dimensionality
  - Same loss function and optimizer as Step 2
  - Validation monitoring for overfitting detection
- **Performance Comparison**: 
  - Compares results with feature-engineered LSTM (Step 2)
  - Shows impact of feature engineering on deep learning performance
  - Demonstrates trade-offs between raw data processing and feature selection

**Expected Output**: LSTM performance on raw data, comparison with feature-engineered approach

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

3. **Dataset**: The system analyzes 2,830,743 network flow records with 79 features from the CICIDS 2017 dataset, covering various attack types including DDoS, Port Scan, Web Attacks, and Infiltration

4. **Scalability**: The models can effectively classify both normal and malicious network traffic with high precision and recall rates

### Interpretation of Results

- **Accuracy**: Represents the percentage of correctly classified network flows (normal vs. malicious)
- **Precision**: Measures the accuracy of positive predictions (malicious traffic detection)
- **Recall**: Measures the ability to find all malicious traffic instances
- **F1-Score**: Harmonic mean of precision and recall, providing balanced performance metric

The high accuracy rates (>99% for traditional ML, >97% for LSTM) indicate that the system can effectively distinguish between normal network traffic and various types of cyber attacks, making it suitable for real-world network security applications.