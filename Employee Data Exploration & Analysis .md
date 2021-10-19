#           👨👩  Employee Data Exploration & Analysis 
    Author: Brandon Tan 
    Date: 10/17/2021
## 📚 Table of Content 
>1.	Introduction (Business Problem) 
>2.	Project Goals and Objectives 
>3.	Project Flow and Sequence  
       1. Data Preparation 
           - Removing Constant Feature Column
       2. Data Understanding & Basic Statistics
           - Data Accuracy 
           - Time Period Recorded for Data 
           - Employee Levels Distributions
           - Department Head Counts
           - Cost Center Head Counts
           - Employee Status
       3. EDA (Data Exploration) 
           - Ratio for Level against Department & Cost Center
               - Level V Department
               - Level Comparison for low & High for each Department
               - Low Level Analytics Department (L1-L4)
               - High Level Anlytics Department (LM5 -LM7)
               - Level V Cost Center
               - Low Level Analytics Cost Center (L1-L4)
               - High Level Analytics Cost Center (LM5 -LM7)
           - Data Analysis (Extracting information from different variables) 
               - The most effective way to acquire talent
               - Which Hiring Source attracts what level of employees
               - Hiring Source compare to departments
>4.	Key Findings & Suggestions 
>5.	Appendix 
        - Tools and Libraries used 
        - Suggestions & Questions 
## 👔 Introduction / Business Problem 
> Money Lion is one stop shop for all our financial needs. The company has recently reached a milestone of going public, while much of the success came from their innovation and vision to solve people's financial needs, it is no doubt a strong backend business system is part of the success. Today I will be exploring and analysing their employee data which reflects the hiring efficiency and employee productivity. 

## 🎯 Project Goals and Objectives 
> The objective is to extract actionable insights that would increase the overall efficiency of the hiring process, increse employee productivity and to mitigate losses on hiring investments. 

### A. Data Preparation
1. This project is done both in excel and jupyter notebook(python)
      - Work done in excel includes: sorting / filtering / adding Cost Center column / Removing incomplete columns
      - Work Done in python: removing Location column
2. For this particular project for we do not remove any rows that has the slightest information. Since we do not need to do predictive analysis, we are not feeding the data for training hence any information retained is valuable for our discovery. However, since location is a constant feature, it does not provide any relevant information to our analysis, we will remove it. 
3. Null value datas are wild cards that have weight in either category of each unique column 

**Import Libraries**


```python
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt 
import seaborn as sns
import hvplot.pandas
import warnings
```

**Reading CSV File**


```python
# Reading people ops file
df = pd.read_csv('People Ops Analyst Test.csv')
# Display max column
# pd.set_option('display.max_columns', None)
# pd.set_option('display.max_rows', None)
```

**Removing Location Column**


```python
# Remove Location 
df.drop(['Location'], axis=1, inplace = True)
```


