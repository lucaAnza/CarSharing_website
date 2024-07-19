# Guida passo per passo per la realizzazione del sito 🇮🇹


## Step1 - init 

```bash

pipenv install django
pipenv shell
django-admin startproject <Project_name>
python manage.py startapp <App_name>

```

## Step2

### Creation of *views.py*

```python

from django.http import HttpResponse

def home_page(request):
    response = "Hi i am luke!"
    return HttpResponse(response)

# Parametri passati con ?
def elenca_params(request):
    response = ""
    for k in request.GET:
        response += request.GET[k] + " "
    return HttpResponse(response)
# Parametri passati con /
def two_params(request , nome , eta):
    response = nome + " " + str(eta)
    return HttpResponse(response)

```

### Add path on *urls.py*

```python

from .views import home_page,elenca_params

urlpatterns = [
    path('admin/', admin.site.urls),
    path('home/' , home_page , name="homepage"),
    path("", home_page,name="homepage"),
    path("elenca/", elenca_params ,name="elenca") # Parametri passati con ?
    path('parametri/<str:nome>/<int:eta>/', view_func, name=’alias’)  # Parametri passati con /
]

```

## Step3 - Aggiunta template

1. Creazione /Template nella root del progetto.
2. Aggiungi in *urls.py* `'DIRS': [os.path.join(BASE_DIR, "templates")],`
3. Aggiunti *base.html*, *base_extended.html* on /templates.
    Base:

    ```html
        <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">

        {% block head %} {% endblock %}

        <title> {% block title %}{% endblock %}</title>
    </head>
    <body>
        {% block content %}

        {% endblock %}
    </body>
    </html>
    ```

    Extended:

    ```html
    {% extends "base.html" %}

    {% block title %} {{title}} {% endblock %}

    {% block content %}
        <h1>
            {% if user.is_authenticated %}
                Ciao, {{user.name}}
            {% else %}
                Ciao, guest!
            {% endif %}
        </h1>

        {% for i in lista %}
            <p> {{i}} </p>
        {% end for %}
    {% endblock %}
    ```
4. Add this on *views.py*

    ```python
    def hello_template(request):
            ctx= { "title" : "Hello Template",
                   "lista" : [datetime.now() , datetime.today().strftim('%A') , datetime.today().strftime('%B')]}
        return render(request, template_name = 'base_extended.html' , context = ctx)
    ```
