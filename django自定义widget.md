## 自定义widget

myapp
|____widgets.py
|____admin.py
|____apps.py
|____views.py
|____tests.py
|____forms.py
|____models.py

```
# widget.py

from django import forms
from django.utils.html import format_html
#from django.forms.utils import 
from django.utils.safestring import mark_safe


class TestWidget(forms.Textarea):

    def __init__(self, attrs=None):
        super(TestWidget, self).__init__(attrs)

    def render(self, name, value, attrs=None):
        s = [format_html('<textarea>\r\n</textarea>'),]
        return mark_safe('\n'.join(s))
```

```
models.py

from django.db import models
from mdeditor.fields import MDTextField
# Create your models here.
class Test(models.Model):
    md = models.TextField()
    code = models.TextField('hhh')
```

```
# forms.py

from django import forms
from .widgets import TestWidget
from .models import Test

class TestForm(forms.ModelForm):

    code = forms.CharField(widget=TestWidget())

    class Meta:
        model = Test
        fields = ["md", 'code']
```

```
admin.py

from django.contrib import admin
from . import models
from .forms import TestForm

# Register your models here.
class TestAdmin(admin.ModelAdmin):
    form = TestForm
    list_display = ['md', 'code']

admin.site.register(models.Test, TestAdmin)
```

然后在后台数据库查看结果。