```python
df.head(10)
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
      <th>Department</th>
      <th>Job Title</th>
      <th>Hiring Source</th>
      <th>Hire Date</th>
      <th>Level</th>
      <th>Employment Status</th>
      <th>Cost Center</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AI Engineering</td>
      <td>Chief AI Officer</td>
      <td>NaN</td>
      <td>7/1/2014</td>
      <td>L8</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Information Security Audit</td>
      <td>Information Security &amp; Engineering Portfolio M...</td>
      <td>NaN</td>
      <td>7/1/2014</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AI Engineering</td>
      <td>Chief Engineer</td>
      <td>Referral</td>
      <td>8/1/2014</td>
      <td>LM6</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Data Science</td>
      <td>Director of Analytics</td>
      <td>NaN</td>
      <td>1/12/2015</td>
      <td>LM6</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>11/23/2015</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>7/4/2016</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Product Engineering</td>
      <td>Director of Mobile</td>
      <td>Sourced</td>
      <td>11/7/2016</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>7</th>
      <td>AI Engineering</td>
      <td>Application Architect</td>
      <td>Referred</td>
      <td>12/12/2016</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>8</th>
      <td>R&amp;D Engineering</td>
      <td>Lead R&amp;D Engineer</td>
      <td>NaN</td>
      <td>6/1/2017</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Data Science</td>
      <td>Senior Data Scientist</td>
      <td>NaN</td>
      <td>6/19/2017</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
  </tbody>
</table>
</div>



### B. Data Understanding and Basic Stats

**1. Data Accuracy**

We want to see if this data sample is accurate and correctly stored. 
   - Job Titlle with Levels (the logic assumption here is that a high level executive would not be listed as L1/ L2) 

We will only sample L1-L2 to see if any high level excutives are mistakenly recorded.



```python
# Checking for data accuracy 
search_values = ['L1','L2']
temp_df = df[df.Level.str.contains('|'.join(search_values))]
temp_df['Job Title'].value_counts()
```




    Data Scientist                     12
    Mobile Engineer                     8
    Software Engineer                   6
    Data Engineer                       5
    Automation QA Engineer              4
    Data Analyst                        3
    Mobile Engineer (R&D)               3
    AI Engineer                         3
    HR Executive                        2
    Product Analyst                     2
    Cybersecurity Analyst               2
    Service Desk Analyst                2
    Senior Software Engineer            2
    Talent Acquisition Executive        2
    Software Engineer, R&D              1
    People Ops & Legal Associate        1
    Content Writer                      1
    System Administrator                1
    Senior System Administrator         1
    Application Security Specialist     1
    Marketing Campaign Executive        1
    Security Engineer                   1
    QA Engineer                         1
    Product Designer                    1
    Copywriter                          1
    R&D Engineer                        1
    Product Manager                     1
    Marketing Campaign Analyst          1
    AI DevOps                           1
    Name: Job Title, dtype: int64



Base on personal logic from the sample tested, there were no high level executives that were mistakenly listed under L1 -L2 hence we would assume that the data is accurate.

**2. Time Period Recorded for Data**


```python
df['Hire Date'] =  pd.to_datetime(df['Hire Date'], format= '%m/%d/%Y')
df.sort_values(by='Hire Date',ascending=True)
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
      <th>Department</th>
      <th>Job Title</th>
      <th>Hiring Source</th>
      <th>Hire Date</th>
      <th>Level</th>
      <th>Employment Status</th>
      <th>Cost Centre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>92</th>
      <td>Information Security Audit</td>
      <td>Information Security &amp; Engineering Portfolio M...</td>
      <td>NaN</td>
      <td>2014-07-01</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>32</th>
      <td>AI Engineering</td>
      <td>Chief AI Officer</td>
      <td>NaN</td>
      <td>2014-07-01</td>
      <td>L8</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>31</th>
      <td>AI Engineering</td>
      <td>Chief Engineer</td>
      <td>Referral</td>
      <td>2014-08-01</td>
      <td>LM6</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Data Science</td>
      <td>Director of Analytics</td>
      <td>NaN</td>
      <td>2015-01-12</td>
      <td>LM6</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2015-06-08</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2015-11-02</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>200</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>2015-11-23</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>199</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>2016-07-04</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>198</th>
      <td>Product Engineering</td>
      <td>Director of Mobile</td>
      <td>Sourced</td>
      <td>2016-11-07</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>30</th>
      <td>AI Engineering</td>
      <td>Application Architect</td>
      <td>Referred</td>
      <td>2016-12-12</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>15</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-03-21</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>210</th>
      <td>R&amp;D Engineering</td>
      <td>Lead R&amp;D Engineer</td>
      <td>NaN</td>
      <td>2017-06-01</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Data Science</td>
      <td>Senior Data Scientist</td>
      <td>NaN</td>
      <td>2017-06-19</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>29</th>
      <td>AI Engineering</td>
      <td>Director of Infrastructure &amp; Security</td>
      <td>Applied</td>
      <td>2017-07-03</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Product</td>
      <td>Director of Product</td>
      <td>Applied</td>
      <td>2017-08-01</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>131</th>
      <td>Product</td>
      <td>Director of Product</td>
      <td>Applied</td>
      <td>2017-09-04</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>41</th>
      <td>AI Service Delivery</td>
      <td>Senior AI PM</td>
      <td>Referred</td>
      <td>2017-11-01</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2017-11-06</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Data Engineering</td>
      <td>Data Engineering Lead</td>
      <td>Sourced</td>
      <td>2017-12-04</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>NaN</td>
      <td>2018-01-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2018-01-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2018-02-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Data Science</td>
      <td>Visiting Academic</td>
      <td>NaN</td>
      <td>2018-02-01</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>NaN</td>
      <td>2018-02-20</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Data Science</td>
      <td>Research Scientist</td>
      <td>NaN</td>
      <td>2018-03-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Product Engineering</td>
      <td>Software QA Manager</td>
      <td>Referred</td>
      <td>2018-03-05</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>28</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>Sourced</td>
      <td>2018-04-02</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>130</th>
      <td>Product</td>
      <td>Senior Product Designer</td>
      <td>NaN</td>
      <td>2018-04-16</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2018-05-04</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2018-05-04</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>27</th>
      <td>AI Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>2018-05-14</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>129</th>
      <td>Product</td>
      <td>Senior Product Manager</td>
      <td>Referred</td>
      <td>2018-06-04</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Product Engineering</td>
      <td>QA Engineer</td>
      <td>NaN</td>
      <td>2018-06-18</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>26</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>Sourced</td>
      <td>2018-07-02</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>NaN</td>
      <td>2018-07-02</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Data Science</td>
      <td>Senior Data Scientist</td>
      <td>NaN</td>
      <td>2018-07-09</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>127</th>
      <td>Product</td>
      <td>Content Writer</td>
      <td>Sourced</td>
      <td>2018-07-09</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>NaN</td>
      <td>2018-07-16</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Cybersecurity</td>
      <td>Application Security Specialist</td>
      <td>NaN</td>
      <td>2018-08-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>NaN</td>
      <td>2018-09-03</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>114</th>
      <td>People Ops</td>
      <td>Talent Acquisition Executive</td>
      <td>Jobstreet</td>
      <td>2018-09-11</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>189</th>
      <td>Product Engineering</td>
      <td>Senior QA Engineer</td>
      <td>NaN</td>
      <td>2018-10-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Product</td>
      <td>Senior Product Manager</td>
      <td>NaN</td>
      <td>2018-10-01</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>188</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>NaN</td>
      <td>2018-10-08</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>NaN</td>
      <td>2018-10-08</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Information Technology Services</td>
      <td>Lead, Workplace Technology</td>
      <td>NaN</td>
      <td>2018-10-10</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>187</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Applied</td>
      <td>2018-10-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>NaN</td>
      <td>2018-10-15</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>40</th>
      <td>AI Service Delivery</td>
      <td>AI Product Manager</td>
      <td>NaN</td>
      <td>2018-11-05</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2018-11-05</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Product Engineering</td>
      <td>Lead, Site Reliability Engineer</td>
      <td>Referral</td>
      <td>2018-11-05</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>125</th>
      <td>Product</td>
      <td>Senior Product Manager</td>
      <td>Sourced</td>
      <td>2018-11-19</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>209</th>
      <td>R&amp;D Engineering</td>
      <td>R&amp;D Engineer</td>
      <td>NaN</td>
      <td>2018-11-19</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Finance</td>
      <td>Account Manager cum Payroll</td>
      <td>Referral</td>
      <td>2018-12-03</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Product</td>
      <td>Product Analyst</td>
      <td>NaN</td>
      <td>2018-12-03</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2018-12-03</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Cybersecurity</td>
      <td>Head of Cybersecurity</td>
      <td>NaN</td>
      <td>2019-01-02</td>
      <td>LM6</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Referral</td>
      <td>2019-01-02</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>113</th>
      <td>People Ops</td>
      <td>HR Executive</td>
      <td>Referral</td>
      <td>2019-01-07</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>111</th>
      <td>People Ops</td>
      <td>Talent Acquisition Executive</td>
      <td>Job Portal</td>
      <td>2019-01-14</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Information Security Audit</td>
      <td>Information Security Analyst</td>
      <td>NaN</td>
      <td>2019-01-14</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Data Science</td>
      <td>Associate Data Scientist</td>
      <td>NaN</td>
      <td>2019-01-14</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>112</th>
      <td>People Ops</td>
      <td>People Ops &amp; Legal Associate</td>
      <td>Referral</td>
      <td>2019-01-14</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>182</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2019-01-14</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Cybersecurity</td>
      <td>Security Engineer</td>
      <td>NaN</td>
      <td>2019-01-28</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>181</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>NaN</td>
      <td>2019-01-28</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>14</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019-01-30</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>180</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Applied</td>
      <td>2019-02-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Referred</td>
      <td>2019-02-11</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>179</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2019-02-11</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Cybersecurity</td>
      <td>Cybersecurity Analyst</td>
      <td>NaN</td>
      <td>2019-02-11</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>39</th>
      <td>AI Service Delivery</td>
      <td>Senior VP - AI</td>
      <td>Referral</td>
      <td>2019-02-14</td>
      <td>LM7</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>25</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>NaN</td>
      <td>2019-02-18</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>208</th>
      <td>R&amp;D Engineering</td>
      <td>Senior R&amp;D Engineer</td>
      <td>NaN</td>
      <td>2019-02-18</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>178</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Sourced</td>
      <td>2019-03-01</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>177</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>inbound</td>
      <td>2019-03-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>110</th>
      <td>People Ops</td>
      <td>HR Executive</td>
      <td>Referral</td>
      <td>2019-03-11</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Product Engineering</td>
      <td>SRE level II</td>
      <td>Referral</td>
      <td>2019-03-18</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Referred</td>
      <td>2019-03-25</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>38</th>
      <td>AI Service Delivery</td>
      <td>AI Product Manager</td>
      <td>NaN</td>
      <td>2019-04-01</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>109</th>
      <td>People Ops</td>
      <td>Director of Finance &amp; HR</td>
      <td>Referral</td>
      <td>2019-04-08</td>
      <td>LM5</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>13</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019-04-22</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Information Technology Services</td>
      <td>Lead, IT Service Management</td>
      <td>JobStreet</td>
      <td>2019-05-06</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>174</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Referral</td>
      <td>2019-05-06</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Cybersecurity</td>
      <td>CSIRT Manager</td>
      <td>Referral</td>
      <td>2019-05-27</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Cybersecurity</td>
      <td>Application Security Specialist</td>
      <td>Referral</td>
      <td>2019-06-06</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>24</th>
      <td>AI Engineering</td>
      <td>Senior AI DevOps</td>
      <td>Referred</td>
      <td>2019-06-10</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>173</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2019-06-10</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Cybersecurity</td>
      <td>Principal Cybersecurity Engineer</td>
      <td>Referral</td>
      <td>2019-06-10</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>23</th>
      <td>AI Engineering</td>
      <td>AI DevOps</td>
      <td>Referred</td>
      <td>2019-06-17</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Product Engineering</td>
      <td>QA Engineer</td>
      <td>Applied</td>
      <td>2019-07-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Referred</td>
      <td>2019-08-13</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>12</th>
      <td>NaN</td>
      <td>Domestic helper</td>
      <td>NaN</td>
      <td>2019-09-24</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019-10-07</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Source</td>
      <td>2019-10-21</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Referred</td>
      <td>2019-11-04</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019-11-08</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019-11-11</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Referral</td>
      <td>2019-11-18</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Information Technology Services</td>
      <td>System Administrator</td>
      <td>Referral</td>
      <td>2019-12-16</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Marketing Analytics</td>
      <td>Marketing Analytics Manager</td>
      <td>Sourced</td>
      <td>2020-01-02</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Marketing Analytics</td>
      <td>Senior Data Analyst</td>
      <td>NaN</td>
      <td>2020-01-06</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Sourced</td>
      <td>2020-02-03</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Referral</td>
      <td>2020-02-03</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>108</th>
      <td>People Ops</td>
      <td>Senior HR associate</td>
      <td>NaN</td>
      <td>2020-02-10</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Information Technology Services</td>
      <td>Service Desk Analyst</td>
      <td>Applied</td>
      <td>2020-02-24</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Sourced</td>
      <td>2020-03-09</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Information Technology Services</td>
      <td>Senior System Administrator</td>
      <td>Applied</td>
      <td>2020-04-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Referral</td>
      <td>2020-05-04</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Referral</td>
      <td>2020-05-04</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2020-05-04</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>37</th>
      <td>AI Service Delivery</td>
      <td>AI Product Manager</td>
      <td>NaN</td>
      <td>2020-05-20</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Sourced</td>
      <td>2020-06-09</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Cybersecurity</td>
      <td>Cybersecurity Analyst</td>
      <td>Applied</td>
      <td>2020-06-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Product Engineering</td>
      <td>Junior Site Reliability Engineer</td>
      <td>NaN</td>
      <td>2020-07-01</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>164</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Sourced</td>
      <td>2020-07-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Product</td>
      <td>Product Analyst</td>
      <td>Applied</td>
      <td>2020-08-10</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Referred</td>
      <td>2020-08-10</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Product</td>
      <td>Product Analyst</td>
      <td>Applied</td>
      <td>2020-09-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2020-09-14</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>163</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Applied</td>
      <td>2020-09-14</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Applied</td>
      <td>2020-09-14</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>161</th>
      <td>Product Engineering</td>
      <td>Automation QA Engineer</td>
      <td>Sourced</td>
      <td>2020-09-21</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2020-09-21</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Data Science</td>
      <td>Senior Data Scientist</td>
      <td>Applied</td>
      <td>2020-10-05</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Marketing Analytics</td>
      <td>Data Analyst</td>
      <td>Referral</td>
      <td>2020-10-05</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2020-10-05</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2020-10-05</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>NaN</td>
      <td>2020-10-12</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2020-10-19</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>36</th>
      <td>AI Service Delivery</td>
      <td>Senior AI PM</td>
      <td>NaN</td>
      <td>2020-11-09</td>
      <td>L3</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Applied</td>
      <td>2020-12-07</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Product Engineering</td>
      <td>Lead Software Engineer</td>
      <td>Sourced</td>
      <td>2020-12-07</td>
      <td>L4</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>207</th>
      <td>R&amp;D Engineering</td>
      <td>Mobile Engineer (R&amp;D)</td>
      <td>Referral</td>
      <td>2021-01-04</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2021-01-04</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2021-01-11</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>Sourced</td>
      <td>2021-01-18</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Cybersecurity</td>
      <td>Cybersecurity Analyst</td>
      <td>Sourced</td>
      <td>2021-02-15</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>206</th>
      <td>R&amp;D Engineering</td>
      <td>R&amp;D Engineer</td>
      <td>Referral</td>
      <td>2021-02-15</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>35</th>
      <td>AI Service Delivery</td>
      <td>Head AI Strategy</td>
      <td>NaN</td>
      <td>2021-02-15</td>
      <td>LM5</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>157</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Sourced</td>
      <td>2021-02-22</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Product Engineering</td>
      <td>Director of Engineering</td>
      <td>NaN</td>
      <td>2021-03-01</td>
      <td>LM5</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>22</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>Applied</td>
      <td>2021-03-01</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Information Technology Services</td>
      <td>Service Desk Analyst</td>
      <td>Applied</td>
      <td>2021-03-01</td>
      <td>L2</td>
      <td>Extended Probation</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Marketing Analytics</td>
      <td>Digital Graphic Designer</td>
      <td>NaN</td>
      <td>2021-03-01</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Marketing Analytics</td>
      <td>Marketing Campaign Executive</td>
      <td>Referral</td>
      <td>2021-03-08</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>21</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>Applied</td>
      <td>2021-03-08</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>205</th>
      <td>R&amp;D Engineering</td>
      <td>Mobile Engineer (R&amp;D)</td>
      <td>Referral</td>
      <td>2021-03-15</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>154</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Applied</td>
      <td>2021-03-15</td>
      <td>L2</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Sourced</td>
      <td>2021-03-15</td>
      <td>L2</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2021-03-15</td>
      <td>L1</td>
      <td>Full-Time</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>153</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-04-01</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Data Science</td>
      <td>Research Intern (Part-timer)</td>
      <td>NaN</td>
      <td>2021-04-01</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Data Science</td>
      <td>Research Intern (Part-timer)</td>
      <td>Referral</td>
      <td>2021-04-01</td>
      <td>NaN</td>
      <td>Contractor</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>204</th>
      <td>R&amp;D Engineering</td>
      <td>Software Engineer, R&amp;D</td>
      <td>Internal</td>
      <td>2021-04-01</td>
      <td>L2</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>20</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>NaN</td>
      <td>2021-04-05</td>
      <td>L2</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>152</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>Referral</td>
      <td>2021-04-05</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Marketing Analytics</td>
      <td>Marketing Campaign Analyst</td>
      <td>Applied</td>
      <td>2021-04-05</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Applied</td>
      <td>2021-04-05</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>203</th>
      <td>R&amp;D Engineering</td>
      <td>Mobile Engineer (R&amp;D)</td>
      <td>Referral</td>
      <td>2021-04-05</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>107</th>
      <td>People Ops</td>
      <td>People Ops intern</td>
      <td>Applied</td>
      <td>2021-04-05</td>
      <td>L0</td>
      <td>Internship</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>151</th>
      <td>Product Engineering</td>
      <td>Software Engineer</td>
      <td>NaN</td>
      <td>2021-04-19</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>202</th>
      <td>R&amp;D Engineering</td>
      <td>Senior PM, R&amp;D</td>
      <td>Referral</td>
      <td>2021-04-19</td>
      <td>L4</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>34</th>
      <td>AI Service Delivery</td>
      <td>Agile Coach</td>
      <td>inbound</td>
      <td>2021-05-03</td>
      <td>L4</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>Applied</td>
      <td>2021-05-10</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>Referral</td>
      <td>2021-05-10</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>150</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Referral</td>
      <td>2021-05-10</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Data Science</td>
      <td>Data Analyst</td>
      <td>Applied</td>
      <td>2021-05-10</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>106</th>
      <td>People Ops</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-05-10</td>
      <td>NaN</td>
      <td>Internship</td>
      <td>Office Management</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Product Engineering</td>
      <td>Senior Frontend Software Engineer</td>
      <td>Applied</td>
      <td>2021-05-19</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>147</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Sourced</td>
      <td>2021-05-19</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Applied</td>
      <td>2021-05-19</td>
      <td>L1</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Cybersecurity</td>
      <td>Security Engineer</td>
      <td>Sourced</td>
      <td>2021-05-19</td>
      <td>L1</td>
      <td>Probation</td>
      <td>Security</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Applied</td>
      <td>2021-05-19</td>
      <td>L1</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Product Engineering</td>
      <td>Senior Frontend Software Engineer</td>
      <td>NaN</td>
      <td>2021-05-19</td>
      <td>L2</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Marketing Analytics</td>
      <td>Copywriter</td>
      <td>Sourced</td>
      <td>2021-05-19</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>142</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-06-01</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>143</th>
      <td>Product Engineering</td>
      <td>Mobile Engineer</td>
      <td>Referral</td>
      <td>2021-06-01</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>144</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-06-01</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>141</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Applied</td>
      <td>2021-06-01</td>
      <td>L2</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>117</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Referral</td>
      <td>2021-06-01</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>33</th>
      <td>AI Service Delivery</td>
      <td>Product Manager, R&amp;D</td>
      <td>NaN</td>
      <td>2021-06-01</td>
      <td>L2</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>Applied</td>
      <td>2021-06-08</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Data Science</td>
      <td>Data Analyst</td>
      <td>Applied</td>
      <td>2021-06-08</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>19</th>
      <td>AI Engineering</td>
      <td>Lead AI Engineer</td>
      <td>Sourced</td>
      <td>2021-06-08</td>
      <td>L4</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Agency</td>
      <td>2021-06-08</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>201</th>
      <td>R&amp;D Engineering</td>
      <td>Lead R&amp;D Engineer</td>
      <td>Applied</td>
      <td>2021-06-14</td>
      <td>L4</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>139</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>Applied</td>
      <td>2021-06-14</td>
      <td>L2</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>116</th>
      <td>Product</td>
      <td>Product Designer</td>
      <td>Applied</td>
      <td>2021-06-28</td>
      <td>L2</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>AI Engineering</td>
      <td>AI Engineer</td>
      <td>Sourced</td>
      <td>2021-07-05</td>
      <td>L2</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>Applied</td>
      <td>2021-07-05</td>
      <td>L2</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Data Engineering</td>
      <td>Data Engineer</td>
      <td>Referred</td>
      <td>2021-07-05</td>
      <td>L1</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>Full-Time</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>137</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Data Science</td>
      <td>Data Analyst</td>
      <td>Applied</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Data Science</td>
      <td>Data Scientist</td>
      <td>NaN</td>
      <td>2021-07-05</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-07-12</td>
      <td>L3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-07-12</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Product Engineering</td>
      <td>Junior Site Reliability Engineer</td>
      <td>Applied</td>
      <td>2021-07-12</td>
      <td>NaN</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Applied</td>
      <td>2021-07-12</td>
      <td>L3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-07-19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-07-19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Applied</td>
      <td>2021-08-02</td>
      <td>L3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Product</td>
      <td>Product Manager</td>
      <td>Applied</td>
      <td>2021-08-02</td>
      <td>L3</td>
      <td>Probation</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>134</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-08-09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-08-23</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Data Science</td>
      <td>Data Scientist (Intern)</td>
      <td>NaN</td>
      <td>2021-09-06</td>
      <td>NaN</td>
      <td>Internship</td>
      <td>AI &amp; Data Services</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Product Engineering</td>
      <td>Senior Software Engineer</td>
      <td>NaN</td>
      <td>2021-09-06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Engineering</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-10-04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Data contains information from year 2014 – 2021

**Hiring Trend Overtime**


```python
df['Hire Date'] = pd.to_datetime(df['Hire Date'])
df.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2227c6a46c8>




![png](output_17_1.png)


The Hiring rate has increased dramatically starting from year 2018 - 2021

**3. Level- We want to see if the compay consist of all levels of people**


```python
# Create a duplicated dataframe for analysis 
df_copy = df
df_copy = df_copy.loc[~df_copy['Level'].str.contains('nan',na=False)]
df_copy['Level'].value_counts().hvplot.bar(
    title="Level Counts", xlabel='Levels', ylabel='Count', 
    width=500, height=350)

