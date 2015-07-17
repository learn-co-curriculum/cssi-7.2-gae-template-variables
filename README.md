# Google App Engine Template Variables
##Passing Template Variables

### Using Variables in Templates
In Jinja  the syntax for interpolating strings is to put the expression between mustaches, like this: {{ variable }}. 

In hello.html, instead of Hello World, write Hello {{ name }}. Now our page will change to greet anyone whose name is stored in the variable 'name'.

### Passing Variables to the Template
But we need to get the variable 'name' from our controller, helloworld.py and pass it to the template, hello.html.

In our helloworld.py:
```python
class HelloHandler(webapp2.RequestHandler):
  def get(self):
    template_vars = {"name": "CSSI Chicago"}
    template = jinja_environment.get_template('templates/hello.html')
    self.response.out.write(template.render(template_vars))
```
Every time we call render(), we can pass it a dictionary of values as an argument. The Python code gets and possible manipulates the variable and then it is passed to the template using render().


### Getting Data from the Request

We’ve made our HelloWorld webapp a little smarter, because we can pass it a dictionary of key-value pairs and use them in the template. But right now that key:value pair is hardcoded in our handler. Instead, let's get the data from the user.


### The GET Method
The GET method is one way to get use data. It receives data based on a the URL. Facebook for example, uses a URL parameter called login_attempt to keep track of how many times you have tried to login.

`https://www.facebook.com/login.php?login_attempt=1`

Everything after the ? in a url is called the query string. 

#### The Query String
Query strings are almost always a set of paired parameter names and values, separated by ampersands (&). A complex one might look like this:

`http://api.umd.io/v0/courses?dept_id=CMSC&semester=201501&credits=3&department=Computer%20Science&grading_method=Audit`

The route is api.umd.io/v0/courses and the url parameters are:

| dept_id       | CMSC        | 
| ------------- |-------------|
| semester      | 20150       | 
| credits       | 3           | 
| department     | Computer Science   | 
|grading_method|Audit|



### Accessing the URL Parameters
You need to use self.request.get() to access the url parameters.

Here's an updated HelloWorld Handler in helloworld.py that gets the 'name' parameter from the url and passes it into the template.
```python
class HelloHandler(webapp2.RequestHandler):
  def get(self):
    template_vars = {'name': self.request.get('name')}
    template = jinja_environment.get_template('templates/hello.html')
    self.response.out.write(template.render(template_vars))
```
Notice that in this handler, we are defining how to respond with the GET method (`def get(self)`). This handler is perfect for getting data from the URL. 

Now we have changed the handler so that it gets the name from the URL and passes it to the hello.html template.

#Conclusion
Template variables are passed from the handler to the template as an argument in the render() function. The variables can either be a dictionary or a list. To get variables from a url, use `self.request.get()`.
