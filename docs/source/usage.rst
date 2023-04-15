Using the API
=============

Getting Started
---------------

If you haven't already installed and setup the pip package, see the `Getting Started` page.

Mappings
--------

``POST /api/v1/mappings``

The free text ``mappings`` endpoint takes submitted text and determines the most likely ATT&CK technique(s) that are described in the text.

There are two inputs for the function:

- ``text``: *Required.* This is the text you wish to have mapped to ATT&CK.
 
- ``source``: *Optional.* By default, this is set to ``free text`` but you can change this by passing in a different value. 
  
  **Important Note.** Mappings which have a source of ``free text`` are not available via the ``Insights`` endpoint. More details can be found below in that section.

>>> text = "In the campaign we observed..."
>>> narcapi.mappings(text, "https://goapollo.ai/blog/abc")
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

-----------------------

``POST /api/v1/mappings_bulk``

The ``mappings_bulk`` endpoint takes a URL or list of URLs as input, splits the website's text into distinct sentences, and runs them each through the mapping functionality. 

The input should be formatted as a Python list of URL strings, even if there's just 1 URL being submitted.

- ``sources``: *Required* This is the URL(s) that you want to be submitted and mapped to ATT&CK.

>>> sources = ["https://goapollo.ai/blog/abc", "https://goapollo.ai/blog/def"]
>>> narcapi.mappings_bulk(sources)
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

Chains
------

``POST /api/v1/chains``

The ``chains`` endpoint takes submitted text and determines the operational flow of the text described, and maps that flow to ATT&CK. An operational flow, or `Attack Flow <https://mitre-engenuity.org/blog/2022/10/27/attack-flow/>`_, is the sequence of actions and behaviors that lead from one to the next. We've decided to call this a "Chain."

Similar to the ``mappings`` endpoint, there are two inputs:

- ``text``: *Required.* This is the text you wish to parse the operational flow for.
 
- ``source``: *Optional.* By default, this is set to ``free text`` but you can change this by passing in a different value. 

  **Important Note.** Chains which have a source of ``free text`` are not available via the ``Insights`` endpoint. More details can be found below in that section.

>>> text = "In the campaign we observed..."
>>> narcapi.chains(text, "https://goapollo.ai/blog/abc")
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

--------------------

``POST /api/v1/chains_bulk``

Similar to the ``mappings_bulk`` endpoint, the ``chains_bulk`` endpoint takes a URL or list of URLs and parses the operational flow (chain), before mapping those steps to ATT&CK.

- ``sources``: *Required* This is the URL(s) that you want to determine chains for.

>>> sources = ["https://goapollo.ai/blog/abc", "https://goapollo.ai/blog/def"]
>>> narcapi.chains_bulk(sources)
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

Insights
--------

The ``insights`` endpoints are for the retrieval of ``mappings`` or ``chains`` objects that have been previously recorded across our user base. **We do not currently store mappings or chains by individual user.** As mentioned previously, only those objects that **do not** have a source value of ``free text`` and **do** contain ``https://`` (which is required upon submission) will be returned via these endpoint. This is to maintain a relative amount of privacy for those users who may submit internal or non-public text to the API. 

The core intent behind the ``insights`` endpoint is to only return technique mappings or chains that are available publicly, which is indicating by an active URL. As we assess how our users interact with the API, this functionality may be updated but will maintain that core intent.

----------------------------

``GET /api/v1/mappings_insights``

The ``mappings_insights`` function takes in a single argument, an ATT&CK technique ID, and returns the most recent 50 ``mappings`` objects that contain the technique ID in their ``technique_probability`` dictionary. 

>>> technique_id = "T1059.001"
>>> narcapi.mappings_insights(technique_id)
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

**Extra details**

The sort order for all ``insights`` objects is determined by the ``reported_date`` field. This field is determined in two different ways: 

- If the ``mappings`` object was the result of a ``free text`` submission, the ``reported_date`` will be the date of submission.

- If the ``mappings`` object was the result of a URL submission, the ``reported_date`` will be the date of reporting found via the URL processing. 

Though this may lead to some inconsistencies, especially if time series analysis is an objective, we believe the simplicity of the endpoints outweighs that edge case. 

---------------------

``GET /api/v1/chains_insights``

Like the ``mappings_insights``, the ``chains_insights`` endpoint accepts a single ATT&CK technique ID as input, and returns the most recent 50 chains objects that contain the technique ID within their ``flow`` field. 

>>> technique_id = "T1059.001"
>>> narcapi.chains_insights(technique_id)
[{'created_by': 'apollo_ai', 'analysis_date': ... }]

