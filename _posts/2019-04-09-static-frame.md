---
layout: post
title: What's StaticFrame?
subtitle: Introducing a Pandas alternative with a pragmatic twist
gh-repo: beverast/beverast.github.io
gh-badge: [star, fork, follow]
bigimg: /img/sf-icon.png
tags: [open source, library, python, data science]
keywords: staticframe, python, data science, pandas, functional python, numpy, immutable pandas, dataframe, immutability, functional programming, FP, open source, Talk Python to Me, staticframe tutorial, data, data analytics, beverast, jekyll, jupyter, jupyter notebook
comments: true
---
### Key Features
StaticFrame is neither a clone of Pandas nor a replacement. StaticFrame is a library _similar_ to Pandas with strong design decisions focusing on uniformity, explicit behavior, immutability, and less redundancy in functionality. At it's core StaticFrame is a library that implements efficient and powerful data structures, while not trying to do it all- it doesn't offer any visualization tooling for example. Pandas is not a requirement for StaticFrame, but NumPy is because NumPy offers a lot in terms of stability, data structures, and flat out speed for operations.  

### Resources

[StaticFrame on Github](https://github.com/InvestmentSystems/static-frame)

[StaticFrame docs](http://static-frame.readthedocs.io/)

[Christopher Ariza on Talk Python to Me](https://talkpython.fm/episodes/show/204/staticframe-like-pandas-but-safer)

Here's a simple code-oriented Jupyter Notebook highlighting the high level features and behaviors of StaticFrame.


```python
import pandas as pd
import static_frame as sf
```

Utilize `pd.read_csv()` then construct the `sf.Frame` object because Pandas has a very performant implementation.


```python
# Data source: http://flossdata.syr.edu/data/apache/2016/2016-May/
frame = sf.Frame.from_pandas(pd.read_csv('http://flossdata.syr.edu/data/apache/2016/2016-May/apache_people_projects.csv'))
# sf.Frame has many similar helper functions
frame.head(10)
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Frame&gt;</span></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th>svn_id</th><th>real_name</th><th>web_site</th><th>datasource_id</th><th>project_name</th><th>role_on_project</th><th>details</th><th>email</th><th>organization</th><th>timezone</th><th>last_updated</th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><th>0</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>apsite</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>1</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>cayenne</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>2</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>cayenne-pmc</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>3</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>click</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>4</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>incubator</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>5</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>incubator-pmc</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>6</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>member</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>7</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>pmc-chairs</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>8</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>wave</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr><tr><th>9</th><td>aadomowski</td><td>Aleksander Adamowski</td><td>nan</td><td>351</td><td>directory</td><td>Committer</td><td>nan</td><td>nan</td><td>nan</td><td>nan</td><td>2012-12-20 18:01:30</td></tr></tbody></table>



Columns must be accessed using bracket notation as StaticFrame doesn't allow dot notation access.

This is a design decision to reduce naming collision with other Frame attributes, and improve consistency.


```python
print('frame size: ', frame.size)
# shape returns tuple of (# of rows, # of cols)
print('frame shape:', frame.shape)
print('frame dimensions:', frame.ndim)
```

    frame size:  525514
    frame shape: (47774, 11)
    frame dimensions: 2
    

Frame objects have heterogeneous column types; each column must be of exactly one type.

These types are directly inherited from NumPy, and StaticFrame uses all NumPy types available.


```python
frame['last_updated'].dtype
```




    dtype('O')




```python
frame['datasource_id'].dtype
```




    dtype('int64')




```python
# Note: You must use dtype for a single column and dtypes for multiple columns or an entire Frame object.
frame.dtypes
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>svn_id</th><td>object</td></tr><tr><th>real_name</th><td>object</td></tr><tr><th>web_site</th><td>object</td></tr><tr><th>datasource_id</th><td>int64</td></tr><tr><th>project_name</th><td>object</td></tr><tr><th>role_on_project</th><td>object</td></tr><tr><th>details</th><td>object</td></tr><tr><th>email</th><td>object</td></tr><tr><th>organization</th><td>object</td></tr><tr><th>timezone</th><td>float64</td></tr><tr><th>last_updated</th><td>object</td></tr></tbody></table>




```python
# More utility functions
frame['datasource_id'].unique()
```




    array([  123,   351,   352,   353,   354,   355,   356,   357,   358,
             360,   361,   362,   363,   364,   365,   366,   367,   368,
             369,  1578,  1579,  1580,  1581,  1582,  1583,  1584,  1585,
           65935], dtype=int64)




```python
# Calculate percentage of np.NaN values in the 'timezone' column
frame['timezone'].isna().sum() / frame['timezone'].size
```




    0.9885711893498556




```python
frame['project_name'].isna().any()
```




    True



Frame objects expose a base `__getitem__` interface: StaticFrame uses 1 parameter for `.loc[]` and `.iloc[]` to select by ROW.

Use 2 parameters for `.loc[]` and `.iloc[]` to select by ROW and COLUMN.


```python
frame['real_name'].tail()
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>47769</th><td>Alex Shraer</td></tr><tr><th>47770</th><td>Thawan Kooburat</td></tr><tr><th>47771</th><td>Rakesh R</td></tr><tr><th>47772</th><td>Hongchao Deng</td></tr><tr><th>47773</th><td>Raul Gutierrez Segales</td></tr></tbody></table>




```python
frame['svn_id':'project_name'].head()
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Frame&gt;</span></th><th></th><th></th><th></th><th></th><th></th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th>svn_id</th><th>real_name</th><th>web_site</th><th>datasource_id</th><th>project_name</th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><th>0</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>apsite</td></tr><tr><th>1</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>cayenne</td></tr><tr><th>2</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>cayenne-pmc</td></tr><tr><th>3</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>click</td></tr><tr><th>4</th><td>aadamchik</td><td>Andrus Adamchik</td><td>nan</td><td>351</td><td>incubator</td></tr></tbody></table>




```python
# .loc[] returns a Series object of a given ROW
frame.loc[60]
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>svn_id</th><td>acmurthy</td></tr><tr><th>real_name</th><td>Arun Murthy</td></tr><tr><th>web_site</th><td>nan</td></tr><tr><th>datasource_id</th><td>351</td></tr><tr><th>project_name</th><td>hadoop</td></tr><tr><th>role_on_project</th><td>Committer</td></tr><tr><th>details</th><td>nan</td></tr><tr><th>email</th><td>nan</td></tr><tr><th>organization</th><td>nan</td></tr><tr><th>timezone</th><td>nan</td></tr><tr><th>last_updated</th><td>2012-12-20 18:01:30</td></tr></tbody></table>




```python
frame.loc[57:69, ['svn_id', 'project_name', 'role_on_project']]
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Frame&gt;</span></th><th></th><th></th><th></th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th>svn_id</th><th>project_name</th><th>role_on_project</th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th></th><th></th><th></th></tr></thead><tbody><tr><th>57</th><td>aching</td><td>pmc-chairs</td><td>Committer</td></tr><tr><th>58</th><td>acmurthy</td><td>ambari</td><td>Committer</td></tr><tr><th>59</th><td>acmurthy</td><td>apsite</td><td>Committer</td></tr><tr><th>60</th><td>acmurthy</td><td>hadoop</td><td>Committer</td></tr><tr><th>61</th><td>acmurthy</td><td>hadoop-hdfs</td><td>Committer</td></tr><tr><th>62</th><td>acmurthy</td><td>hadoop-mapreduce</td><td>Committer</td></tr><tr><th>63</th><td>acmurthy</td><td>hadoop-pmc</td><td>Committer</td></tr><tr><th>64</th><td>acmurthy</td><td>incubator</td><td>Committer</td></tr><tr><th>65</th><td>acmurthy</td><td>incubator-pmc</td><td>Committer</td></tr><tr><th>66</th><td>acmurthy</td><td>member</td><td>Committer</td></tr><tr><th>67</th><td>acmurthy</td><td>pmc-chairs</td><td>Committer</td></tr><tr><th>68</th><td>acmurthy</td><td>s4</td><td>Committer</td></tr><tr><th>69</th><td>aco</td><td>activemq</td><td>Committer</td></tr></tbody></table>



StaticFrame uses immutable NumPy arrays (flag: writeable=False) to populate its frames.

The underlying data doesn't mutate when operated on- a resultant copy of the original Frame, Series, or Index is returned.


```python
test_scores = sf.Series((80,77,98,90,82,79,85,92,90), index=['student1', 'student2', 'student3', 'student4', 'student5',
                                                             'student6', 'student7', 'student8', 'student9'])
test_scores
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>student1</th><td>80</td></tr><tr><th>student2</th><td>77</td></tr><tr><th>student3</th><td>98</td></tr><tr><th>student4</th><td>90</td></tr><tr><th>student5</th><td>82</td></tr><tr><th>student6</th><td>79</td></tr><tr><th>student7</th><td>85</td></tr><tr><th>student8</th><td>92</td></tr><tr><th>student9</th><td>90</td></tr></tbody></table>



Note: All indices in StaticFrame MUST be unique. This is an option in Pandas, but it's flag `verify_integrity` is set to False by default.


```python
test_scores['student3']
```




    98




```python
# Immutability in action
test_scores['student3'] = test_scores['student3'] + 5
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-33-295be77f58cb> in <module>
          1 # Immutability in action
    ----> 2 test_scores['student3'] = test_scores['student3'] + 5
    

    TypeError: 'Series' object does not support item assignment



```python
# Still immutable, no matter which data is queried or how
test_scores.values[2] = 90
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-37-85dda4112a27> in <module>
          1 # Still immutable, no matter which data is queried or how
    ----> 2 test_scores.values[2] = 90
    

    ValueError: assignment destination is read-only



```python
# Standard zero-based indexing
test_scores.values[2]
```




    98




```python
# Mutating values on a copied object
test_scores.assign['student3'](100)
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>student1</th><td>80</td></tr><tr><th>student2</th><td>77</td></tr><tr><th>student3</th><td>100</td></tr><tr><th>student4</th><td>90</td></tr><tr><th>student5</th><td>82</td></tr><tr><th>student6</th><td>79</td></tr><tr><th>student7</th><td>85</td></tr><tr><th>student8</th><td>92</td></tr><tr><th>student9</th><td>90</td></tr></tbody></table>




```python
# Check that test_scores is still unmutated
test_scores['student3']
```




    98




```python
# Make a copy and mutate values along a slice, inclusive indexing
# Note: Frame.assign() also exposes __getitem__ for easy access for operations
test_scores.assign[:'student7'](test_scores[:'student7'] - 5)
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th><span style="color: #777777">&lt;Series&gt;</span></th></tr></thead><tbody><tr><th>student1</th><td>75</td></tr><tr><th>student2</th><td>72</td></tr><tr><th>student3</th><td>93</td></tr><tr><th>student4</th><td>85</td></tr><tr><th>student5</th><td>77</td></tr><tr><th>student6</th><td>74</td></tr><tr><th>student7</th><td>80</td></tr><tr><th>student8</th><td>92</td></tr><tr><th>student9</th><td>90</td></tr></tbody></table>



`FrameGO`: a 2D frame that allows Grow-Only mutation, i.e. add more data but don't modify any existing values.


```python
test_scores_GO = sf.FrameGO(dict(score=(80,82,85,88,90,93), extra_credit=(1,1,0,1,0,1)),
                            index=['student1', 'student2', 'student3', 'student4', 'student5', 'student6'])
test_scores_GO
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;FrameGO&gt;</span></th><th></th><th></th></tr><tr><th><span style="color: #777777">&lt;IndexGO&gt;</span></th><th>score</th><th>extra_credit</th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th></th><th></th></tr></thead><tbody><tr><th>student1</th><td>80</td><td>1</td></tr><tr><th>student2</th><td>82</td><td>1</td></tr><tr><th>student3</th><td>85</td><td>0</td></tr><tr><th>student4</th><td>88</td><td>1</td></tr><tr><th>student5</th><td>90</td><td>0</td></tr><tr><th>student6</th><td>93</td><td>1</td></tr></tbody></table>




```python
original_scores = test_scores_GO['score'].values
adjusted_scores = []

# "There's gotta be a better way"
# If you have loops like this, its generally a good idea to look in 
# the NumPy layer for a more performant method. Shame on me!

# Populate a list and append as a new column
for idx, value in enumerate(test_scores_GO['extra_credit'].values):
    if value:
        adjusted_scores.append(original_scores[idx]+5)
    else:
        adjusted_scores.append(original_scores[idx])

test_scores_GO['adjusted_scores'] = adjusted_scores
test_scores_GO
```




<table border="1"><thead><tr><th><span style="color: #777777">&lt;FrameGO&gt;</span></th><th></th><th></th><th></th></tr><tr><th><span style="color: #777777">&lt;IndexGO&gt;</span></th><th>score</th><th>extra_credit</th><th>adjusted_scores</th></tr><tr><th><span style="color: #777777">&lt;Index&gt;</span></th><th></th><th></th><th></th></tr></thead><tbody><tr><th>student1</th><td>80</td><td>1</td><td>85</td></tr><tr><th>student2</th><td>82</td><td>1</td><td>87</td></tr><tr><th>student3</th><td>85</td><td>0</td><td>85</td></tr><tr><th>student4</th><td>88</td><td>1</td><td>93</td></tr><tr><th>student5</th><td>90</td><td>0</td><td>90</td></tr><tr><th>student6</th><td>93</td><td>1</td><td>98</td></tr></tbody></table>


