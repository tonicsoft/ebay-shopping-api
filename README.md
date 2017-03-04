# ebay-shopping-api
[![Build Status](https://travis-ci.org/tonicsoft/ebay-shopping-api.svg?branch=master)](https://travis-ci.org/tonicsoft/ebay-shopping-api)
This project is an Apache CXF generated java client for the eBay Shopping API. Use it to avoid the unpleasantries of configuring `wsdl2java` and the manual downloading of WSDL files etc.

# useful links
 - [User guide](https://developer.ebay.com/devzone/shopping/docs/Concepts/ShoppingAPIGuide.html) - Explains how the shopping API works and shows how to make an API call.
 - [Making an API call](https://developer.ebay.com/devzone/shopping/docs/Concepts/ShoppingAPI_FormatOverview.html) - Details on standard parameters required for every API call.
 - [API reference](https://developer.ebay.com/devzone/shopping/docs/CallRef/index.html) - Detailed documentation for each available API call.
 - [Shopping API release notes](https://developer.ebay.com/devzone/shopping/docs/ReleaseNotes.html) - find details about what features of the API will be supported by different versions of this project.
 - [eBay developers program home](https://go.developer.ebay.com/) - Main site for all resources related to the eBay APIs. Here you can sign up to the eBay Developers Program and gain access to the eBay API.
 - WSDL on which this project is based: `http://developer.ebay.com/webservices/VERSION/ShoppingService.wsdl` where `VERSION` is the API version e.g. `989`.

# versioning
Major versions are maintained in a one to one correspondance with the versions of releases detailed on the eBay Developer Program website. For example, version `989.0.0` of this project targets version `989` of the eBay shopping API. Subsequent minor or patch versions of this projects (`981.0.1`, `981.1.0` etc) target the same version of the eBay Shopping API.
