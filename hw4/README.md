

```python
#import dependencies
import pandas as pd
import numpy as np
```


```python
file = "raw_data/students_complete.csv"
df1 = pd.read_csv(file)
df1.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
file = "raw_data/schools_complete.csv"
df2 = pd.read_csv(file)
df2 = df2.rename(index=str,columns={"name":"school"})
df2.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
merge_table = pd.merge(df1,df2,on="school",how="left")
merge_table

school = np.unique(merge_table['school'])
school

#Count of Total Students
totalstudents = merge_table.groupby("type",as_index=False)["Student ID"].count()
totalstudentsdf = pd.DataFrame(totalstudents)
totalstudentsdf = totalstudentsdf.rename(index=str,columns={'Student ID':'Number of Students'})
totalstudentsdf

#Count Distinct Total School IDs
totalschools = merge_table.groupby("type",as_index = False)["School ID"].nunique()
totalschoolsdf = pd.DataFrame(totalschools)
totalschoolsdf = totalschoolsdf.rename(index=str,columns={'School ID':'Number of Schools'})
totalschoolsdf.insert(loc=0,column='type',value=['Charter','District'])
totalschoolsdf

totalbudget = df2.groupby("type",as_index = False)["budget"].sum()
totalbudgetdf = pd.DataFrame(totalbudget)
totalbudgetdf = totalbudgetdf.rename(index = str,columns={'budget':'total_budget'})
totalbudgetdf

averagemath = merge_table.groupby("type",as_index = False)["math_score"].mean()
averagemathdf = pd.DataFrame(averagemath)
averagemathdf = averagemathdf.rename(index = str,columns={'math_score':'average_math_score'})
averagemathdf

averagereading = merge_table.groupby("type",as_index = False)["reading_score"].mean()
averagereadingdf = pd.DataFrame(averagereading)
averagereadingdf = averagereadingdf.rename(index=str,columns={'reading_score':'average_reading_score'})
averagereadingdf

#Set 70% as Passrate
passed_math = merge_table.query('math_score>=70')
passed_reading = merge_table.query('reading_score>=70')
passed_math_count = passed_math.groupby("type",as_index = False)["math_score"].count()
passed_math_countdf = pd.DataFrame(passed_math_count)
passed_math_countdf 

passed_reading_count = passed_reading.groupby("type",as_index = False)["reading_score"].count()
passed_reading_countdf = pd.DataFrame(passed_reading_count)
passed_reading_countdf

#total_math_count
total_math_count = merge_table.groupby("type",as_index = False)["math_score"].count()
total_math_countdf = pd.DataFrame(total_math_count)
total_math_countdf = total_math_countdf.rename(index=str,columns={'math_score':'Total_Math_Count'})
total_math_countdf

#total_reading_count
total_reading_count = merge_table.groupby("type",as_index = False)["reading_score"].count()
total_reading_countdf = pd.DataFrame(total_reading_count)
total_reading_countdf = total_reading_countdf.rename(index=str,columns={'reading_score':'Total_Reading_Count'})
total_reading_countdf

#Pass Rate
percent_passing_math = (passed_math_count['math_score'] /  total_math_count['math_score'])*100
percent_passing_mathdf = pd.DataFrame(percent_passing_math)
percent_passing_mathdf.insert(loc=0,column='type',value=['Charter','District'])
percent_passing_mathdf = percent_passing_mathdf.rename(index=str,columns ={'math_score':'Percent_Passing_Math'})
percent_passing_mathdf
percent_passing_reading = (passed_reading_count['reading_score'] / total_reading_count['reading_score']) * 100
percent_passing_readingdf = pd.DataFrame(percent_passing_reading,columns = {'type','reading_score'})
percent_passing_readingdf = percent_passing_readingdf.rename(index=str,columns = {'reading_score':'Percent_Passing_Reading'})
percent_passing_readingdf = percent_passing_readingdf.set_value('1','type','District')
percent_passing_readingdf = percent_passing_readingdf.set_value('0','type','Charter')
percent_passing_readingdf
Overall_Passing_Rate = (percent_passing_mathdf['Percent_Passing_Math'] + percent_passing_readingdf['Percent_Passing_Reading']) / 2
Overall_Passing_Ratedf = pd.DataFrame(Overall_Passing_Rate,columns = {'Overall_Passing_Rate'})
Overall_Passing_Ratedf.insert(loc=0,column='type',value=['Charter','District'])
Overall_Passing_Ratedf

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Charter</td>
      <td>95.173856</td>
    </tr>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>73.711818</td>
    </tr>
  </tbody>
</table>
</div>



#total count math
total_math_count =merge_table.groupby("type",as_index = False)["math_score"].count()
total_math_countdf = pd.DataFrame(total_math_count)
total_math_countdf


```python

                        

