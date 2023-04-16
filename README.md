# Smart Hire


Smart Hire is an automated interview process application built for the "Hack IT Sapiens" Hackathon. The project aims to streamline the recruitment process, making it faster and easier for HR teams to hire candidates. With Smart Hire, candidates can enter their personal details, upload their resumes, and complete a questionnaire to rate themselves. The application also predicts the candidate's personality using machine learning, along with video and tone analysis to check their confidence level and other traits. HR teams can view all registered candidates' details and update them about their selection or rejection through one-click mail 


# Project Workflow

## Interviewee

+ Enter personal details, upload resume, attempt a questionnaire wherein the candidate has to rate himself/herself
- Personality prediction (based on Big Five Personality Traits model) with OCEAN values and CV analysis
- Video recording in browser. The candidate has to answer a few questions put up by the HR team on the portal.
- Face emotion and Speech Analysis to get insights like confidence level, candidate personality traits
- Application to automate the hiring process
- Personality prediction using ML
- Check confidence and other traits using video and tone analysis
- Send mail to selected/rejected candidates automatically in one-click
- Fast recruitment in larger numbers
- Generate concise insights and provide summary of the candidate's profile
- Handy application for HR/Recruiting Team


     
## Interviewer/HR team/Admin
     - View all the registered candidates’ details
     - View each candidate's profile summary which includes resume, responses to questions, technical skills, personality traits, video and tone analysis result.
     - View all the registered candidates’ details
     - View each candidate's profile summary which includes resume, responses to questions, technical skills, personality traits, video and tone analysis result
     - Update candidate about selection/rejection, further interview process using one-click mail or phone call
     
     Please note that although most of the features have been added, the complete process is not yet automated as this application was made for the Hack IT Sapiens Hackathon. As of now, the application can store the results of multiple users in a MySQL server, but sending mail and generating a candidate profile summary/dashboard can only be done for a single user.
     
     




<br>

_Though most of the features has been added, yet the complete process is not yet automated as this application is made for HackUIET Hackathon. As of now, the application can be used to store the results of multiple users in mysql server, however sending of mail and candidate profile summary/dashboard can only be generated/viewed for a single user 

<br>

# Installation Guide

## Prerequisites
This project requires the following tools to get started:

- Python - The programming language used by Flask.
- MySQL -  A relational database management system based on SQL.
- Virtualenv - A tool for creating isolated Python environments.
- VSCode - A lightweight source code editor which can be used to view, edit, run, and debug source code for applications. You can optionally use any other code editor of your choice such as Sublime Text or Atom.
- AWS Account - A subsidiary of Amazon providing on-demand cloud computing platforms and APIs. 

To get started, install Python and MySQL on your local computer if you don't have them already.

Also, create an AWS Free Tier account, if you don't have it. Services like Amazon S3 and Amazon Transcribe API will be used in this project.
o get started, install Python and MySQL on your local computer if you don't have them already. Also, create an AWS Free Tier account if you don't have one. Services like Amazon S3 and Amazon Transcribe API will be used in this project

## Getting Started

**Step 1. Clone the repository into a new folder and then switch to code directory**

```
$ git clone [https://github.com/jigarvyasidea/smart-hire]
$ cd smart-hire
```

**Step 2. Create a Virtual Environment and install Dependencies.**

If you don't have the virtualenv command yet, you can find installation [instructions here](https://virtualenv.readthedocs.io/en/latest/). Learn more about [Virtual Environments](https://www.geeksforgeeks.org/python-virtual-environment/).

```
$ pip install virtualenv
```

Create a new Virtual Environment for the project and activate it.

```
$ virtualenv venv
$ source venv/bin/activate
```
Once the virtual environment is activated, the name of your virtual environment will appear on left side of terminal. In our case, venv named virtual environment is active.

Next, we need to install the project dependencies in this virtual environment, which are listed in `requirements.txt`.

