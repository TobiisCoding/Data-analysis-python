# Data-analysis-python
This is were I put all my data analysis project code for python
this code was created to find:
Which NYC schools have the best math results?

The best math results are at least 80% of the *maximum possible score of 800* for math.
Save your results in a pandas DataFrame called best_math_schools, including "school_name" and "average_math" columns, sorted by "average_math" in descending order.
What are the top 10 performing schools based on the combined SAT scores?

Save your results as a pandas DataFrame called top_10_schools containing the "school_name" and a new column named "total_SAT", with results ordered by "total_SAT" in descending order ("total_SAT" being the sum of math, reading, and writing scores).
Which single borough has the largest standard deviation in the combined SAT score?

Save your results as a pandas DataFrame called largest_std_dev.
The DataFrame should contain one row, with:
"borough" - the name of the NYC borough with the largest standard deviation of "total_SAT".
"num_schools" - the number of schools in the borough.
"average_SAT" - the mean of "total_SAT".
"std_SAT" - the standard deviation of "total_SAT".
Round all numeric values to two decimal places.


# Re-run this cell 
import pandas as pd

# Read in the data
schools = pd.read_csv("schools.csv")

# Preview the data
print(schools.info())

# Start coding here...
# Add as many cells as you like...

#Question 1 - Which NYC schools have the best math results?
# I created a variable that would take a subset of the schools average_math column higher than 80%
best_math_schools_1 = schools[schools['average_math']/800*100 > 80]

#created a new variable and used the .sort_values method to sort the schools in descending order
best_math_schools = best_math_schools_1[['school_name','average_math']].sort_values('average_math', ascending=False)
print(best_math_schools)


#Question 2 - What are the top 10 performing schools based on the combined SAT scores?
#I created a new table called the 'total_SAT' I then added all the columns that had the average scores for each exams togther to get the total.
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']
# I then extracted the top ten schools by using the .nlargest method.
top_10_schools = schools.nlargest(10, 'total_SAT')[['school_name', 'total_SAT']]
print(top_10_schools)

#Question 3 - Which single borough has the largest standard deviation in the combined SAT score?
# I grouped the schools by borough and total_SAT was the values, I then used the agg function to get the mean of the total score, the count of the number of schools and the standard deviations
grouped_schools = schools.groupby('borough')['total_SAT'].agg(num_schools='count',average_SAT='mean',std_SAT='std')
# I then created another variable and used the nlargest method to get the largest standard deviation and .round method to round all figures by 2. 
largest_std_dev = grouped_schools.nlargest(1,'std_SAT').round(2)
print(largest_std_dev)
