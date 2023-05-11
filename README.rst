ATMS Computing Documentation
=======================================

.. image:: https://readthedocs.org/projects/computing-docs/badge/?version=latest
    :target: https://computing-docs.readthedocs.io/en/latest/?badge=latest
    :alt: Documentation Status   

This GitHub repository contains documentation for computing
within the Department of Atmospheric Sciences at the University of Illinois.

The latest version of documentation may be viewed at https://computing-docs.readthedocs.io/en/latest/

Steps for producing documentation locally
-----------------------------------------

For contribution/testing purposes, the documentation may be compiled locally by changing into
the ``docs`` directory by:

``cd docs``

And the HTML code can then be produced by:

``make html``

with the result being viewable by accessing ``build/html/index.html``.

Recommended workflow for contributing to this documentation project
-------------------------------------------------------------------

#. Fork this repository by clicking the Fork button on the atmsillinois/uiuc-atmos-computing GitHub page or clicking `here <https://github.com/atmsillinois/uiuc-atmos-computing/fork>`_
#. Clone the forked repository locally: ``git clone https://github.com/<GitHub username>/<whatever you named your fork repo>.git``
#. Checkout a new branch locally that you intend to work on (as an example, called ``new_documentation``): ``git checkout -b new_documentation``
#. Consider submitting an `Issue <https://github.com/atmsillinois/uiuc-atmos-computing/issues>`_ listing your anticipated contribution or comment on an existing Issue.
#. Make desired changes to this local branch to address your target issue.
#. Push this new branch to your Fork: ``git push origin new_documentation``
#. Create a `Pull Request <https://github.com/atmsillinois/uiuc-atmos-computing/compare>`_ with ``atmsillinois/uiuc-atmos-computing`` ``main`` by selecting "compare across forks" and suggesting that your new branch be incorporated. This may be initially done as a "Draft" if your work is not yet finalized.