```
(venv) $ pip install -r requirements.txt
```
For NLP operations, the [resume parser](https://omkarpathak.in/pyresparser/) package uses spacy and nltk. Install them using below commands:
```
# spaCy
python -m spacy download en_core_web_sm

# nltk
python -m nltk.downloader words
```


**Step 3. Setup your database to store information of the candidates**

Go to MySQL Command-Line Client, and login to the database server using the username and password. Then execute the below statements:

```
CREATE DATABASE databasename;
USE databasename;
CREATE TABLE candidates (id int(11) NOT NULL AUTO_INCREMENT, candidatename varchar(50) NOT NULL, email varchar(50) NOT NULL, password varchar(50)NOT NULL, PRIMARY KEY(id));
```
![mysql](https://user-images.githubusercontent.com/80967728/232264682-68b8d5f0-5c83-4c4f-9a1f-990c93e1276e.png)



To look at the candidates table structure, execute


```
DESCRIBE candidates; 
```

**Step 4. Set up Amazon Transcribe API for speech to text conversion**

- Sign in to your Amazon console and create a _S3 bucket_ and give it a unique name. Note your AWS region as it will be required later.
- Go to _IAM dashboard_, add a new User. Then click on add permissions and grant the following two permissions - _AmazonTranscribeFullAccess_ and _AmazonS3FullAccess_.
- Then under Security Credentials, click on _Create access key_ to get your credentials i.e,  'aws_access_key_id' and 'aws_secret_access_key'.
<img width="1600" alt="aws" src="https://user-images.githubusercontent.com/80967728/232264708-a445ac5c-a78a-451c-b7f4-a5e3a470b1b1.png">



**Step 5. Setting up IBM Watson for tone analysis**
- Go to [IBM Cloud catalog](https://cloud.ibm.com/catalog), under category choose _AI / Machine Learning_. Then choose _Tone Analyzer_ service.
- To create an instance of Tone Analyzer service, click on _Create_ on right hand side.
- Now we need 2 things - _service url_ and _api key_. So click on _Manage_ and copy your credentials.

**Step 6. Update environment variables.**

To run the project, you need to configure the application to run locally. This will require updating a set of environment variables specific to your environment.

In the same directory, create a local environment file

```
(venv) $ touch .env   
(venv) $ nano .env    
```

To get help on how to Set and Get Environment Variables in Python, visit [here](https://able.bio/rhett/how-to-set-and-get-environment-variables-in-python--274rgt5).

_Now You have to simply duplicate the __.env.sample__ file and just insert your credentials._

In the file _.env_ , 
- store your aws credentials i.e aws region, unique bucket name, language code, aws access key id and secret key in following variables:

```
my_region = ""
bucket_name = ""
lang_code = ""
aws_access_key_id = ""
aws_secret_key = ""
```

- store your watson tone analyzer credentials in the following variables:

```
ibm_apikey = ""
ibm_url = ""
```
- configure MySql username and password

```
mysql_password = ""
mysql_user = ""
```

- interviewer mail and password

```
mail_username = ""
mail_pwd = ""
```

- company's official email and password for members of HR team to sign in into the portal.
```
company_mail = ""
company_pswd = ""
```
<br>


__So, basically your project structure would look like:__




**Step 7: Run the server**

Set the FLASK_APP environment variable.
```
(venv) $ export FLASK_APP=app.py
```

Now we're ready to start our flask server:
```
(venv) $ flask run
```




#How it work let understand by iamges

## Interviews signup pages 

![registration and login page](https://user-images.githubusercontent.com/80967728/232264774-7e9bc6a6-dbd4-4a47-aa27-0af92eccab19.png)



## Candidate Presonal details page 



![condidate-signup](https://user-images.githubusercontent.com/80967728/232264819-30d9f642-c7e1-489e-b2a8-572a2034bd62.png)



## Interview panel 
![video-question-interview](https://user-images.githubusercontent.com/80967728/232264828-69f3a92f-b89c-45f5-a7db-9a8bc7905978.png)


## After complete interview page 

![After-interview-slid](https://user-images.githubusercontent.com/80967728/232264846-b6afc767-7af8-4e10-a3a5-6f53d134800e.png)


## condidate deshboard
![deshboardprofile](https://user-images.githubusercontent.com/80967728/232264900-0570abfc-7f8f-4945-8b39-3dd2ca37a488.png)



## sand email after inter![sand email](https://user-images.githubusercontent.com/80967728/232264884-66050e0d-a98c-47f3-a32d-09f548679423.png)
view


## Selection and rejetion email 
![congratualttionemail](https://user-images.githubusercontent.com/80967728/232264919-268f87d2-24f7-4f4e-b22f-57f668964b9d.png)







