# How TO Use Serializers In Django Rest Framework (DRF)

## Introduction:
Django Rest Framework (DRF) is a powerful and flexible toolkit built on top of Django, designed to help developers build robust and scalable web Application Programming Interfaces (APIs) efficiently.
Some of the key features of DRF include:
-   Viewsets and routers
-   Support for authentication and permission
-   Support for file uploads and downloads
-   Support for pagination and filtering
-   Support for API documentation
-   Support for testing
-   Model serialization and deserialization
-   Support for nested serializers
and a lot more

### Overview Of Serializing And Deserializing
Serialization is converting an object or data structure into a format that can be easily stored or transmitted. This typically involves converting the object into a byte stream or a textual format like JavaScript Object Notation (JSON) or eXtensible Markup Language (XML). The data can be stored in a file or database as soon as it is serialized.


In a simpler term, one can also say serializing is a process of converting complex, native data structure such as objects, arrays, or classes—into a simpler, standard format like JSON (JavaScript Object Notation). This helps data to be easily read by machine and humans and allows for more seamless storage.

Some importance of Serializers include custom representation; the flexibility to choose what field and object you want to include or exclude, format data or even calculate additional fields to include in the output. 
Serializers enable ease in integrations with Application Programming Interface (API). And a lot more benefits

Deserialization, on the other hand, is the reverse — it converts serialized data (like JSON) back into complex native Python objects. Deserialization is essential for retrieving and processing data received from clients or external APIs.

### HOW TO SERIALIZE DATA IN NATIVE PYTHON

In native Python, serializing and deserializing of data can be done using the built-in `pickle`, `marshal` and `json` modules. The example below will be illustrated using the built-in `json` module. This module can be used to transmit data between different languages, human-readable, secure for untrusted data. Below is an example of how to serialize a Python dictionary into a JSON string:

```python
import json

data = {'name': 'Testing', 'age': 2, 'location': 'Nigeria'}

# Serialize to JSON and write to a file
with open('data.json', 'w') as file:
 json.dump(data, file)

# Convert Python object to JSON string
jsonified = json.dumps(data)
print(jsonified)

```
> The code block above imported the Python built-in `json` module, then proceeded to create an object (dictionary) with the name `data`. `w` stands for write mode. It creates a new file if it does not exist, if it already exists, it overwrites the content of the file. The `json.dumps()` function is used to convert the Python object into a JSON formatted string. The output of the jsonified data is printed out.


```python
 OUTPUT
```

To enable data consistency, the process of deserializing data is important. To convert data back into a Python object, use the `json.load()` method. Below is an example of how it can be done:

```python
# Read and load data from the JSON file
with open('data.json', 'r') as file:
 loaded_data = json.load(file)

# Deserialize JSON string back into a Python object
deserialized_data = json.loads(jsonified)

print(loaded_data)
print(deserialized_data)

```
> it opens the file for reading, if the file does not exist, it throws an error. The `r` stands for `read mode`. It reads the JSON data, convert it back into a python object then prints it.

This is how to serialize and deserialize in Python without the use of any framework.


## HOW TO SERIALIZE AND DESERIALIZE DATA IN DJANGO REST FRAMEWORK(DRF)
DRF serializers are crucial tools for converting complex Django data types like models into JSON, and parsing JSON back into Django objects. This is especially useful when handling API request and response cycles, making it easier to build powerful APIs quickly.
While Python provides basic tools for serializing and deserializing data, DRF simplifies the process when working with Django models and APIs. DRF's `serializers.Serializer` class offers powerful functionality to handle complex data transmissions between Python objects and JSON
Below is a step-by-step guide on how to serialize and deserialize in DRF, demonstrated by creating a sample project. 


### Set up project Environment
1. Creating a virtual environment is considered a best practice while building Pythonic apps. 
Run the command below to create a virtual environment on your Linux machine.
```bash
python -m venv venv
```
> The code block above will create a virtual environment. The first `venv` is the command to create a virtual environment, while the second `venv` is the name of the virtual environment, the convention is using ``venv` but It can be any name of your choice.

2. Activate the virtual environment on your Linux terminal with the command below

```bash
source venv/bin/activate
```


3. Install Necessary Packages;  Install Django and  Django Rest Framework:

```bash
pip install django djangorestframework
```

4. Create a Django Project. Run the command on your terminal to create a sample project

```bash
django-admin startproject sampleProject
```
> This will create a directory called `sampleProject` with the basic feature of a project in it

5. Navigate into the Project directory

```bash
cd sampleProject
```
6. Rename the Nested `sampleProject`: When Django creates a project, it automatically nests a folder with the same name as your project (in this case, `sampleProject`). To avoid naming conflicts and confusion later on (especially when dealing with import paths), it is a best practice to rename the inner folder to something else, like `core`.

Use the command below to rename it:

```bash
mv sampleProject core
```
> This command renames the inner `sampleProject` folder (the one containing `settings.py`) to `core`. This helps keep your project structure clean and avoids conflicts between the parent and nested folders.

7. Configure Settings: Update settings.py, urls.py, and wsgi.py to point to the `core` app as discussed in your draft.
Navigate to your `core` app
```bash
cd core
```
Open and edit your `settings.py` to now point at the renamed `core` app. Change your `ROOT_URLCONF` and `WSGI_APPLICATION` to point at core

```bash
ROOT_URLCONF = 'core.urls' 
...
WSGI_APPLICATION = 'core.wsgi.application'

