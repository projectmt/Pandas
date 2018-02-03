
## PyCity Schools

### Analysis:
- Scores are not dependant on higher budget spent per student
- Charter Schools performed the highest in all metrics



```python
# Dependencies
import numpy as np
import pandas as pd
```


```python
#reader csvs
student_file = "Resources/students_complete.csv"
school_file = "Resources/schools_complete.csv"

studentdf = pd.read_csv(student_file)
schooldf = pd.read_csv(school_file)

#sample of student/school df
#schooldf = schooldf["School ID","School","type","size","budget"]
schooldf.columns = ['School ID','school','type','size','budget']
```


```python
dfcomp = pd.merge(studentdf, schooldf, how="left", on=["school","school"])
dfcomp.head()
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
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate Totals 
school_count = len(dfcomp["school"].unique())
student_count = dfcomp["Student ID"].count()

# Calculate Total Budget
ttl_budget = schooldf["budget"].sum()

```


```python
# Calculate Average Scores
avg_math_score = dfcomp["math_score"].mean()
avg_reading_score = dfcomp["reading_score"].mean()
overall_pass_rate = (avg_math_score + avg_reading_score) / 2
```


```python
# Calculate the Percentage of Pass Rates
pass_math = dfcomp[(dfcomp["math_score"] > 70)].count()["name"]
pass_math_percent = pass_math / (student_count) * 100


pass_reading = dfcomp[(dfcomp["reading_score"] > 70)].count()["name"]
pass_reading_percent = pass_reading / (student_count) * 100
```


```python
district_summary = pd.DataFrame({"Total Schools": [school_count], 
                                 "Total Students": [student_count], 
                                 "Total Budget": [ttl_budget],
                                 "Average Math Score": [avg_math_score], 
                                 "Average Reading Score": [avg_reading_score],
                                 "% Passing Math": [pass_math_percent],
                                 "% Passing Reading": [pass_reading_percent],
                                 "% Overall Passing Rate": [overall_pass_rate]})

district_summary = district_summary[["Total Schools", "Total Students", "Total Budget",
                                     "Average Math Score", 
                                     "Average Reading Score",
                                     "% Passing Math",
                                     "% Passing Reading",
                                     "% Overall Passing Rate"]]


#format values
district_summary["Total Schools"] = district_summary["Total Schools"].map("{0:,.0f}".format)
district_summary["Total Students"] = district_summary["Total Students"].map("{0:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${0:,.0f}".format)
district_summary["Average Math Score"] = district_summary["Average Math Score"].map("{0:,.2f}%".format)
district_summary["Average Reading Score"] = district_summary["Average Reading Score"].map("{0:,.2f}%".format)
district_summary["% Passing Math"] = district_summary["% Passing Math"].map("{0:,.2f}%".format)
district_summary["% Passing Reading"] = district_summary["% Passing Reading"].map("{0:,.2f}%".format)
district_summary["% Overall Passing Rate"] = district_summary["% Overall Passing Rate"].map("{0:,.2f}%".format)

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
      <th>% Overall Passing Rate</th>
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
      <td>72.39%</td>
      <td>82.97%</td>
      <td>80.43%</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary


```python
schooldf.head()
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
#School Type
school_types = schooldf.set_index(["school"])["type"]

# Total students
per_school_count = dfcomp["school"].value_counts()

# school budget and capita
per_school_budget = schooldf.groupby(["school"]).mean()["budget"]
per_school_capita = per_school_budget / per_school_count

#Average Scores
avg_school_math = dfcomp.groupby(["school"]).mean()["math_score"]
avg_school_reading = dfcomp.groupby(["school"]).mean()["reading_score"]

#Pass rate
school_pass_math = dfcomp[(dfcomp["math_score"] > 70)]
school_pass_reading = dfcomp[(dfcomp["reading_score"] > 70)]

per_school_pass_math = school_pass_math.groupby(["school"]).count()["name"] / per_school_count * 100
per_school_pass_reading = school_pass_reading.groupby(["school"]).count()["name"] / per_school_count * 100
overall_pass_rate = (per_school_pass_math + per_school_pass_reading) / 2


per_school_summary = pd.DataFrame({"School Type": school_types,
                                   "Total Students": per_school_count,
                                   "Total School Budget": per_school_budget,
                                   "Per Student Budget": per_school_capita,
                                   "Average Math Score": avg_school_math,
                                   "Average Reading Score": avg_school_reading,
                                   "% Passing Math": per_school_pass_math,
                                   "% Passing Reading": per_school_pass_reading,
                                   "% Overall Passing Rate": overall_pass_rate})

# Minor data munging
per_school_summary = per_school_summary[["School Type", "Total Students", "Total School Budget", "Per Student Budget",
                                         "Average Math Score", "Average Reading Score", 
                                         "% Passing Math", "% Passing Reading", 
                                         "% Overall Passing Rate"]]

