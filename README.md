# optimization-testing
Use Node, npm, PhantomJS and Phantomas to measure the number of requests and size of files needed to render a web page. This simple test is one way to help optimize a web page for download speed. 

You'll need to install node and npm to use this tool. For instructions on installing Node and npm:

* [Mac Installation Instructions](http://treehouse.github.io/installation-guides/mac/node-mac.html)
* [Windows Installation Instructions](http://treehouse.github.io/installation-guides/windows/node-windows.html)

1. Clone or download this repo
2. In the folder you've downloaded, you'll find a `public` folder. In that folder, replace the `index.html` with your own `index.html` file. In addition, put any other asset folders/files such as scripts, images, fonts, and CSS reference by the index.html file. This test only works on a single page, the `index.html` file and won't scan an entire set of web pages.
3. Using a console or terminal program naviagate to the main folder for this repo on your computer: `optimization-testing`.
4. Type `npm install` and wait for the npm packages to install.
5. Once the packages have installed type `npm start`. This starts a web server, so that another service `phantomas` can test your page. Visit the `index.html` page in a browser at http://localhost:8080/ to make sure that your page is loading correctly.
6. Open another console or terminal session. Navigate to the `optimization-testing` folder and then type `npm test`. After a few moments you'll see a report with a bunch of data like this:

```
.-----------------------------------------------------------------------------------------------------------.
| Report from 5 run(s) for <http://localhost:8080/> using phantomas v1.15.1                                 |
|-----------------------------------------------------------------------------------------------------------|
|             Metric             |     min      |     max      |   average    |    median    |    stddev    |
|--------------------------------|--------------|--------------|--------------|--------------|--------------|
| requests                       |           44 |           44 |           44 |           44 |            0 |
| gzipRequests                   |            0 |            0 |            0 |            0 |            0 |
| postRequests                   |            0 |            0 |            0 |            0 |            0 |
| httpsRequests                  |            0 |            0 |            0 |            0 |            0 |
| notFound                       |            0 |            0 |            0 |            0 |            0 |
| bodySize [bytes]               |     11762342 |     12963732 |   12296442.4 |     12109244 |    448435.06 |
| contentLength [bytes]          |     12963732 |     12963732 |     12963732 |     12963732 |            0 |
| httpTrafficCompleted [ms]      |          198 |          293 |        244.2 |          244 |        32.62 |
| timeToFirstByte [ms]           |            7 |           34 |         13.6 |            7 |        10.46 |
| timeToLastByte [ms]            |           19 |           45 |         26.6 |           21 |         9.91 |
| requestsWithTimeout            |            0 |            0 |            0 |            0 |            0 |
'-----------------------------------------------------------------------------------------------------------'
```
Some important bits to look at: 

* The `requests` row indicates the number of requests for resources required to load the page. In other words how many files -- images, css, JavaScript, etc. -- the browser tried to download.
* The `notFound` row indicates files that weren't found and would indicate missing images or files requested by the `index.html` file.
* The `bodySize [bytes]` row indicates the amount of content that was "sent on the wire" -- that is, size of files that were sent to the browser. This is a better indicator of download efficiency than `contentLength`. `contentLength` is the size of the files after begin decoded. If you've sent gzipped files, then the `bodySize` will be smaller and more accurate metric for determining the size of data sent from the server. Notice that the `bodySize` number varies -- this is a limitation of PhantomJS, but this test loads the page and it's assets 5 times, so the `average` column will give you a good indication of total amount of data transferred from the server to the browser. In the example above, the total payload is 12963732 bytes, or 12.96MB -- that's a lot of data to download!
