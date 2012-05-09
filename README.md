# express-device

## why express-device?

I'm really into node.js and lately I've been palying a lot with it. One of the steps I wanted to take in my learning path was to build a node.js module and published it to npm. 

Then I had an idea: why not develop a server side library to mimic the behaviour that Twitter's [Bootstrap](http://twitter.github.com/bootstrap/scaffolding.html#responsive) has in order to identify where a browser is running on a desktop, tablet or phone. This is great for a [responsive design](http://en.wikipedia.org/wiki/Responsive_Web_Design), but on the server side.

## how it came to be?

First I started to search how I could parse the user-agent string and how to differentiate tablets from smartphones. I found a couple good references, such as:
- http://www.useragentstring.com/pages/useragentstring.php
- http://detectmobilebrowsers.com/
- http://www.quirksmode.org/js/detect.html
- http://googlewebmastercentral.blogspot.pt/2011/03/mo-better-to-also-detect-mobile-user.html
- http://www.htmlgoodies.com/beyond/webmaster/toolbox/article.php/3888106/How-Can-I-Detect-the-iPhone--iPads-User-Agent.htm
- http://windowsteamblog.com/windows_phone/b/wpdev/archive/2011/08/29/introducing-the-ie9-on-windows-phone-mango-user-agent-string.aspx

But then I came across with Brett Jankord's [blog](http://www.brettjankord.com). He developed [Categorizr](http://www.brettjankord.com/2012/01/16/categorizr-a-modern-device-detection-script/) which is what I was trying to do for node.js but for PHP. He has the code hosted at [github](https://github.com/bjankord/Categorizr). So, express-device parsing methods (to extract device type) are based on Categorizr. Also I've used all of his [compilation of user-agent strings](http://brettjankord.com/categorizr/categorizr-results.php) to build my unit tests.

## how to use it?

To install it you only need to do
    $ npm install express-device

express-device is built on top of connect\express frameworks. Here's an example on how to configure express to use it:

```javascript
app.configure(function(){
    app.set('view engine', 'ejs');
    app.set('view options', { layout: false });
    app.set('views', __dirname + '/views');
    
    app.use(express.bodyParser());
    app.use(device.capture());
});
```
By doing this you're enabling the **request** object to have a property called **device**, which have the following properties:
<table>
    <tr><td><strong>Name</strong></td><td><strong>Field Type</strong></td><td><strong>Description</strong></td><td><strong>Possible Values</strong></td></tr>
    <tr>
        <td>type</td>
        <td>string</td>
        <td>It gets the device type for the current request</td>
        <td>desktop, tv, tablet ot phone</td>
    </tr>
</table>

express-device also comes with some [dynamic helpers](http://expressjs.com/guide.html#app.dynamichelpers\(\)) that will help you to build a responsive design:
<table>
    <tr>
        <td>is_desktop</td>
        <td>It returns true in case the device type is "desktop"; false otherwise</td>
    </tr>
    <tr>
        <td>is_phone</td>
        <td>It returns true in case the device type is "phone"; false otherwise</td>
    </tr>
    <tr>
        <td>is_tablet</td>
        <td>It returns true in case the device type is "tablet"; false otherwise</td>
    </tr>
    <tr>
        <td>is_mobile</td>
        <td>It returns true in case the device type is "phone" or "tablet"; false otherwise</td>
    </tr>
    <tr>
        <td>is_tv</td>
        <td>It returns true in case the device type is "tv"; false otherwise</td>
    </tr>
    <tr>
        <td>device_type</td>
        <td>It returns the device type string parsed from the request</td>
    </tr>
</table>
In order to enable this methods you have to call **app.enableDeviceHelpers()**.

Here's an example on how to use them (using [EJS](https://github.com/visionmedia/ejs) view engine):
```html
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1>Hello World!</h1>
    <% if (is_desktop) { %>
    <p>You're using a desktop</p>
    <% } %>
    <% if (is_phone) { %>
    <p>You're using a phone</p>
    <% } %>
    <% if (is_tablet) { %>
    <p>You're using a tablet</p>
    <% } %>
</body>
</html>
```

You can check a full working example [here](https://github.com/rguerreiro/express-device/tree/master/example).

## where to go from here?

Currently express-device is on version 0.1.0. There are a couple of things that I have in mind to add, such as:
- different view rendering based on device type
- parsing the OS

Feel free to add issues with your own ideas or make pull requests (prefered method).