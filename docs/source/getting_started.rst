Getting Started
===============

Installation
------------

To use the NARC API, first install the package using pip:

.. code-block:: console

   $ pip install narcapi

API Key variable
----------------

You must have an active account with Apollo AI. Your API key can be found in your user console at https://manage.goapollo.ai

Then, set your API key as the ``NARC_API_KEY`` environment variable.

Importing
---------

Import the package before using it. Let's also test that our API key is set and found by the package.

>>> from narcapi import package as narcapi
>>> narcapi.api_key

First NARC call
-------------------

Let's make sure everything is working as it should. Let's try to hit the `mappings` endpoint with the following text:

**In the campaign we observed, BlackByte operators gained initial access by exploiting the ProxyShell vulnerabilities (CVE-2021-34473, CVE-2021-34523, CVE-2021-31207) present on the customer’s Microsoft Exchange server**

>>> text = "In the campaign we observed, BlackByte operators gained initial access by exploiting the ProxyShell vulnerabilities (CVE-2021-34473, CVE-2021-34523, CVE-2021-31207) present on the customer’s Microsoft Exchange server."
>>> narcapi.mappings(text)
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

You should receive a json response from the API that shows our text has been mapped to `T1190`. 

If so, you're all set! 

Check out the Usage page for a full rundown of the endpoints and their functionality.