```






<div id='1002'>





  <div class="bk-root" id="7e052197-8c46-4200-8405-3296798422e3" data-root-id="1002"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"5c55cacc-cb5c-441f-bf0e-be6a5af18504":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"overlay":{"id":"1028"}},"id":"1026","type":"BoxZoomTool"},{"attributes":{},"id":"1027","type":"ResetTool"},{"attributes":{"children":[{"id":"1003"},{"id":"1007"},{"id":"1071"}],"margin":[0,0,0,0],"name":"Row01626","tags":["embedded"]},"id":"1002","type":"Row"},{"attributes":{},"id":"1047","type":"AllLabels"},{"attributes":{},"id":"1012","type":"CategoricalScale"},{"attributes":{"factors":["L2","L1","L3","L4","LM5","LM6","L0","L8","LM7"],"tags":[[["index","index",null]]]},"id":"1004","type":"FactorRange"},{"attributes":{"axis":{"id":"1016"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1018","type":"Grid"},{"attributes":{"tools":[{"id":"1006"},{"id":"1023"},{"id":"1024"},{"id":"1025"},{"id":"1026"},{"id":"1027"}]},"id":"1029","type":"Toolbar"},{"attributes":{},"id":"1020","type":"BasicTicker"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1028","type":"BoxAnnotation"},{"attributes":{},"id":"1014","type":"LinearScale"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"line_alpha":{"value":0.1},"top":{"field":"Level"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1040","type":"VBar"},{"attributes":{},"id":"1058","type":"UnionRenderers"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1049"},"group":null,"major_label_policy":{"id":"1050"},"ticker":{"id":"1020"}},"id":"1019","type":"LinearAxis"},{"attributes":{"callback":null,"renderers":[{"id":"1042"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Level","@{Level}"]]},"id":"1006","type":"HoverTool"},{"attributes":{},"id":"1017","type":"CategoricalTicker"},{"attributes":{"axis":{"id":"1019"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1022","type":"Grid"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"top":{"field":"Level"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1039","type":"VBar"},{"attributes":{},"id":"1049","type":"BasicTickFormatter"},{"attributes":{"end":65.9,"reset_end":65.9,"reset_start":0.0,"tags":[[["Level","Level",null]]]},"id":"1005","type":"Range1d"},{"attributes":{"coordinates":null,"data_source":{"id":"1036"},"glyph":{"id":"1039"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1041"},"nonselection_glyph":{"id":"1040"},"selection_glyph":{"id":"1044"},"view":{"id":"1043"}},"id":"1042","type":"GlyphRenderer"},{"attributes":{"below":[{"id":"1016"}],"center":[{"id":"1018"},{"id":"1022"}],"height":350,"left":[{"id":"1019"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1042"}],"sizing_mode":"fixed","title":{"id":"1008"},"toolbar":{"id":"1029"},"width":500,"x_range":{"id":"1004"},"x_scale":{"id":"1012"},"y_range":{"id":"1005"},"y_scale":{"id":"1014"}},"id":"1007","subtype":"Figure","type":"Plot"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"line_alpha":{"value":0.2},"top":{"field":"Level"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1041","type":"VBar"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01631","sizing_mode":"stretch_width"},"id":"1071","type":"Spacer"},{"attributes":{},"id":"1037","type":"Selection"},{"attributes":{"data":{"Level":[60,42,38,22,8,3,1,1,1],"index":["L2","L1","L3","L4","LM5","LM6","L0","L8","LM7"]},"selected":{"id":"1037"},"selection_policy":{"id":"1058"}},"id":"1036","type":"ColumnDataSource"},{"attributes":{"source":{"id":"1036"}},"id":"1043","type":"CDSView"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"Level"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1044","type":"VBar"},{"attributes":{},"id":"1023","type":"SaveTool"},{"attributes":{},"id":"1024","type":"PanTool"},{"attributes":{},"id":"1050","type":"AllLabels"},{"attributes":{},"id":"1046","type":"CategoricalTickFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01630","sizing_mode":"stretch_width"},"id":"1003","type":"Spacer"},{"attributes":{"axis_label":"Levels","coordinates":null,"formatter":{"id":"1046"},"group":null,"major_label_policy":{"id":"1047"},"ticker":{"id":"1017"}},"id":"1016","type":"CategoricalAxis"},{"attributes":{"coordinates":null,"group":null,"text":"Level Counts","text_color":"black","text_font_size":"12pt"},"id":"1008","type":"Title"},{"attributes":{},"id":"1025","type":"WheelZoomTool"}],"root_ids":["1002"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"5c55cacc-cb5c-441f-bf0e-be6a5af18504","root_ids":["1002"],"roots":{"1002":"7e052197-8c46-4200-8405-3296798422e3"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



Visualizing the data we can see that the data does not have employees of level L5, L6, and L7 (Individual Contributor). This would mean that the talent could be further motivated to achieve greater heights where they can transcend to a L5/L6/L7 employee. Understanding this, the next step is to think about how to create an environment / path that would inspired people to think out of the box.


We can also see that there is one anomaly which is L8 and its not classified under the leveling framework which is the Chief AI Officer.


**4. Department- Which department hires the most people**


```python
# Visualizing data
df_copy['Department'].value_counts().hvplot.barh(
    title="Department Hire Counts", xlabel='Departments', ylabel='Count', 
    width=500, height=350)
```






<div id='1123'>





  <div class="bk-root" id="6488cf5d-2d60-4a1d-afb2-d9ea07edf81c" data-root-id="1123"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"b4ebc73a-4c54-438f-92a6-54f8963b4f25":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1168","type":"AllLabels"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01823","sizing_mode":"stretch_width"},"id":"1124","type":"Spacer"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1161","type":"HBar"},{"attributes":{"below":[{"id":"1137"}],"center":[{"id":"1140"},{"id":"1143"}],"height":350,"left":[{"id":"1141"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1163"}],"sizing_mode":"fixed","title":{"id":"1129"},"toolbar":{"id":"1150"},"width":500,"x_range":{"id":"1125"},"x_scale":{"id":"1133"},"y_range":{"id":"1126"},"y_scale":{"id":"1135"}},"id":"1128","subtype":"Figure","type":"Plot"},{"attributes":{"source":{"id":"1157"}},"id":"1164","type":"CDSView"},{"attributes":{},"id":"1144","type":"SaveTool"},{"attributes":{},"id":"1145","type":"PanTool"},{"attributes":{},"id":"1158","type":"Selection"},{"attributes":{},"id":"1148","type":"ResetTool"},{"attributes":{"coordinates":null,"group":null,"text":"Department Hire Counts","text_color":"black","text_font_size":"12pt"},"id":"1129","type":"Title"},{"attributes":{},"id":"1146","type":"WheelZoomTool"},{"attributes":{"children":[{"id":"1124"},{"id":"1128"},{"id":"1192"}],"margin":[0,0,0,0],"name":"Row01819","tags":["embedded"]},"id":"1123","type":"Row"},{"attributes":{"coordinates":null,"data_source":{"id":"1157"},"glyph":{"id":"1160"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1162"},"nonselection_glyph":{"id":"1161"},"selection_glyph":{"id":"1165"},"view":{"id":"1164"}},"id":"1163","type":"GlyphRenderer"},{"attributes":{"overlay":{"id":"1149"}},"id":"1147","type":"BoxZoomTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01824","sizing_mode":"stretch_width"},"id":"1192","type":"Spacer"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1162","type":"HBar"},{"attributes":{},"id":"1138","type":"BasicTicker"},{"attributes":{},"id":"1167","type":"BasicTickFormatter"},{"attributes":{},"id":"1171","type":"AllLabels"},{"attributes":{},"id":"1133","type":"LinearScale"},{"attributes":{"axis_label":"Departments","coordinates":null,"formatter":{"id":"1170"},"group":null,"major_label_policy":{"id":"1171"},"ticker":{"id":"1142"}},"id":"1141","type":"CategoricalAxis"},{"attributes":{"axis":{"id":"1137"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1140","type":"Grid"},{"attributes":{"data":{"Department":[68,29,18,15,10,10,9,9,9,7,6,2,1],"index":["Product Engineering","Data Science","Product","AI Engineering","Cybersecurity","R&D Engineering","People Ops","Data Engineering","AI Service Delivery","Marketing Analytics","Information Technology Services","Information Security Audit","Finance"]},"selected":{"id":"1158"},"selection_policy":{"id":"1179"}},"id":"1157","type":"ColumnDataSource"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1165","type":"HBar"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1149","type":"BoxAnnotation"},{"attributes":{},"id":"1135","type":"CategoricalScale"},{"attributes":{"callback":null,"renderers":[{"id":"1163"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Department","@{Department}"]]},"id":"1127","type":"HoverTool"},{"attributes":{},"id":"1142","type":"CategoricalTicker"},{"attributes":{"end":74.7,"reset_end":74.7,"reset_start":0.0,"tags":[[["Department","Department",null]]]},"id":"1125","type":"Range1d"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1167"},"group":null,"major_label_policy":{"id":"1168"},"ticker":{"id":"1138"}},"id":"1137","type":"LinearAxis"},{"attributes":{},"id":"1170","type":"CategoricalTickFormatter"},{"attributes":{"factors":["Product Engineering","Data Science","Product","AI Engineering","Cybersecurity","R&D Engineering","People Ops","Data Engineering","AI Service Delivery","Marketing Analytics","Information Technology Services","Information Security Audit","Finance"],"tags":[[["index","index",null]]]},"id":"1126","type":"FactorRange"},{"attributes":{},"id":"1179","type":"UnionRenderers"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1160","type":"HBar"},{"attributes":{"axis":{"id":"1141"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1143","type":"Grid"},{"attributes":{"tools":[{"id":"1127"},{"id":"1144"},{"id":"1145"},{"id":"1146"},{"id":"1147"},{"id":"1148"}]},"id":"1150","type":"Toolbar"}],"root_ids":["1123"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"b4ebc73a-4c54-438f-92a6-54f8963b4f25","root_ids":["1123"],"roots":{"1123":"6488cf5d-2d60-4a1d-afb2-d9ea07edf81c"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



As a whole Product Engineers are hired the most. This shows where the company's emphasis and vision is on, making sure the application/ platform is properly configured for consumers.

**5. Cost Centers- Which cost center has the most head counts?**



```python
df_copy['Cost Center'].value_counts().hvplot.bar(
    title="Cost Center Head Counts", xlabel='Cost Center', ylabel='Count', 
    width=500, height=350)
```






<div id='1244'>





  <div class="bk-root" id="ed3d891f-08e0-434b-933e-a33031859b48" data-root-id="1244"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"680cf32c-2af6-466c-ac0d-58f3a873f031":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1262","type":"BasicTicker"},{"attributes":{"children":[{"id":"1245"},{"id":"1249"},{"id":"1313"}],"margin":[0,0,0,0],"name":"Row01968","tags":["embedded"]},"id":"1244","type":"Row"},{"attributes":{"tools":[{"id":"1248"},{"id":"1265"},{"id":"1266"},{"id":"1267"},{"id":"1268"},{"id":"1269"}]},"id":"1271","type":"Toolbar"},{"attributes":{"axis":{"id":"1261"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1264","type":"Grid"},{"attributes":{"below":[{"id":"1258"}],"center":[{"id":"1260"},{"id":"1264"}],"height":350,"left":[{"id":"1261"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1284"}],"sizing_mode":"fixed","title":{"id":"1250"},"toolbar":{"id":"1271"},"width":500,"x_range":{"id":"1246"},"x_scale":{"id":"1254"},"y_range":{"id":"1247"},"y_scale":{"id":"1256"}},"id":"1249","subtype":"Figure","type":"Plot"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"line_alpha":{"value":0.1},"top":{"field":"Cost_Center"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1282","type":"VBar"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01972","sizing_mode":"stretch_width"},"id":"1245","type":"Spacer"},{"attributes":{"coordinates":null,"data_source":{"id":"1278"},"glyph":{"id":"1281"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1283"},"nonselection_glyph":{"id":"1282"},"selection_glyph":{"id":"1286"},"view":{"id":"1285"}},"id":"1284","type":"GlyphRenderer"},{"attributes":{"callback":null,"renderers":[{"id":"1284"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Cost Center","@{Cost_Center}"]]},"id":"1248","type":"HoverTool"},{"attributes":{},"id":"1279","type":"Selection"},{"attributes":{"coordinates":null,"group":null,"text":"Cost Center Head Counts","text_color":"black","text_font_size":"12pt"},"id":"1250","type":"Title"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"line_alpha":{"value":0.2},"top":{"field":"Cost_Center"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1283","type":"VBar"},{"attributes":{"axis_label":"Cost Center","coordinates":null,"formatter":{"id":"1288"},"group":null,"major_label_policy":{"id":"1289"},"ticker":{"id":"1259"}},"id":"1258","type":"CategoricalAxis"},{"attributes":{"source":{"id":"1278"}},"id":"1285","type":"CDSView"},{"attributes":{},"id":"1265","type":"SaveTool"},{"attributes":{},"id":"1266","type":"PanTool"},{"attributes":{},"id":"1269","type":"ResetTool"},{"attributes":{"factors":["Engineering","AI & Data Services","Security","Office Management "],"tags":[[["index","index",null]]]},"id":"1246","type":"FactorRange"},{"attributes":{"data":{"Cost_Center":[86,79,18,10],"index":["Engineering","AI & Data Services","Security","Office Management "]},"selected":{"id":"1279"},"selection_policy":{"id":"1300"}},"id":"1278","type":"ColumnDataSource"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"top":{"field":"Cost_Center"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1281","type":"VBar"},{"attributes":{},"id":"1267","type":"WheelZoomTool"},{"attributes":{},"id":"1288","type":"CategoricalTickFormatter"},{"attributes":{"overlay":{"id":"1270"}},"id":"1268","type":"BoxZoomTool"},{"attributes":{},"id":"1289","type":"AllLabels"},{"attributes":{},"id":"1300","type":"UnionRenderers"},{"attributes":{},"id":"1254","type":"CategoricalScale"},{"attributes":{"axis":{"id":"1258"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1260","type":"Grid"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1291"},"group":null,"major_label_policy":{"id":"1292"},"ticker":{"id":"1262"}},"id":"1261","type":"LinearAxis"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"Cost_Center"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1286","type":"VBar"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer01973","sizing_mode":"stretch_width"},"id":"1313","type":"Spacer"},{"attributes":{},"id":"1256","type":"LinearScale"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1270","type":"BoxAnnotation"},{"attributes":{},"id":"1291","type":"BasicTickFormatter"},{"attributes":{},"id":"1292","type":"AllLabels"},{"attributes":{},"id":"1259","type":"CategoricalTicker"},{"attributes":{"end":93.6,"reset_end":93.6,"reset_start":0.0,"tags":[[["Cost Center","Cost Center",null]]]},"id":"1247","type":"Range1d"}],"root_ids":["1244"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"680cf32c-2af6-466c-ac0d-58f3a873f031","root_ids":["1244"],"roots":{"1244":"ed3d891f-08e0-434b-933e-a33031859b48"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



**6. Employment Status**


```python
df_copy['Employment Status'].value_counts().hvplot.area(
    title="Department Hire Counts", xlabel='Departments', ylabel='Count', 
    width=500, height=350).opts(bgcolor='#fbecc4')
