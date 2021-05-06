===============
NB-dispatch API
===============

Overview
========

The Nucleome Browser uses the nb-dispatch library as a hierarchical event dispatcher to enable communication cross-panel or cross-domain.
The nb-dispatch is inherited from d3-dispatch, meaning that nb-dispatch has the same initialize method and the same function interface on and call as d3-dispatch.

In Nucleome Browser, nb-dispatch will use the Nucleome Bridge to dispatch events between tabs from different domains if chrome browser extension Nucleome Bridge is installed, otherwise, it will use the web browser's Broadcast Channel API to dispatch events between tabs within the same domain.

Users can use `nb-dispatch <https://github.com/nucleome/nb-dispatch>`_ to connect their customized webs to Nucleome Browser and other supported data portals (e.g., the UCSC Genome Browser and the WashU EpiGenome Browser). 
It is worth noting that the Nucleome Bridge browser extension is required to communicate with the UCSC Genome Browser and the WashU Epigenome Browser. 
Once a website is connected, navigation or highlight operations on the Nucleome Browser can dispatch to customized websites or \textit{vice versa}.
Other websites that are allowed to connect via Nucleome Bridge are listed in the manifest.json file (white list of websites) of the Nucleome Bridge. 
Currently, a whitelist of websites hosted in these two domains is automatically supported by Nucleome Bridge is shown below.

- `*://*.openmicroscopy.org/*`,
- `*://epigenomegateway.wustl.edu/*`,
- `*://*.4dnucleome.org/*`,
- `*://127.0.0.1:*/*`,
- `*://localhost:*/*`,
- `https://docs.google.com/*`,
- `https://*.googleusercontent.com/*`,
- `*://bl.ocks.org/*`,
- `*://*.slack.com/*`,
- `*://codepen.io/*`,
- `*://cdpn.io/*`,
- `*://jsfiddle.net/*`,
- `*://fiddle.jshell.net/*`,
- `*://*.allencell.org/*`,
- `*://allencell.org/*`

    Users can host their website on GitHub Gist and use `https://bl.ocks.org <https://bl.ocks.org>`_ to visualize it. Read more from `https://bl.ocks.org/-/about <https://bl.ocks.org/-/about>`_. A few demos of usage of the nb-dispatch is shown at `https://bl.ocks.org/nb1page <https://bl.ocks.org/nb1page>`_.

Please contact us if you want to add your website to the allow-list of websites in Nucleome Bridge.

Installation
============

If you use NPM, install nb-dispatch via ``npm install@nucleome/nb-dispatch``.
You can also load it on your webpage. 

.. code-block:: html

    <script src="https://unpkg.com/@nucleome/nb-dispatch"></script>
    <script>
    var c = nb.dispatch("update","brush");
    c.connect(function(status){
        //add your code callback.
    });
    </script>

A minimal example
=================

Below is a minimal example showing the usage of the nb-dispatch library. 
The live version can be accessed from this `minimal example link <https://bl.ocks.org/zocean/017a33abb667cc35247fbc7cc8b0704c>`_.
Basically, in this example, we show how to control the Nucleome Browser to go to a genomic region or highlight multiple regions (Part I). 
We also show how to monitor operations on Nucleome Browser via nb-dispatch.
Note that this example requires the Nucleome Bridge browser extension.