per_school_summary["Total School Budget"] = per_school_summary["Total Students"].map("${:,.2f}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.2f}".format)

# Display the data frame
per_school_summary
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$4,976.00</td>
      <td>$628.00</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>64.630225</td>
      <td>79.300643</td>
      <td>71.965434</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,858.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$2,949.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$2,739.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$1,468.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$4,635.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$427.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$2,917.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$4,761.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$962.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$3,999.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$1,761.00</td>
      <td>$600.00</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,635.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>90.214067</td>
      <td>92.905199</td>
      <td>91.559633</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$2,283.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,800.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
  </tbody>
</table>
</div>



## Top Performing Schools (By Passing Rate)


```python
top_schools = per_school_summary.sort_values(["% Overall Passing Rate"], ascending=False)
top_schools.head(5)
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$2,283.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$962.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,800.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,858.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$427.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)


```python
bottom_schools = per_school_summary.sort_values(["% Overall Passing Rate"], ascending=True)
bottom_schools.head(5)
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$3,999.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$2,917.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$4,761.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$2,949.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$4,635.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
  </tbody>
</table>
</div>



## Math Scores by Grade


```python
nineth_grade = dfcomp[(dfcomp["grade"] == "9th")]
tenth_grade = dfcomp[(dfcomp["grade"] == "10th")]
eleventh_grade = dfcomp[(dfcomp["grade"] == "11th")]
twelfth_grade = dfcomp[(dfcomp["grade"] == "12th")]
```


```python
nineth_grade_scores = nineth_grade.groupby(["school"]).mean()["math_score"]
tenth_grade_scores = tenth_grade.groupby(["school"]).mean()["math_score"]
eleventh_grade_scores = eleventh_grade.groupby(["school"]).mean()["math_score"]
twelfth_grade_scores = twelfth_grade.groupby(["school"]).mean()["math_score"]
```


```python
nineth_grade_scores = nineth_grade_scores.map("{0:,.2f}".format)
tenth_grade_scores = tenth_grade_scores.map("{0:,.2f}".format)
eleventh_grade_scores = eleventh_grade_scores.map("{0:,.2f}".format)
twelfth_grade_scores = twelfth_grade_scores.map("{0:,.2f}".format)
```


```python
scores = pd.DataFrame({"9th": nineth_grade_scores, "10th": tenth_grade_scores,
                                "11th": eleventh_grade_scores, "12th": twelfth_grade_scores})

scores = scores[["9th", "10th", "11th", "12th"]]
scores.index.name = " "
```


```python
scores
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.08</td>
      <td>77.00</td>
      <td>77.52</td>
      <td>76.49</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.09</td>
      <td>83.15</td>
      <td>82.77</td>
      <td>83.28</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.40</td>
      <td>76.54</td>
      <td>76.88</td>
      <td>77.15</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.36</td>
      <td>77.67</td>
      <td>76.92</td>
      <td>76.18</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.04</td>
      <td>84.23</td>
      <td>83.84</td>
      <td>83.36</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.44</td>
      <td>77.34</td>
      <td>77.14</td>
      <td>77.19</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.79</td>
      <td>83.43</td>
      <td>85.00</td>
      <td>82.86</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.03</td>
      <td>75.91</td>
      <td>76.45</td>
      <td>77.23</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.19</td>
      <td>76.69</td>
      <td>77.49</td>
      <td>76.86</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.63</td>
      <td>83.37</td>
      <td>84.33</td>
      <td>84.12</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.86</td>
      <td>76.61</td>
      <td>76.40</td>
      <td>77.69</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.42</td>
      <td>82.92</td>
      <td>83.38</td>
      <td>83.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.59</td>
      <td>83.09</td>
      <td>83.50</td>
      <td>83.50</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.09</td>
      <td>83.72</td>
      <td>83.20</td>
      <td>83.04</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.26</td>
      <td>84.01</td>
      <td>83.84</td>
      <td>83.64</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Spending


```python
# Establish the bins 
spending_bins = [0, 600, 625, 650, 675]
group_names = ["<$600", "$600-625", "$650-675", ">$675"]

per_school_summary["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)

spending_math_scores = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
spending_passing_math = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]

spending_reading_scores = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
spending_passing_reading = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]

overall_passing_rate = (spending_math_scores + spending_reading_scores) / 2

spending_summary = pd.DataFrame({"Average Math Score" : spending_math_scores,
                                 "Average Reading Score": spending_reading_scores,
                                 "% Passing Math": spending_passing_math,
                                 "% Passing Reading": spending_passing_reading,
                                 "% Overall Passing Rate": overall_passing_rate})

spending_summary = spending_summary[["Average Math Score", 
                                     "Average Reading Score", 
                                     "% Passing Math", "% Passing Reading",
                                     "% Overall Passing Rate"]]

