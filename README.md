# python-challenge

# PyCity Schools Analysis
Below is the analysis completed based on the files provided as part of the challenge.  The final analysis can be located at the bottom of this README.md document

# Dependencies and Setup
import pandas as pd

from pathlib import Path

# File to Load
school_data_to_load = Path("Resources/schools_complete.csv")

student_data_to_load = Path("Resources/students_complete.csv")

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)

student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.
school_data_complete = pd.merge(student_data, school_data, how="left", on="school_name")

school_data_complete.head()

![image](https://github.com/user-attachments/assets/03b25180-ab65-499e-9ad3-465466199bc6)

# District Summary

# Calculate the total number of unique schools
school_count = school_data_complete["school_name"].nunique()

school_count

15
# Calculate the total number of students
student_count = school_data_complete["student_name"].count()

student_count

39170
# Calculate the total budget
As the combined data set now contained multiple line items for the school budget, totalling the true budget values for each school needed to be based on each school as a single data point.  There were two ways to go about retrieving this value.  It could have been based on isolating on the school data csv file independently from the merged data, in order to calculate based on the original data to avoid duplicate line items.  The below approach worked off the combined data set, using unique to isolate the budget value.  

total_budget = (school_data_complete["budget"].unique()).sum()

total_budget

24649428
# Calculate the average (mean) math score
average_math_score = school_data_complete["math_score"].mean()

average_math_score

78.98537145774827
# Calculate the average (mean) reading score
average_reading_score = school_data_complete["reading_score"].mean()

average_reading_score

81.87784018381414
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]

passing_math_percentage = passing_math_count / float(student_count) * 100

passing_math_percentage

74.9808526933878
# Calculate the percentage of students who passed reading (hint: look at how the math percentage was calculated)
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]

passing_reading_percentage = passing_reading_count / float(student_count)*100

passing_reading_percentage

85.80546336482001
# Use the following to calculate the percentage of students that passed math and reading - using both variables created above, to calculate the overall passing rate. 
passing_math_reading_count = school_data_complete[
    (school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
].count()["student_name"]

overall_passing_rate = passing_math_reading_count /  float(student_count) * 100

overall_passing_rate

65.17232575950983

# Create a high-level snapshot of the district's key metrics in a DataFrame.  
Utilizing the variables defined from the work done in the above code, a DataFrame  was created with the column names to link the required inforation. 

district_summary = pd.DataFrame({"Total Schools": [school_count],
                                "Total Students": [student_count],
                                "Total Budget": [total_budget], "Average Math Score": [school_data_complete["math_score"].mean()], 
                                "Average Reading Score": [school_data_complete["reading_score"].mean()],
                                "% Passing Math": [passing_math_percentage],
                                "% Passing Reading": [passing_reading_percentage],
                                "% Overall Passing": [overall_passing_rate]})

# Formatting
district_summary["Total Students"] = district_summary["Total Students"].map("{:,}".format)

district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)

# Display the formatted DataFrame
district_summary

