## This is a project to go through a lesson about FLASK framework from Code Institute

### Install the framework
First you need to install the frameworks to your IDE, enter this code into your terminal

```
pip3 install flask
```

Then your IDE will install the flask Frameworks.

### Create our run.py
You can either create it in the explorer or write this code in the terminal:

```
touch run.py
```

Now its time to implement Flask to our file

### Implement Flask
To make the Flask framework to work this code needs to be implemented

```python
import os
from flask import Flask

app = Flask(__name__)

if __name__ == "__main__":
    app.run(
        host=os.environ.get("IP", "0.0.0.0"),
        port=int(os.environ.get("PORT", "5000")),
        debug=True)  # Change this when it's going to be submitted
```

### Hello World!
To make the notorious Hello World, is to make a decorator route from the flask frameworks:

```python
@app.route("/")
def index():
    return "Hello World!"
```

### Run the code
To make a simple "Hello World!" appear when you launch the code from the terminal with this line of code:

```
python3 run.py
```

### Run HTML
You can build the entier HTML code within the python file, but that will just make it messy.

```python
@app.route("/")
def index():
    return "<h1>Hello</h1><h2>World!</h2>"
```

This will render just fine, but build a whole HTML page here would as stated above.. messy..
So to work around this, you can import the HTML file using templates by adding "render_template".

```python
from flask import Flask, render_template
```

This will allow you to optimize the code and make it look clean and neat.

```python
@app.route("/")
def index():
    return render_template("index.html")
```

To make this work, you need to create a folder called <strong>"templates"</strong> and within the folder, create the <strong>"index.html"</strong>
There you can have the boilerplate HTML within the <strong>"index.html"</strong>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask frameworks</title>
</head>
<body>
    <h1>Hello</h1><h2>World!</h2>
</body>
</html>
```

If you launch the code again, it will look the same.
Now lets add a navigation bar and take away the <strong>"Hello world"</strong>

```html
<body>
    <nav>
        <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#">About</a></li>
        </ul>
    </nav>
    <h1>Home Page</h1>
</body>
```

Time to add the <strong>About</strong> page, just copy and paste the <strong>index.html</strong> and rename
it to <strong>about.html</strong>

And within the run.py file we will make a new route to the new page.

```python
@add.route("/about")
def about():
    return render_template("about.html")
```

As you can see instead of just <strong>("/")</strong> we added <strong>("/about")</strong>, this is because
using the first will indicate its the <strong>index</strong> that we all know is the base of all websites.

Now to make these link to each other, we need to change our HTML a bit.

```html
<body>
    <nav>
        <ul>
            <li><a href="{{ url_for('index') }}">Home</a></li>
            <li><a href="{{ url_for('about') }}">About</a></li>
        </ul>
    </nav>
    <h1>Home Page</h1>  <!-- About Page, within about.html-->
</body>
```

The curle braclets enclosement is recognized by the Flask frameworks to expect lines of code to be executed.
Within the <strong>url_for()</strong> the content needs to match up with our <strong>routes</strong> that we created
within our run.py file.

```python
@add.route("/about")
def about():  # <---- This one!
    return render_template("about.html")
```

Otherwise it wont work.

### Recap code

Your run.py should look like this now:

```python
import os
from flask import Flask, render_template

app = Flask(__name__)


@app.route("/")
def index():
    return render_template("index.html")


@app.route("/about")
def about():
    return render_template("about.html")


if __name__ == "__main__":
    app.run(
        host=os.environ.get("IP", "0.0.0.0"),
        port=int(os.environ.get("PORT", "5000")),
        debug=True)  # Change this when it's going to be real launch
```

### Important! 
As the comment above states. <strong>debug=True</strong> needs to be set to <strong>False</strong>
when your going to launch your site, otherwise it's a security risk!

### Run the code
So when you refresh your page, you can now press the navigation elements to make it go to the different pages.

### Base content
Now there is a nice trick to save time within the Flask frameworks, that will let you have a base template of
HTML so you don't need to change for example the navigation bar on each file.

Make a new file called: <strong>base.html</strong>
And add this content to it.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask frameworks</title>
</head>
<body>
    <nav>
        <ul>
            <li><a href="{{ url_for('index') }}">Home</a></li>
            <li><a href="{{ url_for('about') }}">About</a></li>
        </ul>
    </nav>
</body>
</html>
```

Now we will add a smart function to this.

```html
{% block content %}  <!-- content, is just the name I chose. This can be called what ever you like! -->
{% endblock content %}
```

Within these the code that will be different from the <strong>index.html and about.html</strong> will be written here.

This is how it looks like:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask frameworks</title>
</head>
<body>
    <nav>
        <ul>
            <li><a href="{{ url_for('index') }}">Home</a></li>
            <li><a href="{{ url_for('about') }}">About</a></li>
        </ul>
    </nav>

    {% block content %}
    {% endblock content %}

</body>
</html>
```

Now lets edit our <strong>index.html and about.html</strong>

```html
{% extends "base.html" %}
{% block content %}
    <h1>Home Page</h1>
{% endblock content %}
```

This is all we need for the <strong>index.html</strong>, same goes for the about page with the difference within the <strong>h1</strong> tag.

Now to explain this, we need to extend the <strong>base.html</strong> for <strong>index.html</strong> so it's included within our file. Then just add as
we did in the <strong>base.html</strong> the block and endblock, which hold our unique code for that page.

```html
<h1>Home Page</h1>
```

And if you now refresh your page when you done the changes above, you will now see the same thing!

### Create a contact page
To see this in power, lets add another page. <strong>contact.html</strong>

First create a route within the <strong>run.py</strong>

```python
@add.route("/contact")
def contact():
    return render_template("contact.html")
```

Then duplicate either <strong>index.html or about.html</strong> and just change the <strong>h1</strong> tag to

```html
<h1>Come work with Us!</h1>
```

Now to add this to our navigation bar, go to your <strong>base.html</strong> file and add this:

```html
<nav>
        <ul>
            <li><a href="{{ url_for('index') }}">Home</a></li>
            <li><a href="{{ url_for('about') }}">About</a></li>
            <li><a href="{{ url_for('contact') }}">Contact</a></li>
        </ul>
    </nav>
```

Remember that the <strong>url_for()</strong> path needs to match the route name within <strong>run.py</strong>.

Now refresh the page, and you will see that you have three pages to choose from and all works just fine!