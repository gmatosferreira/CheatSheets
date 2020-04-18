# Django CheatSheet

Some tricks I've taken note of during Django projects development.

**Index**
- [ModelField](#ModelField) 
- [QuerySet](#QuerySets) 
- [UsefulModules](#OtherStuff)
    - [Datetime](#Datetime)

## ModelField
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


## QuerySets

Get all objects
```Python
<modelName>.objects.all()
```

Get all objects that match a condition
```Python
<modelName>.objects.filter(attr1=value1, attr2=value2, ...)
#Filter through atribute value
<modelName>.objects.filter(attr1__id=value)
#Filter through atribute atributes value
```

Get models that refers to one as ForeignKey through *related_name*
```Python
<modelName>.objects.get(id=X).<relatedName>.all()
```


Get list of elements of a specific attribute for a model 
```Python
<modelName>.objects.all().values_list(<attrName>, flat=True)
```

## UsefulModules

### Datetime
[Python documentation on this library](https://docs.python.org/3/library/datetime.html) 

Django model field **DateField** is an instance of **datetime.time** 


Django model field **DateTimeField** is an instance of **datetime.datetime**
Django offers a file that helps managing this fields: [**django.utils.timezone** ](https://docs.djangoproject.com/en/2.2/_modules/django/utils/timezone/)
```Python
from django.utils import timezone

#Get current time
timezone.now()

#Set current time default
<fieldName> = models.DateTimeField(default=timezone.now)
```

