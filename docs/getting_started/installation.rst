.. _Miniconda: https://conda.pydata.org/miniconda.html
.. _Mambaforge: https://github.com/conda-forge/miniforge#mambaforge
.. _Mamba: https://github.com/mamba-org/mamba
.. _Conda: https://conda.pydata.org
.. _instructions: https://mamba.readthedocs.io/en/latest/installation.html
.. _platform: https://github.com/freeseek/gtc2vcf


.. _getting_started-installation:

============
Installation
============


Iliad is Snakemake workflow management system and can be cloned from the `GitHub repository <https://github.com/ncherric/Iliad>`_.
It largely reduces the learning curve and software dependency installations by taking advantage of pre-built execution environments for Iliad. 
It still requires the installation of the native package manager for the Snakemake application itself and Singularity.
Many HPC users at an academic institution will already have Singularity on their clusters.

.. _conda-install:

Step 1: Install Snakemake and Snakedeploy
=============================================

Snakemake and Snakedeploy are best installed via the `Mamba package manager <https://github.com/mamba-org/mamba>`_ (a newer package manager for Conda replacement). 
From our testing, Conda is perfectly fine as an alternative, and you can use them interchangeably below.
If you have neither Conda nor Mamba, it can be installed via `Mambaforge <https://github.com/conda-forge/miniforge#mambaforge>`_. 
For other options see `here <https://github.com/mamba-org/mamba>`_.

You will need to add Mamba to PATH so follow their instructions_ for such.

Given that Mamba (or Conda) is installed, run the following in your command line interface tool 
(i.e. `Putty <https://www.putty.org/>`_, 
`MacOS Terminal <https://support.apple.com/guide/terminal/open-or-quit-terminal-apd5265185d-f365-44cb-8b09-71a064a42125/mac>`_,
or `VS Code <https://code.visualstudio.com/>`_).
If you need guidance for a specific platform you are using, see the specific `platform`_ preparation guides and they will help lead you to this point.

.. code-block:: console

    $ mamba create -c conda-forge -c bioconda --name iliadEnv snakemake snakedeploy openpyxl

to install Snakemake, Snakedeploy, and a necessary python library for parsing user input tables in an isolated environment.
For all following commands ensure that this environment is activated via


.. code-block:: console

    $ mamba activate iliadEnv

If you're on a `Windows machine`, best use the command prompt (not powershell) in order to get into the environment.

**Estimated time: 15-20 minutes**

Step 2: Install Singularity
============================

Follow the `instructions guide <https://docs.sylabs.io/guides/3.6/user-guide/quick_start.html>`_ provided by Singularity to install the program under the Operating System of your choosing.
If you are on an HPC server, it may have a Singularity module available to load already.

Make sure Singularity is in your PATH and accessible by Snakemake.

**Estimated time: 30-60 minutes**

Step 3: Deploy workflow
============================

Given that Snakemake and Snakedeploy are installed and available (see Step 1), the workflow can be deployed as follows.

First, create an appropriate project working directory on your system and enter it:

.. code-block:: console

    $ mkdir -p path/to/project-workdir
    $ cd path/to/project-workdir

In all following steps, we will assume that you are inside of that directory.

Git clone the `GitHub repository <https://github.com/ncherric/Iliad>`_.

.. code-block:: console

    $ git clone https://github.com/ncherric/Iliad.git
    $ cd path/to/project-workdir/Iliad

Two important folders found in the Iliad directory are **workflow** and **config**.
The former contains rules and scripts that a designated Snakefile in Iliad call on to run a specific module.
The latter contains configuration files which will be modified in the next step in order to configure the workflow to your needs.
Later, when executing the workflow, Snakemake will automatically find the main Snakefile in the workflow subfolder which is the **Raw Sequence Read Data** module.
However, there are other Snakefiles that are specific to the other Modules:

* Snakefile -> Raw Sequence Read Data
* cram_Snakefile -> Stored Sequence Read Data
* snpArray_Snakefile -> SNP Array Data
* LiftoverTo38_Snakefile -> Submodule to liftover GRCh37 assembly VCF data to GRCh38 assembly
* LiftoverTo37_Snakefile -> Submodule to liftover GRCh38 assembly VCF data to GRCh37 assembly


Consider to put this directory under version control, e.g. by managing it via a (private) Github repository.
Visit the Tutorial page for further info about each of the Modules in the bulleted list above.


**side note**
( Once this pipeline is publicly available, and added to the Snakemake Workflow Catalog, run below. For now, just **clone ABOVE** )

.. code-block:: console

    $ snakedeploy deploy-workflow https://github.com/snakemake-workflows/Iliad . --tag v1.0.0


Step 4: Configure Workflow
============================

To configure this workflow, modify config/config.yaml according to your needs, following the explanations provided in the file.


Step 5: Run workflow
============================

Given that the workflow has been properly deployed and configured, it can be executed as follows.

For running the workflow while deploying any necessary software via singularity and conda (using the Mamba package manager by default), run Snakemake with

.. code-block:: console

    $ snakemake -p --use-singularity --use-conda --cores 1 --jobs 1 --snakefile workflow/snpArray_Snakefile --default-resource=mem_mb=10000 --latency-wait 120


Snakemake will automatically detect the main Snakefile in the workflow subfolder and execute the workflow module that has been defined by the deployment in step 2.
