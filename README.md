# django-extranet
django to service users

---

- Dev environement

```
iterm2 with 3 tabs each with screen session:
:sessionname docker
:sessionname s2
:sessionname s3
screen -d -r s docker
    0$ -> docker-compose up -d; docker-compose logs -f
    1$ -> docker container exec -it --user squid dev bash
       -> source ~/django-extranet/venv/bin/activate
       -> cd ~/django-extranet/project/
       -> python manage.py shell
       -> import some-app.views
screen -d -r s s2
screen -d -r s s3

```

- Quick start an app:

```
# make sure venv is active
source venv/bin/activate

# 
cd ~/django-extranet/project/
python manage.py startapp userplans

# ~/django-extranet/project/settings.py
|INSTALLED_APPS = [
 ...
 'userplans.apps.UserplansConfig',
]

# ~/django-extranet/project/urls.py
urlpatterns = [
   ...
    path('userplans/', include('userplans.urls')),
]

# create: ~/django-extranet/project/userplans/urls.py
from django.urls import path

from . import views

app_name = "userplans"
urlpatterns = [
    path('', views.index, name='index'),
]

# ~/django-extranet/project/userplans/views.py
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world.")

# ~/django-extranet/project/templates/project/topmenu.html
{% url 'userplans:index' as thisurl %}
<a class="nav-link p-0 {% if request.path == thisurl %}active{% endif %}"
href="{{ thisurl }}">Info User</a>

# templates
mkdir -p ~/django-extranet/project/userplans/templates/userplans/

```

- update production instance

```
docker container exec -it --user=squid production bash
cd ~/django-production/; git pull; killall -HUP gunicorn
```

