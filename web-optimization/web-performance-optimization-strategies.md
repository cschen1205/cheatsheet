# Front-End and Middle-End Optimization
* 80+% of the time on modern web pages is due to the number of CSS stle sheets,Javascript files,and images
* Many performance improvements don't require server-side code change

## Minimize Transport Data 

### HTTP Compression
The transport data can be compressed using gzip or other compression method to reduce the traffic

The following parameters in spring boot properties file configure the gzip content encoding:

<pre>
server.compression.enabled = true
server.compression.min-response-size = 1024
server.compression.mime-types = application/json,application/xml,application/xhtml+xml,text/html,text/xml,text/plain,application/javascript,application/font-sfnt,application/font-woff,application/font-woff2
</pre>

The configuration adds the "Accept-Encoding: gzip" to the request header and "Content-Encoding: gzip" to the response header of the resource downloaded

The compression can resuts in ~53% less bandwidth, ~25% faster keynote measurements

Note: do not try to compress images such as png and gif as images already use compression and HTTP compression may actually make the file even larger

### Image Optimization

Another way is to minify the css and js files, such as using gulp / grunt: uglify minification

The images can also be minimized for web, the following link provides a tool to do so:

* http://imageoptimizer.net/
* http://pmt.sourceforge.net/pngcrush/

## Reduce HTTP requests
Reduce the number of requests for a single page when loading the page using bundling

This can be done by reducing the number of CSS, JS or images that a page needs to load:

### Bundle js and css files 
Techniques:
* Combine multiple javascript files into a single javascript file
* Combine multiple CSS files into a single CSS file

### Bundle images

* CSS Sprite: Combine multiple images into a single image and refers to them using css class 

Tools:
* gulp / grunt

### Inline images in html/css file

Technique:
* inline images in the css instead of separate download from server

Tools:
* http://dataurl.net/#cssoptimizer

## Cache as much as possible

### Cache Javascripts 
Reduce the traffic between client and server, this can be done by caching the content instead of reloading every time

Benefits:
* Reduce the load on the server
* Make it faster for the returned visitor in the subsequent visits to the same page (in our case the entire site since it is a SPA)

Tools:
* basket.js

### Content Expiration

Content expiration makes uses of web browser's caching capability, currently the caching can be enabled by adding the security.ignored
in the properties file such as the one below (as by default spring security forces no-store in cache-control header):

<pre>
security.ignored = /jslib/kendo/styles/fonts/**,/bundle/fonts/**,/bundle/lib-bundle.min.css
</pre>

The expiration or max-age properties can be set by adding the following codesnippet to the WebMvcConfig class

<pre>
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
  registry.addResourceHandler("/jslib/kendo/styles/fonts/**")
        .addResourceLocations("classpath:/static/jslib/kendo/styles/fonts/")
        .setCacheControl(CacheControl.maxAge(2, TimeUnit.DAYS));
  ...
}
</pre>

## Resource preloading

Sometimes we can improve performance by preloading resources such as javascripts, images before hitting a particular page

For example, we can preload and cache angularjs files before the login and load these js files immediately from cache after login

This way, there is no waiting time for the js files to be loaded from server after login. 

Same principal applies to other resources such as images.

## Resource lazy loading

Another way to optimize performance by loading and executing javascripts as needed. 

In our case, we won't use this option as it will require more work, below is a sample codesnippet to show how to do it

<pre>
var script = document.createElement('script');
script.src='js/client/client-module.js';
script.async = false;
script.onLoad = function(){
    console.log('loaded and executed');
};
script.onstateready = function(state) {
    
};
</pre>

## Move resources closer to the user

CDN can be used to move resource closer to the user

## Improve the rendering and DOM loading time

This can be done by place CSS styles in the header and placing js files at the bottom of the <body> section of the html page

This allows the dom to be loaded and rendered as soon as they are downloaded without the need to wait for js file download

