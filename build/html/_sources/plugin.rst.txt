=======
Plugins
=======

Nucleome Browser allows users to sync genome coordinates between other web applications/pages.
The Nucleome Bridge web extension is available in `Chrome store <https://chrome.google.com/webstore/detail/nucleome-bridge/djcdicpaejhpgncicoglfckiappkoeof>`_, and can be used in several web browsers such as Chrome, Brave, Opera, and Edge (or any web browsers based on Chromium).
It also works in browsers that support `external_connectable <https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json/externally_connectable>`_ web API.

Nucleome Bridge extension
-------------------------

Nucleome Browser can communicate genome coordinates event within the `UCSC Genome Browser <https://genome.ucsc.edu>`_ and the `WashU Epigenome Browser <https://epigenomegateway.wustl.edu>`_ through the `Nucleome Bridge <https://chrome.google.com/webstore/detail/nucleome-bridge/djcdicpaejhpgncicoglfckiappkoeof?hl=en>`_.

When the user opens these two web applications in Nucleome Bridge, genome coordinates can be automatically synced between the nucleome browser and these two genome browsers. Meanwhile, the user can highlight regions in Nucleome Browser, the UCSC Genome Browser, and/or WashU Epigenome Browser will respond as well. This feature makes users can easily compare tracks in two or three different genome browsers.

.. figure:: img/figures_chapter_4/ch4_3_browser.png
    :align: center
    :figwidth: 640px

    Use nucleome bridge web browser extension to sync genomic coordinates between nucleome browser, UCSC genome browser, and WashU genome browser. 

Once the Nucleome Bridge web browser extension is installed, the user can open UCSC Genome Browser or WashU Epigenome Browser by clicking the Nucleome Bridge icon.

When the user opens UCSC Genome Browser or WashU Epigenome Browser from Nucleome Bridge, a piece of JavaScript code will insert into this browser and pass the genomic coordinates event message between this browser and Nucleome Bridge. When the user navigates genome from this browser, the updated genome coordinates will post to Nucleome Bridge, and Nucleome Bridge will pass genome coordinates to other connected web applications include Nucleome Browser. On the other hand, when the user update genome coordinates or highlights some genome regions, these genome coordinates will post to Nucleome Bridge, and Nucleome Bridge will pass to UCSC/WashU browser and this browser will update its genome coordinates automatically. Users can open multiple UCSC/WashU windows.

.. figure:: img/figures_chapter_4/ch4_nucleome_bridge.png
    :align: center
    :figwidth: 400px

    Nucleome Bridge web browser extension

Google Spreadsheet add-on
-------------------------

As a proof-of-concept, by importing nb-dispatch into Google Sheet app script, we provide a Google Sheet add-on to make Nucleome Browser work with the Google Sheet web application.

When users navigate to some genomic regions they are interested, they would like to markdown this region for future investigation. 
We provide this function in a web application integration approach.

Here is `an example template <https://docs.google.com/spreadsheets/d/1gD6rWNDDkxS-PTb4DmCsV1B57MircZ18RfNxVgjHOhk>`_. 
Users can clone this sheet and use it as a note application or even revise it into a more powerful tool.

