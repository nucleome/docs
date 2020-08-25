================
Data preparation
================

Data type supported
===================

Nucleome Browser supports multimodal data for visualization and exploration.
For genomic data, Nucleome Browser supports data in bigBed, bigWig, tabix, and .hic format.
For imaging data, Nucleome Browser utilizes the OMERO server to host imaging data.
The supported formats of imaging data for OMERO server are list at the `OMERO's website <https://docs.openmicroscopy.org/bio-formats/5.9.2/supported-formats.html>`_.
For 3D genome structure data, Nucleome Browser uses a custom format that can be easily transformed from popular formats such as `cmm <http://www.cgl.ucsf.edu/chimera/docs/ContributedSoftware/volumepathtracer/volumepathtracer.html#markerfiles>`_.

Genomic data
============

We provide a command-line tool (NucleServer) or its GUI version (NucleData) to host your own data server across macOS, Windows, and Linux systems. 
Both NucleServer and NucleData will start a local data server given a configuration file provided by the user.
Once the data server is started, it will connect to the Nucleome Browser hosted in our main portal (`http://vis.nucleome.org <http://vis.nucleome.org>`_) and you can load the data from your local data server in Nucleome Browser main portal.
`NucleServer <https://github.com/nucleome/nucleserver>`_ and `NucleData <https://github.com/nucleome/nucledata>`_ for different platforms can be downloaded from GitHub.

NucleServer and NucleData
-------------------------

**Installation**

Once the program is downloaded, make sure to use the ``chmod`` command to make the program executable in Linux and Mac system as shown below:

.. code-block:: bash

    # NucleServer
    chmod +x nucleserver
    # NucleData
    chmod +x nucledata


In Windows, the command-line tool such as Windows Terminal and PowerShell will be required to run NucleServer.
Alternatively, you can download the latest source code and compile it.
NucleServer is written in GoLang ( version 1.11+ ) and hosted on Github.
After the Golang environment is installed, the source code can be cloned using the following command.

.. code-block:: bash

    # clone the latest source code from GitHub
    go get -u github.com/nucleome/nucleserver

**Prepare a configuration file**

Both NucleServer and NucleData requires an Excel file to specify the source of data and their configurations.
We recommend you create this Excel file using the Google Sheet and downloading it in the .xlsx format so that you easily share it with others.
Alternatively, you can create an Excel file using the Microsoft Excel software.
A template of the configuration Excel file can be downloaded from `https://vis.nucleome.org/static/ndata/cnb.xlsx <https://vis.nucleome.org/static/ndata/cnb.xlsx>`_.

:numref:`worksheet_overall` shows the overall architecture of the configuration Excel file.
The configuration file (.xlsx format) is formulated as 2+ worksheets.

.. figure:: img/figures_chapter_6/Data_sheet_overall.png
    :name: worksheet_overall
    :align: center
    :figwidth: 640px

    The overall structure of the configuration Excel file

The first worksheet must be named as ``Config``, and the second one must be named as ``Index``. 
The rest worksheets contain the details of the source of data and configuration.

.. figure:: img/figures_chapter_6/Data_sheet_name.png
    :align: center
    :figwidth: 480px

    Examples of worksheet in a configuration file

- **Config sheet**:

    The Config worksheet contains global parameters.
    The first row of the Config worksheet is the header and should be named as ``Var`` and ``Value``, respectively.
    Currently, the ``root`` parameter under the column of Var is the only parameter required for NucleServer and NucleData.
    This ``root`` parameter stores the root path for all genomic data files that are stored on the local computer.
    The purpose of this parameter is to help users conveniently migrate data between computers.
    For example, if all the files are stored in a folder under the path (``/home/tom/project/genomic-data/``), you can set the value of root in the column of Value as /home/john/project/genomic-data/.
    Later, when you fill in the worksheet of data, you can just put the filename of a data in the file link column (see below) and NucleServer and NucleData will know where to look for these files.
    It is possible that some of the data are stored on the cloud.
    In that case, you can just put the URLs of data in the worksheet of data.
    NucleServer and NucleData assume that file links of data do not start with HTTP or HTTPS are all stored locally on your computer.

.. figure:: img/figures_chapter_6/Data_sheet_config.png
    :align: center
    :figwidth: 640px

    Required parameters in the Config worksheet

- **Index sheet**:

    The Index sheet contains the info of all the Data Sheets. 
    The meaning of each column is shown in :numref:`sheet_index`.
    The first row of the Index sheet is the header.
    Each row indicates one data worksheet.
    The first column is the genome assembly of data. 
    Nucleome Browser supports all the reference assembly provided by the UCSC genome browser.
    The genome assembly name should be in lowercase letters and all data in that data worksheet is based on the same genome assembly.
    The second column is the name of a specific data worksheet.
    The third column is set for future features and currently not used (you can put 'track' here).

.. figure:: img/figures_chapter_6/Data_sheet_index_v3.png
    :name: sheet_index
    :align: center
    :figwidth: 640px

    Scheme of the Index worksheet

- **Data sheet**

    The fourth and fifth columns of the Index sheet contain the most important parameters of data.
    There are two types of ways to configure this column: **two-column** setting and **four-column** setting. 
    The fourth column indicates the short label of each data.
    In the two-column setting, the fourth column is the column-index of the short label of each data, and the fifth column refers to the column-index of the file path (or URL) in the data worksheets.
    In the four-column setting, the fourth column is the column-index of the short label of each data, and the fifth column refers to the column-index of the file path (or URL), URL of meta-information, and long label.
    Note that in the four-column setting, the order of the column-index must be the file path, URL of meta-information link matters. 
    The column-index of the file path in a data worksheet can be arbitrary but it must be the first one in the fifth column in the Index sheet. 

