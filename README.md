# Using AWS ML Translate to Translate Slang and Non-Traditional Language

![Slang](https://assets.ltkcontent.com/images/105395/Modern-American-Slang_0066f46bde.jpg)

Hi! Welcome to our github repository. If you haven't checked it out yet, we highly recommend looking at our blog first. It is a more holistic depiction of what our project is about and showcases our results and analysis. 

This readme file contains all the information you need in order to navigate this github repository; whether you want to duplicate our project or take aspects of it and do your own thing with it. We tried our best to explain how the python notebooks and the AWS services work together for those with no experience with either, but some definitely doesn't hurt. We are going to assume that you have access to AWS services through your own AWS account.

## Overview 

The [data folder](https://github.com/lcunild/QTM-350-Final-Project/tree/main/Data) of this repository is where the important notebook files are stored. In particular, the [Twitter API Data](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Twitter_API_Data.ipynb) and [Regression](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Regression.ipynb) files are the most crucial for you to understand. Finally, we created a walkthrough to showcase how the method of using AWS translate that we used ([asynchronous batch processing](https://docs.aws.amazon.com/translate/latest/dg/async.html)) works. We will discuss this method in more detail shortly.

Our project has four parts or workflows. Our API workflow relates to using the Twitter API to collect data and then storing that data so that it can be translated. Our AWS translate workflow relates to actually translating your Twitter data and storing it so that you can analyze it. The S3 workflow explains how we stored and accessed our data. Our analysis workflow is where we performed our data analysis and draw our conclusions. 


![Architecture Diagram](https://github.com/lcunild/QTM-350-Final-Project/blob/d158cc946c03de1de535a30bb2c1bbb3a0eb7919/Architecture%20Design%20Diagram.jpg)

This architecture diagram helps visualize how different workflows interact. As you can see, data collection is the first 'real' step, but cannot be completed without having a SageMaker notebook instance to make API calls and an S3 bucket to store the files. You will notice that this kind of interconnectivity is very characteristic of cloud computing and is a running theme of the project. 

## Twitter API Workflow 

To retrieve our Twitter data, we had to make a developer account so we could access the [Twitter API](https://developer.twitter.com/en/docs/twitter-api). This application was very easy. We explained what we were going to use the API for and got approeved in minutes.

We selected the twitter accounts we wanted, got their user IDs and retreived their most recent tweets all using the API. We then reformatted the list of tweets into a [text file](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/TwitterData.txt) so it could be translated. All of the code for these steps are available in the Twitter API Data file mentioned above. This code can be used for any users, not just the users that we selected. For example, [Connor Mcgregor](https://twitter.com/TheNotoriousMMA) could be added as a user simply by entering his twitter handle into the for loop used to make the user list. Similar modifications to our code are just as simple.

## S3 Workflow

In order to use AWS translate it is essential to have the resulting data from API stored in an S3 Bucket as a text file. This allows you to easily upload the text file in Translate and call on the translator's output files in a notebook instance. [Here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) is a guide breaking down how to use S3 and create a bucket.

![Creating a bucket](https://qtm350twitterproject.s3.amazonaws.com/TranslateWalkthrough/Translate-job.png)

With this bucket we can store our text files of tweets and use AWS Translate's asynchronous batch processing!

## Translate Workflow 

AWS Translate's asynchronous batch processing service allows us to translate large text files that are hosted inside an S3 bucket.

We have a very detailed [walkthrough](https://qtm350twitterproject.s3.amazonaws.com/TranslateWalkthrough/FinalProjectTranslateWalkthrough.html) of how to use the AWS Translate asynchronous batch processing service here. If you are relatively familiar with AWS translate, scroll down to the section labeled asynchronous bath processing. If you are new to AWS translate, you will likely find the entire walk through helpful as it demonstrates how to interact with the machine learning based translation service. 

The begininging of the walkthrough demonstrates how to access the translator from within a jupyter notebook. In the example, a section of Fyodor Dostoevsky's *The Beggar Boy at Christ’s Christmas Tree* (1876) is translated. We did not use this method for our project because we used much larger files of text but it is still important to know that translate can be accessed using a command line interface rather than with files stored in an S3 bucket. There are other ways of using translate. If you are interested the documentation is [here](https://docs.aws.amazon.com/translate/latest/dg/how-it-works.html)

This image shows what the asynchronous batch procssing interface in Translate looks like. You can see all the text files of tweets that we used in our project!
 
 ![Translate](https://qtm350twitterproject.s3.amazonaws.com/TranslateWalkthrough/Console2-screenshot.png)


## Analysis Workflow
In this section we will break down how to interpret and analyze the text files that Translate outputs. It is necessary that you import both the original tweets and re-translated tweets. To then get the tweets back into a list use the `.splitlines()` function on each text file iterate through the new list to remove empty lines, so that each element in the list is a tweet.
 
To test whether the prevalence of slang impacts the accuracy of the translation we ran a regression of the match percentage of the re-translated and original tweet on the percentage of words which are ‘slang words’ in the original tweet. 

In order to do this we must:

* Create two new methods, `slang()` and `match()`. 

	* The `slang()` method gets a list of words from the built-in LINUX dictionary and compares each word of each tweet to the words in the dictionary. It then 	      counts how many words in the tweet do not appear in the dictionary, and calculates the percentage of the tweet which is a slang word.
	
	* The `match()` method uses the `.token_set_ratio()` method from the `fuzzywuzzy` [package](https://github.com/seatgeek/fuzzywuzzy) to compare the re-		  translated tweet to the original tweet, giving a match percentage to gauge the similarity between the two, and therefore a metric to measure the accuracy 	      of the Translate service at translating the tweet. 

* Create data frames for each test language, containing the slang percentage of the original tweet and its corresponding match percentage after the translations. 
* Plot the distribution of the match percentages for each language, as well as the slang percentages of the original tweets, using a histogram
* Run a regression for each language, of match percentage on slang percentage for each tweet. 



## Getting Started by Yourself 
There are a few things you should do in order to be set up for success when you attempt to duplicate our work.
* You should make an S3 bucket on AWS.
	* If you do not know how to do this, refer to the [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html).
* You should make a notebook instance on AWS SageMaker and open it in Jupyter lab
	* If you do not know how to do this refer to the [AWS documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html).
* Navigate to your SageMaker directory within the terminal and clone this repository by using this bash command

	git clone https://github.com/lcunild/QTM-350-Final-Project

Once you have successfully cloned our repository you are ready to begin! Start by running the [Twitter_API_Data](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Twitter_API_Data.ipynb) notebook to collect the data, inputting whichever users you would like to collect data for. Then, follow the [Translate walkthrough](https://qtm350twitterproject.s3.amazonaws.com/TranslateWalkthrough/FinalProjectTranslateWalkthrough.html) to translate the tweets. Finally, run the [Regression](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Regression.ipynb) notebook to analyze your results!

Note: if you have not used any of the packages in the Python notebooks before, you will need to run them in a new cell before you can continue with the code. To do this, simply run the command `!pip install packageName`, replacing packageName with the name of the package to import. For example, to install the `fuzzywuzzy` package, which we use to analyse the results in our experiment, you should run `! pip install fuzzywuzzy` before importing the packages needed to run the code in the notebook.

## Individual Experimentation

You should now have a good enough grasp on how our project works and that has hopefully inspired you with your own hypothesis or another idea that you want to play around on Translate with. As mentioned above, you can easily change the text for analysis by changing which twitter users’ tweets are pulled. By studying our [regression.ipynb](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Regression.ipynb) and the analysis on our blog you can get a good idea of how to analyze and interpret your results.
You can also leave Twitter completely and translate your own text files hosted in an S3 bucket. That would be a fun way of testing your AWS and data science skills!

![Individual experimentation](https://media.istockphoto.com/photos/robot-with-education-hud-picture-id966248982?k=20&m=966248982&s=612x612&w=0&h=gq35V9G0kfjKu0ttr90c8p0VraNtqPDkTvqWQ8oXzCk=)

## Resources

### AWS Services

The following AWS services are used:

#### Amazon S3 Bucket

A source bucket to store tweets and data from the Twitter Api

#### Amazon Sagemaker

Create notebook instances for conducting analysis and interacting with the S3 bucket and Twitter API

#### Amazon Translate

Translates inputted text to a different languages and then back to english again using asynchronous batch processing






