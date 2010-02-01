===========================================================
 mixpanel-celery - Asynchronous event tracking for Mixpanel
===========================================================

:Version: 0.0.1

Introduction
============

mixpanel-celery helps you use `Celery`_ to asynchronously track your `Mixpanel`_
events. You want to perform your tracking asynchronously, because waiting for HTTP
requests to Mixpanel to complete every time you want to record something important
isn't ideal when you've already worked so hard to tune your performance.

mixpanel-celery works great with Django, but because Celery works with just
python, so does mixpanel-celery.

Installation
============

The easiest way to install the current development version of mixpanel-celery is
via ``pip``::

    $ pip install -e git+git://github.com/winhamwr/mixpanel-celery.git


Configuration
=============

For easy test usage with Django, set your Mixpanel api token in your project's
``settings.py`` file with the ``MIXPANEL_API_TOKEN`` variable. Then set::

    ``CELERY_ALWAYS_EAGER`` = True

So that all of your `Celery`_ tasks will run in-line for now.

Then add ``mixpanel`` to your list of ``INSTALLED_APPS``.

Note: Obviously you'll want to actually configure ``Celery`` using one of the
many available backends for actual production use and ``Celery`` has great
documentation on that.

Usage
=====

Basic python example tracking an event called 'my_event'::

.. code-block: python

    from mixpanel.tasks import track_event

    track_event('my_event', {'user_id': 1}, token='YOUR_API_TOKEN')


Example usage in a Django view::

.. code-block:: python

    from mixpanel.tasks import track_event
    from django.shortcuts import render_to_response

    def test_view(request, template='test/test_view.html'):
        """
        Show a user a test page. Records
        """
        user = request.user

        # We should record that the user hit this page
        track_event('hit_test_view', {'user_id': user.pk})

        context = RequestContext(request, {})
        return render_to_response(template, context_instance=context)


Bug Tracker
===========

If you have feedback about bugs, features or anything else, the github issue
tracking is a great place to report them:
http://github.com/winhamwr/mixpanel-celery/issues

License
=======

This software is licensed under the ``New BSD License``. See the ``LICENSE``
file in the top distribution directory for the full license text.

Versioning
==========

This project uses `Semantic Versioning`_.

.. _`Celery`: http://ask.github.com/celery/
.. _`Mixpanel`: http://mixpanel.com/
.. _`sphinx`: http://sphinx.pocoo.org/
.. _`online mixpanel-celery documentation`: http://winhamwr.github.com/mixpanel-celery/
.. _`Semantic Versioning`: http://semver.org/