```


```python
                                                        
                                                                        
```


```python
#District Summary Snapshot

dmerge1 = pd.merge(totalschoolsdf,totalstudentsdf,on = "type",how="left")
dmerge2 = pd.merge(dmerge1,totalbudgetdf,on="type",how="left")
dmerge3 = pd.merge(dmerge2,averagemathdf,on="type",how="left")
dmerge4 = pd.merge(dmerge3,averagereadingdf,on="type",how="left")
dmerge5 = pd.merge(dmerge4,percent_passing_mathdf,on="type",how="left")
dmerge6 = pd.merge(dmerge5,percent_passing_readingdf,on="type",how="left")
dmerge7 = pd.merge(dmerge6,Overall_Passing_Ratedf,on="type",how="left")
dmerge7 = dmerge7.drop(dmerge7.index[0])
dmerge7
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Number of Schools</th>
      <th>Number of Students</th>
      <th>total_budget</th>
      <th>average_math_score</th>
      <th>average_reading_score</th>
      <th>Percent_Passing_Math</th>
      <th>Percent_Passing_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>7</td>
      <td>26976</td>
      <td>17347923</td>
      <td>76.987026</td>
      <td>80.962485</td>
      <td>66.518387</td>
      <td>80.905249</td>
      <td>73.711818</td>
    </tr>
  </tbody>
</table>
</div>




```python
#School Metrics
Student_Count = merge_table.groupby(["School ID","school"],as_index = False)["Student ID"].count()
Student_Count = Student_Count.rename(index=str,columns = {'Student ID':'Total Students'})

school_total_students = Student_Count['Total Students']
school_total_students = pd.to_numeric(school_total_students,errors='coerce',downcast='float').tolist()
school_total_students

```




    [2917.0,
     2949.0,
     1761.0,
     4635.0,
     1468.0,
     2283.0,
     1858.0,
     4976.0,
     427.0,
     962.0,
     1800.0,
     3999.0,
     4761.0,
     2739.0,
     1635.0]




```python
Student_Budget = df2.groupby(["School ID","school","type"],as_index = False)["budget"].sum()
Student_Budget = Student_Budget.rename(index=str,columns = {'budget':'Total School Budget'})
Student_Budget['Total_Students'] = school_total_students
Student_Budget
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
      <td>2917.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
      <td>2949.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1056600</td>
      <td>1761.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>3022020</td>
      <td>4635.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
      <td>1468.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
      <td>2283.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
      <td>1858.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bailey High School</td>
      <td>District</td>
      <td>3124928</td>
      <td>4976.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>248087</td>
      <td>427.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
      <td>962.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1049400</td>
      <td>1800.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
      <td>3999.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
      <td>4761.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
      <td>2739.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
      <td>1635.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
Per_Student_Budget = Student_Budget['Total School Budget'] / Student_Count['Total Students']
Per_Student_Budget = pd.DataFrame(Per_Student_Budget)
Per_Student_Budget = Student_Budget.join(Per_Student_Budget,how="outer")
Per_Student_Budget = Per_Student_Budget.rename(index = str,columns = {0:"Per Student Budget"})
```


