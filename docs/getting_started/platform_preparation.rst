.. _instructions: https://mamba.readthedocs.io/en/latest/installation.html

.. _getting_started-platform_preparation:

====================
Platform Preparation
====================
########################

Linux
======

If you are on a High-Performance Cluster (HPC) or remote server that runs on a Linux OS, like Ubuntu or Red Hat for example, 
then please follow the instructions_ on the previous installation page!


Windows / PC
============

#. Step 1. Freely download the UBUNTU app from the Windows store

#. Step 2. pen the UBUNTU app and create a username and pw

#. Step 3. Run the following:

obtain mamba package manager:

.. code-block:: console

    curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"

install mambaforge (type 'yes' to all interactive prompts)

.. code-block:: console

    bash Mambaforge-$(uname)-$(uname -m).sh

#. Step 4. RESTART

#. Step 5. Install conda environment for Iliad which contains Snakemake

.. code-block:: console

    mamba create -c conda-forge -c bioconda --name IliadEnv snakemake snakedeploy openpyxl

#. Step 6. Change directories in the UBUNTU command line to get into proper location of your project folder

.. code-block:: console

    cd mnt/DRIVE/Path/To/Iliad/

#. Step 7. Activate your conda environment (IliadEnv)

.. code-block:: console

    Mamba activate IliadEnv

#. Step 8. Get your current location to add to the configuration file

.. code-block:: console

    pwd

#. Step 9. Copy that location and paste into the ``Iliad/config/config.yaml`` as your working directory. This should start with ``/mnt/`` and end with a forward slash ``/`` like so  ``/mnt/Drive/Path/To/Iliad/``


MacOS
=====