```






<div id='1365'>





  <div class="bk-root" id="5db3443c-7dad-49f6-a8cd-71c03b6094db" data-root-id="1365"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"9444a2a9-4f2c-4742-98d4-539a4a5d3e37":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1377","type":"LinearScale"},{"attributes":{},"id":"1413","type":"AllLabels"},{"attributes":{},"id":"1383","type":"BasicTicker"},{"attributes":{},"id":"1380","type":"CategoricalTicker"},{"attributes":{"tools":[{"id":"1369"},{"id":"1386"},{"id":"1387"},{"id":"1388"},{"id":"1389"},{"id":"1390"}]},"id":"1392","type":"Toolbar"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1412"},"group":null,"major_label_policy":{"id":"1413"},"ticker":{"id":"1383"}},"id":"1382","type":"LinearAxis"},{"attributes":{"fill_alpha":1,"fill_color":"#30a2da","hatch_alpha":0.1,"hatch_color":"#30a2da","line_alpha":1,"x":{"field":"x"},"y":{"field":"y"}},"id":"1403","type":"Patch"},{"attributes":{},"id":"1412","type":"BasicTickFormatter"},{"attributes":{"axis":{"id":"1382"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1385","type":"Grid"},{"attributes":{"callback":null,"renderers":[{"id":"1405"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Employment Status","@{Employment_Status}"]]},"id":"1369","type":"HoverTool"},{"attributes":{"factors":["Full-Time","Probation","Internship","Contractor","Extended Probation"],"tags":[[["index","index",null]]]},"id":"1367","type":"FactorRange"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02167","sizing_mode":"stretch_width"},"id":"1366","type":"Spacer"},{"attributes":{"source":{"id":"1399"}},"id":"1406","type":"CDSView"},{"attributes":{},"id":"1409","type":"CategoricalTickFormatter"},{"attributes":{"end":150.6,"reset_end":150.6,"reset_start":0.0,"tags":[[["Employment Status","Employment Status",null]]]},"id":"1368","type":"Range1d"},{"attributes":{"fill_alpha":0.2,"fill_color":"#30a2da","hatch_alpha":0.2,"hatch_color":"#30a2da","line_alpha":0.2,"x":{"field":"x"},"y":{"field":"y"}},"id":"1404","type":"Patch"},{"attributes":{"fill_color":"#30a2da","hatch_color":"#30a2da","x":{"field":"x"},"y":{"field":"y"}},"id":"1407","type":"Patch"},{"attributes":{"coordinates":null,"data_source":{"id":"1399"},"glyph":{"id":"1402"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1404"},"nonselection_glyph":{"id":"1403"},"selection_glyph":{"id":"1407"},"view":{"id":"1406"}},"id":"1405","type":"GlyphRenderer"},{"attributes":{"coordinates":null,"group":null,"text":"Department Hire Counts","text_color":"black","text_font_size":"12pt"},"id":"1371","type":"Title"},{"attributes":{"data":{"x":["Full-Time","Probation","Internship","Contractor","Extended Probation","Extended Probation","Contractor","Internship","Probation","Full-Time"],"y":{"__ndarray__":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPA/AAAAAAAACEAAAAAAAAAIQAAAAAAAgEZAAAAAAAAgYUA=","dtype":"float64","order":"little","shape":[10]}},"selected":{"id":"1400"},"selection_policy":{"id":"1421"}},"id":"1399","type":"ColumnDataSource"},{"attributes":{},"id":"1386","type":"SaveTool"},{"attributes":{},"id":"1421","type":"UnionRenderers"},{"attributes":{},"id":"1387","type":"PanTool"},{"attributes":{},"id":"1410","type":"AllLabels"},{"attributes":{},"id":"1390","type":"ResetTool"},{"attributes":{"fill_color":"#30a2da","hatch_color":"#30a2da","x":{"field":"x"},"y":{"field":"y"}},"id":"1402","type":"Patch"},{"attributes":{},"id":"1388","type":"WheelZoomTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02168","sizing_mode":"stretch_width"},"id":"1434","type":"Spacer"},{"attributes":{"overlay":{"id":"1391"}},"id":"1389","type":"BoxZoomTool"},{"attributes":{},"id":"1400","type":"Selection"},{"attributes":{"background_fill_color":"#fbecc4","below":[{"id":"1379"}],"center":[{"id":"1381"},{"id":"1385"}],"height":350,"left":[{"id":"1382"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1405"}],"sizing_mode":"fixed","title":{"id":"1371"},"toolbar":{"id":"1392"},"width":500,"x_range":{"id":"1367"},"x_scale":{"id":"1375"},"y_range":{"id":"1368"},"y_scale":{"id":"1377"}},"id":"1370","subtype":"Figure","type":"Plot"},{"attributes":{"axis_label":"Departments","coordinates":null,"formatter":{"id":"1409"},"group":null,"major_label_policy":{"id":"1410"},"ticker":{"id":"1380"}},"id":"1379","type":"CategoricalAxis"},{"attributes":{"children":[{"id":"1366"},{"id":"1370"},{"id":"1434"}],"margin":[0,0,0,0],"name":"Row02163","tags":["embedded"]},"id":"1365","type":"Row"},{"attributes":{},"id":"1375","type":"CategoricalScale"},{"attributes":{"axis":{"id":"1379"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1381","type":"Grid"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1391","type":"BoxAnnotation"}],"root_ids":["1365"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"9444a2a9-4f2c-4742-98d4-539a4a5d3e37","root_ids":["1365"],"roots":{"1365":"5db3443c-7dad-49f6-a8cd-71c03b6094db"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



Most of the employees are hired fulltime and very few are on a contractual basis. While having a lot of fulltimers is great when there is a lot of work to be done, an important factor to consider is whether it increases the productivity and worth the ROI. 

### C. Exploratory Data Analysis 

**1. Ratio for Level against Department & Cost Center**

To compare the distribution of Levels between Department and Cost Centers we split it to two groups:
   - low_level (L1 - L4)
   - high_level (LM5 - LM7)

**i) Level v Department**


```python
# Copy data file
df_1 = df
# Remove Hiring Source 
df_1.drop(['Hiring Source'], axis=1, inplace = True)
# Drop Na from data after Hiring Source is removed to remove empty 'level' columns
df_1 = df_1.dropna()
```


```python
# Creat a new dataframe with the counted values for each level to department 
lvd_df = pd.DataFrame({'Count' : df_1.groupby( ["Department", "Level"] ).size()}).reset_index()
lvd_df.hvplot.table(columns=['Department', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='1486'>





  <div class="bk-root" id="4b8981b5-b267-4a75-bab7-86c479814048" data-root-id="1486"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"fc48246b-4219-4d0c-a67b-e789daf29858":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1501","type":"IntEditor"},{"attributes":{"editor":{"id":"1496"},"field":"Level","formatter":{"id":"1495"},"title":"Level"},"id":"1497","type":"TableColumn"},{"attributes":{},"id":"1489","type":"Selection"},{"attributes":{},"id":"1500","type":"NumberFormatter"},{"attributes":{"columns":[{"id":"1492"},{"id":"1497"},{"id":"1502"}],"reorderable":false,"source":{"id":"1488"},"view":{"id":"1507"},"width":800},"id":"1505","type":"DataTable"},{"attributes":{"data":{"Count":[1,5,2,5,1,1,3,3,1,1,1,3,4,1,1,1,3,3,2,1,16,3,3,1,1,1,1,2,2,2,3,2,2,1,1,4,1,1,2,5,8,1,2,7,26,14,9,2,4,2,1,2,1],"Department":["AI Engineering","AI Engineering","AI Engineering","AI Engineering","AI Engineering","AI Engineering","AI Service Delivery","AI Service Delivery","AI Service Delivery","AI Service Delivery","AI Service Delivery","Cybersecurity","Cybersecurity","Cybersecurity","Cybersecurity","Cybersecurity","Data Engineering","Data Engineering","Data Engineering","Data Engineering","Data Science","Data Science","Data Science","Data Science","Finance","Information Security Audit","Information Security Audit","Information Technology Services","Information Technology Services","Information Technology Services","Marketing Analytics","Marketing Analytics","Marketing Analytics","People Ops","People Ops","People Ops","People Ops","People Ops","Product","Product","Product","Product","Product","Product Engineering","Product Engineering","Product Engineering","Product Engineering","Product Engineering","R&D Engineering","R&D Engineering","R&D Engineering","R&D Engineering","R&D Engineering"],"Level":["L1","L2","L3","L4","L8","LM6","L2","L3","L4","LM5","LM7","L1","L2","L3","L4","LM6","L1","L2","L3","LM5","L1","L2","L3","LM6","L3","L2","L4","L1","L2","L4","L1","L2","L3","L0","L1","L2","L3","LM5","L1","L2","L3","L4","LM5","L1","L2","L3","L4","LM5","L1","L2","L3","L4","LM5"]},"selected":{"id":"1489"},"selection_policy":{"id":"1508"}},"id":"1488","type":"ColumnDataSource"},{"attributes":{},"id":"1491","type":"StringEditor"},{"attributes":{},"id":"1490","type":"StringFormatter"},{"attributes":{"editor":{"id":"1501"},"field":"Count","formatter":{"id":"1500"},"title":"Count"},"id":"1502","type":"TableColumn"},{"attributes":{"children":[{"id":"1487"},{"id":"1505"},{"id":"1512"}],"margin":[0,0,0,0],"name":"Row02325","tags":["embedded"]},"id":"1486","type":"Row"},{"attributes":{"editor":{"id":"1491"},"field":"Department","formatter":{"id":"1490"},"title":"Department"},"id":"1492","type":"TableColumn"},{"attributes":{},"id":"1495","type":"StringFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02329","sizing_mode":"stretch_width"},"id":"1487","type":"Spacer"},{"attributes":{},"id":"1508","type":"UnionRenderers"},{"attributes":{},"id":"1496","type":"StringEditor"},{"attributes":{"source":{"id":"1488"}},"id":"1507","type":"CDSView"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02330","sizing_mode":"stretch_width"},"id":"1512","type":"Spacer"}],"root_ids":["1486"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"fc48246b-4219-4d0c-a67b-e789daf29858","root_ids":["1486"],"roots":{"1486":"4b8981b5-b267-4a75-bab7-86c479814048"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



Splitting the Data between two groups (L1-l4) & (LM5 - LM7), namely low_level_df & high_level_df


```python
search_values_low = ['L0','L1','L2','L3','L4']
low_level_df = lvd_df[lvd_df.Level.str.contains('|'.join(search_values_low))]
```


```python
search_values_high = ['LM5','LM6','LM7']
high_level_df = lvd_df[lvd_df.Level.str.contains('|'.join(search_values_high))]
```
Ratio between the two groups 

```python
def calculate_ratio(x,y,z): 
    low_sum = x['Count'].sum()
    high_sum = y['Count'].sum()
    total_sum = z['Count'].sum()
    low_ratio = round((low_sum / total_sum)*100,2)
    high_ratio = round((high_sum / total_sum)*100,2)
    print('Low Level Percentage:',low_ratio)
    print('High Level Percentage:', high_ratio)
calculate_ratio(low_level_df,high_level_df,lvd_df)
```

    Low Level Percentage: 92.05
    High Level Percentage: 6.82
    

About 92.05% of employess belong in the lower level group as compared 6.82% in the upper level.

**ii) Level comparison for low and high for each Department**


```python
# We first take a look at all the departments available
lvd_df['Department'].unique()
```




    array(['AI Engineering', 'AI Service Delivery', 'Cybersecurity',
           'Data Engineering', 'Data Science', 'Finance',
           'Information Security Audit', 'Information Technology Services',
           'Marketing Analytics', 'People Ops', 'Product',
           'Product Engineering', 'R&D Engineering'], dtype=object)




```python
# Write a function for the comparison 
def low_high_comparison(x,low,high,value): 
    search_values = [x]
    low_df = low[low[value].str.contains('|'.join(search_values))]
    low_com = low_df['Count'].sum()
    high_df = high[high[value].str.contains('|'.join(search_values))]
    high_com = high_df['Count'].sum()
    total_com = low_com + high_com
    low_level_perc = round((low_com / total_com)* 100,2)
    high_level_perc = round((high_com / total_com)* 100,2)
    print('%s Low Level Percentage:'%x,low_level_perc)
    print('%s High Level Percentage:'%x,high_level_perc)
    print('===========================================')
```


```python
for x in lvd_df['Department'].unique():
    low_high_comparison(x,low_level_df,high_level_df,'Department')
```

    AI Engineering Low Level Percentage: 92.86
    AI Engineering High Level Percentage: 7.14
    ===========================================
    AI Service Delivery Low Level Percentage: 77.78
    AI Service Delivery High Level Percentage: 22.22
    ===========================================
    Cybersecurity Low Level Percentage: 90.0
    Cybersecurity High Level Percentage: 10.0
    ===========================================
    Data Engineering Low Level Percentage: 88.89
    Data Engineering High Level Percentage: 11.11
    ===========================================
    Data Science Low Level Percentage: 95.65
    Data Science High Level Percentage: 4.35
    ===========================================
    Finance Low Level Percentage: 100.0
    Finance High Level Percentage: 0.0
    ===========================================
    Information Security Audit Low Level Percentage: 100.0
    Information Security Audit High Level Percentage: 0.0
    ===========================================
    Information Technology Services Low Level Percentage: 100.0
    Information Technology Services High Level Percentage: 0.0
    ===========================================
    Marketing Analytics Low Level Percentage: 100.0
    Marketing Analytics High Level Percentage: 0.0
    ===========================================
    People Ops Low Level Percentage: 87.5
    People Ops High Level Percentage: 12.5
    ===========================================
    Product Low Level Percentage: 94.74
    Product High Level Percentage: 5.26
    ===========================================
    Product Engineering Low Level Percentage: 96.55
    Product Engineering High Level Percentage: 3.45
    ===========================================
    R&D Engineering Low Level Percentage: 90.0
    R&D Engineering High Level Percentage: 10.0
    ===========================================
    

Most low level departments are above the 85 percentile except for AI Service Delivery.

**iii) Low Level Analysis (L1-L4)** 


