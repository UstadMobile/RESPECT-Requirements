# EduCHIP Compatible

EduCHIP (_Communication Harmonization for Interoperable Programs_) compatible is the use of existing edtech APIs and HTTP standards to create apps and content that can work in low resource environments.

Teachers/students/education institutions using EduCHIP compatible apps can:
* Use one account to access any EduCHIP compatible app
* Reduce download bandwidth usage/costs 95%+
* Work offline when connectivity is not available and sync data when a connection is available
* Keep their data on a server in a location of their choice

Edtech developers can quickly/easily adapt their apps to use the standards using the upcoming EduCHIP runtime library, allowing their apps to integrate with a variety of Learning Management Systems.

## Terminology:

* **Provider app**: the primary app used to manage course enrolments, student information, typically a Learning Management System (LMS) e.g. [Moodle](https://moodle.org), [Ustad Mobile](https://www.ustadmobile.com), etc. or Student Information System (SIS) e.g. ClassLink
* **Consumer app**: an education app (such as courseware, eg a math app, other subject app, library app, etc) that will consume information provided from the provider app (as above).

## Scenarios

**A) Single sign on in consumer app (e.g. math app)**: A user (e.g. teacher, student, etc) installs their provider app (e.g. Learning Management System) and a third-party API consumer app (e.g. a math app etc). The student opens the math app and taps "sign in with LMS app" and completes the OAuth flow. The LMS app _may_ support running the OAuth flow offline using app links. The user then accepts or decline permissions requested by the math app. If accepted, the math app can now access the user profile information (e.g. name, grade, etc). 

If the LMS app is installed and supports [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec), this can be done entirely offline, otherwise the math app can store/retrieve profile information offline and sync when a connection becomes available.

**B) User launches consumer app (e.g. math app) from provider app (e.g. LMS)**: The users opens their course in the LMS app which lists lessons they are expected to complete. The LMS app launches the math app (using a URL identifier for a given  content item as per LTI or CMI-5). If the consumer app is installed this will launch the math app using app links. When the user completes the content item they return to the LMS app. 

If the LMS app supports [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec) then data can be sent and received between the apps entirely offline. If not supported, then the math app can store/retrieve profile information offline and sync when a connection becomes available.

# Components

* [HTTP-IPC](https://github.com/UstadMobile/HTTP-IPC-Spec) - Allows a consumer app to communicate with the provider app without requiring Internet access and make API calls using existing standards such as OneRoster, LTI, xAPI, etc.
* [Resiliant Asset Delivery (RAD)](RAD.md) - Allows apps to retrieve assets (e.g files over http) as flexibly as possible; this includes loading via USB stick, retrieving data from nearby devices instead of the Internet where possible to reduce bandwidth usage (e.g. download once instead of 30 times when there are 30 users), and opting in (with user consent) to using a caching http proxy (e.g. if provided on a school network) to load assets that do not contain personal information.

# Common Requirements

* Simple to use (metrics to be determined e.g. number of clicks to accomplish common tasks)
* For all HTTP downloads that do not contain personal/sensitive information:
  * The response body of any given URL must not vary based on request headers (e.g. it must not vary based on client user-agent etc) such that it can be shared with other devices.
  * The ```cache-control: public``` directive must be included in the HTTP response.
  * The server MUST support HTTP partial range requests (all major web servers including Apache, Nginx, etc already support this)
* Installed size MUST be not more than 25% higher Google build for billions guidelines
* 15 minutes of usage MUST use less than 25MB of data on average
  * E.g. A parent with a bundle of half the average data usage in Sub-Saharan Africa allocates 20% of their data to education and has 5 children, each of whom use the app for 15mins/day at home.
  * Data downloaded using with the ```cache-control: public``` where a user can opt-in to using a proxy cache  (e.g. as per [Resiliant Asset Delivery (RAD)](RAD.md)) shall not count towards the total.
* 15 minutes of usage by 5 devices carrying out the same activities nearby with WiFi and Bluetooth switched on must use less than 35MB of data
  * E.g. the app should support peer-to-peer sharing of assets that do not contain personally identifiable information in school scenarios to reduce bandwidth usage. 
* MUST allow users to download key activities (e.g. lessons) for later use offline.


# Provider App Requirements

* MUST support [LTI Assignment and Gradebook Service (AGS)](https://www.imsglobal.org/spec/lti-ags/v2p0), [OneRoster](https://www.1edtech.org/standards/oneroster), and [OAuth](https://oauth.net/2/).
* SHOULD support [Experience API](https://xapi.com)
* MAY support [HTTP-IPC](https://github.com/UstadMobile/HTTP-IPC-Spec).

# Consumer App Requirements

* Single sign-on:
  * MUST support single sign using [OAuth](https://oauth.net/2/) which MUST work offline if the provider app is installed with app links [HTTP/IPC Spec](https://github.com/UstadMobile/HTTP-IPC-Spec?tab=readme-ov-file#offline-oauth-flow).
  * MUST use [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec?tab=readme-ov-file#offline-oauth-flow) to access REST APIs offline where the service is offered by the provider app
  * MUST support using the OneRoster API to retrieve information defined using the OneRoster standard (e.g. grade level, enrolments etc) where this information is required, except if the consumer app is only used via the provider app (e.g. Scenario B only)
  * MAY ask the user additional information that is not part of the OneRoster standard (e.g. "How much do you like math?")  
* Progress reporting
  * MUST store all profile information required for the user to resume subsequent sessions using the OneRoster and/or Experience API (e.g. to allow the user to resume a session on a different device).
  * MUST store learner progress data using the [LTI Assignment and Gradebook Service (AGS)](https://www.imsglobal.org/spec/lti-ags/v2p0).
  * SHOULD store granular learner activity data using [Experience API statements](https://www.xapi.com/) (e.g. when a user answers a question) or [Caliper Analytics](https://www.1edtech.org/standards/caliper).
* Offline support
  * MUST be able to function fully offline where LTI and OneRoster are provided via [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec) and any http assets have been pre-downloaded.
* Other
  * Android apps SHOULD follow Google's [Build for Billions](https://developer.android.com/docs/quality-guidelines/build-for-billions) guidelines including app size and startup time.
  * Android apps SHOULD use [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) to schedule sending any pending data such that data will be transmitted when the user is next reconnected data will be sent automatically without requiring the user to directly open the app itself.
  * Android apps MUST NOT require proprietary services e.g. Google Play Services and must work on Android (AOSP) derivative operating systems (e.g. OpenHarmony OS)






