# RESPECT Requirements

RESPECT (_Resilient Edtech Standards Project for Educational Courseware and Technologies_) is a set of technical requirements to ensure edtech apps perform well in scenarios common in low and middle income countries (LMIC) such as:

* __When Internet access is slow or unreliable:__ teachers and students need to be able use edtech apps without interruption, even when connectivity is slow, unreliable, or intermittent.
* __When Internet access is relatively expensive:__ teachers and parents want apps to reduce download data usage to avoid having to spend more on mobile credit.
* __When teachers and students want to use multiple apps__: they don't want to have to re-enter profile and class details. Different apps can handle different subjects (e.g. math, science, language) or use cases (eg homework submission). Users want be able to use a single account, and each app should automatically receive the required profile information (name, role, class enrollments, etc) when the user grants permission.
* __When phones have limited capacity__: teachers and students need apps that are small enough, light enough, and fast enough to fit on their phone and load quickly
* __When students and teachers want to view progress__: they should be able to see progress information from multiple apps in one place. It should be possible an education organisation to decide where personal data is stored.

Teachers/students/education institutions using apps that meet the RESPECT requirements can:
* Use one account to access any RESPECT compliant app
* Reduce download bandwidth usage/costs 95%+
* Work offline when connectivity is not available and sync data when a connection is available
* Keep their data on a server in a location of their choice
* Install the app on almost any phone/device they are likely to have because the app will be sufficiently small, fast, and light
* Expect apps to work together; for example if they complete a lesson in a math app their grade should appear in their learning management system. 

The upcoming [libRESPECT](https://github.com/UstadMobile/librespect) library will provide edtech developers with an easy-to-use open-source component they can easily embed in their own apps to meet the RESPECT requirements. libRESPECT can help manage offline content download, caching, and synchronization of learner progress data.

# Technical Requirements

The technical requirements are a set of objective criteria that can be applied to an edtech app to determine if it meets the RESPECT requirements.

## Terminology:

* **Provider app**: the primary app used to manage enrolments and student information, typically a Student Information System (SIS) e.g. ClassLink, OpenSIS, etc.
* **Consumer app**: an education app (such as courseware, eg a math app, other subject app, utility apps such as messengers, etc) that will consume information provided from the provider app (as above).

## Scenarios

**A) Single sign on in consumer app (e.g. math app)**: A user (e.g. teacher, student, etc) installs their **provider app** (e.g. student information system) and a third-party API **consumer app** (e.g. a math app etc). The student opens the math app and taps "sign in with _**provider app** name_" and completes the OAuth flow. The **provider app** _may_ support running the OAuth flow offline using app links. The user then accepts or decline permissions requested by the math app. If accepted, the math app can now access the user profile information (e.g. name, grade, etc). 

If the **provider app** is installed on the same device and supports [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec), this can be done entirely offline, otherwise the math app can store/retrieve profile information offline and sync when a connection becomes available.

**B) User launches consumer app from provider app**: The user may be presented with a list of available **consumer apps** by the **provider app**. When the user launches the **consumer app** from the **provider app** they will be automatically signed in. The **provider app** may optionally provide a learning unit ID (URL) to instruct the **consumer app** to launch a particular learning unit.

In both cases this is done as an LTI launch. The operating system's (e.g. Android, iOS, Windows, Linux, etc) mapping of URLs to installed apps (e.g. [app links on Android](https://developer.android.com/training/app-links/verify-android-applinks)) is used to determine which app to launch.

If the **provider app** supports [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec) then data can be sent and received between the apps entirely offline. If not supported, then the math app can store/retrieve profile information offline and sync when a connection becomes available.

## Common Requirements

* Simple to use (metrics to be determined e.g. number of clicks to accomplish common tasks)
* For all HTTP downloads that do not contain personal/sensitive information:
  * The response body of any given URL must not vary based on request headers (e.g. it must not vary based on client user-agent etc) such that it can be shared with other devices.
  * The ```cache-control: public``` directive must be included in the HTTP response.
  * The server MUST support [HTTP range requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests) (all major web servers including Apache, Nginx, etc already support this)
* Installed size MUST be not more than 25% higher Google build for billions guidelines
* 15 minutes of usage MUST use less than 25MB of data on average
  * E.g. A parent with a bundle of half the average data usage in Sub-Saharan Africa allocates 20% of their data to education and has 5 children, each of whom use the app for 15mins/day at home.
  * Data downloaded using with the ```cache-control: public``` where a user can opt-in to using a proxy cache  (e.g. as per [Resiliant Asset Delivery (RAD)](RAD.md)) shall not count towards the total.
* 15 minutes of usage by 5 devices carrying out the same activities nearby with WiFi and Bluetooth switched on must use less than 35MB of data
  * E.g. the app should support peer-to-peer sharing of assets that do not contain personally identifiable information in school scenarios to reduce bandwidth usage. 
* MUST allow users to download key activities (e.g. lessons) for later use offline.

## Provider App Requirements

* MUST support [LTI Assignment and Gradebook Service (AGS)](https://www.imsglobal.org/spec/lti-ags/v2p0), [OneRoster](https://www.1edtech.org/standards/oneroster), and [OAuth](https://oauth.net/2/).
* SHOULD support [Experience API](https://xapi.com)
* MAY support [HTTP-IPC](https://github.com/UstadMobile/HTTP-IPC-Spec).

## Consumer App Requirements

* Single sign-on:
  * MUST support single sign using [OAuth](https://oauth.net/2/) which MUST work offline if the **provider app** is installed with app links [HTTP/IPC Spec](https://github.com/UstadMobile/HTTP-IPC-Spec?tab=readme-ov-file#offline-oauth-flow).
  * MUST use [HTTP/IPC](https://github.com/UstadMobile/HTTP-IPC-Spec?tab=readme-ov-file#offline-oauth-flow) to access REST APIs offline where the service is offered by the **provider app**
  * MUST support using the OneRoster API to retrieve information defined using the OneRoster standard (e.g. grade level, enrolments etc) where this information is required, except if the **consumer app** is only used via the **provider app** (e.g. Scenario B only)
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






