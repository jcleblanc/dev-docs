# API Basics and Overview

## Overview

The Box API gives you access to a set of secure content management features for use in your own app, such as file storage, preview, search, commenting, and metadata. It strives to be [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer) and is organized around the main resources from the Box web interface.

Before you do anything, you should [create a free Box account](https://account.box.com/signup/n/developer) that you can test the API against and [register for an API key](https://app.box.com/developers/services/edit/) so that you can make API calls.

If you are building custom applications for users with a Box account, you can follow the [authentication instructions](/docs/setting-up-an-oauth-app) using OAuth 2. 

If you are building custom applications and wish to leverage the Box API without requiring a Box account, you will need to use Box Platform. You can find a tutorial for building with Box Platform [here]((/docs/setting-up-a-jwt-app). 

#### Example Requests 

Sample API calls are provided next to each method using [cURL](http://curl.haxx.se/), a standard command line tool. All you need to do is drop in your specific parameters, and you can test the calls from the command line. If the command line isn’t your preference, a great alternative is [Postman](https://www.box.com/blog/using-postman-to-get-started-with-the-content-api-and-view-api-2/), an easy-to-use [Chrome extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en) for making HTTP requests. Or you can always use one of our [SDKs](/docs/sdks) instead of making direct API calls.

#### Input/Output Format

Both request body data and response data are formatted as [JSON](http://www.json.org/). Extremely large numbers (for example the folder size) are returned in [IEEE754 format](http://en.wikipedia.org/wiki/Double-precision_floating-point_format) (the same way doubles are stored in Java), e.g. `1.2318237429383e+31`.

#### Date Format

All timestamps (both those sent in requests and those returned in responses) should be formatted as shown in our examples. We support [RFC 3339](https://www.ietf.org/rfc/rfc3339.txt) timestamps. The preferred way to pass in a date is by converting the time to UTC such as this: 2013-04-17T09:12:36-00:00. 

In cases where timestamps are rounded to a given day, you may omit the time component. So instead of 2013-04-17T13:35:01+05:00 you can use 2013-04-17. If a time (and not just a date) however, is specified in a request then the calendar date in the Pacific timezone at the moment specified is the day that is accepted.

Box supports the subset of dates after the start of the [Unix epoch](http://en.wikipedia.org/wiki/Unix_time#Encoding_time_as_a_number): 1970-01-01T00:00:00+00:00  (00:00:00 UTC on January 1, 1970).

As a note, the timestamp you receive from the Box API is based on the settings in the Admin console. If you are a part of an enterprise, it will be the default user settings set by your admin.

#### gzip

If you would like responses from Box to be compressed for faster response times, simply include an `Accept-Encoding` header with a value of `gzip, deflate`, and responses will be [gzipped](http://en.wikipedia.org/wiki/Gzip).

#### Getting Help

If you have any questions or feedback, please post on the [Box developer forum](https://community.box.com/t5/Developer-Forum/bd-p/DeveloperForum).

### Suppressing Notifications

If you are making administrative API calls (that is, your application has “Manage an Enterprise” scope, and the user signing in is a co-admin with the correct "Edit settings for your company" permission) then you can suppress both email and [webhook](/docs/work-with-webhooks) notifications by including a `Box-Notifications` header with the value `off`. This can be used, for example, for a virus-scanning tool to download copies of everyone’s files in an enterprise, without every collaborator on the file getting an email saying “The Acme-Virus-scanner just downloaded your ["Acme exploding tennis balls instructions"](http://en.wikipedia.org/wiki/Acme_Corporation). All actions will still appear in users updates feed and the audit-logs.

Notification suppression is available to be turned on for approved applications only. [Contact us](http://community.box.com/t5/custom/page/page-id/BoxSearchLithiumTKB) to explain your application’s use of notification suppression. 

<aside class="warning">
[User creation](#create-an-enterprise-user), [comment](#comment-object), [collaboration creation](#add-a-collaboration), [change user's login](#changing-a-users-primary-login), and [task assignment](#create-a-task-assignment) notifications cannot be suppressed using this header.
</aside>

### CORS

[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing), or cross-origin resource sharing, is a mechanism that allows a web page to make XMLHttpRequests to another domain (i.e. a domain different from the one it was loaded from). CORS is supported in a [specific set of modern browsers](http://caniuse.com/cors). The Box API supports CORS on an app-by-app basis. You can add a comma separated list of origins that can make CORS requests to the API, through the developer console.

A quick tutorial can be found [here](https://blog.box.com/blog/uploading-files-with-cors/).

<aside class="notice">
Some browsers will return a CORS-like error, even when CORS is enabled for your application. In these scenarios, an HTTP response code will often be included (e.g. 400 or 401) which will provide further direction where you may want to focus troubleshooting. No further CORS enablement is done in the Box backend.
</aside>