.. code-block:: html

    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width" />
        <title>An exmaple of nb-dispatch</title>
        <!-- Load nb-dispatch library -->
        <script src="https://unpkg.com/@nucleome/nb-dispatch"></script>
    </head>
    <body>
        <p>This is a minimal example showing how to create a customized website to communicate with Nucleome Browser using <a href="https://github.com/nucleome/nb-dispatch">nb-dispatch</a></p>
        <p>Open <a href="https://vis.nucleome.org" target="_blank">Nucleome Browser</a> </p>
        <h3>Part I: Control Nucleome Browser</h3>
        <p>Click the following buttons to goto centain genomic region or highligh multiple regions</p>
        <div id="connection"></div>
        <!-- Create two buttons. -->
        <!-- Click the 'Navigate' button to navigate to target region -->
        <button onclick="navigate()" >Navigate to chr1:0-10M </button>
        <!-- Click the 'Highlight' button to highligh regions on the genome with color -->
        <button onclick="highlight()"> Highlight multi-regions </button>
        <h3>Part II: Monitor the operations of Nucleome Browser</h3>
        <p>Navigate or highlight regions on Nucleome Browser and show the current genomic region or highlighted regions below</p>
        <div> 
            Current genomic region
            <div id="current" style="min-height:50px"></div>
        </div>
        <div>
            Current highlighted region
            <div id="current_highlight" style="min-height:50px"></div>
        </div>
        <script>
            var chan = nb.dispatch("navigate","highlight")
            chan.connect(function(d){
                document.getElementById("connection").innerHTML = d.connection
            })
            var navigate = function(){
                chan.call("update",this,[{chr:"chr1",start:0,end:10000000}])
            }
            var highlight = function() {
            chan.call("brush",this,[{
                chr:"chr1",start:0,end:1000000,color:"red"
            },{
                chr:"chr1",start:2000000,end:3000000,color:"orange"
            },{
                chr:"chr1",start:4000000,end:5000000, color:"yellow"
            },{
                chr:"chr1",start:6000000, end:7000000, color:"green"
            },{
                chr:"chr1",start:8000000, end:9000000, color:"blue"
            }])
            }
            var regionText = function (d) {
                return d.chr + ":" + (d.start+1) + "-" + d.end;
            };
            var regionsText = function (regions) {
                var r = [];
                regions.forEach(function (d) {
                    r.push(regionText(d));
                });
                return r.join(",");
            };
            var a = nb.dispatch("update","brush")
            a.connect(function(d){
                document.getElementById("connection").innerHTML = d.connection
            })
            a.on("update",function(d){
                document.getElementById("current").innerHTML=regionsText(d)
            })
            a.on("brush",function(d){
                document.getElementById("current_highlight").innerHTML=regionsText(d)
            })
    </script>
    </body>
    </html>

API reference
=============

The nb-dispatch is built based on d3-dispatch. 
It has the same initialize method and the same function interface on and call as d3-dispatch.
Below are specific features provided by nb-dispatch that are different from d3-dispatch. 

- **nb.dispatch** *(types)*:
    
    Create new nb-dispatch event object for specific event types.
    Here, each type should be a string such as ``update`` and ``brush``.
    Event type ``update`` indicates navigating to the current genomic region.
    Event type ``brush`` indicates highlight certain genomic regions. Example of usage:

    .. code-block:: javascript

        var c = nb.dispatch("update","brush")
        c.connect(function(){
            console.log(c.status())
            c.disconnect()
        }

- **nb.dispatch.call** *(type[, that[, arguments...]])*:
    
    Invoke each callback for a specific type (update or brush).
    Arguments are a list of genomic regions each of which has a format of {genome:string, chr:string, start:int, end:int, color:color}.
    Note that the position of start and end is 0-base.
    Color is only effective for the brush type.

    .. code-block:: javascript

        var navigate = function(){
            chan.call("update",this,[{chr:"chr1",start:0,end:10000000}])
        }
        var highlight = function() {
            chan.call("brush",this,[{
                chr:"chr1",start:0,end:1000000,color:"red"
            },{
                chr:"chr1",start:2000000,end:3000000,color:"orange"
            }])
        }

- **nb.dispatch.connect** *(callback)*:
    
    Connect to event-dispatch hub via the HMTL BroadCast Channel or Nucleome Bridge extension

- **nb.dispatch.disconnect** *()*:
    
    Disconnect from the HMTL BroadCast Channel or Nucleome Bridge extension

- **nb.dispatch.status** *()*:
    
    Check out the status of current connected channel.
    The output is one of ``Extension``, ``Channel``, or ``None``

- **nb.dispatch.chanId** *(channelName)*:
    
    Set the channel ID before connect to it.
    If there are no arguments, it will return the current channel ID.
    The default channel ID is ``cnbChan0```