```python
AverageReadingScore = merge_table.groupby(["School ID","school"],as_index = False)["reading_score"].mean()
AverageReadingScore = AverageReadingScore[['School ID','reading_score']]
AverageReadingScore
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>reading_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>81.182722</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>81.158020</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>83.725724</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>80.934412</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>83.816757</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>83.989488</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>83.975780</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>81.033963</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>83.814988</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>84.044699</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>83.955000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>80.744686</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>80.966394</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>80.746258</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>83.848930</td>
    </tr>
  </tbody>
</table>
</div>




```python
AverageMathScore = merge_table.groupby(["School ID","school","type"],as_index = False)["math_score"].mean()
AverageMathScore = AverageMathScore[['School ID','math_score']]
Per_Student_Budget = pd.merge(Per_Student_Budget,AverageReadingScore,on="School ID",how = "left")
Per_Student_Budget = Per_Student_Budget.rename(index=str,columns={'reading_score':'Average_Reading_Score'})
Per_Student_Budget
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
      <th>Per Student Budget</th>
      <th>Average_Reading_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
      <td>2917.0</td>
      <td>655.0</td>
      <td>81.182722</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
      <td>2949.0</td>
      <td>639.0</td>
      <td>81.158020</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1056600</td>
      <td>1761.0</td>
      <td>600.0</td>
      <td>83.725724</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>3022020</td>
      <td>4635.0</td>
      <td>652.0</td>
      <td>80.934412</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
      <td>1468.0</td>
      <td>625.0</td>
      <td>83.816757</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
      <td>2283.0</td>
      <td>578.0</td>
      <td>83.989488</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
      <td>1858.0</td>
      <td>582.0</td>
      <td>83.975780</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bailey High School</td>
      <td>District</td>
      <td>3124928</td>
      <td>4976.0</td>
      <td>628.0</td>
      <td>81.033963</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>248087</td>
      <td>427.0</td>
      <td>581.0</td>
      <td>83.814988</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
      <td>962.0</td>
      <td>609.0</td>
      <td>84.044699</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1049400</td>
      <td>1800.0</td>
      <td>583.0</td>
      <td>83.955000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
      <td>3999.0</td>
      <td>637.0</td>
      <td>80.744686</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
      <td>4761.0</td>
      <td>650.0</td>
      <td>80.966394</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
      <td>2739.0</td>
      <td>644.0</td>
      <td>80.746258</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
      <td>1635.0</td>
      <td>638.0</td>
      <td>83.848930</td>
    </tr>
  </tbody>
</table>
</div>




```python
Score = pd.merge(Per_Student_Budget,AverageMathScore,on="School ID",how = "left")
Score = Score.rename(index=str,columns={'math_score':'Average_Math_Score'})
Score
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
      <th>Per Student Budget</th>
      <th>Average_Reading_Score</th>
      <th>Average_Math_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
      <td>2917.0</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
      <td>2949.0</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1056600</td>
      <td>1761.0</td>
      <td>600.0</td>
      <td>83.725724</td>
      <td>83.359455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>3022020</td>
      <td>4635.0</td>
      <td>652.0</td>
      <td>80.934412</td>
      <td>77.289752</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
      <td>1468.0</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
      <td>2283.0</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
      <td>1858.0</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bailey High School</td>
      <td>District</td>
      <td>3124928</td>
      <td>4976.0</td>
      <td>628.0</td>
      <td>81.033963</td>
      <td>77.048432</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>248087</td>
      <td>427.0</td>
      <td>581.0</td>
      <td>83.814988</td>
      <td>83.803279</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
      <td>962.0</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1049400</td>
      <td>1800.0</td>
      <td>583.0</td>
      <td>83.955000</td>
      <td>83.682222</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
      <td>3999.0</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
      <td>4761.0</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
      <td>2739.0</td>
      <td>644.0</td>
      <td>80.746258</td>
      <td>77.102592</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
      <td>1635.0</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
    </tr>
  </tbody>
</table>
</div>




```python
school_passed_math = passed_math.groupby("School ID",as_index=False)["math_score"].count()
school_passed_reading = passed_reading.groupby("School ID",as_index=False)["reading_score"].count()
school_passed_reading = school_passed_reading['reading_score']
school_passed_math = school_passed_math['math_score']
school_passed_math = pd.to_numeric(school_passed_math,errors='coerce',downcast='float').tolist()
school_passed_math
percent_passed_math = np.array(school_passed_math) / np.array(school_total_students)
percent_passed_math
percent_passed_reading = np.array(school_passed_reading) / np.array(school_total_students)
percent_passed_reading



