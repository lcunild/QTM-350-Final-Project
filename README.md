# Using AWS ML Translate to Translate Slang and Non-Traditional Language

Hi! Welcome to our github repository. If you haven't check it out yet, we highly recommend looking at our [blog]() first. It is a more wholistic and less technical summary of what our project was about and showcases our results. This read me file contatins all the information you need in order to duplicate our project, or take aspects of it and do you own thing with it. We tried our best to explain how the python notebooks and the aws services work together so you should not need any experience with either in order to navigate and use this github, but some expereince definelty doesn't hurt. 

## Overview 

## AWS S3 Buckets

## Twitter API



## Proposal




We want to use the [Amazon Translate](http://qtm350projectproposal.s3-website-us-east-1.amazonaws.com) service and the Twitter API to pull tweets in various languages including English. After that, we want to use the Translate service to translate the tweets into English (or from English into another language, then back to English) to determine how well the service handles slang and non-traditional language.

For example, we want to see how Translate will handle a phrase such as “on fleek” or any other of a number of slang phrases. As the project progresses, we plan to increase the length of the text that we submit for translation to the Translate service, and see how it performs. We think there are two potential results from here; the service could perform worse, because there is more language and slang for it to become confused on, or the service could perform better, because there may be more context and surrounding words that help it determine the meaning of the slang. 

We would also like to see how the AWS ML Translate service compares to other translators, such as Google, in translating slang. We tested out the word "crib" in Google translate, by using the phrase "I am going to my crib". We found that the translator translated the phrase literally, rather than identifying the meaning of the slang phrase, so the translation into both Portuguese and Spanish was not correct. We want to see if ML Translate behaves similarly, or whether it can identify slang phrases, and provide a translation which conveys the same meaning, rather than just translating the phrase word for word.

Once we have the strings before and after using Translate, we will use the `re.sub()` function to remove all unwanted punctuation from the text, and then use `split()` function to split the string into each individual word and save into a list. From there, we can compare the list of the words before and after using the translate service. We also will use `re.match()` to conclude on what percentage of the words from the two lists match in order to quantify our results and determine how well the translate function is working.