```python
low_level_df.hvplot.table(columns=['Department', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='1524'>





  <div class="bk-root" id="8fe06d38-36f4-4757-b96c-f0dd39e8dc4b" data-root-id="1524"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"2276214f-940d-4ef9-a918-34d4e573d108":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"columns":[{"id":"1530"},{"id":"1535"},{"id":"1540"}],"reorderable":false,"source":{"id":"1526"},"view":{"id":"1545"},"width":800},"id":"1543","type":"DataTable"},{"attributes":{"data":{"Count":[1,5,2,5,3,3,1,3,4,1,1,3,3,2,16,3,3,1,1,1,2,2,2,3,2,2,1,1,4,1,2,5,8,1,7,26,14,9,4,2,1,2],"Department":["AI Engineering","AI Engineering","AI Engineering","AI Engineering","AI Service Delivery","AI Service Delivery","AI Service Delivery","Cybersecurity","Cybersecurity","Cybersecurity","Cybersecurity","Data Engineering","Data Engineering","Data Engineering","Data Science","Data Science","Data Science","Finance","Information Security Audit","Information Security Audit","Information Technology Services","Information Technology Services","Information Technology Services","Marketing Analytics","Marketing Analytics","Marketing Analytics","People Ops","People Ops","People Ops","People Ops","Product","Product","Product","Product","Product Engineering","Product Engineering","Product Engineering","Product Engineering","R&D Engineering","R&D Engineering","R&D Engineering","R&D Engineering"],"Level":["L1","L2","L3","L4","L2","L3","L4","L1","L2","L3","L4","L1","L2","L3","L1","L2","L3","L3","L2","L4","L1","L2","L4","L1","L2","L3","L0","L1","L2","L3","L1","L2","L3","L4","L1","L2","L3","L4","L1","L2","L3","L4"]},"selected":{"id":"1527"},"selection_policy":{"id":"1546"}},"id":"1526","type":"ColumnDataSource"},{"attributes":{},"id":"1539","type":"IntEditor"},{"attributes":{"children":[{"id":"1525"},{"id":"1543"},{"id":"1550"}],"margin":[0,0,0,0],"name":"Row02422","tags":["embedded"]},"id":"1524","type":"Row"},{"attributes":{"editor":{"id":"1539"},"field":"Count","formatter":{"id":"1538"},"title":"Count"},"id":"1540","type":"TableColumn"},{"attributes":{},"id":"1528","type":"StringFormatter"},{"attributes":{},"id":"1546","type":"UnionRenderers"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02427","sizing_mode":"stretch_width"},"id":"1550","type":"Spacer"},{"attributes":{"editor":{"id":"1529"},"field":"Department","formatter":{"id":"1528"},"title":"Department"},"id":"1530","type":"TableColumn"},{"attributes":{},"id":"1529","type":"StringEditor"},{"attributes":{},"id":"1533","type":"StringFormatter"},{"attributes":{},"id":"1534","type":"StringEditor"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02426","sizing_mode":"stretch_width"},"id":"1525","type":"Spacer"},{"attributes":{},"id":"1538","type":"NumberFormatter"},{"attributes":{},"id":"1527","type":"Selection"},{"attributes":{"editor":{"id":"1534"},"field":"Level","formatter":{"id":"1533"},"title":"Level"},"id":"1535","type":"TableColumn"},{"attributes":{"source":{"id":"1526"}},"id":"1545","type":"CDSView"}],"root_ids":["1524"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"2276214f-940d-4ef9-a918-34d4e573d108","root_ids":["1524"],"roots":{"1524":"8fe06d38-36f4-4757-b96c-f0dd39e8dc4b"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



From the table above we can sort 'Level/Department/Count' by clickng on it to see the distribution and amount of employees in Department v Levels. 


```python
# Create new Dataframe to see which department hired the most for (L1-L4)
low_level_df_2 = df_1[df_1.Level.str.contains('|'.join(search_values_low))]
```


```python
low_level_df_2['Department'].value_counts().hvplot.barh(
    title="Department Hires Low", xlabel='Department', ylabel='Count', 
    width=500, height=350)
```






<div id='1562'>





  <div class="bk-root" id="5b6c0766-f20b-4d49-b606-e6c9d44878a9" data-root-id="1562"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"3dba32d8-dbce-4848-9a3e-d747c1e08e8b":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"axis":{"id":"1580"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1582","type":"Grid"},{"attributes":{},"id":"1572","type":"LinearScale"},{"attributes":{"source":{"id":"1596"}},"id":"1603","type":"CDSView"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1599","type":"HBar"},{"attributes":{},"id":"1610","type":"AllLabels"},{"attributes":{},"id":"1618","type":"UnionRenderers"},{"attributes":{"coordinates":null,"data_source":{"id":"1596"},"glyph":{"id":"1599"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1601"},"nonselection_glyph":{"id":"1600"},"selection_glyph":{"id":"1604"},"view":{"id":"1603"}},"id":"1602","type":"GlyphRenderer"},{"attributes":{"tools":[{"id":"1566"},{"id":"1583"},{"id":"1584"},{"id":"1585"},{"id":"1586"},{"id":"1587"}]},"id":"1589","type":"Toolbar"},{"attributes":{"callback":null,"renderers":[{"id":"1602"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Department","@{Department}"]]},"id":"1566","type":"HoverTool"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1600","type":"HBar"},{"attributes":{},"id":"1577","type":"BasicTicker"},{"attributes":{},"id":"1583","type":"SaveTool"},{"attributes":{},"id":"1584","type":"PanTool"},{"attributes":{"axis_label":"Department","coordinates":null,"formatter":{"id":"1609"},"group":null,"major_label_policy":{"id":"1610"},"ticker":{"id":"1581"}},"id":"1580","type":"CategoricalAxis"},{"attributes":{},"id":"1587","type":"ResetTool"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1601","type":"HBar"},{"attributes":{},"id":"1585","type":"WheelZoomTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02585","sizing_mode":"stretch_width"},"id":"1631","type":"Spacer"},{"attributes":{},"id":"1606","type":"BasicTickFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02584","sizing_mode":"stretch_width"},"id":"1563","type":"Spacer"},{"attributes":{"overlay":{"id":"1588"}},"id":"1586","type":"BoxZoomTool"},{"attributes":{},"id":"1609","type":"CategoricalTickFormatter"},{"attributes":{},"id":"1607","type":"AllLabels"},{"attributes":{"data":{"Department":[56,22,16,13,9,9,8,7,7,7,6,2,1],"index":["Product Engineering","Data Science","Product","AI Engineering","Cybersecurity","R&D Engineering","Data Engineering","Marketing Analytics","AI Service Delivery","People Ops","Information Technology Services","Information Security Audit","Finance"]},"selected":{"id":"1597"},"selection_policy":{"id":"1618"}},"id":"1596","type":"ColumnDataSource"},{"attributes":{"axis":{"id":"1576"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1579","type":"Grid"},{"attributes":{"below":[{"id":"1576"}],"center":[{"id":"1579"},{"id":"1582"}],"height":350,"left":[{"id":"1580"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1602"}],"sizing_mode":"fixed","title":{"id":"1568"},"toolbar":{"id":"1589"},"width":500,"x_range":{"id":"1564"},"x_scale":{"id":"1572"},"y_range":{"id":"1565"},"y_scale":{"id":"1574"}},"id":"1567","subtype":"Figure","type":"Plot"},{"attributes":{"coordinates":null,"group":null,"text":"Department Hires Low","text_color":"black","text_font_size":"12pt"},"id":"1568","type":"Title"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1588","type":"BoxAnnotation"},{"attributes":{},"id":"1574","type":"CategoricalScale"},{"attributes":{},"id":"1581","type":"CategoricalTicker"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1606"},"group":null,"major_label_policy":{"id":"1607"},"ticker":{"id":"1577"}},"id":"1576","type":"LinearAxis"},{"attributes":{"factors":["Product Engineering","Data Science","Product","AI Engineering","Cybersecurity","R&D Engineering","Data Engineering","Marketing Analytics","AI Service Delivery","People Ops","Information Technology Services","Information Security Audit","Finance"],"tags":[[["index","index",null]]]},"id":"1565","type":"FactorRange"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1604","type":"HBar"},{"attributes":{"end":61.5,"reset_end":61.5,"reset_start":0.0,"tags":[[["Department","Department",null]]]},"id":"1564","type":"Range1d"},{"attributes":{"children":[{"id":"1563"},{"id":"1567"},{"id":"1631"}],"margin":[0,0,0,0],"name":"Row02580","tags":["embedded"]},"id":"1562","type":"Row"},{"attributes":{},"id":"1597","type":"Selection"}],"root_ids":["1562"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"3dba32d8-dbce-4848-9a3e-d747c1e08e8b","root_ids":["1562"],"roots":{"1562":"5b6c0766-f20b-4d49-b606-e6c9d44878a9"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



From the Barh Chart we can hover over the diagram to see the exact number of hire in each department. 


```python
# Histogram to see department distribution by levels
sns.displot(low_level_df_2, x="Level", hue="Department",multiple="stack",height=15,palette='Paired')
```




    <seaborn.axisgrid.FacetGrid at 0x15817318f88>




![png](output_52_1.png)


Low Level Findings: 
    1. Most L1 hires are Data Scientist 
    2. Most L2 & L3 & L4 hires are Product Engineers 
    3. Most employees fall under the level ranking of L2.

**iv) High Level Analysis (LM5-LM7)**


```python
high_level_df.hvplot.table(columns=['Department', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=300)
```






<div id='1683'>





  <div class="bk-root" id="de62c1d9-9583-4795-978f-57450b8b6dc7" data-root-id="1683"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"f6dc0881-3f44-4bd6-bcf9-8a48f6f4b052":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1698","type":"IntEditor"},{"attributes":{"editor":{"id":"1693"},"field":"Level","formatter":{"id":"1692"},"title":"Level"},"id":"1694","type":"TableColumn"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02716","sizing_mode":"stretch_width"},"id":"1684","type":"Spacer"},{"attributes":{"data":{"Count":[1,1,1,1,1,1,1,2,2,1],"Department":["AI Engineering","AI Service Delivery","AI Service Delivery","Cybersecurity","Data Engineering","Data Science","People Ops","Product","Product Engineering","R&D Engineering"],"Level":["LM6","LM5","LM7","LM6","LM5","LM6","LM5","LM5","LM5","LM5"]},"selected":{"id":"1686"},"selection_policy":{"id":"1705"}},"id":"1685","type":"ColumnDataSource"},{"attributes":{},"id":"1697","type":"NumberFormatter"},{"attributes":{},"id":"1686","type":"Selection"},{"attributes":{},"id":"1705","type":"UnionRenderers"},{"attributes":{"columns":[{"id":"1689"},{"id":"1694"},{"id":"1699"}],"height":300,"reorderable":false,"source":{"id":"1685"},"view":{"id":"1704"},"width":800},"id":"1702","type":"DataTable"},{"attributes":{"children":[{"id":"1684"},{"id":"1702"},{"id":"1709"}],"margin":[0,0,0,0],"name":"Row02712","tags":["embedded"]},"id":"1683","type":"Row"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02717","sizing_mode":"stretch_width"},"id":"1709","type":"Spacer"},{"attributes":{},"id":"1688","type":"StringEditor"},{"attributes":{"editor":{"id":"1698"},"field":"Count","formatter":{"id":"1697"},"title":"Count"},"id":"1699","type":"TableColumn"},{"attributes":{},"id":"1687","type":"StringFormatter"},{"attributes":{"editor":{"id":"1688"},"field":"Department","formatter":{"id":"1687"},"title":"Department"},"id":"1689","type":"TableColumn"},{"attributes":{},"id":"1692","type":"StringFormatter"},{"attributes":{},"id":"1693","type":"StringEditor"},{"attributes":{"source":{"id":"1685"}},"id":"1704","type":"CDSView"}],"root_ids":["1683"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"f6dc0881-3f44-4bd6-bcf9-8a48f6f4b052","root_ids":["1683"],"roots":{"1683":"de62c1d9-9583-4795-978f-57450b8b6dc7"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



The High Level Department are evenly spread across the top 5 most employed departments. This mean for the top 5 departments, each of them have a leader to guide and distribute knowledge for their respective field. The only department with average hires that lacks a leader is the Marketing Analytics department. 


```python
# Create new Dataframe to see which department hired the most for (L1-L4)
high_level_df_2 = df_1[df_1.Level.str.contains('|'.join(search_values_high))]
high_level_df_2['Department'].value_counts().hvplot.barh(
    title="Department Hires High", xlabel='Department', ylabel='Count', 
    width=500, height=350)
```






<div id='1721'>





  <div class="bk-root" id="57b41b2d-38e9-4136-be92-dd985b6c39fb" data-root-id="1721"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"b2de1261-21ef-4c2b-9159-2d55ba8364ef":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"axis":{"id":"1735"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1738","type":"Grid"},{"attributes":{"axis_label":"Department","coordinates":null,"formatter":{"id":"1768"},"group":null,"major_label_policy":{"id":"1769"},"ticker":{"id":"1740"}},"id":"1739","type":"CategoricalAxis"},{"attributes":{"factors":["Product Engineering","AI Service Delivery","Product","People Ops","Data Science","AI Engineering","Data Engineering","Cybersecurity","R&D Engineering"],"tags":[[["index","index",null]]]},"id":"1724","type":"FactorRange"},{"attributes":{},"id":"1740","type":"CategoricalTicker"},{"attributes":{"source":{"id":"1755"}},"id":"1762","type":"CDSView"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1763","type":"HBar"},{"attributes":{},"id":"1777","type":"UnionRenderers"},{"attributes":{"axis":{"id":"1739"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1741","type":"Grid"},{"attributes":{"callback":null,"renderers":[{"id":"1761"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Department","@{Department}"]]},"id":"1725","type":"HoverTool"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1759","type":"HBar"},{"attributes":{},"id":"1765","type":"BasicTickFormatter"},{"attributes":{"children":[{"id":"1722"},{"id":"1726"},{"id":"1790"}],"margin":[0,0,0,0],"name":"Row02870","tags":["embedded"]},"id":"1721","type":"Row"},{"attributes":{"below":[{"id":"1735"}],"center":[{"id":"1738"},{"id":"1741"}],"height":350,"left":[{"id":"1739"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1761"}],"sizing_mode":"fixed","title":{"id":"1727"},"toolbar":{"id":"1748"},"width":500,"x_range":{"id":"1723"},"x_scale":{"id":"1731"},"y_range":{"id":"1724"},"y_scale":{"id":"1733"}},"id":"1726","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"1769","type":"AllLabels"},{"attributes":{"coordinates":null,"data_source":{"id":"1755"},"glyph":{"id":"1758"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1760"},"nonselection_glyph":{"id":"1759"},"selection_glyph":{"id":"1763"},"view":{"id":"1762"}},"id":"1761","type":"GlyphRenderer"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02874","sizing_mode":"stretch_width"},"id":"1722","type":"Spacer"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1758","type":"HBar"},{"attributes":{},"id":"1768","type":"CategoricalTickFormatter"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Department"},"y":{"field":"index"}},"id":"1760","type":"HBar"},{"attributes":{},"id":"1742","type":"SaveTool"},{"attributes":{"data":{"Department":[2,2,2,1,1,1,1,1,1],"index":["Product Engineering","AI Service Delivery","Product","People Ops","Data Science","AI Engineering","Data Engineering","Cybersecurity","R&D Engineering"]},"selected":{"id":"1756"},"selection_policy":{"id":"1777"}},"id":"1755","type":"ColumnDataSource"},{"attributes":{},"id":"1743","type":"PanTool"},{"attributes":{"coordinates":null,"group":null,"text":"Department Hires High","text_color":"black","text_font_size":"12pt"},"id":"1727","type":"Title"},{"attributes":{},"id":"1746","type":"ResetTool"},{"attributes":{},"id":"1744","type":"WheelZoomTool"},{"attributes":{},"id":"1756","type":"Selection"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer02875","sizing_mode":"stretch_width"},"id":"1790","type":"Spacer"},{"attributes":{"end":2.1,"reset_end":2.1,"reset_start":0.0,"tags":[[["Department","Department",null]]]},"id":"1723","type":"Range1d"},{"attributes":{"overlay":{"id":"1747"}},"id":"1745","type":"BoxZoomTool"},{"attributes":{},"id":"1766","type":"AllLabels"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1747","type":"BoxAnnotation"},{"attributes":{},"id":"1731","type":"LinearScale"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1765"},"group":null,"major_label_policy":{"id":"1766"},"ticker":{"id":"1736"}},"id":"1735","type":"LinearAxis"},{"attributes":{"tools":[{"id":"1725"},{"id":"1742"},{"id":"1743"},{"id":"1744"},{"id":"1745"},{"id":"1746"}]},"id":"1748","type":"Toolbar"},{"attributes":{},"id":"1736","type":"BasicTicker"},{"attributes":{},"id":"1733","type":"CategoricalScale"}],"root_ids":["1721"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"b2de1261-21ef-4c2b-9159-2d55ba8364ef","root_ids":["1721"],"roots":{"1721":"57b41b2d-38e9-4136-be92-dd985b6c39fb"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Histogram to see department distribution by levels
sns.displot(high_level_df_2, x="Level", hue="Department",multiple="stack",height=8,palette='Set2')
```




    <seaborn.axisgrid.FacetGrid at 0x1581cbae888>




![png](output_58_1.png)


High Level Findings
    1. There are no Individual Contributors (L5-L7) 
    2. Most high level employees are LM5
    3. Marketing Analystics lack a leader

**v) Level v Cost Centers**


