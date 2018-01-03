========================
python-facebook-insights
========================

Collect and store `Facebook Insights`_ metrics. This is a clone of original project https://github.com/nevimov/django-facebook-insights, to remove django dependency and make it work other python based web applications like flask/cherry etc.,

This app provides a flexible abstract model with method `fetch()`. It gets all
required metrics with a single `batch request`_ to Graph API.

**Requirements**:

* facebook-sdk 1.0.0+

.. contents::
   :depth: 1
   :backlinks: top


Installation
------------

Install the app with pip::

    $ pip install python-facebook-insights

  Finally, provide a valid access token with the 'read_insights' permission using
setting `FACEBOOK_INSIGHTS_ACCESS_TOKEN`.


Usage example
-------------

In the simplest case, all you need to do is to write code similar to the
one below::

    from facebook_insights.metrics import fetch_metrics


Now, you can instantiate the model and call fetch() to get the metrics from
Facebook's servers::

>>> graph_id = '1234567890'
>>> metrics = ['page_impressions', 'page_engaged_users']
>>> page_metrics = fetch_metrics(graph_id, metrics)
>>> page_impressions = page_metrics['page_impressions']
>>> page_impressions.values


Extracting field values
-----------------------

Values associated with page metrics are quite complex. They are available for
several periods (e.g. day, week, 28 days) and include data for 3 consecutive
days. By contrast, values of most of post metrics are available only for one
period (lifetime) and represent the current state of things.

The extraction of metric values is the responsibility of the
`get_field_value()` method. The default implementation works as follows:

* If a metric has several periods, return the dictionary of mappings between
  the periods and the last available values for these periods serialized into
  JSON, for example, `'{"day": 10, "week": 70, "days_28": 300"}'`. The data
  for previous days are discarded.
* If a metric is provided only for a single period, then simply return the
  value (serialize, if it's not a number).

Feel free to override the method, if you want something else.


Reporting bugs
--------------

If you've found a bug:

* Check to see if there's an existing issue/pull request for the bug.

  | PR:     https://github.com/prasadvk/python-facebook-insights/pulls
  | Issues: https://github.com/prasadvk/python-facebook-insights/issues

* If there isn't one, file an issue. A bug report should include:

  * a description of the problem
  * instructions on how to recreate the bug
  * versions of your Python interpreter and Django


Contributing code
-----------------

* Fork the project on GitHub to your account.

* Clone the repository::

    $ git clone https://github.com/prasadvk/python-facebook-insights

* Setup a virtual environment::

    $ virtualenv venv
    $ source venv/bin/activate
    $ pip install -U pip
    $ pip install -r requirements.txt

* In directory 'tests' create a file named 'secret.py'. In this file, set
   the `FACEBOOK_INSIGHTS_ACCESS_TOKEN` setting'.  Alternatively, define an
   environment variable with the same name.

* Run tests to ensure everything is OK::

    $ python runtests.py

  You can use *-h* or *--help* to see options available to the script.

* Create a topic branch and commit your changes there.

* Push the branch up to GitHub.

* Create a new pull request.


.. _Object Insights:
.. _Facebook Insights: https://developers.facebook.com/docs/graph-api/reference/v2.11/insights
.. _batch request: https://developers.facebook.com/docs/graph-api/making-multiple-requests
