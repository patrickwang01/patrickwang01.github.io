---
layout: post
title: Blog Post 2 - Web Development
---
{% raw %}
In this post, I will show you how to create and personally design your own web application using `Flask`!

## Introduction

For my example web application, I will be designing a simple message bank that accepts user-submitted messages and also provides a way to show random messages that are stored within the bank!

The main tool that we will use to accomplish this task is `Flask`, a web-development framework. Additioanlly, to personalize our website, we will use some simple `CSS` commands/modifications. 

Here is the link to the GitHub repository that this tutorial and code is based off of:

***https://github.com/patrickwang01/blogpost-web_development***

## 1. Enabling Submissions by User

We are going to start with the submission portion of our message bank first, since we need some way to insert messages into the bank so that we can see them later.

To begin, we are going to run the following commands to create our actual website application: 

```python
from flask import Flask, g, render_template, request
app = Flask(__name__)
```

This will import Flask and it's tools that we will use later on as well as create the actual app. 

## Creating the 'Submit' Template

First, before we write any functions, let's create an `html` template that contains three user-interactive elements:

- A text box to input the message
- A text box to input the username or handle
- A submit button

Let's call this template `submit.html` and if we run the code block below:

```html
{% extends 'base.html' %}

{% block header %}
  <h1>{% block title %}Submit{% endblock %}</h1>
{% endblock %}

{% block content %}

<form method = "post" enctype = "multipart/form-data">
    <label for = "message">Your message:</label>
    <input type = "text" name = "message" id = "message">  <!-- create message input box -->
    <br> <br>
    <label for = "handle">Your name or handle:</label>
    <input type = "text" name = "handle" id = "handle"> <!-- create handle input box-->
    <br> <br>
    <input type="submit" value="Submit message!"> <!-- create submit button -->
</form>

{% if thanks %}
<br>
Thanks for submitting a message!
{% endif %}

{% endblock %}
```
<br> 
It should look something like this:

![pic16b-submit.png](/images/pic16b-submit.png)

Here's an example of me inputting a message and my name into the text boxes! As we can see, there are the three interactive elements that we could click on and type in. We set the `method = post` so that the data the user submits is sent to the server and stored. Afterwards, we set the labels for each element and run the codes `<input type = "text" name = "message" id = "message">` and `<input type = "text" name = "handle" id = "handle">` to actually create the text boxes that the user can click and type into. To add on a small little detail, we call `{% if thanks %}` to add on a thank you response after the user submits a message!

## Establishing the Database 

Now that we have our user-interactive elements, we'll go about creating our functions that will enable the user to submit their own messages into the bank. 

The first thing we will do is create a `.py` file that will contain all of the functions that we define and pass into our templates. 

We will write our first function that will create an initial database that stores all the messages submitted by the user. 

If we run the code block below:
```python
import sqlite3

def get_message_db():
    
    # establish connection
    g.message_db = sqlite3.connect("messages_db.sqlite")
    cursor = g.message_db.cursor()
    
    # create messages table
    cursor.execute("CREATE TABLE IF NOT EXISTS messages (id INTEGER, handle TEXT, message TEXT)")

    return g.message_db
```

Our return object is the connection that we have established. If we analyze the function more deeply, we see that we establish a database `g.message_db` first and then create a cursor object. Afterwards, we use the cursor to run our SQL command that creates the table `messages` with three columns, `id`, `handle`, and `message`. With this function, we have created our table with appropriate columns and can move onto the next function!

## Inserting Messages 

Now that we have our table, we need to design a function that inserts the messages submitted by the user into the table. To do this, we will use the inputs obtained from our `submit.html` template. 

Running the code block below:
```python
def insert_message(request):
    
    # get message and handle inputs from 'submit.html'
    message = request.form["message"]
    handle = request.form["handle"]

    g.message_db = sqlite3.connect("messages_db.sqlite")
    cursor = g.message_db.cursor()

    # count number of rows in table
    rows = len(cursor.fetchall())

    # set parameters for insertion
    params = (1 + rows, handle, message)

    # insert message into table 
    cursor.execute("INSERT INTO messages(id, handle, message) VALUES (?, ?, ?)", params)
    
    g.message_db.commit()
    g.message_db.close()

    return message, handle
```

We are able to insert the message and username into the `messages` table. Firstly, our parameter argument is the `request` variable that we imported in the beginning. Using this variable enables us to obtain the `message` and `handle` inputs that the user submitted previously. Afterwards, we set the parameters that we will use into our SQL command and insert them into the appropriate columns all in one row. To ensure that each message has a unique ID number, we use the `len()` function to count the length of the table -- which in this case, is the number of rows -- and add one to that value. Lastly, we write `g.message_db.commit()` to save our insertion into the table and `g.message_db.close()` to close the database after we finish using it!

## Rendering the 'Submit' Template

