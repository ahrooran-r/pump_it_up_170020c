# pump_it_up_170020c

Submission for Machine Learning project Pump-It-UP

https://github.com/ahrooran-r/pump_it_up_170020c

### Preface

1. Many if the feature engineering processes I have done are taken from previous 
successful submissions by others. However I have modified them somewhat in hopes of 
gaining better results.

2. I could not be able to try Neural Networks or deep learning processes because 
    
    1. the hardware of my PC is incapable of running such intensive task for long time
    2. they seem to be a little overkill because even with typical algorithms, results seem to be good

3. Almost all pre-processing steps are marked with references of original work.

### About the code

There are 4 jupyter files.
1. pre-process: My initial pre-processing attempts. Comprises of 90% work done.
2. pre-process_2: After the initial attempt I thought on using KNNImputer to replace missing values with approximate values.
However it didn't end so well. So I keep the original in a separate file in case if you want to run it yourself.
3. model_pre_test: Because I could not extensively test each model due to hardware limitations,
I first tried to fit the data with vanilla models of each different ones available in `sci-kit learn` library.
In the end RandomForest came out to be the best fit.
4. model_rfc: this is where I experimented with tuning the hyper parameters a bit and actual prediction happened.

So if you want to reproduce the results, run:
1. pre-process.ipynb
2. model_rfc.ipynb

### Pre-processing

1. Check for duplicates in the data, both `train` and `test`
2. Categorizing data into `train` and `test` type and merge them together into single data frame for combined processing
3. Did a quick run on the data to reveal following: `["Column", "Dtype", "N_Unique", "Unique_vals"]`

### Feature Engineering

1. Grouped vast number categorical values in following columns to minimal amount
    1. `installer`
    2. `funder`
    3. `scheme_management`
2. created `age` column from `date_recorded` and `construction_year`
3. Grouped `construction_year` into separate bins 
4. Replace boolean cols with median values where missing
    1. `public_meeting`
    2. `permit`
5. Apply log transform to following:
    1. `amount_tsh`
    2. `population`
6. Replace `age` with mean values where missing
7. Working with location data
    1. Filled missing values in `gps_height` with third-party table
    2. Filled `latitude` and `longitude` with `region_code`
    3. Created `haversine_distance` column from `latitude` and `longitude`.
    4. Convert `latitude`, `longitude` to `x_coordinate`, `y_coordinate` and `z_coordinate`
    5. Create a `location_cluster` with DBScan from `latitude` and `longitude`
8. Dropped columns:
    1. `wpt_name`
    2. `subvillage`
    3. `scheme_name`
    4. `ward`
    5. `region`
    6. `lga`
    7. `recorded_by`
    8. `latitude`
    9. `longitude`
    10. `extraction_type`
    11. `extraction_type_group`
    12. `management`
    13. `management_group`
    14. `payment`
    15. `quality_group`
    16. `source`
    17. `num_private`
    18. `district_code`
    19. `region_code`
9. Converted `object` columns to `category` and numeric columns to `float64`
10. Do a Min-Max Scaling on following columns
    1. `amount_tsh`
    2. `gps_height`
    3. `population`
    4. `haversine_distance`
    5. `x_coordinate`
    6. `y_coordinate`
    7. `z_coordinate`
    8. `age`
11. Do a One-Hot Encoding on categorical columns
12. Removed highly co-related columns above threshold = 0.9
13. Did KNN-Imputation on missing columns in `pre-process_2.ipynb`
    1. Replaced `missing` as `np.nan` in all categorical columns
    2. Label-Encoded categorical columns
    3. Filled missing values with KNN-Imputer
    4. One-Hot Encoded categorical columns
    5. Merged both numerical and categorical columns back
    6. Did cor-relational analysis on resulting dataset
    
### Model

Tried many models such as:
1. BernoulliNB()
2. GaussianNB()
3. DecisionTreeClassifier()
1. ExtraTreeClassifier()
1. ExtraTreesClassifier()
1. KNeighborsClassifier()
1. LinearSVC(multi_class = 'crammer_singer')
1. LogisticRegression(multi_class="multinomial")
1. RandomForestClassifier(criterion="entropy")
1. AdaBoostClassifier()
1. XGBClassifier()
1. XGBRFClassifier()
1. HistGradientBoostingClassifier()
1. LGBMClassifier()
1. CatBoostClassifier(verbose=0)
1. SVC()
1. GaussianProcessClassifier(multi_class="one_vs_one")
1. GradientBoostingClassifier()
1. GaussianProcessClassifier(multi_class="one_vs_rest")
1. LinearSVC(multi_class="ovr")
1. LogisticRegression(multi_class="ovr")
1. SGDClassifier()
1. Perceptron()


Model selected in the end is `RandomForest` with hyper parameters:
1. criterion="entropy"
2. n_estimators=2000
3. max_depth=110
4. max_features="sqrt"

### Accuracy metric

Used evaluation metric: `f1_score(, , average="weighted")`

Achieved accuracy: **0.8092**

Current Rank: **2338** / 12410

Proof of submission:

![777](https://user-images.githubusercontent.com/46846338/133297002-41710230-74df-453d-b4a6-b26930b1bee7.png)
