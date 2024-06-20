# FullStack Development Done BY Anish Kumar (1AY21CS028)
## ScreenShot of OutPut
<img src="https://i.ibb.co/cwxsMFK/Whats-App-Image-2024-06-19-at-06-37-41-31e037db.jpg" alt="Whats-App-Image-2024-06-19-at-06-37-41-31e037db" border="0">

## Code


```bash
settings.py
DATABASES = {
'default': {
'ENGINE': 'django.db.backends.sqlite3',
'NAME': BASE_DIR / 'db.sqlite3',
}
}
INSTALLED_APPS = [
 …
'employee'
]
urls.py
from django.contrib import admin
from django.urls import path
from employee import views
urlpatterns = [
path('admin/', admin.site.urls),
path('emp', views.emp),
path('show',views.show),
path('edit/<int:id>', views.edit),
path('update/<int:id>', views.update),
path('delete/<int:id>', views.destroy),
]
Forms.py
from django import forms
from employee.models import Employee
class EmployeeForm(forms.ModelForm):
class Meta:
model = Employee
fields = "__all__"
models.py
# Create your models here.
from django.db import models
class Employee(models.Model):
eid = models.CharField(max_length=20)
ename = models.CharField(max_length=100)
eemail = models.EmailField()
econtact = models.CharField(max_length=15)
class Meta:
db_table = "employee"
views.py
# Create your views here.
from django.shortcuts import render, redirect
from employee.forms import EmployeeForm
from employee.models import Employee
# Create your views here.
def emp(request):
if request.method == "POST":
form = EmployeeForm(request.POST)
if form.is_valid():
try:
form.save()
return redirect('/show')
except:
pass
else:
form = EmployeeForm()
return render(request,'index.html',{'form':form})
def show(request):
employees = Employee.objects.all()
return render(request,"show.html",{'employees':employees})
def edit(request, id):
employee = Employee.objects.get(id=id)
return render(request,'edit.html', {'employee':employee})
def update(request, id):
employee = Employee.objects.get(id=id)
form = EmployeeForm(request.POST, instance = employee)
if form.is_valid():
form.save()
return redirect("/show")
return render(request, 'edit.html', {'employee': employee})
def destroy(request, id):
employee = Employee.objects.get(id=id)
employee.delete()
return redirect("/show")
apps.py
from django.apps import AppConfig
class EmployeeConfig(AppConfig):
default_auto_field = 'django.db.models.BigAutoField'
name = 'employee'
edit.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Index</title>
{% comment %} {% load staticfiles %} {% endcomment %}
{% comment %} <link rel="stylesheet" href="{% static 'css/style.css' %}" /> {% endcomment %}
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet"
integrity="sha384-
rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
crossorigin="anonymous">
</head>
<body>
<form method="POST" class="post-form" action="/update/{{employee.id}}">
{% csrf_token %}
<div class="container">
<br />
<div class="form-group row">
<label class="col-sm-1 col-form-label"></label>
<div class="col-sm-4">
<h3>Update Details</h3>
</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Id:</label>
<div class="col-sm-4">
<input
type="text"
name="eid"
id="id_eid"
required
maxlength="20"
value="{{ employee.eid }}"
/>
</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Name:</label>
<div class="col-sm-4">
<input
type="text"
name="ename"
id="id_ename"
required
maxlength="100"
value="{{ employee.ename }}"
/>
</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Email:</label>
<div class="col-sm-4">
<input
type="email"
name="eemail"
id="id_eemail"
required
maxlength="254"
value="{{ employee.eemail }}"
/>
</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Contact:</label>
<div class="col-sm-4">
<input
type="text"
name="econtact"
id="id_econtact"
required
maxlength="15"
value="{{ employee.econtact }}"
/>
</div>
</div>
<div class="form-group row">
<label class="col-sm-1 col-form-label"></label>
<div class="col-sm-4">
<button type="submit" class="btn btn-success">Update</button>
</div>
</div>
</div>
</form>
</body>
</html>
show.html
<!DOCTYPE html>
<html lang="en">
 <head>
 <meta charset="UTF-8" />
 <title>Employee Records</title>
 {% comment %} {% load staticfiles %} {% endcomment %}
 {% comment %} <link rel="stylesheet" href="{% static 'css/style.css' %}" /> {% endcomment %}
 <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet"
integrity="sha384-
rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
crossorigin="anonymous">
 </head>
 <body>
 <table class="table table-striped table-bordered table-sm">
 <thead class="thead-dark">
 <tr>
 <th>Employee ID</th>
 <th>Employee Name</th>
 <th>Employee Email</th>
 <th>Employee Contact</th>
 <th>Actions</th>
 </tr>
 </thead>
 <tbody>
 {% for employee in employees %}
 <tr>
 <td>{{ employee.eid }}</td>
 <td>{{ employee.ename }}</td>
 <td>{{ employee.eemail }}</td>
 <td>{{ employee.econtact }}</td>
 <td>
 <a href="/edit/{{ employee.id }}"
 ><span class="glyphicon glyphicon-pencil">Edit</span></a
 >
 <a href="/delete/{{ employee.id }}">Delete</a>
 </td>
 </tr>
 {% endfor %}
 </tbody>
 </table>
 <br />
 <br />
 <center><a href="/emp" class="btn btn-primary">Add New Record</a></center>
 </body>
</html>
Index.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Index</title>
{% comment %} {% load staticfiles %}
<link rel="stylesheet" href="{% static 'css/style.css' %}" /> {% endcomment %}
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet"
integrity="sha384-
rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
crossorigin="anonymous">
</head>
<body>
<form method="POST" class="post-form" action="/emp">
{% csrf_token %}
<div class="container">
<br />
<div class="form-group row">
<label class="col-sm-1 col-form-label"></label>
<div class="col-sm-4">
<h3>Enter Details</h3>
</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Id:</label>
<div class="col-sm-4">{{ form.eid }}</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Name:</label>
<div class="col-sm-4">{{ form.ename }}</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Email:</label>
<div class="col-sm-4">{{ form.eemail }}</div>
</div>
<div class="form-group row">
<label class="col-sm-2 col-form-label">Employee Contact:</label>
<div class="col-sm-4">{{ form.econtact }}</div>
</div>
<div class="form-group row">
<label class="col-sm-1 col-form-label"></label>
<div class="col-sm-4">
<button type="submit" class="btn btn-primary">Submit</button>
</div>
</div>
</div>
</form>
</body>
</html>
```
