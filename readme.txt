Ch 5 Lesson 
Django for Beginners by William S. Vincent

-Create a database model where we can store and display posts
from our users. 
-Create post models
# posts/models.py
from django.db import models
class Post(models.Model):
	text = models.TextField()

Activating models
1. First we create a migration file with the makemigrations command which gen-
erate the SQL commands for preinstalled apps in our INSTALLED_APPS setting.
Migrationfilesdonotexecutethosecommandsonourdatabasefile,ratherthey
are a reference of all new changes to our models. This approach means that we
have a record of the changes to our models over time.
2. Second we build the actual database with migrate which does execute the
instructions in our migrations file.
Command Line
(mb) $ python manage.py makemigrations posts
(mb) $ python manage.py migrate posts

Create superuser
(mb) $ python manage.py createsuperuser

Restart the Django server with python manage.py runserver and in your browser go
to http://127.0.0.1:8000/admin/

-Explicitly tell Django what to display in the admin page
# posts/admin.py
from django.contrib import admin
from .models import Post
admin.site.register(Post)

To display a descriptive and helpful representation of our database entry.
Within the posts/models.py file, add a new function __str__ as
follows:
Code
# posts/models.py
from django.db import models
class Post(models.Model):
	text = models.TextField()
	def __str__(self):
		"""A string representation of the model."""
		return self.text[:??]
		
		
To display posts on the home page with the generic class-based ListView.
# posts/views.py
from django.views.generic import ListView
from .models import Post
class HomePageView(ListView):
	model = Post
	template_name = 'home.html'

Create home.html
<!-- templates/home.html -->
<h1>Message board homepage</h1>
<ul>
	{% for post in object_list %}
		<li>{{ post }}</li>
	{% endfor %}
</ul>

# settings.py
TEMPLATES = [
	{
		...
		'DIRS': [os.path.join(BASE_DIR, 'templates')],
		...
	},
]

# mb_project/urls.py
from django.contrib import admin
from django.urls import path, include
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', include('posts.urls')),
]

Create and update posts/urls.py:
# posts/urls.py
from django.urls import path
from . import views
urlpatterns = [
	path('', views.HomePageView.as_view(), name='home'),
]

Compile and run