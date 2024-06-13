# Chatter Detection and Mitigation in CNC Machining Using Audio Signal Analysis and TinyML

## Preface
Chatter is an undesirable phenomenon in CNC machining that manifests as vibrations, leading to poor surface finish, reduced tool life, and potential damage to the workpiece or machine. Detecting and mitigating chatter is critical for maintaining machining quality and efficiency. This project aims to develop a model that classifies machining conditions based on audio signals captured during the machining process and adjusts machining parameters (speed, feed, and depth of cut) to minimize chatter. Additionally, TinyML is employed to train the chatter detection and control model directly on a microcontroller, enabling real-time and on-device decision-making.

## Objectives
1. **Develop a Classification Model**: Train a machine learning model on audio signals to classify machining conditions into No Chatter, Mild Chatter, and Harmful Chatter.
2. **Parameter Optimization**: Adjust machining parameters (spindle speed, feed rate, and depth of cut) to minimize chatter and improve machining performance.
3. **TinyML Implementation**: Utilize TinyML to train and deploy the chatter detection and control model on a microcontroller for real-time, on-device processing.
4. **Validation and Testing**: Validate the model using real-time machining data and evaluate its effectiveness in reducing chatter.

## Methodology
![Chatter Removal Model drawio](https://github.com/semi-infiknight/ChatteringML/assets/97100765/80998f44-9bc7-43ce-a9ef-8bb2e3092f8c)


### Data Collection
Audio signals are captured using high-sensitivity microphones placed near the machining area on a CNC machine. The signals are recorded at different machining conditions, varying spindle speeds, feed rates, and depths of cut. Data is labeled based on visual inspection and expert judgment of the surface finish and vibration levels.

- **Parameters**:
  - Sampling rate: 44.1 kHz
  - Duration of each recording: 10 seconds
  - Number of recordings: 300 (100 for each class: No Chatter, Mild Chatter, Harmful Chatter)

### Feature Extraction
Features are extracted from the audio signals to capture the characteristics associated with different levels of chatter. Common features include:

1. **Time-Domain Features**: Root Mean Square (RMS), Peak Amplitude, Zero Crossing Rate (ZCR).
2. **Frequency-Domain Features**: Power Spectral Density (PSD), Spectral Centroid, Spectral Bandwidth.
3. **Statistical Features**: Mean, Variance, Skewness, Kurtosis.

### Model Training
A supervised machine learning approach is used to classify the audio signals. The dataset is split into training (70%), validation (15%), and test (15%) sets. Various classifiers such as Support Vector Machine (SVM), Random Forest, and Convolutional Neural Networks (CNN) are evaluated.
  - Feature vector size: 50
  - Training set size: 210 recordings
  - Validation set size: 45 recordings
  - Test set size: 45 recordings

### Hyperparameter Tuning
Hyperparameters are optimized using grid search and cross-validation techniques to improve model performance. Metrics such as accuracy, precision, recall, and F1-score are used for evaluation.

### TinyML Implementation
To enable real-time chatter detection and control directly on the CNC machine, TinyML is employed. TinyML allows machine learning models to run on microcontrollers and edge devices with limited computational resources.

1. **Model Compression**: The trained model is compressed and quantized to fit within the memory and processing constraints of the microcontroller.
2. **Deployment**: The compressed model is deployed on a microcontroller (e.g., Arduino Nano 33 BLE Sense) that is interfaced with the CNC machine.
3. **Real-Time Processing**: The microcontroller processes the audio signals in real-time, classifies the machining condition, and adjusts the machining parameters accordingly.

### Controller Framework
The overall framework of the chatter detection and mitigation system is divided into three main layers: Machine Interface, Optimization Layer, and Reinforcement Layer.

#### Machine Interface
1. **Initialize**: The process starts with sensor data acquisition, where audio signals are captured.
2. **State Representation and Preprocessing**: The captured signals are preprocessed, and relevant features are extracted.
3. **Hyperparameter Tuning**: This step ensures the model is compatible with CNC operations by fine-tuning hyperparameters.
4. **Actuation Control**: For motors, this step involves controlling the actuation to achieve a state with no chatter.

#### Optimization Layer
1. **Classification Model**: The preprocessed signals are fed into a classification model that identifies the presence of chatter (No Chatter, Mild Chatter, or Harmful Chatter).
2. **Chatter Detection Algorithm**: If chatter is detected, the system sorts the severity of the chatter and decides whether to iterate further.
3. **Optimization Filter**: The optimization filter processes the classified data to prepare it for the reinforcement model.

#### Reinforcement Layer
1. **Action Selection**: Based on the optimization filter, actions such as adjusting speed, feed, and depth of cut are selected.
2. **Execute Adjustments**: The selected actions are executed to adjust the machining parameters.
3. **Feedback and Reward Loop**: This loop provides feedback based on the results of the adjustments and updates the reinforcement model to refine future actions.

- **Reward Functions**:
  - **Smoothness Penalty**: This penalty considers the changes in speed, feed, and depth of cut to ensure smooth transitions.
  - **Chatter Reward**: This reward is weighted based on the detected chatter level.
  - **Efficiency Reward**: This reward focuses on machining efficiency, inversely related to the machining time.

## Results
The performance of the classification model is evaluated on the test set. The effectiveness of the parameter optimization strategy is validated through experiments on the CNC machine.
  - Classification accuracy: 95%
  - Reduction in harmful chatter incidents by 80%
  - Improved surface finish quality

