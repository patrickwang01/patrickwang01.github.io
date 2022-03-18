---
layout: post 
title: Reflection Blog Post
---

***Yu-Hsin Huang, Cara Keogh, and Patrick Wang worked on this Reflection Post together.***


### 1. Overall, what did you achieve in your project? 

Overall, we created a locally served webapp which shows users a number of images of clothing that they might like based on their style preferences. We created a number of templates and functions, along with a neural network model, to achieve this task. Our webapp has five pages. The first two pages welcome the user and get their name to personalize the experience before asking them to move to the recommender process. In the first part of the process, the user is asked to choose from six categories of garments for which they want to get a recommendation. Then, the user moves to a page which displays five randomly generated images of clothing in the category they chose. They pick which image they like the best and then move to the final page. The last page displays images with high similarity to the one the user originally chose. In order to determine which images are similar to the user’s choice, we wrote an algorithm that uses the Vgg16 model to analyze an image and compare it to other images in the dataset, ultimately producing a similarity score by which to rank how similar the items are to each other. 

<br>

### 2. What are two aspects of your project that you are especially proud of? 

First of all, we are proud of our work on successfully building a useful and creative webapp. The process of turning our project proposal into practice takes lots of time, however, the result we have achieved makes us very proud of ourselves. While building our webapp, we get the chance to review what we have learned throughout the quarter and research on more extended topics such as some advanced techniques of writing HTML and CSS. Secondly, we are very proud of our recommender model. Our model first does the image identification on each image in our dataset, then performs cosine similarity on each of them to find the five most similar products. The algorithm is quite complex and requires some deep convolutional network knowledge. We did lots of research on different types of models, and after closely comparing each of them, we decided to use the Vgg16 model as it can reach high accuracy scores and is closely related to our project topic. When researching these models, we were introduced to many machine learning concepts we have not heard of before, and thus we are very proud of our growth throughout the research process.

<br>

### 3. What are two things you would suggest doing to further improve your project? 

To further improve our project, we can modify our algorithm to allow more clothing data. With a larger database, we can recommend outfits that are more closely related to users’ preferences. Moreover, the dataset we are using right now contains lots of old-fashioned clothing images, if we could find other more updated datasets, our webapp can become more practical. We can also add a function that allows users to upload their own images to show preferences. With this function, users can best indicate what types of clothing they want our application to recommend, rather than choosing the relatively favored outfit out of the five given images.

<br>

### 4. How does what you achieved compare to what you set out to do in your proposal?

In our initial proposal, we wanted users to be able to upload their own images and have the ability to match different clothing pieces together. However, we decided that this would be difficult to complete in the time allotted for the project, so we instead decided that our final product would just suggest clothing items that were already in our database based on the user’s style preferences. We were able to get both our web app and our model working. However, we hit a road bump when it came time to connect the output of the model with the final page of our webapp to actually show the user the similar items. After working on it for a while, we were able to resolve the issue and display the final page correctly. We are happy that we were able to achieve the two overall goals of the project, creating a webapp and an image classification model, as well as achieving overall success.

<br>

### 5. What are three things you learned from the experience of completing your project?

Since our web application is built entirely upon the Flask framework, we spent a large portion of time learning and practicing our skills using Flask. We focused a lot on the “GET” and “POST” methods as well as passing variables into .html files from .py files and vice versa. Another thing that we learned more about was convolutional neural networks when designing the recommendation algorithm. Since the algorithm uses a neural network to function, we had to spend time understanding what actually happens in these networks and how it applies to our situation. Lastly, throughout the project, we gained much more experience in using GitHub as a whole and getting used to the push and pull flow. We needed to understand when to push, so we don’t encounter any merging issues when pulling multiple commits at once. 

<br>

### 6. How will your experience completing this project help you in your future studies or career?

**Patrick:** 
For my future career, I would like to dive into a more data-focused role/field, so I would be doing a lot of work in Python/using a lot of data science packages. When completing this project, I spent a lot of time understanding the neural network that our recommendation algorithm used and learning this gave me more exposure to machine learning applications in the real world. I could picture how these models could be used by companies to help analyze data and make informed decisions for the future with the results. Additionally, a large portion of my time was spent debugging and troubleshooting errors that kept occurring and this made me realize that programming is much more difficult than I ever imagined. However, this made the act that much more rewarding and fulfilling whenever I did fix an issue. Lastly, working on this quarter-long project with my group mates felt very similar to a real-world situation where we are tasked to create a product together, which I found really fun and enjoyable. It’s nice to work with others to make something we’re all proud of! Overall, these roadblocks and eventual successes really made me even more interested in pursuing a career that involves programming.

<br>

**Cara:**
I will be working in the finance field upon graduation, and Python is being used more and more in this sector. Although I probably will not need to create an image classifier in my work, I think that the overall process of doing this project from start to finish has taught me a lot of new skills in Python and about being a coder in general. I spent a lot of time simply learning about different functions and applications. I used some of these in the project, and many others I didn’t use but am now aware of and can see myself using them for different projects in the future. The process of discovery, trial and error, and planning my time with this project will help me when I come across new coding challenges. I also learned what some of my weaknesses are in coding, so I will be trying to improve upon those as well! Thank you to both of my teammates for helping this idea come to life :)  

<br>

**Yu-Hsin:**
First of all, as listed in part 5, I have learned many technical skills in this project. By utilizing many Python packages, data analysis techniques, machine learning knowledge, and web development skills throughout the process of completing the project, I become more familiar with coding in some sense. When I encounter projects that require these skills, I will be able to pick up faster and have enough knowledge to explore more advanced topics in the future. Secondly, working on a data science project as a group has taught me how to use Git and Github to do version control and manage my works while collaborating with my teammates. This can very well prepare me for my future career. When collaborating with my colleagues, Git and Github will be very useful tools to manage and store our works together. Lastly, in order to complete this quarter-long project, my teammates and I have been meeting every week to keep track of our progress, which practices my project management skill. In the proposal stage of the project, our team set up different milestones and goals to make sure we have a clear direction of what to do next. I think this project management skill is very helpful in terms of planning out a relatively long-term goal or a project that lasts for a long period of time. Even though we do not follow our milestones as we planned out, I believe by practicing and using this project management skill more often, I will be able to estimate my ability more accurately. Furthermore, possessing good project management can also enable me to work efficiently and submit quality works on time in either my future studies or career.