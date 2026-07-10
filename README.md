## End to End Machine Learning Project 


### Life cycle of Machine learning Project
#### Understanding the Problem Statement

1. Data Collection

2. Data Checks to perform

3. Exploratory data analysis

4. Data Pre-Processing

5. Model Training

6. Choose best model

#### Problem statement
1. This project understands how the student's performance (test scores) is affected by other variables such as Gender, Ethnicity, Parental level of education, Lunch and Test preparation course.

2. Data Collection

> Dataset Source - https://www.kaggle.com/datasets/spscientist/students-performance-in-exams?datasetId=74977

> The data consists of 8 column and 1000 rows.





For Deployment: (AWS Elastic Beanstalk) (step by step)
make: make a folder: .ebextension>python.config>
~~~
option_settings:
  "aws:elasticbeanstalk:container:python":
    WSGIPath: application:application
 ~~~
 make a folder then application.py> copy full code of app.py
 Then push it on Github
    
1. open aws then sign in
2. Search for Elastic Beanstalk open application icon
3. create application
4. application name: , platform = python
5. click on sample application then create application
6. search on aws, "codepipeline", click on CodePipeline, create pipeline, pipeline name: , click next
7. source provider: Github (version 1), click connect to the Github, confirm it
8. select repository name, branch: main, Github webhooks, click next
9. Build Provider: Skip
10. Deploy provider: AWS Elastic Beanstalk, region, application name, environment name, next
11. Review: create pipeline
12. if error occurs: delete app.py file
13. Deployment Success.


For Deployment: (Azure)
1. open azure, sign-in, create the resource
2. create Web App, subscription name:, resource group:, name:, publish: code, runtiome: python 3.8, next
3. continuous deployment: enable, configure with your github account, organisation: github username, repository: repo name, branch: main, review+create, create
4. wait then reload github project page.
5. .github/workflows will be created on your repository, click on it
6. .yaml fill would be created
7. go to github actions, add or update azure....
8. Deployment Completed.

