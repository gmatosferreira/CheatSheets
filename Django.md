# Django Cheat Sheet

Some tricks I've taken note of during Django projects development.

**Table of contents**
- [Model Field](#Model_Field) 
- [QuerySet](#QuerySets) 
- [Useful Modules](#Useful_Modules)
    - [Datetime](#Datetime)
- [Reset migrations](#Reset_migrations)
- [Sources](#Sources)



## Model_Field

**Choices**
Model field definition on *models*

```Python
class <className>(models.Model):
  <optionsVarName> = (
    ('<option1>', '<value1>'),
    ('<option2>', '<value2>'),
    ...
  )
  <fieldName> = models.CharField(max_length=1, choices=<optionsVarName>)
```
Usage on *view*
```Python
<className>.get_<fieldName>_display()
```



**Create model instance with foreign key id**

```python
<modelName>.objects.create(attrs..., <FKAttr>_id=<n>)
```



##### Datetime with default values

```python
class <className>(models.Model):
	<fieldName> = <modelName>.DateTimeField(auto_now=True)
    # Updates field to now everytime the object is saved (created or updated)
    <fieldName> = <modelName>.DateTimeField(auto_now_add=True)
    # Sets the field to now when object created
```



##### Refer to Django user model

```python
from django.contrib.auth.models import User
```



## QuerySets

Get **all objects**
```Python
<modelName>.objects.all()
```

Get **all objects that match a condition**
```Python
<modelName>.objects.filter(attr1=value1, attr2=value2, ...)
# Filter through atribute value
<modelName>.objects.filter(attr1__id=value)
# Filter through atribute atributes value
<modelName>.objects.filter(relatedName__active=True)
# Filter through models that are used as FK by relatedName, with has attr active=True
# To filter those not refered by FK user relatedName__isnull=True
```

Get **all objects which have attr greater or smaller than**

```python
User.objects.filter(userprofile__level__gte=0)
User.objects.filter(userprofile__level__lte=0)
```

Get **all models** that refers to one as **ForeignKey** through *related_name*

```Python
<modelName>.objects.get(id=X).<relatedName>.all()
```


Get **list of elements of a specific attribute** for a model 
```Python
<modelName>.objects.all().values_list(<attrName>, flat=True)
```



## Useful_Modules

### Datetime
[Python documentation on this library](https://docs.python.org/3/library/datetime.html) 

Django model field **DateField** is an instance of **datetime.time** 

Django model field **DateTimeField** is an instance of **datetime.datetime**

Django offers a file that helps managing this fields: [**django.utils.timezone** ](https://docs.djangoproject.com/en/2.2/_modules/django/utils/timezone/)

```Python
from django.utils import timezone

# Get current time
timezone.now()

# Set current time default
<fieldName> = models.DateTimeField(default=timezone.now)

# Add/Subtract days
timezone.now() +/- timezone.timedelta(days=3)
# Others are: seconds, microseconds, milliseconds, minutes, hours, weeks
```



## Reset_migrations

After deleting DB file, run following commands on terminal.

```bash
find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
find . -path "*/migrations/*.pyc"  -delete

python manage.py makemigrations
python manage.py migrate
python manage.py migrate --run-syncdb
```



## Sources

- [Django Documentation](https://docs.djangoproject.com/en/3.0/)