```




    array([ 0.81316421,  0.80739234,  0.95854628,  0.80862999,  0.97138965,
            0.96539641,  0.97039828,  0.8193328 ,  0.96252927,  0.95945946,
            0.96611111,  0.80220055,  0.81222432,  0.79299014,  0.97308869])




```python
#School Summary
Score['Percent_Passed_Math'] = percent_passed_math*100
Score['Percent_Passed_Reading'] = percent_passed_reading*100
Score['Overall_Passing_Rate'] = (percent_passed_math + percent_passed_reading) / 2 * 100
Score = Score.sort_values('Overall_Passing_Rate')
Score

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
      <th>Per Student Budget</th>
      <th>Average_Reading_Score</th>
      <th>Average_Math_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
      <td>3999.0</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
      <td>2949.0</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
      <td>2917.0</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
      <td>4761.0</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
      <td>2739.0</td>
      <td>644.0</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>3022020</td>
      <td>4635.0</td>
      <td>652.0</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bailey High School</td>
      <td>District</td>
      <td>3124928</td>
      <td>4976.0</td>
      <td>628.0</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>248087</td>
      <td>427.0</td>
      <td>581.0</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1056600</td>
      <td>1761.0</td>
      <td>600.0</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1049400</td>
      <td>1800.0</td>
      <td>583.0</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
      <td>2283.0</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
      <td>1468.0</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
      <td>962.0</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
      <td>1635.0</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
      <td>1858.0</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top 5 Performing Schools
Top_5 = Score.tail(5)
Top_5
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
      <th>Per Student Budget</th>
      <th>Average_Reading_Score</th>
      <th>Average_Math_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
      <td>2283.0</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
      <td>1468.0</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
      <td>962.0</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
      <td>1635.0</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
      <td>1858.0</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Bottom 5 Performing Schools
Bottom_5 = Score.head(5)
Bottom_5
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total School Budget</th>
      <th>Total_Students</th>
      <th>Per Student Budget</th>
      <th>Average_Reading_Score</th>
      <th>Average_Math_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
      <td>3999.0</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
      <td>2949.0</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
      <td>2917.0</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
      <td>4761.0</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
      <td>2739.0</td>
      <td>644.0</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Math Scores By Grade By School
MathScoreByGrade = merge_table.groupby(["grade","school"],as_index=False)["math_score"].mean()
MathScoreByGrade = MathScoreByGrade.rename(index = str,columns={"math_score":"Average_Math_Score"})
MathScoreByGrade
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>grade</th>
      <th>school</th>
      <th>Average_Math_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10th</td>
      <td>Bailey High School</td>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10th</td>
      <td>Cabrera High School</td>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10th</td>
      <td>Figueroa High School</td>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10th</td>
      <td>Ford High School</td>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10th</td>
      <td>Griffin High School</td>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10th</td>
      <td>Hernandez High School</td>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10th</td>
      <td>Holden High School</td>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10th</td>
      <td>Huang High School</td>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10th</td>
      <td>Johnson High School</td>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10th</td>
      <td>Pena High School</td>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10th</td>
      <td>Rodriguez High School</td>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10th</td>
      <td>Shelton High School</td>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10th</td>
      <td>Thomas High School</td>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>13</th>
      <td>10th</td>
      <td>Wilson High School</td>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>14</th>
      <td>10th</td>
      <td>Wright High School</td>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>15</th>
      <td>11th</td>
      <td>Bailey High School</td>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11th</td>
      <td>Cabrera High School</td>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>17</th>
      <td>11th</td>
      <td>Figueroa High School</td>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>18</th>
      <td>11th</td>
      <td>Ford High School</td>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>19</th>
      <td>11th</td>
      <td>Griffin High School</td>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>20</th>
      <td>11th</td>
      <td>Hernandez High School</td>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>21</th>
      <td>11th</td>
      <td>Holden High School</td>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>11th</td>
      <td>Huang High School</td>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>23</th>
      <td>11th</td>
      <td>Johnson High School</td>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>24</th>
      <td>11th</td>
      <td>Pena High School</td>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>25</th>
      <td>11th</td>
      <td>Rodriguez High School</td>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>26</th>
      <td>11th</td>
      <td>Shelton High School</td>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>27</th>
      <td>11th</td>
      <td>Thomas High School</td>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>28</th>
      <td>11th</td>
      <td>Wilson High School</td>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>29</th>
      <td>11th</td>
      <td>Wright High School</td>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>30</th>
      <td>12th</td>
      <td>Bailey High School</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>31</th>
      <td>12th</td>
      <td>Cabrera High School</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>32</th>
      <td>12th</td>
      <td>Figueroa High School</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>33</th>
      <td>12th</td>
      <td>Ford High School</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>34</th>
      <td>12th</td>
      <td>Griffin High School</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>35</th>
      <td>12th</td>
      <td>Hernandez High School</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>36</th>
      <td>12th</td>
      <td>Holden High School</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>37</th>
      <td>12th</td>
      <td>Huang High School</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>38</th>
      <td>12th</td>
      <td>Johnson High School</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>39</th>
      <td>12th</td>
      <td>Pena High School</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>40</th>
      <td>12th</td>
      <td>Rodriguez High School</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>41</th>
      <td>12th</td>
      <td>Shelton High School</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>42</th>
      <td>12th</td>
      <td>Thomas High School</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>43</th>
      <td>12th</td>
      <td>Wilson High School</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>44</th>
      <td>12th</td>
      <td>Wright High School</td>
      <td>83.644986</td>
    </tr>
    <tr>
      <th>45</th>
      <td>9th</td>
      <td>Bailey High School</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>46</th>
      <td>9th</td>
      <td>Cabrera High School</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>47</th>
      <td>9th</td>
      <td>Figueroa High School</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>48</th>
      <td>9th</td>
      <td>Ford High School</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>49</th>
      <td>9th</td>
      <td>Griffin High School</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>50</th>
      <td>9th</td>
      <td>Hernandez High School</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>51</th>
      <td>9th</td>
      <td>Holden High School</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>52</th>
      <td>9th</td>
      <td>Huang High School</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>53</th>
      <td>9th</td>
      <td>Johnson High School</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>54</th>
      <td>9th</td>
      <td>Pena High School</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>55</th>
      <td>9th</td>
      <td>Rodriguez High School</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>56</th>
      <td>9th</td>
      <td>Shelton High School</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>57</th>
      <td>9th</td>
      <td>Thomas High School</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>58</th>
      <td>9th</td>
      <td>Wilson High School</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>59</th>
      <td>9th</td>
      <td>Wright High School</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reading Scores By Grade By School