![image](https://github.com/user-attachments/assets/ae184e21-c8e6-4dfc-a33b-5c94b6383575)

# Secondary check of variables datatypes
Initially, I attempted to format each column to present with a cleaner format, however, in doing so it converted the values to an object which caused issues later in the code.  This was identified by using .dtypes, which is present now throughout the code as a double check to ensure the values are formatted correctly. 

![image](https://github.com/user-attachments/assets/940949f6-659b-42da-82e5-237a1fab5e08)


# School Summary

# Use the code provided to select the type per school from school_data
school_types = school_data.set_index(["school_name"])["type"]

school_types

![image](https://github.com/user-attachments/assets/a81d9514-cbf9-432e-91c0-40b98a0b24d6)


# Calculate the total student count per school from school_data
per_school_counts = school_data.groupby("school_name")["size"].sum()

per_school_counts

![image](https://github.com/user-attachments/assets/45cb946a-72e3-4ade-a2dc-c2ae743daa51)


# Calculate the total school budget and per capita spending per school from school_data.  
Using the groupby function I was able to successfully determine the budget for each school.  I then utilize the code above for school counts to assign the per_school_captia value.

per_school_budget = per_school_budget = school_data.groupby("school_name")["budget"].sum()

per_school_capita = per_school_budget / per_school_counts

per_school_budget, per_school_capita

![image](https://github.com/user-attachments/assets/419a6a20-8b31-4441-9d69-be16ff86b13a)


# Calculate the average test scores per school from school_data_complete
per_school_math = school_data_complete.groupby("school_name")["math_score"].mean()

per_school_reading = school_data_complete.groupby("school_name")["reading_score"].mean()

per_school_math, per_school_reading

![image](https://github.com/user-attachments/assets/965c8025-aa31-424f-916e-857a51873a2d)


# Calculate the number of students per school with math scores of 70 or higher from school_data_complete
students_passing_math = school_data_complete[school_data_complete["math_score"] >= 70]

school_students_passing_math = students_passing_math.groupby("school_name").size()

school_students_passing_math

![image](https://github.com/user-attachments/assets/ad521cf5-0e2d-4d1d-901e-22eeeb0f091d)


# Calculate the number of students per school with reading scores of 70 or higher from school_data_complete
students_passing_reading = school_data_complete[school_data_complete["reading_score"] >= 70]

school_students_passing_reading = students_passing_reading.groupby("school_name").size()

school_students_passing_reading

![image](https://github.com/user-attachments/assets/10ad93f9-7aab-42c3-bd6d-402971a2626e)


# Use the provided code to calculate the number of students per school that passed both math and reading with scores of 70 or higher
students_passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]

school_students_passing_math_and_reading = students_passing_math_and_reading.groupby(["school_name"]).size()

# Use the provided code to calculate the passing rates
per_school_passing_math = school_students_passing_math / per_school_counts * 100

per_school_passing_reading = school_students_passing_reading / per_school_counts * 100

overall_passing_rate = school_students_passing_math_and_reading / per_school_counts * 100

# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame({"School Type": school_types,
                                    "Total Students": per_school_counts,
                                    "Total School Budget": per_school_budget,
                                    "Per Student Budget": per_school_capita,
                                    "Average Math Score": per_school_math,
                                    "Average Reading Score": per_school_reading,
                                    "% Passing Math": per_school_passing_math,
                                    "% Passing Reading": per_school_passing_reading,
                                    "% Overall Passing": overall_passing_rate})
# Formatting
per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,.2f}".format)

per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.2f}".format)


# Display the DataFrame
per_school_summary

![image](https://github.com/user-attachments/assets/fc52ddda-e88f-4ed8-a537-5c1fdb6e2fcf)

#as mentioned previously, a double check of the data types to ensure nothing was misformatted so the values could be used to calculate in later code.
print(per_school_summary.dtypes)

![image](https://github.com/user-attachments/assets/e4bdeaa8-1f01-44e5-8a19-ca927eee2a24)


# Highest-Performing Schools (by % Overall Passing)
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows.
top_schools = per_school_summary.sort_values("% Overall Passing", ascending=False)

top_schools.head(5)

![image](https://github.com/user-attachments/assets/3c53bc8f-8d1c-4249-838f-e116fab52e5d)

# Bottom Performing Schools (By % Overall Passing)
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
bottom_schools = per_school_summary.sort_values("% Overall Passing", ascending=True)

bottom_schools.head(5)

![image](https://github.com/user-attachments/assets/7ba7bdc2-3740-4c45-a9c0-fb9898fb2bd7)

# Math Scores by Grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]

tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]

eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]

twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by `school_name` and take the mean of the `math_score` column for each.
ninth_grade_math_scores = ninth_graders.groupby("school_name")["math_score"].mean()

tenth_grader_math_scores = tenth_graders.groupby("school_name")["math_score"].mean()

eleventh_grader_math_scores = eleventh_graders.groupby("school_name")["math_score"].mean()

twelfth_grader_math_scores = twelfth_graders.groupby("school_name")["math_score"].mean()

# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
math_scores_by_grade = pd.DataFrame({"9th": ninth_grade_math_scores,
                                     "10th": tenth_grader_math_scores,
                                     "11th": eleventh_grader_math_scores,
                                     "12th": twelfth_grader_math_scores})

# Minor data wrangling
math_scores_by_grade.index.name = None

# Display the DataFrame
math_scores_by_grade

![image](https://github.com/user-attachments/assets/765a8a91-e21c-4b06-8a96-cf5ace25f800)

# Reading Score by Grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]

tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]

eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]

twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by `school_name` and take the mean of the the `reading_score` column for each.
ninth_grade_reading_scores = ninth_graders.groupby("school_name")["reading_score"].mean()

tenth_grader_reading_scores = tenth_graders.groupby("school_name")["reading_score"].mean()

eleventh_grader_reading_scores = eleventh_graders.groupby("school_name")["reading_score"].mean()

twelfth_grader_reading_scores = twelfth_graders.groupby("school_name")["reading_score"].mean()

# Combine each of the scores above into single DataFrame called `reading_scores_by_grade`
reading_scores_by_grade = pd.DataFrame({"9th": ninth_grade_reading_scores,
                                     "10th": tenth_grader_reading_scores,
                                     "11th": eleventh_grader_reading_scores,
                                     "12th": twelfth_grader_reading_scores})

# Minor data wrangling

reading_scores_by_grade.index.name = None

# Display the DataFrame
reading_scores_by_grade

![image](https://github.com/user-attachments/assets/f85c4abc-1506-4abf-9edd-d624fbee7aab)

# Scores by School Spending
# Establish the bins
spending_bins = [0, 585, 630, 645, 680]

labels = ["<$585", "$585-630", "$630-645", "$645-680"]

labels

![image](https://github.com/user-attachments/assets/43733440-314d-4179-a41f-1ef829846e45)


# Create a copy of the school summary for later aggregations 
I chose to display the results of the copied data to ensure there were no missing or irregular datapoints as a secondary validation due to my issues with the misclassified variables I explained earlier in my analysis. 

school_spending_df = per_school_summary.copy()

school_spending_df

![image](https://github.com/user-attachments/assets/0d426969-9293-43a3-af36-01ec1fca3f1b)

 # Use `pd.cut` on the per_school_capita Series from earlier to categorize per student spending based on the bins.
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, bins=spending_bins, labels=labels)

# Convert Spending Ranges (Per Student) to a string
school_spending_df["Spending Ranges (Per Student)"] = school_spending_df["Spending Ranges (Per Student)"].astype(str)

school_spending_df

![image](https://github.com/user-attachments/assets/8c90c050-b273-4ca7-b026-5e25622e7760)

#  Calculate averages for the desired columns.
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()

spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()

spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()

spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()

overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()

# Assemble into DataFrame
spending_summary = pd.DataFrame({"Average Math Score": spending_math_scores,
                                "Average Reading Score": spending_reading_scores,
                                "% Passing Math": spending_passing_math,
                                "% Passing Reading": spending_passing_reading,
                                "% Overall Passing": overall_passing_spending
})

# Display results
spending_summary

![image](https://github.com/user-attachments/assets/67b4e68d-8384-4710-9949-cac80349d177)

# Scores by School Size
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]

labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

labels

![image](https://github.com/user-attachments/assets/4c0b3a67-637f-4fa3-aafe-37f052a5676e)

# Create a copy of the school summary for later aggregations
school_size_df = per_school_summary.copy()
# Use `pd.cut` on the per_school_counts Series from earlier to categorize school size based on the bins.
school_size_df["School Size"] = pd.cut(school_size_df["Total Students"], bins=size_bins, labels=labels)

# Convert School Size to a string
school_size_df["School Size"] = school_size_df["School Size"].astype(str)

school_size_df

![image](https://github.com/user-attachments/assets/881c5476-2f81-48ff-b0d3-0cd8381685f0)

# Calculate averages for the desired columns.
size_math_scores = school_size_df.groupby(["School Size"])["Average Math Score"].mean()

size_reading_scores = school_size_df.groupby(["School Size"])["Average Reading Score"].mean()

size_passing_math = school_size_df.groupby(["School Size"])["% Passing Math"].mean()

size_passing_reading = school_size_df.groupby(["School Size"])["% Passing Reading"].mean()

size_overall_passing = school_size_df.groupby(["School Size"])["% Overall Passing"].mean()

# Create a DataFrame called `size_summary` that breaks down school performance based on school size (small, medium, or large).
# Use the scores above to create a new DataFrame called `size_summary`
size_summary = pd.DataFrame({"Average Math Score": size_math_scores,
                             "Average Reading Score": size_reading_scores,
                             "% Passing Math": size_passing_math,
                             "% Passing Reading":size_passing_reading,
                             "% Overall Passing": size_overall_passing})

# Display results
size_summary

![image](https://github.com/user-attachments/assets/09453a74-72a8-46f3-bff5-b47b914786d3)

# Scores by School Type
# Group the per_school_summary DataFrame by "School Type" and average the results.
average_math_score_by_type = per_school_summary.groupby(["School Type"])["Average Math Score"].mean()

average_reading_score_by_type = per_school_summary.groupby(["School Type"])["Average Reading Score"].mean()

average_percent_passing_math_by_type = per_school_summary.groupby(["School Type"])["% Passing Math"].mean()

average_percent_passing_reading_by_type = per_school_summary.groupby(["School Type"])["% Passing Reading"].mean()

average_percent_overall_passing_by_type = per_school_summary.groupby(["School Type"])["% Overall Passing"].mean()

# Assemble the new data by type into a DataFrame called `type_summary`
type_summary = pd.DataFrame({"Average Math Score": average_math_score_by_type,
                            "Average Reading Score": average_reading_score_by_type,
                            "% Passing Math": average_percent_passing_math_by_type,
                            "% Passing Reading": average_percent_passing_reading_by_type,
                            "% Overal Passing": average_percent_overall_passing_by_type})

# Display results
type_summary

![image](https://github.com/user-attachments/assets/e9c59874-c9f3-4911-b8e9-3bd872494f90)

# Analysis - Using metrics calculated from the code above, I used certain dataframes to help articulate and re-enforce my analysis.

One of my main observations was that Charter Schools have a much higher passing rate.  Using the Dataframe created to breakdown the school's passing rates by school type, I used a standard horizontal bar graph graph to create a visual to help showcase this difference.  As you can see from the graph, while the average scores for both Math and Reading do not have a big gap, the Passing Percentages (math, reading & overall) are dramatically lower in district school than charter schools. Especially for overall passing rate.  This would suggest, that based on the way overall passing rate is calculated, that students in the District schools tend to struggle with one of the two subjects.  To clarify further, since the overall passing rate is determined by the combination of the "Math Passing Percentage" and the "Reading Passing percentage", the "Overall Passing Percentage" being significantly lower suggests that one of these parameters is bringing down the overall total.  If you look at the excel file example provided the student from these district schools are resulting in fails because their math score is bringing down their overall score to the point they are failing.  This is despite their high marks in reading. 

type_summary.plot.barh()

![image](https://github.com/user-attachments/assets/60fd0b7a-1d42-4a2d-994c-3e0df58be1bf)

![image](https://github.com/user-attachments/assets/0d1f61b0-c807-41cb-9672-ccc346c8140b)


To dive further into the observations, another factor that attributes to the "Overall Passing Percentage" success rate is the size of the school. If you look at the below graph, you can see that larger schools have a dramatically lower "Overall Passing Percentage" success rate than medium to small schools. This is in spite of these schools having similar "Per Student Budget". One might draw the conclusion that due to the small school sizes, students are more likely to succeed because they receive more individually attention from their teachers. Keeping school sizes small has a direct correlation to the success of the school. This is why Charter schools have a higher "Overall Passing Percentage" because on average, they tend to have a dramatically smaller Total Student count.    

size_summary.plot.bar()

![image](https://github.com/user-attachments/assets/54244f55-d0cb-4637-bf3d-6d6887fca200)

![image](https://github.com/user-attachments/assets/abaff684-d810-4449-9396-69e0a89bcda4)

To further support my conclusion, I calculated the average number of students by school type. This clearly shows that Charter schools statiscally have lower school sizes, which is a key factor in their success.  

student_count_type = school_size_df.groupby(["School Type"])["Total Students"].mean()

student_count_type

![image](https://github.com/user-attachments/assets/d36b6e05-9837-4082-b1d2-eb9763260e29)
