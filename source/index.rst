============================================
Nucleome Browser: A Multimodel Visualization for 4DN Nucleome
============================================

`Nucleome Browser <https://vis.nucleome.org>`_ is a multimodal, interactive data visualization and exploration platform to integrate genomic, imaging, and 3D structure data.
It can empower investigators to fully utilize heterogeneous datasets to formulate hypotheses and gain new insights into the interplay among different constituents in the nucleus and their roles in 3D genome structure and function.

.. figure:: img/other/NB_dispatch_v2.gif
    :align: center
    :figwidth: 640px

    Nucleome Browser consists of multiple comfigurable, communicable, and composable web components (panels) for visualization multi-modal data. Operation in any panel will be synchronized to all connected panels and external web sites such as the UCSC Genome Browser and the WashU Epigenome Browser.

Quick Start
~~~~~~~~~~~

To explore Nucleome Browser, go to the main portal of Nucleome Browser at `https://vis.nucleome.org <https://vis.nucleome.org>`_.
You can find all the resources related to the platform including examples of pre-configured browser sessions, detailed documentation, and a slack channel for discussion and reporting issues.

.. figure:: img/figures_chapter_1/ch1_home_page.png
    :align: center
    :figwidth: 640px
    
    Home page of the Nucleome Browser


Clicking the ``Example`` button (|home_example|) on the top-right toolbar on the home page, you will see a series of thumbnails for browser sessions. 
You can explore those examples by clicking the ``enter`` button (|example_enter|). 
For example, in the **Multi-modal integrative analysis** example, you will see a browser session showing a comparison between K562 and H1 cells. 

.. |home_example| image:: img/other/icon/icon-home-examples.png
    :height: 14px

.. |example_enter| image:: img/other/icon/icon-example-go.png
    :height: 14px

Two genome browser panels on the left show various genomic data between K562 and H1, including Hi-C contact matrices, TSA-seq, and DamID tracks mapping to multiple nuclear bodies (e.g., nuclear speckles, nuclear lamina, and nucleolus), and replication-timing profiles. 
On the right, there are two 3D structure panels show 3D structure with color representing genomic signals TSA-seq Lamina B1 signal in H1 and K562, respectively. 
Notably, in the middle genome browser component, you can see an image track showing DNA FISH data. 
Clicking on one image, a pop-out OMERO.iviewer window will appear allowing you to further explore the image.

.. figure:: img/figures_chapter_1/ch1_example.png
    :align: center
    :figwidth: 640px

    Nucleome Browser faciliates interactive exploration of multi-modal data

.. toctree::
    :hidden:
    :glob: 
    
    design
    components
    plugin
    session
    data_service
    nb_dispatch_api

.. toctree::
    :hidden:
    :glob:
    :titlesonly:

