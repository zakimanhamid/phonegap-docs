---
title: Weinre
url: phonegap-build/debugging/weinre
layout: subpage
---

The open-source Weinre project is available for remotely debugging Javascript applications. A Weinre instance is hosted on PhoneGap Build, or you can run your own instance locally:

- [Using PhoneGap Build's Debug Server](#phonegap-build-debug-server)
- [Running your own local Debug Server](#running-a-local-debug-server)

## PhoneGap Build Debug Server

This section shows how to use standard Web Inspector tools available from the <a href="https://build.phonegap.com" target="_blank">PhoneGap Build site</a> to debug PhoneGap apps while they are running on your device.  See the PhoneGap Build section for details on how to provide the site with project code, and how to get the resulting compiled apps onto your device.

To enable debugging from within the list of existing __Apps__, click on the name of the application to view its details. Click on the __Settings__ tab, then select the __Enable debugging__ check box:

![](/images/phonegap-build/pgbuild_dbg_select.png)

Once the debugging option is selected and you build the project, the __Debug__ button appears along with other options to update code and rebuild the project. When you preview the app on the device, perhaps by scanning the QR code as described in the PhoneGap Build section, pressing __Debug__ opens a web page that allows you to communicate with the app as it runs on the device:

![](/images/phonegap-build/pgbuild_dbg_remote.png)

Pressing the listed target gives you access to the app's internal state.  The debugger may occasionally lose its connection if you pause the app or if the device goes into a standby mode. In that case, the __Remote__ tab may present a new target with which to re-establish the connection.

Once connected, the __Elements__ tab allows you to view the app's full DOM tree as it executes:

![](/images/phonegap-build/pgbuild_dbg_elements.png)

You can use this interface to modify the application as it runs. For example, the default PhoneGap app generated by the CLI (see The Command-line Interface) presents a slowly blinking status message beneath the icon:

![](/images/phonegap-build/pgbuild_dbg_blink.png)

Selecting various elements from the DOM tree in the debug view highlights them on the device if visible, which may help you locate the relevant code:

```html
<div id="deviceready" class="blink">
```

The sidebar on the right allows you to view information about CSS properties applied to each element and the selectors that specify them.  If you double-click on the `class` attribute and delete the `blink` text, the message stops blinking on the device, and the `.blink` selector disappears from the sidebar's __Matched CSS Rules__:

```css
.blink {
  -webkit-animation: fade;
  -webkit-animation-delay: initial;
  -webkit-animation-direction: initial;
  -webkit-animation-duration: 3000ms;
  -webkit-animation-fill-mode: initial;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-name: fade;
  -webkit-animation-timing-function: initial;
}
```

To see the same change without editing the DOM, select the __Console__ tab and type a JavaScript call directly onto the command line. This example selects the element whose `id` is `deviceready` and turns on or off its `blink` class:

```js
document.querySelector('#deviceready').classList.toggle('blink')
```

The method's return value displays within the console. Typing the name of _any_ object allows you to view its evaluated contents:

![](/images/phonegap-build/pgbuild_dbg_toggle.png)

The browser engine's error messages and results of `console.log()` calls also appear on the console.

Navigating back to the __Elements__ view displays the results of the change, but note there may be a delay, and you may need to re-select the DOM node to view its current state.

The Web Inspector offers several other useful features:

- The __Network__ tab lists details on network requests the app makes while executing, along with a timeline interface to visualize a progression of network interactions.

- The __Timeline__ tab similarly visualizes load times for the app's component resources.

- The __Resources__ tab allows you to inspect cookies and the contents of various client-side databases (`localStorage`, `sessionStorage`, Web SQL) set by the Storage API.  Note that this view also allows you to inspect the HTML5 Application Cache, which allows you install an app persistently from a set of network resources. However, this feature is less appropriate for PhoneGap apps, which are already delivered to the device as part of an installed native package.

Despite slight differences in how Web Inspector features appear in different WebKit-based browser environments, the documentation for <a href="https://developers.google.com/chrome-developer-tools/" target="_blank">Google Chrome</a>
and <a href="https://developer.apple.com/library/safari/documentation/AppleApplications/Conceptual/Safari_Developer_Guide/Introduction/Introduction.html" target="_blank">Apple Safari</a> provides a good overview.

## Running a Local Debug Server

PhoneGap Build allows users to use their own debug server with the Build service.

Build uses a tool called Weinre to enable remote debugging of mobile apps.

This guide provides information on setting up your own local server.

The pre-requisites for running Weinre are that you need to have `npm` installed. Once you've installed Weinre you will only be able to use the local server within your own network unless you plan to host it on a publicly accessible location. This will require additional setup that is outside the scope of this guide.

### Set Up Weinre

Once you have `npm` installed, obtaining and installing Weinre is as simple as running the following command in a terminal.

```sh
sudo npm -g install weinre
```

That's it! Now you're ready to run your very own Weinre instance.

### Start Weinre

To start your new local Weinre instance run the following command:

```sh
weinre
```

You will now see output like the following:

```sh
Hardeeps-MacBook-Air:~ hardeep$ weinre
2013-07-01T20:03:34.890Z weinre: starting server at `http://localhost:8080`
```

Weinre is now up and running! If you are running this behind a router that uses NAT you will need to find your IP address. You will use this IP when specifying your configuration with Build.

### Using a Local Weinre Instance with Build

Obtain the ip address of your machine running Weinre. This can be done on Windows by running `ipconfig` or on OSX/Linux by running `ifconfig`.

Now include a reference to the Weinre debug script on your debug server, like so:

```html
<script type="text/javascript" src=http://my.server.ip/target/target-script-min.js#some_unique_key"></script>
```

<div class="alert-info">Make sure your remove this before publishing your app!</div>

### Common Issues

#### *I can't connect to my Local Server*

First of all make sure that your server is running. Chances are if you're using the default configuration you can visit `http://localhost:8080` and it should be responding.

If this works it's most likely the IP address you're providing to Build; please verify that it is correct. A google search such as `windows [version] find ip address` or `OSX [version] find ip address` will help you find articles on getting the right ip.

Assuming that you're using a router running NAT verify that you can visit it within your network by visiting `http://[ip address]:8080`.
