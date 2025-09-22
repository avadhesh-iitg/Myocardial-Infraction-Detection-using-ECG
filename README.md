# ❤️ Myocardial Infarction (MI) Detection from ECG Signals

## Problem Description

Myocardial Infarction (MI), or a **heart attack**, happens when blood flow to the heart muscle is blocked, often leading to serious and life-threatening outcomes.
**Early detection is crucial** — it can save lives by ensuring timely medical intervention.

This project focuses on building an **automated system for classifying ECG signals** into **Normal** and **MI** categories using **machine learning and deep learning techniques**.

---

## Dataset Description

We used publicly available datasets from **[PhysioNet](https://physionet.org/)**:

### 1. PTB Diagnostic ECG Database (PTBDB)

*  [Dataset Link](https://physionet.org/content/ptbdb/1.0.0/)
* 368 ECG recordings from MI patients.
* Sampling frequency: **1000 Hz**.
* Multi-lead signals (only 1 lead per patient used).
* Selected: **368 MI samples** + **80 Healthy samples**.

### 2. MIT-BIH Normal Sinus Rhythm Database (NSRDB)

*  [Dataset Link](https://physionet.org/content/nsrdb/1.0.0/)
* 500 ECG recordings from healthy individuals.
* Sampling frequency: **128 Hz**.

###  Combined Dataset

* **MI**: 368 (from PTBDB)
* **Healthy**: 80 (from PTBDB) + 500 (from NSRDB)
* **Total samples**: **948**

---

##  Preprocessing

### 1. Sampling Rate Adjustment

To unify signals across datasets:

* PTBDB: **1000 Hz → 500 Hz** (downsampled)
* NSRDB: **128 Hz → 500 Hz** (upsampled)
* Final unified sampling frequency: **500 Hz**
* Fixed signal length: **2500 samples**

### 2. Wavelet Transform

* Applied **Discrete Wavelet Transform (DWT)** with `sym4` wavelet at **level 3**
* Extracted **approximation coefficients** → preserved ECG patterns (QRS, P, T waves) while reducing high-frequency noise

---

##  Deep Learning Models

### 1D Convolutional Neural Network (1D CNN)

* Ideal for **time-series data** like ECG signals
* Captures **local temporal features** such as QRS complexes, P-waves, and T-waves
* Used to classify signals into **MI vs. Normal**

### Deep Neural Network (DNN)

* Used as a baseline model with flattened ECG inputs
* Showed lower performance compared to CNNs due to lack of temporal feature learning

---

##  Model Comparison – Test Accuracy

| Model                         | Sampling Rate | Preprocessing       | Test Accuracy |
| ----------------------------- | ------------- | ------------------- | ------------- |
| **1D CNN**                    | 250 Hz        | Raw                 | \~89%         |
| **1D CNN**                    | 500 Hz        | Raw                 | \~89%         |
| **1D CNN + Wavelet (sym4)**   | 500 Hz        | DWT (level 3, sym4) | \**~92%**        |
| **Deep Neural Network (DNN)** | 500 Hz        | Flattened Raw Input | \~87%         |

###  Key Insights

* Increasing sampling rate alone **did not improve accuracy** (250 Hz → 500 Hz, still \~89%).
* **Wavelet preprocessing** improved performance significantly (**92% accuracy**).
* CNNs consistently outperformed simple DNNs, highlighting the importance of temporal feature extraction in ECG signals.

---

##  Future Work

* Explore **Recurrent Neural Networks (RNNs) / LSTMs** for sequential ECG analysis
* Evaluate **transfer learning with pre-trained time-series models**
* Test on additional ECG datasets to improve generalization
* Incorporate **explainability methods (e.g., Grad-CAM for 1D CNNs)** to interpret decisions

---

## 🌍 Motivation

This work is motivated by the belief that **AI can assist doctors in early and accurate MI detection**, reducing diagnosis time and saving lives.
