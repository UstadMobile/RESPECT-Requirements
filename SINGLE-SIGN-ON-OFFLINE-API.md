# Single Sign On
## Android
Roughly:
* LMS app AndroidManifest.xml will accept an intent in the form of local-auths://server-url.com/ e.g. where the online server is server-url.com . This allows a third party consumer app to detect LMS apps by resolving an intent. The third party app MAY use the Android app link verification API to verify the app is authorized for its domain.
* The consumer app will send an intent using the [activity result API](https://developer.android.com/training/basics/intents/result). If the user approves, the result will include a token and a service name for the consumer app to bind to.
* The consumer app binds to the [bound service](https://developer.android.com/develop/background-work/services/bound-services) provided by the LMS app.
* The LMS app service provides a localhost URL for the OneRoster and xAPI APIs that the consumer app can use to retrieve and store user data using the token, the same as they would if using the OneRoster and/or xAPI online over http.

Note: a standard open-source library can implement the above so the developer will need to do only two things: 1) send an intent to request the token and get a localhost URL and 2) send their http requests to the provided URL.

## iOS
To be determined, possibly:

* Provider app login works through using a url scheme where the consumer specifies a return address (e.g. similar to the use of OAuth).
* Consumer app uses [XPC](https://developer.apple.com/documentation/xpc) to send requests

## Windows
To be determined


## Linux
To be determined
