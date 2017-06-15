# gzipServer
Web server that receives gzip'd POST requests and saves them uncompressed locally

To force Firefox to send a Telemetry payload to this server:

1. Start the server: `./gzipServer.py` (you may need to install simplejson Python module)
1. In `about:config`, change the preference "toolkit.telemetry.server" to `"http://localhost"`
1. Restart Firefox to have Telemetry pick up the above pref change
1. Open `about:telemetry` (it has some Telemetry namespaces nicely set up) and open the DevTools console
1. Paste the following into the console:  
   `Cu.import("resource://gre/modules/TelemetrySession.jsm");`  
   `TelemetrySession.testPing();`
1. The script will save the request it receives to *report1.json* in the script's working directory

Note: The procedure above will create a "test ping", which is equivalent to a regular Telemetry "saved-session" ping.

***Alternatives:***

1. If you just need to ***see*** the Telemetry measurements from the current session, you can see them directly on the `about:telemetry` page in Firefox.
1. If you need to see what the full ping looks like, but you don't need to send it to a server, simply exit Firefox and restart. In `about:telemetry` select the last "main" ping from the *archived ping data*. You can switch to the raw ping data to see the full raw contents.
1. You can also get the full ping from the DevTools console by opening the about:telemetry page and running the commands:  
   `Cu.import("resource://gre/modules/TelemetrySession.jsm");`  
  `ping = TelemetrySession.getPayload()`
1. If you need Telemetry from the content process in the ping as well, call `TelemetrySession.requestChildPayloads()` before you call `TelemetrySession.getPayload()`. 
