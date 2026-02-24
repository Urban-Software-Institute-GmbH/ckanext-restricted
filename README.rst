ckanext-restricted
==================

.. contents:: Table of Contents
   :depth: 2
   :local:

.. sectnum::
   :depth: 2


Forking Explained
-----------------

This plugin was forked for two reasons:

The main reason is to adapt the code to work with the YAML schema we are using.

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

The second issue was a 400 Bad Request (The CSRF token is missing) error that occurred after submitting the dataset access request form (as shown in the image below). To resolve this, we had to include {{ h.csrf_input() }} inside the form located at: ckanext/restricted/templates/restricted/restricted_request_access_form.html

Starting from CKAN version 2.10, built-in Cross-Site Request Forgery (CSRF) protection is enabled for all frontend forms. CKAN uses CSRF tokens to prevent security attacks. However, the CSRF token was missing in our custom form template, so we had to add it manually.

.. image:: csrf-token-missing.png
   :alt: 400 Bad Request – CSRF token is missing
   :align: center


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

.. code-block:: bash

    nosetests --nologcapture --with-pylons=test.ini


Registering on PyPI
-------------------

.. code-block:: bash

    python setup.py sdist
    python setup.py register
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
