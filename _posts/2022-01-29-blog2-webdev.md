---
layout: post
title: Blog Post 2 - Web Development
---

In this post, I will show you how to create and personally design your own web application using `Flask` and `CSS`!

## Introduction

For my example web application, I will be designing a simple message bank that accepts user-submitted messages and also provides a way to show random messages that are stored within the bank!

The main tool that we will use to accomplish this task is `Flask`, a web-development framework. Additioanlly, to personalize our website, we will use some simple `CSS` commands/modifications. 

## Enabling Submissions by User

We are going to start with the submission portion of our message bank first, since we need some way to insert messages into the bank so that we can see them later.

To begin, we are going to run the following commands create our actual website page: 

```python
from flask import Flask, g, render_template, request
app = Flask(__name__)
```

This will import Flask and it's tools that we will use later on as well as create the actual app. 

### Creating the 'Submit' Template

First, before we write any functions, let's create an `html` template that contains three user-interactive elements

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
    <input type = "submit" value = "Submit message!"> <!-- create submit button -->
</form>

{% if thanks %}
<br>
Thanks for submitting a message!
{% endif %}

{% endblock %}
```

The output of this should a website that has the three interactive elements. We set the `method = post` so that the data that the user submits is sent to the server and stored. Afterwards, we set the labels for each element and run the codes `<input type = "text" name = "message" id = "message">` and `<input type = "text" name = "handle" id = "handle">` to actually create the text boxes that the user can click and type into. This should look something like below: 



### Establishing the Database 

Now, we will go about creating our functions that will enable the user to submit their own messages into the bank. 

The first thing we will do is create a `.py` file that will contain all of the functions that we will define and will pass these functions into our templates. 

We will write our first function that will create an initial database that will store all messages submitted by the user. 

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

Our return object is the connection that we have established. If we analyze the function more deeply, we see that we establish a database `g.message_db` first and then create cursor object. Afterwards, we use the cursor to run our SQL command that creates the table `messages` with three columns, `id`, `handle`, and `message`. With this function, we have created our table with appropriate columns and can move onto the next function!

### Inserting Messages 

Now that we have our table, we need to design a function that inserts messages submitted by the user into the table. 