```

Open and edit your `wsgi` file to also point at the `core`  app.

```bash
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'core.settings')
```

Navigate to the parent folder `sampleProject`

```bash
cd ..
```
Open and edit your `manage.py` file to point to the `core` app
```bash
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'core.settings')
```
> This will point to the renamed `core` app. It is a way of telling your computer that all the basic configurations are now in the `core` app.
 
Save your changes and exit.

Run your server to ensure everything is working correctly.
```bash
python manage.py runserver
```

#### Example Book CRUD API
Create a sample book `CRUD` endpoints to display how to serialize and deserialize data on DRF.

1. Create the `book` app: Run this command below to create a `book` app

```bash
python manage.py startapp book

```
> This will create an app called `book` with the basic features of an app in it

2. Add the App to `INSTALLED_APPS` in `core/settings.py`: Navigate to the `core`  app
```bash
cd core
```
Open and edit the `settings.py` file, include `book` to the `INSTALLED APP`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
 .....
    'book',
]
```
> This will include the `book` app to the list of installed apps

Save your changes and exit.

Navigate to the `book` app.

```bash
cd book
```
3. Define the `Books Model`: Open and edit the `models.py` file to create a `Books` model

```python
from django.db import models

class Books(models.Model):
    title = models.CharField(max_length=255)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

```
Save your changes and exit.
> This will create a `Books` model with fields for title, content, created
at, and updated at


5. Create the Serializer:

In book/serializers.py, create a serializer for the Book model:
```python
from rest_framework import serializers
from .models import Books

class BookSerializer(serializers.ModelSerializer):
    class Meta:
 model = Books
 fields = ['id', 'title', 'content', 'created_at']

```
### Types of Serializers in Django Rest Framework

In Django Rest Framework, there are two main types of serializers that developers commonly use:

1. serializers.Serializer

2. serializers.ModelSerializer

1. serializers.Serializer

This is the base serializer class in DRF. It is used when you want full control over how your data is validated and transformed. With Serializer, you manually define every field and write the logic for creating and updating objects.

```python
from rest_framework import serializers

class BookSerializer(serializers.Serializer):
 title = serializers.CharField(max_length=255)
 content = serializers.CharField()
 author = serializers.PrimaryKeyRelatedField(queryset=User.objects.all())
 created_at = serializers.DateTimeField()

```
This is similar to Django Forms. You write custom create() and update() methods to tell the serializer how to handle data.

2. serializers.ModelSerializer

This is a shortcut for automatically creating a serializer class based on a Django model. DRF inspects the model and automatically creates fields, validations, and more. It saves time and reduces boilerplate code.

```python
from rest_framework import serializers
from .models import Book


class BookSerializer(serializers.ModelSerializer):
    class Meta:
 fields = (
            'id',
            'title',
            'content',
            'author',
            'created_at',
 )
 model = Books

```
You don’t need to manually define the fields if they already exist in the model. ModelSerializer handles most things under the hood.
### 🔑 Key Differences Between `serializers.Serializer` and `serializers.ModelSerializer`

| Feature                     | `serializers.Serializer` | `serializers.ModelSerializer` |
|----------------------------|----------------------------------------------------------|------------------------------------------------------------|
| **Model binding** | Manual                                                   | Automatic                                                   |
| **Field definition** | Explicitly declared by the developer                    | Automatically generated from the model                     |
| **Suitable for** | Custom logic, input not directly tied to a model        | Quick model-based serialization                             |
| **Requires create()/update()** | Yes, must be manually defined                         | Optional – auto-generated by DRF                            |
| **Flexibility** | High – full control over logic                          | Less flexible, but highly efficient                         |
| **Development speed** | Slower – more code to write                              | Faster – less code


### Using Serializers in DRF

To use serializers in DRF, create a view that handles `HTTP` requests (GET, POST, UPDATE, DELETE). Here is an example below;

```python
from rest_framework.generics import ListCreateAPIView
from .models import Books
from .serializers import BookSerializer

class BookListCreateView(ListCreateAPIView):
    queryset = Books.objects.all()
    serializer_class = BookSerializer
```
This view will allow clients to list all books or create new ones.
 
In the `book`app, create a `urls.py` file 
```bash
touch urls.py
```
Open and edit the `urls.py` file
```python
# book/urls.py
from django.urls import path
from .views import BookListCreateView

urlpatterns = [
    path('books/', BookListCreateView.as_view(), name='book-list-create'),
]

```
Save and exit
Navigate to the `core/urls.py` And in your `core/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('book.urls')),
]

```
## Conclusion
In this tutorial, we have covered the basics of serializers in Django Rest Framework. We have learned how to create serializers from models, how to use them in views, and the differences between `serializers.Serializer` and `serializers.ModelSerializer`. We have also covered how to use serializers in views to handle HTTP requests.

DRF handles serializing and deserializing elegantly, Serializers in DRF make it easy to convert complex data into simple formats like JSON, which is essential for API communication. By using DRF’s built-in serializers, developers can quickly build and manage APIs without needing to manually handle the intricacies of serialization and deserialization. 

Mastering serializers is a key step toward building powerful APIs with Django Rest Framework. By understanding how to control data flow between your application and the outside world, you'll be able to build scalable, maintainable, and secure web services faster and more confidently!

## Further Reading
- [DRF Documentation](https://www.django-rest-framework.org/)
- [Serializer Documentation](https://www.django-rest-framework.org/api-guide/serializers/)
- [ModelSerializer Documentation](https://www.django-rest-framework.org/api-guide/serializers/modelserializer)
- [DRF Tutorial](https://www.django-rest-framework.org/tutorial/1-serialization/)
- [DRF API Guide](https://www.django-rest-framework.org/api-guide/)



