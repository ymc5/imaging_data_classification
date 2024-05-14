# imaging_data_classification
Classification prediction based on pyradiomics feature extracted data. 

I. Objective
In this project, predictive model was built based on model training that was conducted on TCGA brain cancer dataset, and then the model performance was tested on the Rembrandt feature extraction data. During the training process, feature importance was also evaluated, and further research was performed in these features. 

II. Method

Task 1: Pyradiomics feature extraction from Rembrandt dataset
•	Using pyradiomics package (featureextractor function), each T1 image data was processed and pyradiomics feature extraction databased was generated. 
•	A total of 64 patient data were processed and used in the database creation. 
•	The final table was exported in a csv form which was used for task 3. 

Task 2: Build/Train predictive machine learning model for brain cancer type prediction.
•	Used TCGA cancer image dataset (feature extraction processed data) for model training
•	Eliminated oligoastrocytoma disease
•	Data preprocessing
o	Train/Test sets split (8:2)
o	Continuous feature: StandardScaler
o	Categorical outcome (disease): LabelEncoder (0: astrocytoma, 1:GBM, 2: oligodendroglioma)
o	Imbalance adjustment: The dataset is heavily imbalanced with GBM having the highest number of cases, and astrocytoma and oligodendroglioma have low number of data. In this project, several imbalance adjustment technologies were attempted as below
- SMOTE
- ADASYN
- SMOTETomek
- SMOTEENN
- RandomOverSampler
- BorderlineSMOTE
o	Hyperparameter tuning:
- GridSearchCV
- Manual adjustment
•	Model training: Following models were trained 
o	SVM
o	Random Forest
o	XGBoost Classifier
o	Convolutional Neural Network
o	KNN
o	Decision Tree
o	Gradient Boost Classifier
o	Logistic Regression
o	AdaBoost Classifier
o	Stacking
o	Voting Classifier
•	Feature importance: Based on Random Forest, Decision Tree and XGBoost, top 10 features were selected to be compared with other group team members. 

Task 3: Apply predictive model from Task 2 to Rembrandt feature extraction data.
•	Data preprocessing: Rembrandt data from task 1 had to be processed so that it has the same data structure as the data that was used for model training in task 2. 
•	Next, models that were imported from task 2 were applied to this Rembrandt data and the outcome column was created. 

Task 4: Compare the result from Task 3 with the actual result data and evaluate performance 
•	Import 'ground truth data' with the disease labels for the patients from Rembrandt data. 
•	Some data were missing, and a total of 58 patient data was used for comparison. 
•	Each disease names were mapped to number 0, 1, or 2 based on diagnosis. 
•	The predicted result from task 3 and the ground truth data were merged to create a single data frame. 
•	Based on comparison, confusion matrix and classification report were printed out for evaluation.

Task 5: Summary of results
•	Summary of analysis results are summarized herein. 

Task 6: Presentation 
•	Group presentation

III. Analysis Results

•	TCGA best training model
o	Model: Random Forest
o	Data Preprocessing: 
Item	Preprocessing
Imbalance adjustment	SMOTE
Hyperparameter tuning	GridSearchCV
Feature selection	Top 30 (Random Forest)
o	Result
 
o	0: Astrocytoma, 1: GBM, 2: Oligodendroglioma

•	Rembrandt best testing model
o	Model: Gradient Boosting Classifier
o	Data Preprocessing: 
Item	Preprocessing
Imbalance adjustment	RandomOversampler
Hyperparameter tuning	class_weights {0:1, 1:1, 2:2}
Feature selection	None (all features)
o	Result
 
o	 
o	0: Astrocytoma, 1: GBM, 2: Oligodendroglioma



•	Feature Importance List

Feature	Description
diagnostics_Mask-original_VolumeNum	Number of discrete 3D regions are identified within the mask applied to the original image data
original_shape_SurfaceVolumeRatio	Volume enclosed by the surface of a tumor or a specific region of interest within the brain
original_shape_Sphericity	3D shape feature representing a measure of roundness of the tumor, with a value ranging from 0 to 1, where 1 indicates a perfect sphere.
original_glrlm_RunLengthNonUniformity	Non-uniformity of texture in the original image
original_shape_MeshVolume	Represent anatomical structures or pathological lesions, such as tumors
diagnostics_Mask-original_VoxelNum	Number of voxels within a segmented region or mask; density or intensity of the tissue

•	Feature Importance Research

Feature	Description
diagnostics_Mask-original_VolumeNum	- tumor heterogeneity
- insights into complexity of the pathology
original_shape_SurfaceVolumeRatio	NO change in shape of tumor:  
•	Increasing size of tumor → volume grows faster than surface  
o	→ decreases surface: volume ratio 
•	Decreasing size of tumor → volume shrinks faster than surface 
o	→ increases surface:volume ratio
original_shape_Sphericity	Oligodendroglioma is generally round to oval appearing (possibly due to cell nature)
In shape, oligodendroglioma resembles low-grade astrocytoma
GBM and advanced astrocytoma have relatively irregular shapes 
original_glrlm_RunLengthNonUniformity	Oligodendroglioma and early stage astrocytoma may have more homogenous texture. 
Advanced astrocytoma & GBM tumors may exhibit irregular with a mix of solid and cystic areas. 
original_shape_MeshVolume	Increased mesh volume in higher-grade gliomas → Represents aggressiveness and invasion of high-grade tumors across the brain
diagnostics_Mask-original_VoxelNum	→ tumour volume
→ changes in the pathology


IV. Discussion
Model training & performance
•	There were substantial discrepancies between the TCGA data training performance and the Rembrandt data testing performance. 
•	The discrepancies were more substantial in diagnoses with the lowest number of samples within the dataset. 
•	This could be due to possible wide range of grades in astrocytoma patients, although the actual grades of these patients were not confirmed in this study. 
•	The best performing model was Gradient Boosting Classifier (GBC). It is an ensemble machine learning model, in which the model trains on the training set iteratively, with the goal to minimize the loss function. 
•	Since the TCGA dataset was highly imbalanced, RandomOversampler technique was acquired in order to balance for further training. 
•	In addition, class_weight parameter was defined in the GBC model, to put more weight on oligodendroglioma. This is because although the model was classifying oligodendroglioma above average on the TCGA data, it was not classifying properly in the Rembrandt test data, and the aim was to train the model to focus more on classifying oligodendroglioma.
Feature importance
•	Our group identified top 6 features that were important based on the training models. 
•	Each of our team members used Decision Tree Classifier, Random Forest Classifier, and XGBoost Classifier to identify important features and combined all the results. The highest features based on number of counts were eventually selected as important features. 
•	The table above indicates that the overall shape, rather than just one characteristic should all be considered when identifying cancer types. 
•	While visible features such as sphericity, uniformity may be able to be identified with human observation, other features such as surface volume ratio or meshvolume may be identifiable in a more accurate manner using feature extraction techniques. 
•	This could indicate how radiomics feature extraction technique and machine learning models might play an important role in giving accurate diagnoses to patients with brain cancer, which would lead to a better treatment plan and prognosis. 
![image](https://github.com/ymc5/imaging_data_classification/assets/162189778/a94baf53-3e50-4a8a-a67e-125f507dc991)
