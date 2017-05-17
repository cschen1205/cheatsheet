Real user monitoring (RUM) monitors the timing of various operation from web browser

## Gather data from Web Browser
There are 3 categories of timing in RUM:

* Navigation timing: timing on loading and rendering web pages
* Resource timing: timing on loading and rendering web resouces such as images and fonts
* User timing: allow user to mark the navigation timeline with a custom marker and get user-specified timing

### Gather Navigation Timing in Chrome

Open the Developer tools in Chrome (referred to as Chrome DevTool below). In the Chrome DevTool, select the "Console" tab

In the DevTool Console, enter the following command in the search (i.e., the line starting with '>' symbol):

<pre>
performance
</pre>

This will list the statistics about the performance of the web page loaded.

To gather the timing, enter the following command in the search of DevTool Console:

<pre>
performance.timing
</pre>

The returned jsons contains properties with timestamps (in milliseconds)

To gather information about specific interval, said connection, enter the following command:

<pre>
performance.timing.connectEnd - performance.timing.connectStart
</pre>

### Gather Resource Timing in Chrome

In the Chrome DevTool Console, enter the following command in the search:

<pre>
performance.getEntriesByType('resource')
</pre>

The returned json has the following properties of interest:

* name: name of the resource (e.g., image name)
* initiatorType: why the resource is downloaded

The returned json also includes other properties with timing from start to end and gives sub milliseconds values

To format those json into a table, enter the following command in the Chrome DevTool Console:

<pre>
table(performance.getEntriesByType('resource'))
</pre>

### Gather User Timing in Chrome

Firstly in the angularjs javascript codes include the following api calls:

<pre>
performance.mark('[event_name]');
</pre>

This will market the timestamp of the event_name

To compute and mark the timespan between two events event1 and event2, use the following javascript codesnippet:

<pre>
performance.measure('[metric_name]', '[event1_name]', '[event2_name]');
</pre>

To access these user timing, enter the following command in the Chrome DevTool Console:

<pre>
performance.getEntriesByType('mark')
</pre>

and measurements:

<pre>
performance.getEntriesByType('measure')
</pre>

or get the tabular view:

<pre>
table(performance.getEntriesByType('mark'))
</pre>

### Gathering Milestone Timing

The milestone timing such as "Time to First Paint" (described in the performance-metrics.md) can be obtained by
entering the following command in the Chrome DevTool Console:

<pre>
chrome.loadTimes()
</pre>

For example, to get the "Time to First Paint":

<pre>
chrome.loadTimes().firstPaintTime
</pre>

### Gathering Frame Timing

To drill download to more details when analyzing js performance (e.g., painting and compositing), the following commands can be used:

<pre>
performance.getEntriesByType('renderer')
performance.getEntriesByType('composite')
</pre>

## Send data back to server using Beacon

The gathered data from the web browser can be sent back to the server using Beacon:

The following javascript codesnippet shows how:

<pre>
window.addEventListener('unload', function() {
  var rum = {
    navigation: performance.timing,
    resources: performance.getEntriesByType('resource'),
    marks: performance.getEntriesByType('mark'),
    measures: performance.getEntriesByType('measure')
  };
  navigator.sendBeacon('/rum/submit', JSON.stringify(rum));
});
</pre>

Beacon may not be available in browser such as IE or safari, but it can be replaced with json calls or sockjs calls


