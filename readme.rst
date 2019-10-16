|build-status|

In order to build the HTML locally with python3:

.. code-block:: bash
    
    python -m venv .env
    source .env/bin/activate
    pip install -r requirements.txt
    make html
    deactivate

.. |build-status| image:: https://readthedocs.org/projects/musicdsp/badge/?version=latest
    :alt: build status
    :scale: 100%
    :target: https://readthedocs.org/projects/musicdsp