.. figure:: img/figures_chapter_6/Data_sheet_two-four_column.png
    :align: center
    :figwidth: 640px

    Scheme of the Data worksheet. 
    In the two-column setting, the shortLabel and file link is required. In the four-column setting, shortLabe, file link, metaLink, and longLabel is required. Note that the first row is the header and can be named to anything. Other data can be stored in other columns. The order of columns does not matter, as long as the order index of the column is correct in the Index worksheet.

BigBed and bigWig files are binary indexed files with data in multiple resolutions.
For data stored in the web, NucleServer and NucleData will only fetch index files (usually only less than 1\% size of the original file) from web links and store them locally.
The default location for storing the index files is ``<user's home directory>/.nucle/index``.
However, we highly recommend downloading .hic file to your local computer and host them locally to provide the fastest speed of browsing.

**Start a data service**

Start a local server using NucleServer and NucleData is very simple.
The command to start a server in Mac OS or Linux using NucleServer is the following.

.. code-block:: bash

    ./nucleserver start -i [path to the excel configuration file] -p [port default:8611]

The command to start the server in Windows is the following:

.. code-block:: bash

    nucleserver.exe start -i [path to the excel configuration file] -p [port default:8611]

By default, the local server will use the 8611 port.
You can change that by setting the -p argument.
In NucleServer, if everything goes fine, you should see a log similar to below.

.. figure:: img/figures_chapter_6/Server_start_successful.png 
    :align: center
    :figwidth: 640px

    Screenshot showing a log of NucleServer starting a local data server

If you use NucleData, just follow the instructions on the interface to load the Excel file and set the port manually as shown below:

Next, go to Nucleome Browser by typing `http://vis.nucleome.org <http://vis.nucleome.org>`_ in your web browser.
Go to the default genome browser panel or create a new genome browser panel.

.. figure:: img/figures_chapter_6/Create_a_genome_browser_panel.png
    :align: center
    :figwidth: 420px
    
    Steps to create a new genome browser panel

If you use the default port 8611, this local data server should automatically load in the Nucleome Browser.
Otherwise, you can load it manually by following the procedures in Figure~\ref{fig:ch6_load_server_manually}.
First, click the \textbf{config} button in the genome browser panel as shown in the Step 1 in Figure~\ref{fig:ch6_load_server_manually}).
In the configuration interface, click the data server setting button (gear icon in step 2 in Figure~\ref{fig:ch6_load_server_manually}).
In the data server setting menu, type a name for your data server in the ``Id`` column and type the web link of your local data server (e.g., ``http://127.0.0.1:<port id>``, here port id is the port specified in NucleServer or NucleData) in the ``URI`` column.
Click the ``fresh`` button.
Click the ``Update`` button to refresh the interface and you should see **Active (in green text)** in the rightmost column.
Finally, select the tracks you want to visualize from the selection boxes of tracks, and click the ``config`` button again to return to the interface of the genome browser. 

.. figure:: img/figures_chapter_6/Load_data_server_manually.png
    :align: center
    :figwidth: 640px

    Five steps to manually load local data servers. Users can organize their local data server using the local server configuration interface.

Prepare the configuration file using public Google Sheet
--------------------------------------------------------

NucleServer also supports using a Google Sheet as a configuration file in the cloud. 
This feature is particularly useful when you want to share your data server with others. 
Others can simply start a data server using the public URLs of this Google Sheet.
To do that, you need to first prepare a configuration file using Google Sheet and make this Sheet public (anyone with the link can access this sheet , if this sheet is private only you can use it to start a local data server, see below).
Next, you need to identify the unique ID of this Sheet.

.. figure:: img/figures_chapter_6/Google_sheet_id.png
    :align: center
    :figwidth: 640px

    An example of Google Sheet ID

Finally, you can start your server using the Google Sheet ID as shown below: 

.. code-block:: bash

    nucleserver start -i <Google Sheet ID>

Note that if this is the first time you use NucleServer with Google Sheet, it will firstly print a web link in the terminal, asking for permissions.

.. figure:: img/figures_chapter_6/Google_sheet_step_1.png
    :align: center
    :figwidth: 640px

    For the first time to connect NucleServer to Google Sheet, you need to give permission of NucleServer to read the data on your public Google Sheet.

Open that link in a web browser, log in using your Google account, and grant the permissions.

.. figure:: img/figures_chapter_6/Google_sheet_step_2.png
    :align: center
    :figwidth: 420px

    Give permission to NucleServer using your Google account

Once this is done, Google should provide you a token in response.
Copy this token and paste it in the terminal to finish this process and your local server is ready to use. 

.. figure:: img/figures_chapter_6/Google_sheet_step_3.png
    :align: center
    :figwidth: 420px

    Copy the token back to the terminal

A credential token will be stored in ``[Your Home Dir]/.nucle/credentials/gsheet.json`` so that next time you do not need to permit NucleServer to read public Google sheet on your Google account again.

Host private data in a remote server
------------------------------------

We also provide a simple password protection option (currently experimental) for NucleServer.
To do that, simply add an argument ``-c`` with a password when you start a local data server.

.. code-block:: bash

    nucleserver start -i nucle.xlsx -c password

To visualize those private data in the Nucleome Browser, users have to first login with the password through the following web page.

.. code-block:: bash

    http://[yourwebsite]:8611/main.html

3D structure data
=================

We provide multiple useful tools to help users prepare data/web service to visualize genome 3D structure data in Nucleome Browser. 
Those tools can be get from `https://github.com/nucleome/nucle <https://github.com/nucleome/nucle>`_.
