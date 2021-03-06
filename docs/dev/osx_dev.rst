=======================
Mac OSX Developer Setup
=======================

**Under construction**

Base OS: Mac OSX

This guide will setup an ODC core development environment and includes:

 - Anaconda python using conda environments to isolate the odc development environment
 - installation of required software and useful developer manuals for those libraries
 - Postgres database installation with a local user configuration
 - Integration tests to confirm both successful development setup and for ongoing testing
 - Build configuration for local ODC documentation


Required software
-----------------

Postgres:

    Download and install the EnterpriseDB distribution from `here <https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#macosx>`_

Optional packages (useful utilities, docs):

 
Python and packages
-------------------

Python 2.7 and 3.5+ are supported. Python 3.6 is recommended.

Anaconda Python
~~~~~~~~~~~~~~~

`Install Anaconda Python <https://www.continuum.io/downloads#macos>`_

Add conda-forge to package channels::

    conda config --add channels conda-forge

Conda Environments are recommended for use in isolating your ODC development environment from your system installation and other python evironments.

Install required python packages and create an ``odc`` conda environment.

Python 3.5::

    conda env create -n odc --file .travis/environment_py35.yaml sphinx

 
Python 2.7::

    conda env create -n odc --file .travis/environment_py27.yaml sphinx


Activate ``odc`` python environment::

    source activate odc


Postgres database configuration
-------------------------------

This configuration supports local development using your login name.

If this is a new installation of Postgres on your system it is probably wise to set the postgres user password. As the local “postgres” Linux user, we are allowed to connect and manipulate the server using the psql command.

In a terminal, type::


Set a password for the "postgres" database role using the command::

	
and set the password when prompted. The password text will be hidden from the console for security purposes.

Type **Control+D** or **\q** to exit the posgreSQL prompt.


Since the only user who can connect to a fresh install is the postgres user, here is how to create yourself a database account (which is in this case also a database superuser) with the same name as your login name and then create a password for the user::


Now we can create an ``agdcintegration`` database for testing::

 
Connecting to your own database to try out some SQL should now be as easy as::




Open Data Cube source and development configuration
---------------------------------------------------

Download the latest version of the software from the `repository <https://github.com/opendatacube/datacube-core>`_ ::

    git clone https://github.com/opendatacube/datacube-core
    cd datacube-core

We need to specify the database user and password for the ODC integration testing. To do this::

Then edit the ``~/.datacube_integration.conf`` with a text editor and add the following lines replacing ``<foo>`` with your username and ``<foobar>`` with the database user password you set above (not the postgres one, your ``<foo>`` one)::

    [datacube]
    db_hostname: localhost
    db_database: agdcintegration
    db_username: <foo>
    db_password: <foobar>



Verify it all works
-------------------

Run the integration tests::

    cd datacube-core
    ./check-code.sh integration_tests

 
Build the documentation::

    cd datacube-core/docs
    make html

