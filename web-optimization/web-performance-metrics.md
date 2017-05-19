## Measuring Performance

There are 4 categories of metrics measuring web performance:

* Quantitative
* Rule-based
* Milestone
* Perceived

### Metrics: Quantitative

For quantative metrics,tracing tools specifically for HTTP:
* Fiddler or PostMan
* Web Browser Developer Tool (e.g. Chrome DevTool)
* Network Monitor

Functionality of these tools:
* Show complete request and response (not packets)
* Can save archive of session
* Transfer Timeline


### Metrics: Rule-based

Below are some services which provides rule-based metrics measurement on web performance:

* Google PageSpeed Insight (can install PageSpeed Chrome Extension and access it from Chrome DevTool)
* YSlow
* webpagetest.org

Some of the rule-based metrics are listed below:

* Make Fewer HTTP Requests
* Put Javascripts at bottom
* Minify Javascript and CSS
* ...

### Metrics: Milestone

* Time to first byte: time taken from issuing http request to the first byte of response received, usually used to measure server backend performance
* Time to first paint: time taken from issuing http request to the first bit of rendering on the browser
* Time to DomContentLoaded: time taken from issuing http request to the dom content loaded from response (which is the time at which signals that the developer can manipulate the DOM tree, at this point, not all resources are downloaded yet, e.g., CSS and images, etc)
* Time to Load: time taken from issuing http request to all the content loaded in the web browser (at this point the page appears to be completely loaded)

### Metrics: Perceived

Perceived metrics attempt to measure the full user experience (difficult to measure though).
 
One commonly agreed perceived metrics with "Speed Index" which computes the area of rendering html content over time

## Metric Aggregation Methods on Web Performance

Commonly used aggregation method includes:

* mean average
* median value
* {x}%-percentile (e.g. 90%-percentile)
* histogram (bins with fix interval)

