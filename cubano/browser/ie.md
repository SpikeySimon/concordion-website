---
title: "Internet Explorer | Cubano"
description: "Describes the options for configuring the Internet Explorer browser with Cubano"
sitemap:
    priority: 0.6
---

# Internet Explorer

Options for the various [InternetExplorerDriver](https://github.com/SeleniumHQ/selenium/wiki/InternetExplorerDriver) settings are at TODO ?where?

There is some configuration that must be done to Internet Explorer before it can be used by Selenium WebDriver, these steps are described [here](https://github.com/SeleniumHQ/selenium/wiki/InternetExplorerDriver#required-configuration).


## Configuration

##### ie.capability.&lt;any.valid.capability&gt;

Sets capabilities.<br/>
InternetExplorerDriver properties describe many of the capabilities that can be configured.


## Troubleshooting
sendkeys can be very slow (1-2 seconds per character): Mostly likely that is because the IE 32 bit version is being loaded and using 64 bit driver. Use wdm.architecture=32.  ie.capability.requireWindowFocus = true can also fix the problem.
