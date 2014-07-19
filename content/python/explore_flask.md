---
title: "Explore Flask"
date: 2014-07-12 20:10:55
---

## Jinja 模板

### There are two kinds of delimiters.
	
* `{% ... %}` is used to execute statements such as for-loops or assign values.
* `{{ ... }}` is used to print the result of the expression to the template.

### Creating macros

Macros provide a way to modularise the codes; they work like functions.

    {# myapp/templates/layout.html #}
    
    {% from "macros.html" import nav_link with context %}
    <!DOCTYPE html>
    <html lang="en">
        <head>
        {% block head %}
            <title>My application</title>
        {% endblock %}
        </head>
        <body>
            <ul class="nav-list">
                {{ nav_link('home', 'Home') }}
                {{ nav_link('about', 'About') }}
                {{ nav_link('contact', 'Get in touch') }}
            </ul>
        {% block body %}
        {% endblock %}
        </body>
    </html>

nav_link macro is defined in the macros.html.

    {# myapp/templates/macros.html #}
    
    {% macro nav_link(endpoint, text) %}
    {% if request.endpoint.endswith(endpoint) %}
        <li class="active"><a href="{{ url_for(endpoint) }}">{{text}}</a></li>
    {% else %}
        <li><a href="{{ url_for(endpoint) }}">{{text}}</a></li>
    {% endif %}
    {% endmacro %}

### Custom filters

We can define our own filters for use in our Jinja templates. As an example, we'll implement a simple `caps` filter to capitalize all of the letters in a string.

    # myapp/util/filters.py
    
    from .. import app
    
    @app.template_filter()
    def caps(text):
        """Convert a string to all caps."""
        return text.uppercase()

To make our filter available in the templates, we just need to import it in our top-level `__init__.py`
    
    # myapp/__init__.py
    
    # Make sure app has been initialized first to prevent circular imports.
    from .util import filters

Use this filter in the templates.

    <h2>{{ article.title|title }}</h2>