```python
# Creat a new dataframe with the counted values for each level to Cost Center 
lvc_df = pd.DataFrame({'Count' : df_1.groupby( ["Cost Center", "Level"] ).size()}).reset_index()
lvc_df.hvplot.table(columns=['Cost Center', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='1842'>





  <div class="bk-root" id="954cca53-3c67-4aa5-926d-87a2dfe8ce59" data-root-id="1842"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"5d834637-a663-4fa5-8d51-6b95f0b57d35":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1856","type":"NumberFormatter"},{"attributes":{"children":[{"id":"1843"},{"id":"1861"},{"id":"1868"}],"margin":[0,0,0,0],"name":"Row03002","tags":["embedded"]},"id":"1842","type":"Row"},{"attributes":{"columns":[{"id":"1848"},{"id":"1853"},{"id":"1858"}],"reorderable":false,"source":{"id":"1844"},"view":{"id":"1863"},"width":800},"id":"1861","type":"DataTable"},{"attributes":{},"id":"1857","type":"IntEditor"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03007","sizing_mode":"stretch_width"},"id":"1868","type":"Spacer"},{"attributes":{"data":{"Cost_Center":["AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","Engineering","Engineering","Engineering","Engineering","Engineering","Office Management ","Office Management ","Office Management ","Office Management ","Office Management ","Security","Security","Security","Security","Security"],"Count":[27,18,13,8,1,3,2,1,9,31,22,10,4,1,1,4,2,1,5,7,1,4,1],"Level":["L1","L2","L3","L4","L8","LM5","LM6","LM7","L1","L2","L3","L4","LM5","L0","L1","L2","L3","LM5","L1","L2","L3","L4","LM6"]},"selected":{"id":"1845"},"selection_policy":{"id":"1864"}},"id":"1844","type":"ColumnDataSource"},{"attributes":{"editor":{"id":"1857"},"field":"Count","formatter":{"id":"1856"},"title":"Count"},"id":"1858","type":"TableColumn"},{"attributes":{},"id":"1846","type":"StringFormatter"},{"attributes":{"editor":{"id":"1847"},"field":"Cost_Center","formatter":{"id":"1846"},"title":"Cost Center"},"id":"1848","type":"TableColumn"},{"attributes":{},"id":"1847","type":"StringEditor"},{"attributes":{},"id":"1851","type":"StringFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03006","sizing_mode":"stretch_width"},"id":"1843","type":"Spacer"},{"attributes":{},"id":"1852","type":"StringEditor"},{"attributes":{"source":{"id":"1844"}},"id":"1863","type":"CDSView"},{"attributes":{},"id":"1864","type":"UnionRenderers"},{"attributes":{},"id":"1845","type":"Selection"},{"attributes":{"editor":{"id":"1852"},"field":"Level","formatter":{"id":"1851"},"title":"Level"},"id":"1853","type":"TableColumn"}],"root_ids":["1842"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"5d834637-a663-4fa5-8d51-6b95f0b57d35","root_ids":["1842"],"roots":{"1842":"954cca53-3c67-4aa5-926d-87a2dfe8ce59"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Repeating some of the code above for easy visuals
search_values_low = ['L1','L2','L3','L4']
low_level_df_c = lvc_df[lvc_df.Level.str.contains('|'.join(search_values_low))]
search_values_high_c = ['LM5','LM6','LM7']
high_level_df_c = lvc_df[lvc_df.Level.str.contains('|'.join(search_values_high))]
```


```python
for x in lvc_df['Cost Center'].unique():
    low_high_comparison(x,low_level_df_c,high_level_df_c, 'Cost Center')
```

    AI & Data Services Low Level Percentage: 91.67
    AI & Data Services High Level Percentage: 8.33
    ===========================================
    Engineering Low Level Percentage: 94.74
    Engineering High Level Percentage: 5.26
    ===========================================
    Office Management  Low Level Percentage: 87.5
    Office Management  High Level Percentage: 12.5
    ===========================================
    Security Low Level Percentage: 94.44
    Security High Level Percentage: 5.56
    ===========================================
    

The Cost Center with the most low level hires is Engineering and the most high level hire is Office Management.

**vi) Low Level Analysis (L1-L4)** 


```python
low_level_df_c.hvplot.table(columns=['Cost Center', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='1880'>





  <div class="bk-root" id="ce7ca2b6-a978-4c3d-8b47-4c23202f3f97" data-root-id="1880"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"317c41d4-d3f8-49ca-a90a-6bc0d8980c2b":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"1890","type":"StringEditor"},{"attributes":{},"id":"1883","type":"Selection"},{"attributes":{"editor":{"id":"1890"},"field":"Level","formatter":{"id":"1889"},"title":"Level"},"id":"1891","type":"TableColumn"},{"attributes":{},"id":"1894","type":"NumberFormatter"},{"attributes":{},"id":"1902","type":"UnionRenderers"},{"attributes":{},"id":"1895","type":"IntEditor"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03104","sizing_mode":"stretch_width"},"id":"1906","type":"Spacer"},{"attributes":{"columns":[{"id":"1886"},{"id":"1891"},{"id":"1896"}],"reorderable":false,"source":{"id":"1882"},"view":{"id":"1901"},"width":800},"id":"1899","type":"DataTable"},{"attributes":{"data":{"Cost_Center":["AI & Data Services","AI & Data Services","AI & Data Services","AI & Data Services","Engineering","Engineering","Engineering","Engineering","Office Management ","Office Management ","Office Management ","Security","Security","Security","Security"],"Count":[27,18,13,8,9,31,22,10,1,4,2,5,7,1,4],"Level":["L1","L2","L3","L4","L1","L2","L3","L4","L1","L2","L3","L1","L2","L3","L4"]},"selected":{"id":"1883"},"selection_policy":{"id":"1902"}},"id":"1882","type":"ColumnDataSource"},{"attributes":{},"id":"1884","type":"StringFormatter"},{"attributes":{"editor":{"id":"1895"},"field":"Count","formatter":{"id":"1894"},"title":"Count"},"id":"1896","type":"TableColumn"},{"attributes":{"editor":{"id":"1885"},"field":"Cost_Center","formatter":{"id":"1884"},"title":"Cost Center"},"id":"1886","type":"TableColumn"},{"attributes":{"children":[{"id":"1881"},{"id":"1899"},{"id":"1906"}],"margin":[0,0,0,0],"name":"Row03099","tags":["embedded"]},"id":"1880","type":"Row"},{"attributes":{},"id":"1885","type":"StringEditor"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03103","sizing_mode":"stretch_width"},"id":"1881","type":"Spacer"},{"attributes":{},"id":"1889","type":"StringFormatter"},{"attributes":{"source":{"id":"1882"}},"id":"1901","type":"CDSView"}],"root_ids":["1880"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"317c41d4-d3f8-49ca-a90a-6bc0d8980c2b","root_ids":["1880"],"roots":{"1880":"ce7ca2b6-a978-4c3d-8b47-4c23202f3f97"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Create new Dataframe to see which department hired the most for (L1-L4)
low_level_df_3 = df_1[df_1.Level.str.contains('|'.join(search_values_low))]
low_level_df_3['Cost Center'].value_counts().hvplot.barh(
    title="Cost Center Hires Low", xlabel='Cost Center', ylabel='Count', 
    width=500, height=350)
```






