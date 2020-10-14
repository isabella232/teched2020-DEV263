# DEV263 - Secure Cloud Applications by Default – Assisted Migration

## Description

This repository contains the material for the SAP TechEd 2020 session called DEV263 - Session Title. 

## Overview

This session introduces attendees to...

## Requirements
The requirement to follow the migration exercises in this repository are to check whether you make use of one of these security libraries / versions. Only in these cases, an version upgrade / a migration is of relevance.

### Java Development
- SAP internal *container-security api for Java* which was provided with [XS_JAVA 2.0.05](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.05/en-US/6511bc054b0e48369a625a8019fefd53.html)
- *approuter* [npm](https://www.npmjs.com/package/@sap/approuter) < 8.5
- *Java* [*token-client*](https://github.com/SAP/cloud-security-xsuaa-integration/tree/master/token-client) library [maven central](https://search.maven.org/search?q=g:com.sap.cloud.security.xsuaa) < 2.7.3
- [*java-security*](https://github.wdf.sap.corp/CPSecurity/java-container-security) library [maven central](https://search.maven.org/search?q=g:com.sap.cloud.security) < 2.7.5
- *xs-user-holder* [maven central](https://search.maven.org/search?q=g:com.sap.cloud.sjb) < 1.17.5
- *SAP Cloud SDK* [maven central](https://search.maven.org/search?q=g:com.sap.cloud.sdk) < 3.25.0
- *SAP Java Buildpack* < 1.26
- *XSA Java Buildpack* < 1.8.18 was released with XSA PL 129

### Node.JS Development
- *SAP container security api for Node.JS* [npm](https://www.npmjs.com/package/@sap/xssec) < 3.0.6
- *approuter* [npm](https://www.npmjs.com/package/@sap/approuter) < 8.5

### Python Development
- *Python sap_xssec* < 2.1.0
- *approuter* [npm](https://www.npmjs.com/package/@sap/approuter) < 8.5


## Exercises

- [Approuter - Upgrade Version](exercises/approuter)
- [Node.JS - Upgrade Version](exercises/nodejs)
- [SAP Java Buildpack and XSA Java Buildpack - Upgrade Version](exercises/sapjavabuildpack)
    - [Migration Guide to API Version 2](https://github.com/SAP/cloud-security-xsuaa-integration/blob/master/java-security/Migration_SAPJavaBuildpackProjects_V2.md)
- [Java / Spring - Upgrade Version](exercises/java)
    - [Migration Guides](exercises/java/migrationguides)
    - [Migrate Java-container-security to spring-xsuaa](https://github.com/SAP/cloud-security-xsuaa-integration/blob/master/spring-xsuaa/Migration_JavaContainerSecurityProjects.md)
    - [Migrate Java-container-security to java-security with adapter](https://github.com/SAP/cloud-security-xsuaa-integration/blob/master/java-security/Migration_SpringSecurityProjects.md)

## Announcements

### Security Vulnerabilities
Please update to the latest security client library version in order to fix known security vulnerabilities.
Before upgrade, note these general changes and in addition the library specific release notes, especially when upgrading major versions.
 
### SAP_JWT_TRUST_ACL is obsolete
It is no longer possible to use the `SAP_JWT_TRUST_ACL` parameter to specify a dedicated access control list (ACL) for JWT tokens. Changes also apply regarding the granting of security scopes, which are defined and granted in the application security descriptor (xs-security.json). For example, if a business application A wants to call an application B, it is now mandatory that application B grants at least one scope to the calling business application A. Furthermore business application A has to accept these granted scopes or authorities as part of the application security descriptor. For more information, see the release notes on Jam.

**TODO**https://jam4.sapjam.com/blogs/show/oEdyQO183plBoQdrvcPw2w

Communication Between Cloud Foundry Developments [DRAFT]
https://github.wdf.sap.corp/pages/CPSecurity/Knowledge-Base/03_ApplicationSecurity/Syntax%20and%20Semantics%20of%20xs-security.json/ 
https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/517895a9612241259d6941dbf9ad81cb.html 
https://blogs.sap.com/2020/09/03/outdated-sap_jwt_trust_acl/

 
### User Token is replaced with JWT Bearer Token Grant Type
We want to inform you, that there is a replacement of UAA’s proprietary user token flow provided with the [JWT bearer token grant flow](https://docs.cloudfoundry.org/api/uaa/version/74.26.0/index.html#jwt-bearer-token-grant).

With that the “uaa.user” scope and an additional roundtrip is not required anymore. All client libs, that supports token exchange, makes use of the JWT bearer token grant flow. When using one the client libraries listed above to perform the token exchange, update to the latest version and you’re done. 

> Please note that JWT bearer token response provides NO refresh_token. This is no incompatible change, as it was never exposed via the API.
 
### getSubaccountId vs. getIdentityZone vs. getZoneId
For NEW SCP subaccounts it can no longer be guaranteed, that “zone id” ==“subaccount id” == “tenant guid”. That’s why you must make sure, that you use the `getSubaccountId()` method only in case you need it for metering purposes (claim `ext_attr.subaccountid`). And that you use the `getZoneId()`method as tenant discriminator (claim `zid`). Find further details and timelines on zone-enabling SCP subaccounts in CP Security Jam.

### Java-container-security Xsuaa client library is deprecated
As of last week, Fortify reports a deprecation warning in case you still make use of SAP-internal java-container-security library. We recommend that you replace Spring (Boot) based applications with spring-xsuaa. SAP Java Buildpack is the recommendation for J2EE applications and java-security is the library to use for token-validation for native Java applications. You can find more details and the migration guides listed in CP Security Jam.

https://jam4.sapjam.com/blogs/show/rOOEWKahgEU8aarmdVsy60?_lightbox=true
 
### SAP Java Buildpack and XSA Java Buildpack  
As of SAP Java Buildpack version 1.26. and as of XSA Java Buildpack version 1.8.18 ( XSA PL 129), the Java runtime provides the java security library [maven central]. This is a fully compatible change, if you use Java Servlet Security only and the APIs provided by the Buildpack. Optionally you can leverage the latest API as announced with Release Note 2006A.

## How to obtain support
Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2020 SAP SE or an SAP affiliate company. All rights reserved. This file is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.