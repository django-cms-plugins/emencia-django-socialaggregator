.. _django-taggit: https://pypi.python.org/pypi/django-taggit
.. _twitter: https://pypi.python.org/pypi/twitter
.. _python-instagram: https://pypi.python.org/pypi/python-instagram
.. _facebook-sdk: https://pypi.python.org/pypi/facebook-sdk
.. _feedparser: https://pypi.python.org/pypi/feedparser
.. _google-api-python-client: https://pypi.python.org/pypi/google-api-python-client
.. _django-cms: http://www.django-cms.org/

emencia-django-social-aggregator
================================

This app is an aggregator of social network.

A command script will recover data from social network/external site from
**Aggregator** info that you specified into admin. It will stock them into
database, like **Ressource** and you could manage them into admin. You could
regroup **Ressource** by **Feed** and return them into JSON or HTML view.

Optionally you can use it as a plugin for `django-cms`_ if installed.

Requires
********

* `django-taggit`_
* `twitter`_
* `python-instagram`_
* `facebook-sdk`_
* `feedparser`_
* `google-api-python-client`_


Install
*******

In your settings.INSTALLED_APPS : ::
   
    'taggit',
    'socialaggregator',
   
Then import basic settings in your settings file : ::

    from socialaggregator.settings import *

Usage
*****

As a view
---------

First, add this row in your ``urls.py`` : ::

    url(r'^socialaggregator/', include('socialaggregator.urls')),

Then you will access to your feed ressources list as a HTML page with an url like this : ::

    /socialaggregator/feed/sample/

Or you can use the JSON version : ::

    /socialaggregator/feed/sample/?format=json

As a django-cms plugin
----------------------

Just use the plugin named "Socialaggregator Feed Plugin" in your page with selecting the feed you want to list the ressources. The template ``socialaggregator/cms_plugin_feed.html`` will be used to display the feed ressources, override it in your project to use your own HTML layout.

Unified content datas
---------------------

Because feeds can contains ressources from many social networks, a method ``get_unified_render`` exist on the ``Ressource`` model. The method use formatter loaded from the setting ``RESSOURCE_FORMATTER`` if defined, else it will load the default formatter ``socialaggregator.formatter.RessourceFormatterDefault``.

The default formatter return a dict with an unified data scheme, so you can use it in your template without to test if a field is filled or not, etc.. This is optionnal, you can still directly use the ressource instance and play with its fields. You can use it like so : ::

    {% for ressource_item in feed_ressources %}{% with ressource_item.get_unified_render as ressource %}
    <li>
        {% if ressource.title %}<h2>{{ ressource.title }}</h2>{% endif %}
        {% if ressource.description %}<p>{{ ressource.description|safe|linebreaksbr }}</p>{% endif %}
    </li>
    {% endwith %}{% endfor %}

Note that the formatter is not automatically applied, so the JSON view output still return ressource instances serialized.
