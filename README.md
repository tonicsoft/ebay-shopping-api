# ebay-shopping-api
[![Build Status](https://travis-ci.org/tonicsoft/ebay-shopping-api.svg?branch=master)](https://travis-ci.org/tonicsoft/ebay-shopping-api)

This project is an JAX-WS generated java client for the eBay Shopping API. Use it to avoid the unpleasantries of configuring code generators and the manual downloading of WSDL files etc. This project will not help you create an instance of the generated service interface. For this you must choose a JAX-WS implementation such as Apache CXF or Axis2 etc.

# download
Find the latest version of the project in the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.tonicsoft.ebay%22%20AND%20a%3A%22ebay-shopping-api%22).

# usage
For detailed usage help, see the documentation for JAX-WS and your chosen implementation. The eBay Shopping API documentation also needs careful reading to ensure the correct HTTP headers are set. The following is an example of how to create a client and make an API call with Apache CXF. First we create an instance of `ShoppingInterface`, then we set up a request context so that standard eBay HTTP headers will be added to each call. Note that you need an eBay APP-ID.

```java
    public SimpleItemType getEbayItem(String itemId) {
        GetSingleItemRequestType request = new GetSingleItemRequestType();
        request.setItemID(itemId);
        return buildShoppingApiService().getSingleItem(request).getItem();
    }

    private ShoppingInterface buildShoppingApiService() {
        JaxWsProxyFactoryBean serviceFactory = new JaxWsProxyFactoryBean();
        serviceFactory.setServiceClass(ShoppingInterface.class);
        serviceFactory.setAddress("http://open.api.sandbox.ebay.com/shopping");
        Object port = serviceFactory.create();

        ClientProxy.getClient(port).getOutInterceptors().add(new AbstractSoapInterceptor(PRE_PROTOCOL) {
            @Override
            public void handleMessage(SoapMessage message) throws Fault {
                Map httpHeaders = (Map) message.get(PROTOCOL_HEADERS);
                MessageInfo soapMessageInfo = (MessageInfo) message.get(MessageInfo.class.getName());
                httpHeaders.put(
                        "X-EBAY-API-CALL-NAME",
                        singletonList(soapMessageInfo.getOperation().getName().getLocalPart())
                );
            }
        });

        Map<String, List<String>> headers = new HashMap<>();
        headers.put("X-EBAY-API-APP-ID", singletonList(...));
        headers.put("X-EBAY-API-REQUEST-ENCODING", singletonList("SOAP"));
        headers.put("X-EBAY-API-VERSION", singletonList("989"));
        ClientProxy.getClient(port).getRequestContext().put(PROTOCOL_HEADERS, headers);

        return (ShoppingInterface) port;
    }
```

To run the above example you will need the core CXF dependencies, org.apache.cxf:cxf-rt-frontend-jaxws:3.1.4 and org.apache.cxf:cxf-rt-transports-http:3.1.4.

# useful links
 - [User guide](https://developer.ebay.com/devzone/shopping/docs/Concepts/ShoppingAPIGuide.html) - Explains how the shopping API works and shows how to make an API call.
 - [Making an API call](https://developer.ebay.com/devzone/shopping/docs/Concepts/ShoppingAPI_FormatOverview.html) - Details on standard parameters required for every API call.
 - [API reference](https://developer.ebay.com/devzone/shopping/docs/CallRef/index.html) - Detailed documentation for each available API call.
 - [Shopping API release notes](https://developer.ebay.com/devzone/shopping/docs/ReleaseNotes.html) - find details about what features of the API will be supported by different versions of this project.
 - [eBay developers program home](https://go.developer.ebay.com/) - Main site for all resources related to the eBay APIs. Here you can sign up to the eBay Developers Program and gain access to the eBay API.
 - WSDL on which this project is based: `http://developer.ebay.com/webservices/VERSION/ShoppingService.wsdl` where `VERSION` is the API version e.g. `989`.
 - [eBay Trading API SDK](https://github.com/tonicsoft/ebaysdkcore) - A full featured SDK for the eBay Trading API.

# versioning
Major versions are maintained in a one to one correspondance with the versions of releases detailed on the eBay Developer Program website. For example, version `989.0.0` of this project targets version `989` of the eBay shopping API. Subsequent minor or patch versions of this projects (`981.0.1`, `981.1.0` etc) target the same version of the eBay Shopping API.
