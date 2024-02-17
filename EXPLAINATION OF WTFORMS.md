# python code :
# (Step 1) Importing Libraries
from flask import Flask, render_template, request
from wtforms import Form, StringField, validators

# (Step 1.1) Defining App
app = Flask(__name__)

# (Step 2) Defining a Form Class by Inheriting from WTForms
class NewForm(Form):  # Naming convention: use UpperCamelCase for class names
    # (Step 2.2) Defining Fields with Validation Rules
    # Syntax: variable_name = FieldType('label', validators=[validation_rules])
    name = StringField('Name', validators=[validators.InputRequired()])

# (Step 3) Defining Route along with HTTP Methods
@app.route('/', methods=['GET', 'POST'])
# Creating and Defining Function to be Called for Route
def index():  # This function will be called when the route is visited
    # Creating an Instance of the Form
    # instance_name = ClassName(request.form)  # This instance handles form data
    form = NewForm(request.form)
    
    # Check if POST Method is Used and if Data Satisfies Validation Rules
    if request.method == 'POST' and form.validate():
        # Get Data from the Form
        # variable_name = instance_name.field_name.data
        name = form.name.data
        return f'Hello, {name}!'  # Returning a Greeting Message
    
    return render_template('index.html', form=form)

# Running the Flask App
if __name__ == '__main__':
    app.run(host='0.0.0.0', debug=True)


 # HTML code :
# <!DOCTYPE html>
 # <html lang="en">
  #<head>
   #  <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WTForms Example</title>
# </head>
# <body>
     <h1>Enter Your Name</h1>
     <!-- Create a form: syntax - <form method="HTTP_method"> -->
     <form method="POST">
         <!-- Include a CSRF token for security: syntax - {{ form.csrf_token }} -->
         {{ form.csrf_token }}
         <!-- Include label for the name field: syntax - {{ form.field_name.label }} -->
         {{ form.name.label }}:
         <!-- Include input field for the name: syntax - {{ form.field_name() }} -->
         {{ form.name() }}
         <!-- Display any validation errors: syntax - {% for item in collection %} -->
         {% for error in form.name.errors %}
             <!-- syntax - <tag attribute="value">content</tag> -->
             <span style="color: red;">{{ error }}</span>
         {% endfor %}
         <br><br>
         <!-- Submit button: syntax - <button type="button">content</button> -->
         <button type="submit">Submit</button>
     </form>
# </body>
# </html>