Now that we have created functions and a template that enables our user to submit their own messages into the bank, we need a way to actually deploy our `submit.html` template. To do so, we need to setup an `app.route` using a `POST` and `GET` method to support transmission and reception of data. 

When we run the code block below:
```python
@app.route('/', methods = ['POST', 'GET'])
def submit():
    if request.method == "GET":
        return render_template('submit.html')

    else:
        insert_message(request)
        return render_template('submit.html', thanks = True)
```

We get two situations that correspond to the `POST` or the `GET` method. In the `GET` method, the function will just render the `submit.html` template and do nothing else since the user did not actually click on the submit button. In contrast, in the `POST` method, our function will call our `insert_messages()` function and run it first, then renders the template afterwards. In this case, this occurs because the user has actually clicked the submit button and so, the method becomes a `POST` and the data is sent into our server and table. Additionally, writing `thanks = True` will set our `{% if thanks %}` statement in the `submit.html` template to be TRUE and will run the command within it. This allows our application to print out a small thank you message to the user for submitting a message!


## 2. Viewing Random Messages 

We have completed the first half of our message bank, so we can start building the second half, a viewing interface. We're going to create some functions and another template to build another web page that shows randomly selected messages from the database instead of the interactive submission interface from before! 


## Obtaining the Messages

We're going to write a function called `random_messages()` that employs a SQL query to select random rows within our `messages` table limited by a set number parameter. It will then return a list of tuples of the three columns in the table. 

Let's run the code block below:
```python
def random_messages(n):

    g.message_db = sqlite3.connect("messages_db.sqlite")
    cursor = g.message_db.cursor()

    # set input as parameter
    params = str(n)

    # randomly select n rows from table 
    cursor.execute("SELECT * FROM messages ORDER BY RANDOM() LIMIT (?)", params)
    results = cursor.fetchall()

    g.message_db.close()

    # 'results' is a list of tuples ('id', 'handle', 'message')
    return results
```

Diving deeper into the function, we see that the function accepts an integer argument that will be set as a parameter for the max number of rows our SQL query will select. We connect to the database and then call a cursor object to run our SQL command. The query will select only `n` number of rows randomly from our `messages` table. The return object is a list of tuples of (`id`, `handle`, `message`) in which each tuple is a specific row from the table. At the end, we will always close the database connection after we finish using the function.

With our new function that randomly selects entries within the table, we will need to create a new template that will output and show these messages on the page. 

## Creating the 'View' Template

Similar to how we created the `submit.html` template, we will create a new template called `view.html`. This will display the messages that we obtain from the `random_messages()` function. 

Let's run the code block below:
```html
{% extends 'base.html' %}

{% block header %}
  <h1>{% block title %}Random Messages!{% endblock %}</h1>
{% endblock %}

{% block content %}

<body>

  {% for i in entries %}  <!-- loop through the randomly obtained rows in messages table -->
    {{i[2]}}     <!-- print message component -->
    <li>
       {{i[1]}}   <!-- print handle component as a list item -->
    </li>
    <br>
  {% endfor %}

</body>

{% endblock %}
```

This template will accept the return object from our `random_messages()` function and will display each message with it's corresponding handle. The way it does this is by looping through the list of tuples that resulted from our previous function. For each tuple in the list, we will extract the `message` component in which case is `i[2]` because the tuple is formatted like (`id`, `handle`, `message`). Afterwards, we will extract the `handle` component and place it as a list item under the message, so each message has the correct handle. This will loop through the entire list, which contains `n` number of tuples as set by the parameter from the previous function! 

Now that we have both our function and template to display our messages, let's create our last function to render the `view.html` template itself!

## Rendering the 'View' Template

To cap off the second portion of our message bank, we will define one last function that will call `random_messages()` and renders the `view.html` template. 

Running the code block below:
```python
app.route('/view')
def view():
    results = random_messages(5)
    
    # pass list into 'view.html'
    return render_template('view.html', entries = results)
```

We should get an output like the screencap below: 

![pic16b-view.png](/images/pic16b-view.png)

This screencap is the final result of the viewing interface of our simple message bank!

This example shows an output of five random messages, one being the message I submitted previously when we were creating the `submit.html` template! As we can see, each message has its corresponding handle, so we know that our `random_messages()` function and `view.html` template works. 

If we analyze the function, we see that first, we setup an `app.route` but directed to `'/view'` instead since it is another page that is separate from our `submit.html` page. Afterwards, we write a function that calls and stores the return object of `random_messages()` limited to five rows and then renders the `view.html` template. As we render the template, we pass on the stored list of tuples as `entries` into our `view.html` file to be looped through! 

## Conclusion

And just like that, we have successfully implemented both a submission and viewing interface that a user can interact with! Flask makes it possible and that much easier to create and design your own web applications. We didn't go over much `CSS`, but it enables us to change our website's appearance and add a little more flare to it!

{% endraw %}