<div id='1918'>





  <div class="bk-root" id="b37bf4d4-bd87-47e0-babb-50921086d21a" data-root-id="1918"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"fb99e498-b9b0-4ed0-8cb8-f10033d97c16":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"axis_label":"Cost Center","coordinates":null,"formatter":{"id":"1965"},"group":null,"major_label_policy":{"id":"1966"},"ticker":{"id":"1937"}},"id":"1936","type":"CategoricalAxis"},{"attributes":{"axis":{"id":"1932"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1935","type":"Grid"},{"attributes":{},"id":"1943","type":"ResetTool"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"1956","type":"HBar"},{"attributes":{"coordinates":null,"group":null,"text":"Cost Center Hires Low","text_color":"black","text_font_size":"12pt"},"id":"1924","type":"Title"},{"attributes":{"data":{"Cost_Center":[72,66,17,7],"index":["Engineering","AI & Data Services","Security","Office Management "]},"selected":{"id":"1953"},"selection_policy":{"id":"1974"}},"id":"1952","type":"ColumnDataSource"},{"attributes":{},"id":"1962","type":"BasicTickFormatter"},{"attributes":{},"id":"1928","type":"LinearScale"},{"attributes":{},"id":"1930","type":"CategoricalScale"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1944","type":"BoxAnnotation"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"1960","type":"HBar"},{"attributes":{},"id":"1937","type":"CategoricalTicker"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"1962"},"group":null,"major_label_policy":{"id":"1963"},"ticker":{"id":"1933"}},"id":"1932","type":"LinearAxis"},{"attributes":{},"id":"1966","type":"AllLabels"},{"attributes":{"children":[{"id":"1919"},{"id":"1923"},{"id":"1987"}],"margin":[0,0,0,0],"name":"Row03257","tags":["embedded"]},"id":"1918","type":"Row"},{"attributes":{"factors":["Engineering","AI & Data Services","Security","Office Management "],"tags":[[["index","index",null]]]},"id":"1921","type":"FactorRange"},{"attributes":{"axis":{"id":"1936"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1938","type":"Grid"},{"attributes":{"coordinates":null,"data_source":{"id":"1952"},"glyph":{"id":"1955"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1957"},"nonselection_glyph":{"id":"1956"},"selection_glyph":{"id":"1960"},"view":{"id":"1959"}},"id":"1958","type":"GlyphRenderer"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"1957","type":"HBar"},{"attributes":{"callback":null,"renderers":[{"id":"1958"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Cost Center","@{Cost_Center}"]]},"id":"1922","type":"HoverTool"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"1955","type":"HBar"},{"attributes":{},"id":"1965","type":"CategoricalTickFormatter"},{"attributes":{"end":78.5,"reset_end":78.5,"reset_start":0.0,"tags":[[["Cost Center","Cost Center",null]]]},"id":"1920","type":"Range1d"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03262","sizing_mode":"stretch_width"},"id":"1987","type":"Spacer"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03261","sizing_mode":"stretch_width"},"id":"1919","type":"Spacer"},{"attributes":{},"id":"1953","type":"Selection"},{"attributes":{"tools":[{"id":"1922"},{"id":"1939"},{"id":"1940"},{"id":"1941"},{"id":"1942"},{"id":"1943"}]},"id":"1945","type":"Toolbar"},{"attributes":{"below":[{"id":"1932"}],"center":[{"id":"1935"},{"id":"1938"}],"height":350,"left":[{"id":"1936"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1958"}],"sizing_mode":"fixed","title":{"id":"1924"},"toolbar":{"id":"1945"},"width":500,"x_range":{"id":"1920"},"x_scale":{"id":"1928"},"y_range":{"id":"1921"},"y_scale":{"id":"1930"}},"id":"1923","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"1974","type":"UnionRenderers"},{"attributes":{},"id":"1939","type":"SaveTool"},{"attributes":{"source":{"id":"1952"}},"id":"1959","type":"CDSView"},{"attributes":{},"id":"1940","type":"PanTool"},{"attributes":{},"id":"1941","type":"WheelZoomTool"},{"attributes":{},"id":"1933","type":"BasicTicker"},{"attributes":{"overlay":{"id":"1944"}},"id":"1942","type":"BoxZoomTool"},{"attributes":{},"id":"1963","type":"AllLabels"}],"root_ids":["1918"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"fb99e498-b9b0-4ed0-8cb8-f10033d97c16","root_ids":["1918"],"roots":{"1918":"b37bf4d4-bd87-47e0-babb-50921086d21a"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Histogram to see department distribution by levels
