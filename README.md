# Using AWS ML Translate to Translate Slang and Non-Traditional Language
Hi! Welcome to our Github repository. If you haven't checked it out yet, we highly recommend looking at our blog first. It is a more holistic and less technical summary of what our project is about and it showcases our results and analysis. 

This readme file contains all the information you need in order to duplicate our project, or take aspects of it and do your own thing with it. We tried our best to explain how the Python notebooks and the AWS services work together so you should not need any experience with either in order to navigate and use this repo, but some definitely doesn't hurt. We are going to assume that you have access to AWS services through your own AWS account.

## Overview 

Our project has four distinct parts or workflows. Our API workflow relates to using the twitter API to collect data, and then storing that data so that it can be translated. Our AWS Translate workflow relates to actually translating your twitter data and storing it so that you can analyze it. The S3 workflow explains how we stored and accessed our data. Our analysis workflow is where we perform our data analysis and draw our conclusions. 


![Architexture Diagram](https://github.com/lcunild/QTM-350-Final-Project/blob/d158cc946c03de1de535a30bb2c1bbb3a0eb7919/Architecture%20Design%20Diagram.jpg)

This architecture diagram helps visualize how different workflows interact. As you can see, data collection is the first 'real' step, but cannot be completed without having a SageMaker notebook instance to make API calls and an S3 bucket to store the files. You will notice that this kind of interconnectivity is very characteristic of cloud computing and is a running theme of the projecet. 

## Twitter API Workflow 

To retrieve our Twitter data, we had to make a developer account so we could access the [Twitter API](https://developer.twitter.com/en/docs/twitter-api). This application was very easy. We explained what we were going to use the API for and were approeved in minutes.

We selected the twitter accounts we wanted, got their user IDs and retreived their most recent tweets all using the API. We then reformatted the list of tweets into a [text file](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/TwitterData.txt) so it could be translated. All of the code for these steps are available in the [Twitter API Data](https://github.com/lcunild/QTM-350-Final-Project/blob/main/Data/Twitter_API_Data.ipynb) file wihtin our repo. This code can be used for any users, not just the users that we selected. For example, [Kanye West](https://twitter.com/kanyewest) could be added as a user simply by entering his twitter handle into the for loop used to make the user list. Similar modifications to our code are just as simple simple.

## S3 Workflow

In order to use AWS translate it is essential to have the resulting data from API stored in an S3 Bucket as a text file. This allows you to easily call on the translator and the data from the API in a notebook instance. [Here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) is a guide breaking down how to use S3 and create a bucket.

With this bucket, we can now use AWS Translate in a notebook instance 

## Translate Workflow 

For this project we used AWS translate asynchronous batch processing service. This allowed us to translate large text files and host them inside an S3 bucket which made analyzing the results much easier.

 We have a very detailed [walkthrough](https://qtm350twitterproject.s3.amazonaws.com/TranslateWalkthrough/FinalProjectTranslateWalkthrough.html) of how to use the AWS Translate asynchronous batch processing service here. If you are relatively familiar with AWS translate, scroll down to the section labeled asynchronous bath processing. If you are new to AWS translate, you will likely find the entire walk through helpful as it demonstrates how to interact with the machine learning based translation service. 	

For example, this section demonstrates how to access the translator from within a jupyter notebook. In order to activate the AWS Translate service the boto3 package is required. This example translates two tweets from English to Spanish, as indicated by the Target Language Code. We did not use this method for our project because we used much larger files of text but it is still important to know that translate can be accessed from within a notebook rather than with files stored in an S3 bucket.



## Analysis Workflow
In this section we will break down how to interpret and analyze the results. It is necessary that you
import both the original tweets and re-translated tweets. To then get the tweets back into a list use the `.splitlines()` function on each txt file iterate through the new list to remove empty lines, so that each element in the list is a tweet.
 
To test whether the prevalence of slang does impact the accuracy of the translation it will require a regression of the match percentage of the re-translated and original tweet on the percentage of words which are ‘slang words’ in the original tweet. 

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

## Resources

### AWS Services

The following AWS services are used:

#### Amazon S3 Bucket

A source bucket to store tweets and data from the Twitter Api

#### Amazon Sagemaker

Create notebook instances for conducting analysis and interacting with the S3 bucket and Twitter API

#### Amazon Translate

Translates inputted text to a different languages and then back to english again using asynchronous batch processing






