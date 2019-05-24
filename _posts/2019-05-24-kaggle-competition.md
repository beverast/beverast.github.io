
# Reflections on My First Kaggle Competition


My first submission landed me in the top 10, but by the end of the week I found myself squarely near the bottom of the leaderboard... What happend!? This post is a breakdown of what went well, what went wrong, and what went sideways. 

--------------------------------

## Step 0: Understand the Problem

This Kaggle competition used data from The Tanzania Ministry of Water and Taarifa about the functional status of water pumps across the country. There are thousands of these pumps, a limited number of resources (engineers, information about the pumps, etc.) for maintaining them, and its my task for this competition to use this data to classify whether a particular pump is likely to be functional, non-functional, or functional but needs repair. You may already know this, but Kaggle competitions only evaluate machine learning models on the metric of accuracy. There are many issues with evaluating only on accuracy, but that's not within the scope of this post, so we'll just go along with it. As an aside, I also think its important to note that I have absolutely no domain knowledge or expertise in water pumps, so I'll have to rely on my intuition of machine learning and feature engineering to try to come up with techniques to boost my classifiers' scores.

![Taarifa](http://drivendata.materials.s3.amazonaws.com/pumps/taarifadashboard.png)

-----------------

## Step 1: Make a baseline model

The baseline model I'd chosen was a decision tree classifier. Decision trees offer more interpretable results, as well as being more robust to unscaled input data; A good option for a fast first baseline! What data should be used to train a baseline model? To be quite literal about 'baselines' I opted to leave the data untouched, and use only the numeric data.

Below I'm using an excerpt of code to show what the dataset looks like in the raw, and what was used by my baseline model for the first set of predictions, and resultant accuracy score.

The first three observations from the `train_features` dataset:


```python
features_df = pd.read_csv("~/data/train_features.csv")
labels_df = pd.read_csv("~/data/train_labels.csv")
features_df.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>amount_tsh</th>
      <th>date_recorded</th>
      <th>funder</th>
      <th>gps_height</th>
      <th>installer</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>wpt_name</th>
      <th>num_private</th>
      <th>basin</th>
      <th>subvillage</th>
      <th>region</th>
      <th>region_code</th>
      <th>district_code</th>
      <th>lga</th>
      <th>ward</th>
      <th>population</th>
      <th>public_meeting</th>
      <th>recorded_by</th>
      <th>scheme_management</th>
      <th>scheme_name</th>
      <th>permit</th>
      <th>construction_year</th>
      <th>extraction_type</th>
      <th>extraction_type_group</th>
      <th>extraction_type_class</th>
      <th>management</th>
      <th>management_group</th>
      <th>payment</th>
      <th>payment_type</th>
      <th>water_quality</th>
      <th>quality_group</th>
      <th>quantity</th>
      <th>quantity_group</th>
      <th>source</th>
      <th>source_type</th>
      <th>source_class</th>
      <th>waterpoint_type</th>
      <th>waterpoint_type_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>69572</td>
      <td>6000.0</td>
      <td>2011-03-14</td>
      <td>Roman</td>
      <td>1390</td>
      <td>Roman</td>
      <td>34.938093</td>
      <td>-9.856322</td>
      <td>none</td>
      <td>0</td>
      <td>Lake Nyasa</td>
      <td>Mnyusi B</td>
      <td>Iringa</td>
      <td>11</td>
      <td>5</td>
      <td>Ludewa</td>
      <td>Mundindi</td>
      <td>109</td>
      <td>True</td>
      <td>GeoData Consultants Ltd</td>
      <td>VWC</td>
      <td>Roman</td>
      <td>False</td>
      <td>1999</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>vwc</td>
      <td>user-group</td>
      <td>pay annually</td>
      <td>annually</td>
      <td>soft</td>
      <td>good</td>
      <td>enough</td>
      <td>enough</td>
      <td>spring</td>
      <td>spring</td>
      <td>groundwater</td>
      <td>communal standpipe</td>
      <td>communal standpipe</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8776</td>
      <td>0.0</td>
      <td>2013-03-06</td>
      <td>Grumeti</td>
      <td>1399</td>
      <td>GRUMETI</td>
      <td>34.698766</td>
      <td>-2.147466</td>
      <td>Zahanati</td>
      <td>0</td>
      <td>Lake Victoria</td>
      <td>Nyamara</td>
      <td>Mara</td>
      <td>20</td>
      <td>2</td>
      <td>Serengeti</td>
      <td>Natta</td>
      <td>280</td>
      <td>NaN</td>
      <td>GeoData Consultants Ltd</td>
      <td>Other</td>
      <td>NaN</td>
      <td>True</td>
      <td>2010</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>wug</td>
      <td>user-group</td>
      <td>never pay</td>
      <td>never pay</td>
      <td>soft</td>
      <td>good</td>
      <td>insufficient</td>
      <td>insufficient</td>
      <td>rainwater harvesting</td>
      <td>rainwater harvesting</td>
      <td>surface</td>
      <td>communal standpipe</td>
      <td>communal standpipe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34310</td>
      <td>25.0</td>
      <td>2013-02-25</td>
      <td>Lottery Club</td>
      <td>686</td>
      <td>World vision</td>
      <td>37.460664</td>
      <td>-3.821329</td>
      <td>Kwa Mahundi</td>
      <td>0</td>
      <td>Pangani</td>
      <td>Majengo</td>
      <td>Manyara</td>
      <td>21</td>
      <td>4</td>
      <td>Simanjiro</td>
      <td>Ngorika</td>
      <td>250</td>
      <td>True</td>
      <td>GeoData Consultants Ltd</td>
      <td>VWC</td>
      <td>Nyumba ya mungu pipe scheme</td>
      <td>True</td>
      <td>2009</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>gravity</td>
      <td>vwc</td>
      <td>user-group</td>
      <td>pay per bucket</td>
      <td>per bucket</td>
      <td>soft</td>
      <td>good</td>
      <td>enough</td>
      <td>enough</td>
      <td>dam</td>
      <td>dam</td>
      <td>surface</td>
      <td>communal standpipe multiple</td>
      <td>communal standpipe</td>
    </tr>
  </tbody>
</table>
</div>



The subset of numeric-only data (for the model training):


```python
X_train, X_val, y_train, y_val = train_test_split(
    features_df.drop(columns="id"),
    labels_df["status_group"],
    stratify=labels_df["status_group"],
    test_size=0.3,
    random_state=42,
)
X_train.select_dtypes("number").head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>amount_tsh</th>
      <th>gps_height</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>num_private</th>
      <th>region_code</th>
      <th>district_code</th>
      <th>population</th>
      <th>construction_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5974</th>
      <td>0.0</td>
      <td>1410</td>
      <td>36.667568</td>
      <td>-3.353465</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>200</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>31787</th>
      <td>0.0</td>
      <td>1161</td>
      <td>33.637501</td>
      <td>-2.139160</td>
      <td>0</td>
      <td>20</td>
      <td>4</td>
      <td>400</td>
      <td>1996</td>
    </tr>
    <tr>
      <th>27800</th>
      <td>0.0</td>
      <td>1184</td>
      <td>36.854557</td>
      <td>-2.977996</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>550</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>8008</th>
      <td>0.0</td>
      <td>0</td>
      <td>33.023974</td>
      <td>-2.933736</td>
      <td>0</td>
      <td>19</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>39725</th>
      <td>0.0</td>
      <td>1331</td>
      <td>36.528004</td>
      <td>-5.674631</td>
      <td>0</td>
      <td>21</td>
      <td>5</td>
      <td>1</td>
      <td>2006</td>
    </tr>
  </tbody>
</table>
</div>



How useful is this limited data? Well, the baseline scored 66.5% accuracy which meets the 60% minimum.


```python
clf = DecisionTreeClassifier(random_state=42)

clf.fit(X_train.select_dtypes("number"), y_train)
clf.score(X_val.select_dtypes("number"), y_val)
```




    0.665712682379349



I want to see some more fine-grained results on the accuracy of this model. This is a multi-class classification problem, and the model very well could be not guessing a certain class. The table below is the confusion matrix for the model trained on only the unaltered numeric data.


```python
y_pred = clf.predict(X_val.select_dtypes("number"))
columns = [f"Predicted {label}" for label in np.unique(y_val)]
index = [f"Actual {label}" for label in np.unique(y_val)]
pd.DataFrame(confusion_matrix(y_val, y_pred), columns=columns, index=index)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Predicted functional</th>
      <th>Predicted functional needs repair</th>
      <th>Predicted non functional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Actual functional</th>
      <td>7057</td>
      <td>568</td>
      <td>2053</td>
    </tr>
    <tr>
      <th>Actual functional needs repair</th>
      <td>628</td>
      <td>369</td>
      <td>298</td>
    </tr>
    <tr>
      <th>Actual non functional</th>
      <td>2120</td>
      <td>290</td>
      <td>4437</td>
    </tr>
  </tbody>
</table>
</div>



### Step 1 Takeaway

So what went well and what went poorly here? Such a limited sampling of the data (with missing values included) certainly didn't help the model. I know that at least using a `LabelEncoder()` from `sklearn` would only take a moment and would probably increase the score considerably. The key takeaway for me is: **make the baseline model dumb, but don't use low quality data**. Since this is a Kaggle competition and not a business operation, the only metric our models are evaluated on is accuracy. Throughout the project I only evaluated model accuracy because of this fact. I know there are many better metrics such as ROC AUC, precision, recall, et cetera, but only one metric determines my position on the leaderboard.

----------------

# Step 2: Encode Categorical Features & Rescore

My next step was to iterate my baseline model: increase the score by encoding textual features as numerals. First I obtained a ranking of all categorical features sorted by their "cardinality"- the count of unique values. After that, I decided to try One Hot Encoding some of the very low cardinality features, and they had to be low cardinality because On Hot Encoding generates new columns for every unique value in a feature.


```python
X_train_encoded.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>recorded_by_1</th>
      <th>public_meeting_1</th>
      <th>public_meeting_2</th>
      <th>public_meeting_3</th>
      <th>permit_1</th>
      <th>permit_2</th>
      <th>permit_3</th>
      <th>source_1</th>
      <th>source_2</th>
      <th>source_3</th>
      <th>source_4</th>
      <th>source_5</th>
      <th>source_6</th>
      <th>source_7</th>
      <th>source_8</th>
      <th>source_9</th>
      <th>source_10</th>
      <th>management_group_1</th>
      <th>management_group_2</th>
      <th>management_group_3</th>
      <th>management_group_4</th>
      <th>management_group_5</th>
      <th>quantity_1</th>
      <th>quantity_2</th>
      <th>quantity_3</th>
      <th>quantity_4</th>
      <th>quantity_5</th>
      <th>waterpoint_type_1</th>
      <th>waterpoint_type_2</th>
      <th>waterpoint_type_3</th>
      <th>waterpoint_type_4</th>
      <th>waterpoint_type_5</th>
      <th>waterpoint_type_6</th>
      <th>waterpoint_type_7</th>
      <th>payment_1</th>
      <th>payment_2</th>
      <th>payment_3</th>
      <th>payment_4</th>
      <th>payment_5</th>
      <th>payment_6</th>
      <th>payment_7</th>
      <th>basin_1</th>
      <th>basin_2</th>
      <th>basin_3</th>
      <th>basin_4</th>
      <th>basin_5</th>
      <th>basin_6</th>
      <th>basin_7</th>
      <th>basin_8</th>
      <th>basin_9</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>gps_height</th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5974</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-3.353465</td>
      <td>36.667568</td>
      <td>1410</td>
      <td>200</td>
    </tr>
    <tr>
      <th>31787</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-2.139160</td>
      <td>33.637501</td>
      <td>1161</td>
      <td>400</td>
    </tr>
    <tr>
      <th>27800</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-2.977996</td>
      <td>36.854557</td>
      <td>1184</td>
      <td>550</td>
    </tr>
    <tr>
      <th>8008</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-2.933736</td>
      <td>33.023974</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>39725</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-5.674631</td>
      <td>36.528004</td>
      <td>1331</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



After this process it was time to rescore... There are many many ways to encode categorical features, and I was hoping that One Hot was a viable method. I used the library `category_encoders` for this step, not `scikit-learn` for reference. Along with the encoded features I also appended the top numeric features from the first model fit.

Outcome: 74.3% accuracy! Getting there... I included a `classification_report` to be able to get more of a sense of the predictive power of the model at this point.


```python
clf = DecisionTreeClassifier(random_state=42)

clf.fit(X_train_encoded, y_train)
clf.score(X_val_encoded, y_val)
```




    0.7436026936026936




```python
y_pred = clf.predict(X_val_encoded)
print(classification_report(y_val, y_pred))
```

                             precision    recall  f1-score   support
    
                 functional       0.79      0.79      0.79      9678
    functional needs repair       0.36      0.37      0.37      1295
             non functional       0.76      0.75      0.75      6847
    
                   accuracy                           0.74     17820
                  macro avg       0.63      0.64      0.64     17820
               weighted avg       0.74      0.74      0.74     17820
    
    

### Step 2 Takeaway

I didn't notice it until it was too late, but `recorded_by` only has one unique value! Of course a feature with only one value is not going to improve the accuracy of any model. The choice of using a One Hot encoding over Label encoding was also slightly arbitrary. In the 'v1' notebook I employed _only_ the LabelEncoder(), so when I started the 'v2' notebook I decided, arbitrarily, to try One Hot and see if there would be an improvement. It seemed to work, but I should've done a back-to-back comparison.

--------------------------

# Step 3: "Wrangle" Data and Fit a Tree Ensemble Model

Okay, now its time to bring out the big guns! Most Kaggle competitions are won using some variation of ensemble methods, and XGBoost is absolutely one of the most popular options for a tree ensemble machine learning model. Before I got to the data wrangling I wanted to try fitting an XGBoost Classifier on the `X_train_encoded` DataFrame I had constructed.

About the classifier parameters: I used sklearn's `RandomizedSearchCV` class to find (hopefully) a set of optimal hyperparameters for the XGB classifier, hence why the model is called 'best'. This was an iterative process on my end in that as I noticed where optimal values for a certain hyperparameter might be I would tighten the search space. If you'd like to check out the notebooks that have all of this code, not just the snippets, I've linked  to the repository so just click the Octocat at the top of the page to navigate to the notebooks.


```python
y_pred = best.predict(X_val_encoded)
print("Accuracy score: ", accuracy_score(y_val, y_pred))
print(classification_report(y_val, y_pred))
```

    Accuracy score:  0.7873737373737374
                             precision    recall  f1-score   support
    
                 functional       0.79      0.88      0.83      9678
    functional needs repair       0.52      0.30      0.38      1295
             non functional       0.82      0.75      0.78      6847
    
                   accuracy                           0.79     17820
                  macro avg       0.71      0.64      0.66     17820
               weighted avg       0.78      0.79      0.78     17820
    
    

The highest scores for this dataset tend to be around 81% to high 82% accuracy. With minimal data cleaning and feature engineering I'd managed to get to 78.7%, so I was quite excited about data wrangling and getting to the top of the leaderboard. I could smell victory! Oh what sad, sad surprises remained.

Instead of pasting in the code cell of my `wrangle()` function I'll write out the high level steps so make the process more generally understandable. `wrangle()` takes in a DataFrame and returns a modified copy of it following these steps:

1. Impute `np.NaN` Not-a-Number values via "padding". Impute with values that already exist in that column/feature.
2. Bin the latitude and longitude features by rounding them to a specified number of significant digits. (Lower cardinality)
3. Drop all redundant features that contain essentially the same data as other columns. 
4. Convert the remaining categoricals with Label Encoding.
5. Convert the categorical feature `date_recorded` into Unix time, which turned out to one of the most helpful feature engineering steps besides encoding.

How does XGBoost perform now? Well, predictions on the validation set were 80.2% accurate- Fantastic! 


```python
y_pred = best.predict(X_val)
print("Accuracy score: ", accuracy_score(y_val, y_pred))
print(classification_report(y_val, y_pred))
```

    Accuracy score:  0.8029180695847362
                             precision    recall  f1-score   support
    
                 functional       0.80      0.89      0.84      9678
    functional needs repair       0.56      0.32      0.40      1295
             non functional       0.84      0.77      0.80      6847
    
                   accuracy                           0.80     17820
                  macro avg       0.73      0.66      0.68     17820
               weighted avg       0.80      0.80      0.80     17820
    
    

### Step 3 Takeaway

Not so fast. That score is for a small piece of data, which was also a part of the same dataset that the model was trained on. While yes, the X_val set was heldout for testing, so the model isn't 'brain damaged', but it very well could be overfit to the training set. When I submitted this model to Kaggle it turned out to be my best model, coming in at 78.7% on the public leaderboard.

In the v1 notebook my wrangle function is much larger than the v2 function. My original idea was to get all observations with NaNs and impute with a random choice of the top 50 or 100 values for that given feature. I used `np.random.choice()` on a predefined list of those top values for each feature, but it backfired. The random choice function chose the same value over and over for each instance of NaN, so my data ended up being wildly skewed unusable and unsatisfactory for training a model. My scores dropped considerably and I couldn't figure out why for quite a while! The change in v2 lies in the single use of the `fillna()` function, where I specify the method of imputation to be 'padding'. This essentially is what I was trying to do, where values that already exist in the feature are used to impute NaNs. 

Also, the amount of rounding of the latitude and longitude for binning purposes had a large impact on the final importances according to the model's `feature_importances_` attribute. Maybe there's a sweet spot- A middle ground, or maybe there's a better way to perform binning for coordinate data.

----------------

# Step 4: The Secret Weapon - `featuretools`

The day before this competition started I discovered featuretools: a library for automatically generating features for a dataset. I thought I'd be crafty and use it to generate a huge amount of features while my cohorts toiled away deliberating over how to engineer just a few of features by hand. I thought... The attempt at using featuretools only exists in the v1 notebook, because surprise, it was not a magic bullet and didn't help my model at all. This is not to the fault of featuretools, though. Its a very powerful tool when used properly and **with the right kind of data**. From what I understand now featuretools works much better when the set of features is represented by multiple separate DataFrames, where the more relationships that exist between them allows featuretools to generate more and more interesting and complex features based on these relationships. I only had a features DataFrame and a labels DataFrame, with only one relationship. 

I ran this experiment twice, messing with the number of numeric features used and the calculations used. The first pass generated 155(!) features and the second pass generated 37 features. Both offered no increase in accuracy to the XGB classifier. Below are a couple visualizations of this phenomenon.

No generated features had much of an impact of the feature "importances" to XGBoost. I use importances in quotes because its somewhat of a simplified explanation of Shapley values, though it works well for interpreting this chart.


```python
# Plot Shapley values for the top features.
explainer = shap.TreeExplainer(best)
shap_values = explainer.shap_values(X_test, y_test)
shap.summary_plot(shap_values, X_test);
```


![png](output_39_0.png)


Here's the actual list of feature importances including the featuretools features.


```python
pd.Series(best.feature_importances_, X_train.columns).sort_values(ascending=False)
```




    gps_height                   0.144067
    longitude                    0.143900
    ward                         0.121120
    population                   0.090871
    installer                    0.081108
    construction_year            0.062750
    lga                          0.051610
    extraction_type              0.036006
    quantity                     0.028079
    source                       0.026869
    amount_tsh                   0.026827
    waterpoint_type              0.026535
    payment_type                 0.024449
    district_code                0.023448
    region_code                  0.020861
    scheme_management            0.019568
    basin                        0.018107
    management_group             0.010472
    latitude                     0.009846
    water_quality                0.008470
    permit                       0.008386
    source_class                 0.006676
    quality_group                0.005257
    public_meeting               0.004715
    MEAN(features.population)    0.000000
    MEAN(features.ward)          0.000000
    SUM(features.latitude)       0.000000
    MEAN(features.gps_height)    0.000000
    MEAN(features.quantity)      0.000000
    MEAN(features.longitude)     0.000000
    MEAN(features.latitude)      0.000000
    MIN(features.population)     0.000000
    MIN(features.ward)           0.000000
    MIN(features.gps_height)     0.000000
    MIN(features.quantity)       0.000000
    MIN(features.longitude)      0.000000
    MIN(features.latitude)       0.000000
    MAX(features.population)     0.000000
    MAX(features.ward)           0.000000
    MAX(features.gps_height)     0.000000
    MAX(features.quantity)       0.000000
    MAX(features.longitude)      0.000000
    MAX(features.latitude)       0.000000
    SUM(features.population)     0.000000
    SUM(features.ward)           0.000000
    SUM(features.gps_height)     0.000000
    SUM(features.quantity)       0.000000
    SUM(features.longitude)      0.000000
    gps_height_binned            0.000000
    dtype: float32



--------------------------------

# Step 5: Rinse and Repeat - Model Development is Iterative

What now? The featuretools trick that I had up my sleeve was a bust. I can't hit 80% on the leaderboard with the models I've produced thus far. Well, from everything I've learned so far it seems that the best Kaggle scores come from great feature engineering, which tends to come from having some amount of domain expertise. Exploratory data analysis seemed like the best route to try to tease out patterns that may be laying unnoticed in the data. There's not a single feature that truly stands above the rest in terms of importance/weight, but I can try to make some interaction and dependence plots. I wouldn't call it data leakage, but 'quantity' is a categorical feature indicating how much water is available at a given water pump- might it have a pattern associated with functional status of pump? Below is an assortment of visualizations that I used during this project to help me understand the data more clearly.

**Feature Importance**


```python
# Create DataFrame of features and importances
importances = pd.DataFrame(
    {"feature": X_train.columns.tolist(), "importance": best.feature_importances_}
)

# Create bar chart object
bars = (
    alt.Chart(importances)
    .mark_bar()
    .encode(
        alt.X("importance:Q", title="Importance"),
        alt.Y(
            "feature:N",
            title="Feature",
            sort=alt.EncodingSortField(field="importance", op="sum", order="ascending"),
        ),
    )
)

# Plot
bars.properties(height=400)
```




![png](output_46_0.png)



**Partial Dependence Plot of `gps_height`**

How the value of `gps_height` and the probability of a prediction for each class: functional, non-functional, functional needs repair.


```python
pdp_gps_height = pdp.pdp_isolate(
    model=best, dataset=X_val, model_features=X_val.columns, feature='gps_height'
)

fig, axes = pdp.pdp_plot(
    pdp_isolate_out=pdp_gps_height, feature_name='gps_height', center=True, x_quantile=True, 
    ncols=3, plot_lines=True, frac_to_plot=100
)
```


![png](output_48_0.png)


**Quantity Type Bar Chart**

Quantity type 0 consists almost entirely of non-functional water pumps. 


```python
alt.Chart(merged[:4999]).mark_bar().encode(
    alt.X('quantity', title='Water Quantity Level'),
    alt.Y('count(quantity)', title='Water Quality Count'),
    color='status_group',
)
```




![png](output_50_0.png)



**Quantity and GPS Height Interaction Plot**

This describes, for each water pump class, what the "confidence" of a prediction is at various values of the two features.


```python
from pdpbox.pdp import pdp_interact, pdp_interact_plot 

features = ['quantity', 'gps_height']
interaction = pdp_interact(
                model=pdp_model,
                dataset=X_val.loc[X_val['latitude']!=0][:2000],
                model_features=X_val.columns,
                features=features,
)

pdp_interact_plot(interaction, plot_type='grid', feature_names=features);
```


![png](output_52_0.png)


**Latitude/Longitude Plot on Cartesian Grid**

There exist pockets of mostly-functional water pumps and pockets of mostly non-functional water pumps. Water pumps do have a harder time at higher altitudes, so there might be an interaction there.


```python
alt.Chart(merged.loc[merged['latitude']!=0][:2000]).mark_circle().encode(
    alt.X('longitude:Q', title='Longitude', scale=alt.Scale(domain=(30,40), clamp=True)),
    alt.Y('latitude:Q', title='Latitude'),
    color='status_group:N',
    # size='population:Q',
    tooltip=['population', 'status_group', 'quantity']
)
```




![png](output_54_0.png)


