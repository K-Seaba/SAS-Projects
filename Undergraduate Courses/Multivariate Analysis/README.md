# Multivariate Analysis Assignments

### <ins>Objective: </ins>

The objective of this project is to showcase some of the work I've done in my undergraduate module, Multivariate Analysis (WST 311). This is a collection of assignments I've completed during my time in the module using SAS (SAS OnDemand). These assignments are structured similarly to exercises with each question used to cover the practical component of the theory we learned during lectures.

### <ins>Questions: </ins>

Q1. In this exercise we use the Consideration of Future Consequences Scale (CFCS) dataset.

![Q1](https://github.com/K-Seaba/SAS-Projects/assets/83554164/50f74d3a-1042-4a25-a280-3ee3e00c6cb6)

 To answer the questions regarding the CFCS dataset, we first import the data set using ```proc import```. 

 ```sas

*import dataset;
proc import out=CFCS
datafile='/home/u61010751/AssignmentA/CFCS.csv'
dbms=TAB replace;
run;

```

The dataset has 16 columns titled: Q1, Q2, Q3, Q4, Q5, Q6, Q7, Q8, Q9, Q10, Q11, Q12, age, gender, accuracy and country. The entries in columns Q1-Q12 are integers ranging from 1 to 5, the age column conatains integers with ages of respondents, the gender column also coantains integer entries with 1 (Male), 2 (Female), and 3 (Undisclosed), the accuracy column conatains integrs ranging from 0 to 100, and the country column contains countries in abbreviated from as AU, US, CA, etc.

Before working with the data, we first have to clean it according to the following instructions.

```
Clean the data by taking the following into account:

•	Check the data for any responses that are not within the range of the possible values and delete all the data for that respondent. 
•	Delete records where any of the answers to Q1 to Q12 was 0.  
•	Remove all respondents who indicated an accuracy < 60. 
•	Assume that the oldest respondent was 87 years old. 
•	Delete records with responses that are missing. A missing response is when no answer was given. 
•	To check your work, the size of the dataset after deletion of these cases should be 13861. Use only this data further on.
```

In the following code we use the above instructions to clean the data and store it as 'assignA'.

```sas
data assignA;
set CFCS;
    where accuracy >= 60
    and accuracy <= 100
    and Q1 <> 0  
    and Q2 <> 0 
    and Q3 <> 0 
    and Q4 <> 0 
    and Q5 <> 0 
    and Q6 <> 0 
    and Q7 <> 0 
    and Q8 <> 0 
    and Q9 <> 0 
    and Q10 <> 0 
    and Q11 <> 0 
    and Q12 <> 0 
    and age <= 87
    and gender >= 1 
    and gender <= 3;    
run;
```

The data has now been cleaned and contains 13861 rows of data. We can now proceed to answering the questions given in the assignment.

a) To calculate the covariance between Q8 and Q10, we use the ```proc corr``` procedure as follows:

```sas
ods select cov; /** This line specifies that we only want the covariance matrix of Q1 and Q8 **/
proc corr cov data=assignA;
var Q8 Q10;
run;
```

Output:

![q1a](https://github.com/K-Seaba/SAS-Projects/assets/83554164/3fcd4a35-ff8b-4d99-a5ac-cf49f3b82055)

According to the above output, the sample covariance between Q8 and Q10 is -0.449324212 which is negative indicating an inverse correlation between the two variable, i.e., as Q8 increases, Q10 decrease.

b) The correlation between Q8 and Q10 is given as (rounded to 4 decimal places)

```math
cor(Q8,Q10)= \frac{cov(Q8,Q10)}{\sqrt{var(Q8)var(Q10)}}
           = \frac{-0.449324212}{\sqrt{(1.240585714)(1.640056756)}}
           = -0.2208
```
<br>

c) To calculate the average and standard deviation for Q8 for the groups listed in the question we first filter the dataset according to the conditions in the question and then use ```proc means``` to get the mean and standard deviation.


The average and standard deviation for Q8 for Males, Age<=25:

```sas
*Males under 25;
data assignA_Mu25;
set assignA;
	where age<=25
	and gender = 1;
run;

proc means data=assigna_mu25;
var q8;
run;
```

Output:

![mu25](https://github.com/K-Seaba/SAS-Projects/assets/83554164/e4f05c38-37ba-4fcf-9f0c-bd1b92ef2c27)

According to the output, the mean and standard deviation here are 3.6007646 and	1.1450782 respectively.

The average and standard deviation for Q8 for Males, Age>25:

```sas
*Male over 25;
data assignA_Mo25;
set assignA;
	where age>25
	and gender = 1;
run;

proc means data=assignA_Mo25;
var q8;
run;
```

Output:

![mo25](https://github.com/K-Seaba/SAS-Projects/assets/83554164/559532fe-5de3-448a-a4ed-a90524fee0fc)


According to the output, the mean and standard deviation here are 3.5797101 and 1.1367471 respectively.

The average and standard deviation for Q8 for Females, Age<=25:

```sas
*Female under 25;
data assignA_Fu25;
set assignA;
	where age<=25
	and gender = 2;
run;

proc means data=assignA_Fu25;
var q8;
run;
```

Output:

![fu25](https://github.com/K-Seaba/SAS-Projects/assets/83554164/cc563dc1-5794-4d22-8032-a5125b5c1d05)

According to the output, the mean and standard deviation here are 3.5253796 and	1.0733088 respectively.

The average and standard deviation for Q8 for Females, Age>25:

```sas
*Female over 25;
data assignA_Fo25;
set assignA;
	where age>25
	and gender = 2;
run;

proc means data=assignA_Fo25;
var q8;
run;
```

Output:

![fo25](https://github.com/K-Seaba/SAS-Projects/assets/83554164/7384052a-10f8-4acb-ae63-ed0cafa901ad)


According to the output, the mean and standard deviation here are 3.4659816 and	1.1135838 respectively.
