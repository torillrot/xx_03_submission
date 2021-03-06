# Statistical assessment

We will be sending you a basic skills test that will test your skills in R, the surveillance of infectious diseases, and statistical consulting. As a part of this test, you will need to sign up for a free GitHub account (https://github.com/join).

There are TWO tasks that you need to complete. If you have any questions, send an email to RichardAubrey.White@fhi.no 

**ALL TASKS MAY BE COMPLETED IN EITHER NORWEGIAN/DANISH/SWEDISH OR ENGLISH**

# Before you start

You will need to install some FHI R packages. You can do this via:

```
install.packages(c("fhidata","fhi","fhiplot"), repos = c("https://folkehelseinstituttet.github.io/drat", "https://cran.rstudio.com"))
```

# TASK 1

## Background

This is an assignment related to infectious disease surveillance that will test your skills in R. The deadline for submission of the test is found in your email. Please read through all of this document before beginning the test.

The scenario is broadly as follows:

You are responsible for `Disease X`. There is a data file `data_raw/individual_level_data.RDS` which contains individual level daily data for `Disease X` for 356 municipalities between `2010-01-01` and `2020-12-31`. Each row of this dataset corresponds to 1 sick person on that date. To be more explicit:

- If there are 100 rows for `municip0301` on `2010-01-01` it means that there were 100 sick people in `municip0301` on `2010-01-01`.
- If there are 0 rows for `municip0301` on `2010-01-01` it means that there were 0 sick people in `municip0301` on `2010-01-01`.

For each municipality you will use the data between `2010-01-01` and `2019-12-31` to build a regression model that predicts the expected number of sick people. You will then use this model to predict the expected number of sick people between `2020-01-01` and `2020-12-31`. You will then produce Excel sheets and graphs that contain information regarding the suspected outbreaks in 2020.

The dataset `fhidata::norway_locations_b2020` contains a data.frame that links `municip_code` (e.g. `municip0301`) to `municip_name` (e.g. `Oslo`) and `county_code` (e.g. `county03`) and `county_name` (e.g. `Oslo`). These are real municipality numbers (`kommunenummer`) based on the 2020 municipality lists (https://en.wikipedia.org/wiki/List_of_municipality_numbers_of_Norway).

## English <-> Norwegian

`municipality` = `kommune`

`county` = `fylke`

## Assumptions

- Norwegian population data is available here: `fhidata::norway_population_b2020`

## Test: Setup 

1. Create a new repository called `xx_03_submission` in GitHub (https://help.github.com/articles/creating-a-new-repository/)
2. Clone your new repository to your local computer (https://help.github.com/articles/cloning-a-repository/)
3. Download the following zip file: https://github.com/fhi-beta/xx_03/archive/main.zip
4. Copy the files into your local `xx_03_submission` folder

## Folder/File Setup

You are required to put your code in the following locations:

- All functions are to be placed in .R files located in the `code_task1` folder (e.g. see `code_task1/CreateFakeData.R`)
- You will write your master file/script/code (that runs all of the requested analyses by calling the functions in `code/*`) in `Run_task1.R` (this file already has a few lines of code in it)
- Your results will be saved in the `results_task1` folder

## Test: Coding

5. Load in the data file `data_raw/individual_level_data.RDS`
6. Create a dataset that contains the aggregated number of sick people per day per municipality.
7. Ensure that your aggregated dataset includes rows/days with zero sick people (e.g. if there were no rows for `2010-01-01`/`municip0301` in `data_raw/individual_level_data.RDS` then your aggregated dataset will still need to have one row for `2010-01-01`/`municip0301` with the value 0).
8. Collapse your data down to iso-year/iso-weeks for each municipality. If you are not familiar with iso-week/years, there is information available at https://en.wikipedia.org/wiki/ISO_week_date and https://rdrr.io/cran/surveillance/man/isoWeekYear.html
9. Create county and national level ("norge") data by collapsing your municpiality level data

Do the following for each of the 356 municipalities (municip*), 11 counties (county*), and the national level ("norge"):

10. Split the data into training data (`2010-01` to `2019-52`) and production data (`2020-01` to `2020-53`)
11. Use the training data to create a regression model that predicts the expected weekly number of sick people. You will need to determine an appropriate regression model (e.g. does the data have seasonality? a trend?). You can do this by either a) examining the code in the function `CreateFakeDataForOneMunicipality` inside `code_task1/CreateFakeData.R` or b) by performing your own statistical analyses. Please write a comment above your model indicating if you examined the code or performed your own statistical analysis. We consider both techniques to be valid and neither choice will be penalized or rewarded over the other.
12. For the training data, create a 2 standard deviation prediction interval
13. Identify the potential outbreaks in the training data (i.e. number of sick people > prediction interval)
14. Exclude the potential outbreaks from the training data
15. Refit the model using the new training data (without any outbreaks in it)
16. Create a 2 standard deviation prediction interval for the production data
17. Identify the potential outbreaks in the production data
18. Create and save an excel sheet with the potential outbreaks in `results_task1/municip/outbreaks_municipXXXX.xlsx` (i.e. one Excel file for each municipality), `results_task1/county/outbreaks_countyXX.xlsx` (i.e. one Excel file for each county), `results_task1/norge/outbreaks_norge.xlsx` (i.e. one Excel file for Norway)
19. Create and save a figure that provides a good overview of the situation in the municipality in `results_task1/municip/outbreaks_municipXXXX.png` (i.e. one graph for each municipality), `results_task1/county/outbreaks_countyXX.png` (i.e. one graph for each county), `results_task1/norge/outbreaks_norge.png` (i.e. one graph for Norway). An example of one such graph is available at https://github.com/fhi-beta/xx_03/blob/master/example_report/5218---overvaking-av-totaldodelighet-5218-uke-52.pdf

**Note:** the graphs must include titles with the real municipality name (e.g. `Oslo` instead of `municip0301`). This information is available from `fhidata::norway_locations_b2020`.

## Test: Creative Assignment

Every week there is a meeting at Folkehelseinstituttet to discuss the current outbreak situation in Norway. The current date is `2020-12-31`. You need to produce graph(s) and/or table(s) for this meeting that will give the meeting participants a good summary of the current situation for `Disease X` in Norway and the situation in the last few weeks. In this meeting you would have a maximum of 2 minutes to present, so your graph(s) and/or table(s) must be easily understood.

You may use external structural data files for this creative assigment (e.g. shapefiles for Norway) if you think it is appropriate.

The following links may be helpful for you:

- https://folkehelseinstituttet.github.io/fhidata/articles/maps.html
- https://folkehelseinstituttet.github.io/fhiplot/articles/

Please save your graph(s) and/or table(s) into `results_task1/creative_assignment`.

Please provide comments in your code that indicate which parts of your code produce the graph(s) and/or table(s) for this creative assignment.

## Test: Submission

20. Commit your results and push them back to GitHub
21. Verify that your code is viewable in your GitHub repository on the internet (both in `Run_task1.R` and in `code_task1/*.R`)
22. Verify that in your GitHub repository on the internet you have 356 Excel files and 356 graphs in `results_task1/municip/`
23. Verify that in your GitHub repository on the internet you have 11 Excel files and 11 graphs in `results_task1/county/`
24. Verify that in your GitHub repository on the internet you have 1 Excel file and 1 graph in `results_task1/norge/`
25. Verify that in your GitHub repository on the internet you have graph(s) and/or table(s) in `results_task1/creative_assignment/`

# TASK 2

## Background

You are a statistical consultant, and someone has come to you with a problem they need help with. You need to answer their question. **YOU MAY ANSWER IN EITHER NORWEGIAN/DANISH/SWEDISH OR ENGLISH.**

## Test: Question

Hi. I am a researcher on water-bourne viruses. I want to test that Norwegian water is safe to drink. That is, I want to test that there is no association between drinking water and getting diarrhea. I want to do this through a longitudinal survey of people in Norway. On the first day of every month (for a year) I want to ask the survey participants (the same people will be asked each month):

- Do you currently have diarrhea? (i.e. outcome)
- How many glasses of water did you drink yesterday? (i.e. exposure)

I know that, on average, Norwegians have diarrhea 6 times per year.

I also know that 10% of Norwegians drink 0 glasses of water per day, 10% drink 1 glass of water per day, 20% drink 2 glasses of water per day, 30% drink 3 glasses of water per day, 20% drink 4 glasses of water per day, and 10% drink 5 glasses of water per day.

My questions are:

- Using data from my longitudinal survey, what kind of statistics should I do to prove that there is no association between drinking water and getting diarrhea?
- How many people should I recruit for my longitudinal survey?

Thanks in advance!

## Test: Submission

1. If you need to write any R code to answer this task, please write it in `Run_task2.R` and `code_task2/`.
2. Please write a response to the person's email in a .txt, .rtf, .doc, .docx, or .pdf format and save it to: `results_task2/email_answer.XXX`
3. Commit your results and push them back to GitHub

# TASK 1 AND 2 SUBMISSION

Send Birgitte.Freiesleben.DeBlasio@fhi.no and RichardAubrey.White@fhi.no an email with the link to your repository