sns.displot(low_level_df_3, x="Level", hue="Cost Center",multiple="stack",height=15,palette='Paired')
```




    <seaborn.axisgrid.FacetGrid at 0x1580b3cc6c8>




![png](output_68_1.png)


**vii) High Level Analysis (LM5-LM7)** 


```python
high_level_df_c.hvplot.table(columns=['Cost Center', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=200)
```






<div id='2039'>





  <div class="bk-root" id="ab7d3e21-427d-4245-94b3-304ccd6236bb" data-root-id="2039"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"3b2c4652-9989-4476-b9e7-8d4551d09c54":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"source":{"id":"2041"}},"id":"2060","type":"CDSView"},{"attributes":{},"id":"2061","type":"UnionRenderers"},{"attributes":{},"id":"2042","type":"Selection"},{"attributes":{},"id":"2054","type":"IntEditor"},{"attributes":{"editor":{"id":"2049"},"field":"Level","formatter":{"id":"2048"},"title":"Level"},"id":"2050","type":"TableColumn"},{"attributes":{},"id":"2053","type":"NumberFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03394","sizing_mode":"stretch_width"},"id":"2065","type":"Spacer"},{"attributes":{"columns":[{"id":"2045"},{"id":"2050"},{"id":"2055"}],"height":200,"reorderable":false,"source":{"id":"2041"},"view":{"id":"2060"},"width":800},"id":"2058","type":"DataTable"},{"attributes":{"data":{"Cost_Center":["AI & Data Services","AI & Data Services","AI & Data Services","Engineering","Office Management ","Security"],"Count":[3,2,1,4,1,1],"Level":["LM5","LM6","LM7","LM5","LM5","LM6"]},"selected":{"id":"2042"},"selection_policy":{"id":"2061"}},"id":"2041","type":"ColumnDataSource"},{"attributes":{},"id":"2044","type":"StringEditor"},{"attributes":{"editor":{"id":"2054"},"field":"Count","formatter":{"id":"2053"},"title":"Count"},"id":"2055","type":"TableColumn"},{"attributes":{},"id":"2043","type":"StringFormatter"},{"attributes":{"editor":{"id":"2044"},"field":"Cost_Center","formatter":{"id":"2043"},"title":"Cost Center"},"id":"2045","type":"TableColumn"},{"attributes":{"children":[{"id":"2040"},{"id":"2058"},{"id":"2065"}],"margin":[0,0,0,0],"name":"Row03389","tags":["embedded"]},"id":"2039","type":"Row"},{"attributes":{},"id":"2048","type":"StringFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03393","sizing_mode":"stretch_width"},"id":"2040","type":"Spacer"},{"attributes":{},"id":"2049","type":"StringEditor"}],"root_ids":["2039"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"3b2c4652-9989-4476-b9e7-8d4551d09c54","root_ids":["2039"],"roots":{"2039":"ab7d3e21-427d-4245-94b3-304ccd6236bb"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Create new Dataframe to see which department hired the most for (LM5-LM7)
high_level_df_3 = df_1[df_1.Level.str.contains('|'.join(search_values_high))]
high_level_df_3['Cost Center'].value_counts().hvplot.barh(
    title="Cost Center Hires High", xlabel='Cost Center', ylabel='Count', 
    width=500, height=350)
```






<div id='2077'>





  <div class="bk-root" id="81ce9b19-8dda-4ca2-90d6-9b2dcb10672a" data-root-id="2077"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"0dfe2299-65f8-4a36-9121-cf5f4471da14":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"factors":["AI & Data Services","Engineering","Security","Office Management "],"tags":[[["index","index",null]]]},"id":"2080","type":"FactorRange"},{"attributes":{"axis_label":"Cost Center","coordinates":null,"formatter":{"id":"2124"},"group":null,"major_label_policy":{"id":"2125"},"ticker":{"id":"2096"}},"id":"2095","type":"CategoricalAxis"},{"attributes":{},"id":"2096","type":"CategoricalTicker"},{"attributes":{},"id":"2125","type":"AllLabels"},{"attributes":{"axis":{"id":"2095"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2097","type":"Grid"},{"attributes":{"callback":null,"renderers":[{"id":"2117"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Cost Center","@{Cost_Center}"]]},"id":"2081","type":"HoverTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03552","sizing_mode":"stretch_width"},"id":"2146","type":"Spacer"},{"attributes":{"data":{"Cost_Center":[6,4,1,1],"index":["AI & Data Services","Engineering","Security","Office Management "]},"selected":{"id":"2112"},"selection_policy":{"id":"2133"}},"id":"2111","type":"ColumnDataSource"},{"attributes":{"tools":[{"id":"2081"},{"id":"2098"},{"id":"2099"},{"id":"2100"},{"id":"2101"},{"id":"2102"}]},"id":"2104","type":"Toolbar"},{"attributes":{"source":{"id":"2111"}},"id":"2118","type":"CDSView"},{"attributes":{"below":[{"id":"2091"}],"center":[{"id":"2094"},{"id":"2097"}],"height":350,"left":[{"id":"2095"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2117"}],"sizing_mode":"fixed","title":{"id":"2083"},"toolbar":{"id":"2104"},"width":500,"x_range":{"id":"2079"},"x_scale":{"id":"2087"},"y_range":{"id":"2080"},"y_scale":{"id":"2089"}},"id":"2082","subtype":"Figure","type":"Plot"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03551","sizing_mode":"stretch_width"},"id":"2078","type":"Spacer"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"2114","type":"HBar"},{"attributes":{},"id":"2098","type":"SaveTool"},{"attributes":{},"id":"2099","type":"PanTool"},{"attributes":{},"id":"2121","type":"BasicTickFormatter"},{"attributes":{"coordinates":null,"group":null,"text":"Cost Center Hires High","text_color":"black","text_font_size":"12pt"},"id":"2083","type":"Title"},{"attributes":{},"id":"2102","type":"ResetTool"},{"attributes":{"children":[{"id":"2078"},{"id":"2082"},{"id":"2146"}],"margin":[0,0,0,0],"name":"Row03547","tags":["embedded"]},"id":"2077","type":"Row"},{"attributes":{"coordinates":null,"data_source":{"id":"2111"},"glyph":{"id":"2114"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2116"},"nonselection_glyph":{"id":"2115"},"selection_glyph":{"id":"2119"},"view":{"id":"2118"}},"id":"2117","type":"GlyphRenderer"},{"attributes":{},"id":"2100","type":"WheelZoomTool"},{"attributes":{},"id":"2133","type":"UnionRenderers"},{"attributes":{"end":6.5,"reset_end":6.5,"reset_start":0.0,"tags":[[["Cost Center","Cost Center",null]]]},"id":"2079","type":"Range1d"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"2115","type":"HBar"},{"attributes":{"overlay":{"id":"2103"}},"id":"2101","type":"BoxZoomTool"},{"attributes":{},"id":"2092","type":"BasicTicker"},{"attributes":{},"id":"2122","type":"AllLabels"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"2119","type":"HBar"},{"attributes":{},"id":"2087","type":"LinearScale"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Cost_Center"},"y":{"field":"index"}},"id":"2116","type":"HBar"},{"attributes":{"axis":{"id":"2091"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2094","type":"Grid"},{"attributes":{},"id":"2124","type":"CategoricalTickFormatter"},{"attributes":{},"id":"2089","type":"CategoricalScale"},{"attributes":{},"id":"2112","type":"Selection"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2103","type":"BoxAnnotation"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"2121"},"group":null,"major_label_policy":{"id":"2122"},"ticker":{"id":"2092"}},"id":"2091","type":"LinearAxis"}],"root_ids":["2077"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"0dfe2299-65f8-4a36-9121-cf5f4471da14","root_ids":["2077"],"roots":{"2077":"81ce9b19-8dda-4ca2-90d6-9b2dcb10672a"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
# Histogram to see department distribution by levels
sns.displot(high_level_df_3, x="Level", hue="Cost Center",multiple="stack",height=15,palette='Paired')
```




    <seaborn.axisgrid.FacetGrid at 0x1587fa467c8>




![png](output_72_1.png)


**2. Data Analysis (Exploring relationships between different variables)**

For this part we will be exploring:

     i. The most effective way to aquire talent 
     ii. Which hiring source attracts what level of employess
     iii. Hiring Source Compare to departments 
     

**i) The most effective way to acquire talent**


```python
df = pd.read_csv('People Ops Analyst Test.csv')
# Copy data file
df_2 = df
df_2 = df_2.dropna()
# Quick check for null values
df_2.isna().sum()
```




    Department           0
    Job Title            0
    Location             0
    Hiring Source        0
    Hire Date            0
    Level                0
    Employment Status    0
    Cost Center          0
    dtype: int64




```python
df_2['Hiring Source'].value_counts().hvplot.barh(
    title="Cost Center Head Counts", xlabel='Cost Center', ylabel='Count', 
    width=500, height=350)
```






<div id='2198'>





  <div class="bk-root" id="b573f432-3160-4da1-8857-9f72510649ed" data-root-id="2198"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"c226a39b-0e87-439e-84c0-f2338eb37209":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"source":{"id":"2232"}},"id":"2239","type":"CDSView"},{"attributes":{"coordinates":null,"group":null,"text":"Cost Center Head Counts","text_color":"black","text_font_size":"12pt"},"id":"2204","type":"Title"},{"attributes":{},"id":"2219","type":"SaveTool"},{"attributes":{"tools":[{"id":"2202"},{"id":"2219"},{"id":"2220"},{"id":"2221"},{"id":"2222"},{"id":"2223"}]},"id":"2225","type":"Toolbar"},{"attributes":{},"id":"2220","type":"PanTool"},{"attributes":{"below":[{"id":"2212"}],"center":[{"id":"2215"},{"id":"2218"}],"height":350,"left":[{"id":"2216"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2238"}],"sizing_mode":"fixed","title":{"id":"2204"},"toolbar":{"id":"2225"},"width":500,"x_range":{"id":"2200"},"x_scale":{"id":"2208"},"y_range":{"id":"2201"},"y_scale":{"id":"2210"}},"id":"2203","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2223","type":"ResetTool"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.2},"right":{"field":"Hiring_Source"},"y":{"field":"index"}},"id":"2237","type":"HBar"},{"attributes":{},"id":"2221","type":"WheelZoomTool"},{"attributes":{},"id":"2245","type":"CategoricalTickFormatter"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"line_alpha":{"value":0.1},"right":{"field":"Hiring_Source"},"y":{"field":"index"}},"id":"2236","type":"HBar"},{"attributes":{"coordinates":null,"data_source":{"id":"2232"},"glyph":{"id":"2235"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2237"},"nonselection_glyph":{"id":"2236"},"selection_glyph":{"id":"2240"},"view":{"id":"2239"}},"id":"2238","type":"GlyphRenderer"},{"attributes":{"overlay":{"id":"2224"}},"id":"2222","type":"BoxZoomTool"},{"attributes":{},"id":"2213","type":"BasicTicker"},{"attributes":{},"id":"2233","type":"Selection"},{"attributes":{},"id":"2208","type":"LinearScale"},{"attributes":{},"id":"2242","type":"BasicTickFormatter"},{"attributes":{"axis_label":"Cost Center","coordinates":null,"formatter":{"id":"2245"},"group":null,"major_label_policy":{"id":"2246"},"ticker":{"id":"2217"}},"id":"2216","type":"CategoricalAxis"},{"attributes":{"axis":{"id":"2212"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2215","type":"Grid"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2224","type":"BoxAnnotation"},{"attributes":{"end":47.2,"reset_end":47.2,"reset_start":0.0,"tags":[[["Hiring Source","Hiring Source",null]]]},"id":"2200","type":"Range1d"},{"attributes":{},"id":"2210","type":"CategoricalScale"},{"attributes":{"factors":["Applied","Referral","Sourced","Referred","inbound","Agency","Jobstreet","Job Portal","JobStreet","Internal","Source"],"tags":[[["index","index",null]]]},"id":"2201","type":"FactorRange"},{"attributes":{},"id":"2217","type":"CategoricalTicker"},{"attributes":{"axis_label":"Count","coordinates":null,"formatter":{"id":"2242"},"group":null,"major_label_policy":{"id":"2243"},"ticker":{"id":"2213"}},"id":"2212","type":"LinearAxis"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03745","sizing_mode":"stretch_width"},"id":"2267","type":"Spacer"},{"attributes":{"data":{"Hiring_Source":[43,28,23,15,2,1,1,1,1,1,1],"index":["Applied","Referral","Sourced","Referred","inbound","Agency","Jobstreet","Job Portal","JobStreet","Internal","Source"]},"selected":{"id":"2233"},"selection_policy":{"id":"2254"}},"id":"2232","type":"ColumnDataSource"},{"attributes":{"children":[{"id":"2199"},{"id":"2203"},{"id":"2267"}],"margin":[0,0,0,0],"name":"Row03740","tags":["embedded"]},"id":"2198","type":"Row"},{"attributes":{"axis":{"id":"2216"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2218","type":"Grid"},{"attributes":{},"id":"2243","type":"AllLabels"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03744","sizing_mode":"stretch_width"},"id":"2199","type":"Spacer"},{"attributes":{"callback":null,"renderers":[{"id":"2238"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["Hiring Source","@{Hiring_Source}"]]},"id":"2202","type":"HoverTool"},{"attributes":{},"id":"2246","type":"AllLabels"},{"attributes":{"fill_alpha":{"value":1.0},"fill_color":{"value":"#30a2da"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"#30a2da"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"height":{"value":0.8},"left":{"value":0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"right":{"field":"Hiring_Source"},"y":{"field":"index"}},"id":"2240","type":"HBar"},{"attributes":{},"id":"2254","type":"UnionRenderers"},{"attributes":{"fill_color":{"value":"#30a2da"},"hatch_color":{"value":"#30a2da"},"height":{"value":0.8},"right":{"field":"Hiring_Source"},"y":{"field":"index"}},"id":"2235","type":"HBar"}],"root_ids":["2198"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"c226a39b-0e87-439e-84c0-f2338eb37209","root_ids":["2198"],"roots":{"2198":"b573f432-3160-4da1-8857-9f72510649ed"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



if we are spending a lot of money on job portal - inbound, we need to rethink our decision

**ii) Which hiring source attracts what level of employess**


```python
hs_df = pd.DataFrame({'Count' : df_2.groupby( ["Hiring Source", "Level"] ).size()}).reset_index()
```


```python
hs_df.hvplot.table(columns=['Hiring Source', 'Level','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='2319'>





  <div class="bk-root" id="370b3cf9-6167-4d5c-a189-210574d816e7" data-root-id="2319"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"eb7f19e5-718b-45fb-8ab4-9d120cf43f24":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"2341","type":"UnionRenderers"},{"attributes":{"editor":{"id":"2329"},"field":"Level","formatter":{"id":"2328"},"title":"Level"},"id":"2330","type":"TableColumn"},{"attributes":{},"id":"2324","type":"StringEditor"},{"attributes":{},"id":"2334","type":"IntEditor"},{"attributes":{"source":{"id":"2321"}},"id":"2340","type":"CDSView"},{"attributes":{},"id":"2323","type":"StringFormatter"},{"attributes":{},"id":"2328","type":"StringFormatter"},{"attributes":{"data":{"Count":[1,1,19,15,4,2,2,1,1,1,1,9,6,7,3,1,1,1,1,3,3,8,1,5,8,6,2,2,1,1],"Hiring_Source":["Agency","Applied","Applied","Applied","Applied","Applied","Applied","Internal","Job Portal","JobStreet","Jobstreet","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referred","Referred","Referred","Referred","Source","Sourced","Sourced","Sourced","Sourced","Sourced","inbound","inbound"],"Level":["L3","L0","L1","L2","L3","L4","LM5","L2","L2","L4","L1","L1","L2","L3","L4","LM5","LM6","LM7","L1","L2","L3","L4","L1","L1","L2","L3","L4","LM5","L2","L4"]},"selected":{"id":"2322"},"selection_policy":{"id":"2341"}},"id":"2321","type":"ColumnDataSource"},{"attributes":{},"id":"2322","type":"Selection"},{"attributes":{},"id":"2333","type":"NumberFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03876","sizing_mode":"stretch_width"},"id":"2320","type":"Spacer"},{"attributes":{"editor":{"id":"2324"},"field":"Hiring_Source","formatter":{"id":"2323"},"title":"Hiring Source"},"id":"2325","type":"TableColumn"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03877","sizing_mode":"stretch_width"},"id":"2345","type":"Spacer"},{"attributes":{},"id":"2329","type":"StringEditor"},{"attributes":{"children":[{"id":"2320"},{"id":"2338"},{"id":"2345"}],"margin":[0,0,0,0],"name":"Row03872","tags":["embedded"]},"id":"2319","type":"Row"},{"attributes":{"editor":{"id":"2334"},"field":"Count","formatter":{"id":"2333"},"title":"Count"},"id":"2335","type":"TableColumn"},{"attributes":{"columns":[{"id":"2325"},{"id":"2330"},{"id":"2335"}],"reorderable":false,"source":{"id":"2321"},"view":{"id":"2340"},"width":800},"id":"2338","type":"DataTable"}],"root_ids":["2319"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"eb7f19e5-718b-45fb-8ab4-9d120cf43f24","root_ids":["2319"],"roots":{"2319":"370b3cf9-6167-4d5c-a189-210574d816e7"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
sns.displot(hs_df, x="Hiring Source", hue="Level",multiple="stack",height=10,palette='Paired')
```




    <seaborn.axisgrid.FacetGrid at 0x15821f2da88>




![png](output_81_1.png)


Findings
    1. Most effective method to hire High Level Employees are through Referral 
    2. Jobstreet Hiring Source is the least effective in hiring talent and also the worst option to hire high level talent
    3. This data recorded might not be accurate as there are repetitive variables.
    4. Most People that got promoted is only from L1 because internal hires only consist of L2

**iii) Hiring Source Compare to Departments**


```python
hsd_df_d = pd.DataFrame({'Count' : df_2.groupby( ["Hiring Source", "Department"] ).size()}).reset_index()
hsd_df_d.hvplot.table(columns=['Hiring Source', 'Department','Count'], sortable=True, selectable=True,
                  width=800, height=400)
```






<div id='2357'>





  <div class="bk-root" id="450b250a-cf96-4cde-b043-a20245e81581" data-root-id="2357"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"785e4c03-58e7-476b-a335-47bbb144c41a":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"columns":[{"id":"2363"},{"id":"2368"},{"id":"2373"}],"reorderable":false,"source":{"id":"2359"},"view":{"id":"2378"},"width":800},"id":"2376","type":"DataTable"},{"attributes":{},"id":"2361","type":"StringFormatter"},{"attributes":{"editor":{"id":"2372"},"field":"Count","formatter":{"id":"2371"},"title":"Count"},"id":"2373","type":"TableColumn"},{"attributes":{},"id":"2362","type":"StringEditor"},{"attributes":{},"id":"2379","type":"UnionRenderers"},{"attributes":{"editor":{"id":"2362"},"field":"Hiring_Source","formatter":{"id":"2361"},"title":"Hiring Source"},"id":"2363","type":"TableColumn"},{"attributes":{"children":[{"id":"2358"},{"id":"2376"},{"id":"2383"}],"margin":[0,0,0,0],"name":"Row03969","tags":["embedded"]},"id":"2357","type":"Row"},{"attributes":{},"id":"2366","type":"StringFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03973","sizing_mode":"stretch_width"},"id":"2358","type":"Spacer"},{"attributes":{},"id":"2367","type":"StringEditor"},{"attributes":{"source":{"id":"2359"}},"id":"2378","type":"CDSView"},{"attributes":{"data":{"Count":[1,3,1,3,12,3,1,1,6,12,1,1,1,1,1,1,1,3,1,1,1,2,4,3,6,5,4,1,1,3,6,1,4,2,2,1,2,2,10,1,1],"Department":["Product Engineering","AI Engineering","Cybersecurity","Data Engineering","Data Science","Information Technology Services","Marketing Analytics","People Ops","Product","Product Engineering","R&D Engineering","R&D Engineering","People Ops","Information Technology Services","People Ops","AI Engineering","AI Service Delivery","Cybersecurity","Data Science","Finance","Information Technology Services","Marketing Analytics","People Ops","Product","Product Engineering","R&D Engineering","AI Engineering","AI Service Delivery","Data Engineering","Product","Product Engineering","Data Science","AI Engineering","Cybersecurity","Data Engineering","Data Science","Marketing Analytics","Product","Product Engineering","AI Service Delivery","Product Engineering"],"Hiring_Source":["Agency","Applied","Applied","Applied","Applied","Applied","Applied","Applied","Applied","Applied","Applied","Internal","Job Portal","JobStreet","Jobstreet","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referral","Referred","Referred","Referred","Referred","Referred","Source","Sourced","Sourced","Sourced","Sourced","Sourced","Sourced","Sourced","inbound","inbound"]},"selected":{"id":"2360"},"selection_policy":{"id":"2379"}},"id":"2359","type":"ColumnDataSource"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer03974","sizing_mode":"stretch_width"},"id":"2383","type":"Spacer"},{"attributes":{"editor":{"id":"2367"},"field":"Department","formatter":{"id":"2366"},"title":"Department"},"id":"2368","type":"TableColumn"},{"attributes":{},"id":"2371","type":"NumberFormatter"},{"attributes":{},"id":"2372","type":"IntEditor"},{"attributes":{},"id":"2360","type":"Selection"}],"root_ids":["2357"]},"title":"Bokeh Application","version":"2.4.0"}};
    var render_items = [{"docid":"785e4c03-58e7-476b-a335-47bbb144c41a","root_ids":["2357"],"roots":{"2357":"450b250a-cf96-4cde-b043-a20245e81581"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




```python
ax = sns.displot(hsd_df_d, x="Hiring Source", hue="Department",multiple="stack",height=15,palette='Paired')
ax.set_xticklabels(rotation=30)
```




    <seaborn.axisgrid.FacetGrid at 0x158224021c8>




![png](output_85_1.png)


Depending which type of talent we want to hire, we can look at the histogram to see which is the most effective way to hire the specific individual.

## 📋 Key Findings & Suggestion
**Key Findings**
     1.	Promotions only happen to people from L1 since internal hire only consist of L2
     2.	Lack of Individual Contributor (L5, L6, L7)
     3.	Lack of leadership in Marketing Analytics Department 
     4.	Most Expenses spent is on Engineering and AI cost centers 
     5.	Best Hiring Source for high level employees are through referral 
     6.	Job street is the worst option when it comes to hiring talent  

**Suggestion for Improvement**
    1. Since Promotion only happens to people from L1, there is a question of whether talented people are leaving the company or the promotion criteria is hard to achieve. Suggestion here is to explore why talented people are leaving and how do you retain talent from leaving. 
     2. The lack of individual contributors raises the question of whether the talent hired are accurate to the leveling framework or the lack of inspirational events in the company. Suggestion here is to encourage employees to take part in leadership event and to train their ability to think out of the box. 
     3. Most expenses are spent on Engineering and hiring Product Engineers, the question we should ask here is whether the ROI is worth the investment? is everyone in the engineering department contributing to the productivity of the company? Suggestion here is to closely monitor everyone's KPI and do a simple ROI calculation to see whether it yields a positive return whether in the short/long term. 
     4. To minimize the budget and time spend recruitng talent, the company should cut of channels that does not yield a positive return and only focus on channels like 'Applied' / 'Referred' / 'Source'. Understand that we cant control the applicants that applied themselves, but the company could spend more time and budget on 'Referred' and 'Source. 

## 📘 Appendix
>### Tools and Libraries Used

>| Job |  Tools |Description|
| --- | --- | --- |
| Data Cleaning / Wrangling | Pandas / Excel  | Pandas is used to remove constant feature columns while Excel is used for sorting / filtering / adding Cost Center column / Removing incomplete columns  |
| Visualization | Bokeh / Matplotlib / Seaborn  | Bokeh is used to create Bar/Barh/Table Charts while Matplotlib and Seaborn is used to create Line Chart for time series and Histograms  |

>### Data Used
>| File Name | File Type |Description|
| --- | --- | --- |
| People Ops Analyst Test | csv | Used to extract data for exploration and analysis |
| Levelling Framework | pdf | Used to understand the breakdown of how levels work in MoneyLion.
 |