spending_summary
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$600</th>
      <td>83.436210</td>
      <td>83.892196</td>
      <td>90.258770</td>
      <td>93.184236</td>
      <td>83.664203</td>
    </tr>
    <tr>
      <th>$600-625</th>
      <td>83.595708</td>
      <td>83.930728</td>
      <td>90.698944</td>
      <td>92.798056</td>
      <td>83.763218</td>
    </tr>
    <tr>
      <th>$650-675</th>
      <td>78.032719</td>
      <td>81.416375</td>
      <td>68.711132</td>
      <td>80.695926</td>
      <td>79.724547</td>
    </tr>
    <tr>
      <th>&gt;$675</th>
      <td>76.959583</td>
      <td>81.058567</td>
      <td>64.032486</td>
      <td>78.500776</td>
      <td>79.009075</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Spending


```python
#Bins
spending_bins = [0, 585, 615, 645, 675]
group_names = ["<$585", "$585-615", "$615-645", "$645-675"]

per_school_summary["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)

spending_math_scores = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
spending_passing_math = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]

spending_reading_scores = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
spending_passing_reading = per_school_summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]

overall_passing_rate = (spending_math_scores + spending_reading_scores) / 2

spending_summary = pd.DataFrame({"Average Math Score" : spending_math_scores,
                                 "Average Reading Score": spending_reading_scores,
                                 "% Passing Math": spending_passing_math,
                                 "% Passing Reading": spending_passing_reading,
                                 "% Overall Passing Rate": overall_passing_rate})

spending_summary = spending_summary[["Average Math Score", 
                                     "Average Reading Score", 
                                     "% Passing Math", "% Passing Reading",
                                     "% Overall Passing Rate"]]
```


```python
spending_summary
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$585</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>90.350436</td>
      <td>93.325838</td>
      <td>83.694607</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>90.788049</td>
      <td>92.410786</td>
      <td>83.742449</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>79.079225</td>
      <td>81.891436</td>
      <td>73.021426</td>
      <td>83.214343</td>
      <td>80.485330</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>76.997210</td>
      <td>81.027843</td>
      <td>63.972368</td>
      <td>78.427809</td>
      <td>79.012526</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Size


```python
size_bins = [0, 1000, 2500, 5000]
group_names = ["Small", "Medium", "Large"]

per_school_summary["School Size"] = pd.cut(per_school_summary["Total Students"], size_bins, labels=group_names)

total_math_scores = per_school_summary.groupby(["School Size"]).mean()["Average Math Score"]
total_passing_math = per_school_summary.groupby(["School Size"]).mean()["% Passing Math"]

total_reading_scores = per_school_summary.groupby(["School Size"]).mean()["Average Reading Score"]
total_passing_reading = per_school_summary.groupby(["School Size"]).mean()["% Passing Reading"]

overall_passing_rate = (total_passing_math + total_passing_reading) / 2

size_summary = pd.DataFrame({"Average Math Score" : total_math_scores,
                             "Average Reading Score": total_reading_scores,
                             "% Passing Math": total_passing_math,
                             "% Passing Reading": total_passing_reading,
                             "% Overall Passing Rate": overall_passing_rate})

size_summary = size_summary[["Average Math Score", 
                             "Average Reading Score", 
                             "% Passing Math", "% Passing Reading",
                             "% Overall Passing Rate"]]
```


```python
size_summary
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>91.158155</td>
      <td>92.471895</td>
      <td>91.815025</td>
    </tr>
    <tr>
      <th>Medium</th>
      <td>83.357937</td>
      <td>83.885280</td>
      <td>90.098249</td>
      <td>93.246451</td>
      <td>91.672350</td>
    </tr>
    <tr>
      <th>Large</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>71.313543</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Type


```python
type_math_scores = per_school_summary.groupby(["School Type"]).mean()["Average Math Score"]
type_pass_math = per_school_summary.groupby(["School Type"]).mean()["% Passing Math"]

type_reading_scores = per_school_summary.groupby(["School Type"]).mean()["Average Reading Score"]
type_pass_reading = per_school_summary.groupby(["School Type"]).mean()["% Passing Reading"]

overall_pass_rate = (type_pass_math + type_pass_reading) / 2

type_summary = pd.DataFrame({"Average Math Score" : type_math_scores,
                             "Average Reading Score": type_reading_scores,
                             "% Passing Math": type_pass_math,
                             "% Passing Reading": type_pass_reading,
                             "% Overall Passing Rate": overall_pass_rate})


type_summary = type_summary[["Average Math Score", 
                             "Average Reading Score",
                             "% Passing Math",
                             "% Passing Reading",
                             "% Overall Passing Rate"]]

type_summary
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>91.708019</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>71.313543</td>
    </tr>
  </tbody>
</table>
</div>


