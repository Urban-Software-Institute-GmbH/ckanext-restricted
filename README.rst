ckanext-restricted
==================

CKAN extension to restrict access to dataset resources.

.. contents:: Table of Contents
   :depth: 2
   :local:


Forking Explained
-----------------

This plugin was forked in order to adapt the code to work with the YAML schema we are using.

In the original implementation, the plugin expects the restriction data to be saved like this:

.. code-block:: json

    {
      "restricted": {
        "level": "only_allowed_users",
        "allowed_users": "ckanuser7"
      }
    }

or:

.. code-block:: json

    {
      "extras": {
        "restricted": "{\"level\":\"only_allowed_users\",\"allowed_users\":\"ckanuser7\"}"
      }
    }

However, our CKAN Scheming YAML schema saves the data like this:

.. code-block:: json

    {
      "restricted-level": "only_allowed_users",
      "restricted-allowed_users": "ckanuser7"
    }

Therefore, we adapted the plugin to fit our restricted schema.

Our schema:
https://github.com/Urban-Software-Institute-GmbH/ckanext-restricted/blob/master/schemas/ckan_dataset.yaml


ckanext-restricted
------------------

This extension restricts the accessibility of dataset resources.

The dataset metadata remains accessible, but the resource files can be restricted.

Users can request access to a dataset by pressing a button and filling out a form. The dataset owner can then grant access to individual users.

Email notifications are supported for:

- Access requests
- New user registrations (optional)

All restricted fields (except ``level``) are hidden from users who do not have edit permissions.

Screenshots
~~~~~~~~~~~

.. image:: restricted_resources_preview.PNG
   :alt: Package view with restricted resources
   :align: center

.. image:: restricted_resources_metadata.PNG
   :alt: Resource metadata including restriction configuration
   :align: center

.. image:: restricted_resources_request_form.PNG
   :alt: Request form for restricted resources
   :align: center


Requirements
------------

Originally developed for CKAN 2.5.2 and compatible up to CKAN 2.11.x.

Required extensions:

- ckanext-scheming
- ckanext-repeating
- ckanext-composite

Alternative implementation without scheming:
https://github.com/olivierdalang/ckanext-restricted/commit/89693f5e4a2a4dedf2cada289d1bf46bd7991069


Example Scheming Field Definition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

    {
      "field_name": "restricted",
      "label": "Access Restriction",
      "preset": "composite",
      "subfields": [
        {
          "field_name": "level",
          "label": "Level",
          "preset": "select",
          "form_include_blank_choice": false,
          "required": true,
          "choices": [
            {"value": "public", "label": "Public"},
            {"value": "registered", "label": "Registered Users"},
            {"value": "any_organization", "label": "Any Organization Members"},
            {"value": "same_organization", "label": "Same Organization Members"},
            {"value": "only_allowed_users", "label": "Allowed Users Only"}
          ]
        },
        {
          "field_name": "allowed_users",
          "label": "Allowed Users",
          "preset": "tag_string_autocomplete"
        }
      ]
    }


reCAPTCHA Configuration
~~~~~~~~~~~~~~~~~~~~~~~

Add the following to your CKAN config file:

.. code-block:: ini

    ckan.recaptcha.version = 2
    ckan.recaptcha.privatekey = YOUR_PRIVATE_KEY
    ckan.recaptcha.publickey = YOUR_PUBLIC_KEY


Installation
------------

1. Activate your CKAN virtual environment:

.. code-block:: bash

    . /usr/lib/ckan/default/bin/activate

2. Install the extension:

.. code-block:: bash

    pip install ckanext-restricted

3. Add ``restricted`` to ``ckan.plugins`` in your CKAN config file.

4. Restart CKAN.


Config Settings
---------------

Only the Scheming configuration (JSON/YAML schema) is required.


Development Installation
------------------------

.. code-block:: bash

    git clone https://github.com/espona/ckanext-restricted.git
    cd ckanext-restricted
    python setup.py develop
    pip install -r dev-requirements.txt


Running the Tests
-----------------

Run tests:

.. code-block:: bash

    nosetests --nologcapture --with-pylons=test.ini

Run tests with coverage:

.. code-block:: bash

    nosetests --nologcapture --with-pylons=test.ini \
      --with-coverage --cover-package=ckanext.restricted \
      --cover-inclusive --cover-erase --cover-tests


Registering on PyPI
-------------------

Create distribution:

.. code-block:: bash

    python setup.py sdist

Register:

.. code-block:: bash

    python setup.py register

Upload:

.. code-block:: bash

    python setup.py sdist upload


Releasing a New Version
-----------------------

1. Update version in ``setup.py``.
2. Build distribution:

.. code-block:: bash

    python setup.py sdist

3. Upload:

.. code-block:: bash

    python setup.py sdist upload

4. Tag release:

.. code-block:: bash

    git tag 0.0.2
    git push --tags