ReadingScoreByGrade = merge_table.groupby(["grade","school"],as_index=False)["reading_score"].mean()
ReadingScoreByGrade = ReadingScoreByGrade.rename(index = str,columns={"reading_score":"Average_Reading_Score"})
ReadingScoreByGrade
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>grade</th>
      <th>school</th>
      <th>Average_Reading_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10th</td>
      <td>Bailey High School</td>
      <td>80.907183</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10th</td>
      <td>Cabrera High School</td>
      <td>84.253219</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10th</td>
      <td>Figueroa High School</td>
      <td>81.408912</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10th</td>
      <td>Ford High School</td>
      <td>81.262712</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10th</td>
      <td>Griffin High School</td>
      <td>83.706897</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10th</td>
      <td>Hernandez High School</td>
      <td>80.660147</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10th</td>
      <td>Holden High School</td>
      <td>83.324561</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10th</td>
      <td>Huang High School</td>
      <td>81.512386</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10th</td>
      <td>Johnson High School</td>
      <td>80.773431</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10th</td>
      <td>Pena High School</td>
      <td>83.612000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10th</td>
      <td>Rodriguez High School</td>
      <td>80.629808</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10th</td>
      <td>Shelton High School</td>
      <td>83.441964</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10th</td>
      <td>Thomas High School</td>
      <td>84.254157</td>
    </tr>
    <tr>
      <th>13</th>
      <td>10th</td>
      <td>Wilson High School</td>
      <td>84.021452</td>
    </tr>
    <tr>
      <th>14</th>
      <td>10th</td>
      <td>Wright High School</td>
      <td>83.812757</td>
    </tr>
    <tr>
      <th>15</th>
      <td>11th</td>
      <td>Bailey High School</td>
      <td>80.945643</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11th</td>
      <td>Cabrera High School</td>
      <td>83.788382</td>
    </tr>
    <tr>
      <th>17</th>
      <td>11th</td>
      <td>Figueroa High School</td>
      <td>80.640339</td>
    </tr>
    <tr>
      <th>18</th>
      <td>11th</td>
      <td>Ford High School</td>
      <td>80.403642</td>
    </tr>
    <tr>
      <th>19</th>
      <td>11th</td>
      <td>Griffin High School</td>
      <td>84.288089</td>
    </tr>
    <tr>
      <th>20</th>
      <td>11th</td>
      <td>Hernandez High School</td>
      <td>81.396140</td>
    </tr>
    <tr>
      <th>21</th>
      <td>11th</td>
      <td>Holden High School</td>
      <td>83.815534</td>
    </tr>
    <tr>
      <th>22</th>
      <td>11th</td>
      <td>Huang High School</td>
      <td>81.417476</td>
    </tr>
    <tr>
      <th>23</th>
      <td>11th</td>
      <td>Johnson High School</td>
      <td>80.616027</td>
    </tr>
    <tr>
      <th>24</th>
      <td>11th</td>
      <td>Pena High School</td>
      <td>84.335938</td>
    </tr>
    <tr>
      <th>25</th>
      <td>11th</td>
      <td>Rodriguez High School</td>
      <td>80.864811</td>
    </tr>
    <tr>
      <th>26</th>
      <td>11th</td>
      <td>Shelton High School</td>
      <td>84.373786</td>
    </tr>
    <tr>
      <th>27</th>
      <td>11th</td>
      <td>Thomas High School</td>
      <td>83.585542</td>
    </tr>
    <tr>
      <th>28</th>
      <td>11th</td>
      <td>Wilson High School</td>
      <td>83.764608</td>
    </tr>
    <tr>
      <th>29</th>
      <td>11th</td>
      <td>Wright High School</td>
      <td>84.156322</td>
    </tr>
    <tr>
      <th>30</th>
      <td>12th</td>
      <td>Bailey High School</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>31</th>
      <td>12th</td>
      <td>Cabrera High School</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>32</th>
      <td>12th</td>
      <td>Figueroa High School</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>33</th>
      <td>12th</td>
      <td>Ford High School</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>34</th>
      <td>12th</td>
      <td>Griffin High School</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>35</th>
      <td>12th</td>
      <td>Hernandez High School</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>36</th>
      <td>12th</td>
      <td>Holden High School</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>37</th>
      <td>12th</td>
      <td>Huang High School</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>38</th>
      <td>12th</td>
      <td>Johnson High School</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>39</th>
      <td>12th</td>
      <td>Pena High School</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>40</th>
      <td>12th</td>
      <td>Rodriguez High School</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>41</th>
      <td>12th</td>
      <td>Shelton High School</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>42</th>
      <td>12th</td>
      <td>Thomas High School</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>43</th>
      <td>12th</td>
      <td>Wilson High School</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>44</th>
      <td>12th</td>
      <td>Wright High School</td>
      <td>84.073171</td>
    </tr>
    <tr>
      <th>45</th>
      <td>9th</td>
      <td>Bailey High School</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>46</th>
      <td>9th</td>
      <td>Cabrera High School</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>47</th>
      <td>9th</td>
      <td>Figueroa High School</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>48</th>
      <td>9th</td>
      <td>Ford High School</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>49</th>
      <td>9th</td>
      <td>Griffin High School</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>50</th>
      <td>9th</td>
      <td>Hernandez High School</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>51</th>
      <td>9th</td>
      <td>Holden High School</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>52</th>
      <td>9th</td>
      <td>Huang High School</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>53</th>
      <td>9th</td>
      <td>Johnson High School</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>54</th>
      <td>9th</td>
      <td>Pena High School</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>55</th>
      <td>9th</td>
      <td>Rodriguez High School</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>56</th>
      <td>9th</td>
      <td>Shelton High School</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>57</th>
      <td>9th</td>
      <td>Thomas High School</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>58</th>
      <td>9th</td>
      <td>Wilson High School</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>59</th>
      <td>9th</td>
      <td>Wright High School</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Scores By School Spending
