# Pandas


```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
```


```python
#reader csvs
student_file = "Resources/students_complete.csv"
school_file = "Resources/schools_complete.csv"

studentdf = pd.read_csv(student_file)
schooldf = pd.read_csv(school_file)

#sample of student df
studentdf.head()
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
#snips of Studen df
math_score = studentdf[["name","school","math_score"]]
read_score = studentdf[["name","school","reading_score"]]
mathby_grade = studentdf[["school","grade","math_score"]]
readby_grade = studentdf[["school","grade","reading_score"]]
math_ninth = mathby_grade.loc[(mathby_grade["grade"]=="9th")]

```


```python
#snip of Passing Students
pass_math_snip = math_score.loc[math_score['math_score'] > 69]
pass_read_snip = read_score.loc[read_score['reading_score'] > 69]
```


```python
#aggregate values
total_schools = schooldf['name'].count()
total_students = studentdf['name'].count()
total_budget = schooldf['budget'].sum()
avg_math_score = studentdf['math_score'].mean()
avg_reading_score = studentdf['reading_score'].mean()
pass_math = pass_math_snip['math_score'].count()
pass_read = pass_read_snip['reading_score'].count()
per_passing_math = (pass_math/total_students)*100
per_passing_reading = (pass_read/total_students)*100
overall_pass_rate = (per_passing_math + per_passing_reading)/2
ttl_budget = schooldf['budget'].sum()
ttl_students = studentdf['name'].count()

total_schools
```




    15




```python
#lay out district summary table using aggregate values
district_summary = pd.DataFrame({"Total Schools": [total_schools],
            "Total Students": [total_students],
            "Total Budget": [total_budget],
            "Average Math Score": [avg_math_score],
            "Average Reading Score": [avg_reading_score],
            "% Passing Math": [per_passing_math],
            "% Passing Reading": [per_passing_reading],
            "Overall Passing Rate": [overall_pass_rate]})

#reorder values for district summary table
district_summary = district_summary[["Total Schools",
                                     "Total Students",
                                     "Total Budget",
                                     "Average Math Score",
                                     "Average Reading Score",
                                     "% Passing Math",
                                     "% Passing Reading",
                                     "Overall Passing Rate"]]
```


```python
#format values
district_summary["Total Schools"] = district_summary["Total Schools"].map("{0:,.0f}".format)
district_summary["Total Students"] = district_summary["Total Students"].map("{0:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${0:,.0f}".format)
district_summary["Average Math Score"] = district_summary["Average Math Score"].map("{0:,.2f}%".format)
district_summary["Average Reading Score"] = district_summary["Average Reading Score"].map("{0:,.2f}%".format)
district_summary["% Passing Math"] = district_summary["% Passing Math"].map("{0:,.2f}%".format)
district_summary["% Passing Reading"] = district_summary["% Passing Reading"].map("{0:,.2f}%".format)
district_summary["Overall Passing Rate"] = district_summary["Overall Passing Rate"].map("{0:,.2f}%".format)
#print district_summary.to_string(index=False)
district_summary
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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24,649,428</td>
      <td>78.99%</td>
      <td>81.88%</td>
      <td>74.98%</td>
      <td>85.81%</td>
      <td>80.39%</td>
    </tr>
  </tbody>
</table>
</div>




```python
school_snip = schooldf[["name", "type", "budget"]]
school_snip = school_snip.sort_values(["name"], ascending=True)
school_snip = school_snip.reset_index(drop=True)
school_snip = school_snip.rename(columns = {"name":"School",
                                           "type":"School Type",
                                           "budget":"Total School Budget"})
school_snip
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
      <th>School</th>
      <th>School Type</th>
      <th>Total School Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>3124928</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1081356</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>1763916</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>917500</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>248087</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>3094650</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>585858</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>2547363</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1043130</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>1319574</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1049400</td>
    </tr>
  </tbody>
</table>
</div>




