============
Plugin usage
============

Nucleome Browser allows user to sync genome coordinates between other web applications/pages.
The Nucleome Bridge web extension is available in `Chrome store <https://chrome.google.com/webstore/detail/nucleome-bridge/djcdicpaejhpgncicoglfckiappkoeof>`_, and can be used in several web browsers such as Chrome, Brave, Opera, and Edge (based on Chromium).
It also works in browsers which supports `external_connectable <https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json/externally_connectable>`_ web API.

Nucleome Bridge extension
-------------------------

Nucleome Browser can communicate genome coordinates event within the `UCSC Genome Browser <https://genome.ucsc.edu>`_ and the `WashU Epigenome Browser <https://epigenomegateway.wustl.edu>`_ through the `Nucleome Bridge <https://chrome.google.com/webstore/detail/nucleome-bridge/djcdicpaejhpgncicoglfckiappkoeof?hl=en>`_.

When user open these two web applications in Nucleome Bridge, genome coordinates can be automatically sync between nucleome browser and these two genome browsers. User can navigate in any one browser, and the others will respond accordingly. Meanwhile, user can highlight regions in Nucleome Browser, the UCSC Genome Browser and/or WashU Epigenome Browser will respond as well. This feature makes user can easily compare tracks in two or three different genome browsers.

.. figure:: img/figures_chapter_4/ch4_3_browser.png
    :align: center
    :figwidth: 640px

    Use nucleome bridge web browser extension to sync genomic coordinates between nucleome browser, UCSC genome browser, and WashU genome browser. 

Once the Nucleome Bridge web browser extension is installed, user can open UCSC Genome Browser or WashU Epigenome Browser by clicking the Nucleome Bridge icon.

When user open UCSC Genome Browser or WashU Epigenome Browser from Nucleome Bridge, a piece of JavaScript code will insert into this browser and  pass genome coordinates event message between this browser and Nucleome Bridge. When user navigate genome from this browser, the updated genome coordinates will post to Nucleome Bridge, and Nucleome Bridge will pass this genome coordinates to other connected web applications include Nucleome Browser. On the other hand, when user update genome coordinates or highlight some genome regions, these genome coordinates will post to Nucleome Bridge, and Nucleome Bridge will pass to UCSC/WashU browser and this browser will update its genome coordinates automatically. User can open multiple UCSC/WashU windows.

.. figure:: img/figures_chapter_4/ch4_nucleome_bridge.png
    :align: center
    :figwidth: 400px

    Nucleome Bridge web browser extension

Google Spreadsheet add-on
-------------------------

As a proof-of-concept, by importing nb-dispatch into Google Sheet app script, we provide a Google Sheet addon to make Nucleome Browser work with Google Sheet web application.

When user navigate to some genomic regions they are interested, they would like to mark down this region for future investigation. 
We provide this function in a web application integration approach.

Here is `an example template <https://docs.google.com/spreadsheets/d/1gD6rWNDDkxS-PTb4DmCsV1B57MircZ18RfNxVgjHOhk>`_. 
Users can clone this sheet and use it as a note application or even revise it into a more powerful tool.