bins = [550,580,610,640,670]
group_names = ['550-580','581-610','611-640','641-670']
Score['Spending_Rank'] = pd.cut(Score['Per Student Budget'],bins,labels=group_names)
Spending = Score.groupby(["Spending_Rank"],as_index = False)["Average_Math_Score"].mean()
Spending2= Score.groupby(["Spending_Rank"],as_index = False)["Average_Reading_Score"].mean()
Spending3 = Score.groupby(["Spending_Rank"],as_index = False)["Percent_Passed_Math"].mean()
Spending4 = Score.groupby(["Spending_Rank"],as_index = False)["Percent_Passed_Reading"].mean()
Spending = pd.merge(Spending,Spending2,on="Spending_Rank",how="left")
Spending = pd.merge(Spending,Spending3,on="Spending_Rank",how="left")
Spending = pd.merge(Spending,Spending4,on="Spending_Rank",how="left")
Spending['Overall_Passing_Rate'] = (Spending['Percent_Passed_Math'] + Spending['Percent_Passed_Reading']) / 2
Spending
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Spending_Rank</th>
      <th>Average_Math_Score</th>
      <th>Average_Reading_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>550-580</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>1</th>
      <td>581-610</td>
      <td>83.549353</td>
      <td>83.903238</td>
      <td>93.686876</td>
      <td>96.340888</td>
      <td>95.013882</td>
    </tr>
    <tr>
      <th>2</th>
      <td>611-640</td>
      <td>79.474551</td>
      <td>82.120471</td>
      <td>77.139934</td>
      <td>87.468080</td>
      <td>82.304007</td>
    </tr>
    <tr>
      <th>3</th>
      <td>641-670</td>
      <td>77.023555</td>
      <td>80.957446</td>
      <td>66.701010</td>
      <td>80.675217</td>
      <td>73.688113</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Scores By School Size