```python
#sort by
student_ttl = pd.DataFrame(studentdf.groupby("school")["name"].count())
student_ttl.reset_index(inplace=True)
student_ttl = student_ttl.rename(columns = {"name":"Total Students",
                                             "school":"School"})
student_ttl

student_budget = pd.merge(student_ttl, school_snip, on="School")
student_budget["Per Student Budget"] = student_budget["Total School Budget"] / student_budget["Total Students"]
student_budget = student_budget.drop(["Total Students"],axis=1)
student_budget = student_budget.drop(["School Type"],axis=1)
student_budget = student_budget.drop(["Total School Budget"],axis=1)
student_budget

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
      <th>School</th>
      <th>Per Student Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>628.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>582.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>639.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>644.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>625.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>652.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>581.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>650.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>609.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>637.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>638.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>578.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>583.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
mathpassdf = pd.DataFrame(pass_math_snip.groupby("school")["math_score"].count())

mathpassdf.reset_index(inplace=True)
mathpassdf.head()
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
      <th>school</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>3318</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>1749</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>1946</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>1871</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>1371</td>
    </tr>
  </tbody>
</table>
</div>




```python
readpassdf = pd.DataFrame(pass_read_snip.groupby("school")["reading_score"].count())
readpassdf.reset_index(inplace=True)
readpassdf
scoresmerge = pd.merge(readpassdf, mathpassdf, on="school")
scoresmerge = scoresmerge.rename(columns= {"school": "School",
                                          "reading_score": "Passed Reading",
                                          "math_score": "Passed Math"})
scoresmerge.head()
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
      <th>School</th>
      <th>Passed Reading</th>
      <th>Passed Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>4077</td>
      <td>3318</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>1803</td>
      <td>1749</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>2381</td>
      <td>1946</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>2172</td>
      <td>1871</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>1426</td>
      <td>1371</td>
    </tr>
  </tbody>
</table>
</div>




```python
math_avg = pd.DataFrame(studentdf.groupby("school")["math_score"].mean())
read_avg = pd.DataFrame(studentdf.groupby("school")["reading_score"].mean())

math_avg.reset_index(inplace=True)
read_avg.reset_index(inplace=True)

math_avg = math_avg.rename(columns = {"school":"School"})
read_avg = read_avg.rename(columns = {"school":"School"})

merge_df = pd.merge(math_avg, read_avg, on="School")
merge_df = pd.merge(merge_df, student_ttl, on="School")
merge_df = pd.merge(merge_df, scoresmerge, on="School")

merge_df = merge_df.rename(columns = {"math_score": "Avg Math Score",
                                     "reading_score": "Avg Reading Score"})

merge_df["% Passing Math"] = merge_df["Passed Math"] / merge_df["Total Students"] * 100
merge_df["% Passing Reading"] = merge_df["Passed Reading"] / merge_df["Total Students"] * 100
merge_df["% Overall Passing Rate"] = merge_df[["% Passing Math","% Passing Reading"]].mean(axis=1)
school_summary = merge_df.drop(["Passed Reading","Passed Math"], axis=1)
school_summary = pd.merge(school_summary, school_snip, on = "School")
school_summary = pd.merge(school_summary, student_budget, on = "School")
school_summary.head()
```


```python
botfive = school_summary.nsmallest(5,'% Overall Passing Rate')
botfive.reset_index(inplace=True)
botfive.drop(['index'],axis=1)
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
      <th>School</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>Total Students</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
      <th>School Type</th>
      <th>Total School Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rodriguez High School</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>3999</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>District</td>
      <td>2547363</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>2949</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>District</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Huang High School</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>2917</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>District</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Johnson High School</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>4761</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>District</td>
      <td>3094650</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ford High School</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>2739</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>District</td>
      <td>1763916</td>
    </tr>
  </tbody>
</table>
</div>




```python
topfive = school_summary.nlargest(5,'% Overall Passing Rate')
topfive.reset_index(inplace=True)
topfive.drop(['index'],axis=1)
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
      <th>School</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>Total Students</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
      <th>School Type</th>
      <th>Total School Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cabrera High School</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1858</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>Charter</td>
      <td>1081356</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Thomas High School</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>1635</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>Charter</td>
      <td>1043130</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Pena High School</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>962</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>Charter</td>
      <td>585858</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Griffin High School</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>1468</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>Charter</td>
      <td>917500</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wilson High School</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>2283</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>Charter</td>
      <td>1319574</td>
    </tr>
  </tbody>
</table>
</div>