bins = [0,2000,4000,6000]
group_names = ['small','medium','large']
Score['Size_Rank'] = pd.cut(Score['Total_Students'],bins,labels=group_names)
Score = Score[['School ID','school','type','Total School Budget','Total_Students','Per Student Budget','Average_Math_Score','Average_Reading_Score','Percent_Passed_Math','Percent_Passed_Reading','Size_Rank']]
School = Score.groupby(["Size_Rank"],as_index = False)["Average_Math_Score"].mean()
School2= Score.groupby(["Size_Rank"],as_index = False)["Average_Reading_Score"].mean()
School3 = Score.groupby(["Size_Rank"],as_index = False)["Percent_Passed_Math"].mean()
School4 = Score.groupby(["Size_Rank"],as_index = False)["Percent_Passed_Reading"].mean()
School = pd.merge(School,School2,on="Size_Rank",how="left")
School = pd.merge(School,School3,on="Size_Rank",how="left")
School = pd.merge(School,School4,on="Size_Rank",how="left")
School['Overall_Passing_Rate'] = (Spending['Percent_Passed_Math'] + Spending['Percent_Passed_Reading']) / 2
School
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Size_Rank</th>
      <th>Average_Math_Score</th>
      <th>Average_Reading_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>small</td>
      <td>83.502373</td>
      <td>83.883125</td>
      <td>93.585560</td>
      <td>96.593182</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>1</th>
      <td>medium</td>
      <td>78.112137</td>
      <td>81.564235</td>
      <td>72.043261</td>
      <td>83.622873</td>
      <td>95.013882</td>
    </tr>
    <tr>
      <th>2</th>
      <td>large</td>
      <td>77.136883</td>
      <td>80.978256</td>
      <td>66.496861</td>
      <td>81.339570</td>
      <td>82.304007</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Scores By School Type
Type = Score.groupby('type',as_index=False)['Average_Math_Score'].mean()
Type2 = Score.groupby('type',as_index=False)['Average_Reading_Score'].mean()
Type3 = Score.groupby('type',as_index=False)['Percent_Passed_Math'].mean()
Type4 = Score.groupby('type',as_index=False)['Percent_Passed_Reading'].mean()
Type = pd.merge(Type,Type2,on="type",how="left")
Type = pd.merge(Type,Type3,on="type",how="left")
Type = pd.merge(Type,Type4,on="type",how="left")
Type['Overall_Passing_Rate'] = (Type['Percent_Passed_Math'] + Type['Percent_Passed_Reading'])/2
Type
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Average_Math_Score</th>
      <th>Average_Reading_Score</th>
      <th>Percent_Passed_Math</th>
      <th>Percent_Passed_Reading</th>
      <th>Overall_Passing_Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Charter</td>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>95.103660</td>
    </tr>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>73.673757</td>
    </tr>
  </tbody>
</table>
</div>


