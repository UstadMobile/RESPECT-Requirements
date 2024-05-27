# EduChip-Recipe

The EduChip recipe is a methodology to combine existing edtech and HTTP standards to create apps and content that can work in low resource environments (e.g. limited/no connectivity) AND interoperate so everything *just works* together instead of educators having to re-enter class lists and check multiple dashboards.

Scenario A: A user (e.g. teacher, student, etc) installs their Learning Management System (LMS) app and a third-party API consumer app (e.g. a math app etc). The student opens the math app and taps "sign in with LMS app". They can then accept/decline permissions requested by the math app. If they accept, they return to the math app. The math app can now access their profile information (e.g. name, grade, etc) without requiring Internet or access or having to re-enter information. The math app can retrieve and save learner progress data from the LMS app. If the student opens the math app on a different device, the LMS app takes care of syncing the latest data, so the user can resume from where they left off on any device. Communication between the apps does not require Internet access.


Scenario B: A user (e.g. teacher, student, etc) installs their Learning Management System app and a third party API consumer app. They open their course in the LMS app which lists lessons they are expected to complete. The LMS app launches the math app (e.g. using an Intent) and includes the information the math app requires to access the user learner profile and the id of the lesson. The learner completes their lesson in the math app and then returns to the LMS app when finished.

This is a recipe that achieves the same result as using [OAuth](https://oauth.net/2/) or similar single sign-on and online access to OneRoster and Experience API offline.

Terminology:

* Provider app: the primary app used to manage course enrolments, student information, typically a Learning Management System (LMS) e.g. [Moodle](https://moodle.org), [Ustad Mobile](https://www.ustadmobile.com), etc. or Student Information System (SIS) 
* Consumer app: an education app (e.g. a math app, other subject app, library app, etc) that will consume information provided from the provider app (as above).

# Components

* [Offline single sign on and REST API access](SINGLE-SIGN-ON-OFFLINE-API.md) - Allows a consumer (third party) app to request a token from the LMS without requiring Internet access and make [OneRoster](https://www.1edtech.org/standards/oneroster) and [xAPI](https://www.xapi.com) calls to the provider app without requiring Internet access.
* [Resiliant Asset Delivery (RAD)](RAD.md) - Allows a consumer (third party) or provider app to retrieve assets as flexibly as possible; this includes loading via USB stick, retrieving data from nearby devices instead of the Internet where possible to reduce bandwidth usage (e.g. download once instead of 30 times when there are 30 users), and opting in (with user consent) to using a caching http proxy (e.g. if provided on a school network) to load assets that do not contain personal information.

# Provider app requirements

* MUST provide operating system manifest information such that consumer apps can detect the presence of the app.
* When single sign on is requested, MUST show a user interface (if required) to allow the user to accept or decline the request. This can include enforcement of organizational policies
* MUST return a result that includes a localhost http url that the consumer app can use to perform requests.
* MUST provide the OneRoster Roster and Gradebook API to allow a consumer app with a token to store/retrieve information about classes, enrolments, grade level, and student results over the local http url.
* MUST Provide the Experience API on the local http url to allow a consumer app with a token to store xAPI  statements and all xAPI endpoints (State, Activity Profile, etc).
* MAY synchronize user data over a local network based on its own security policies to enable users on another device (e.g. a teacher monitoring student progress) to see student progress data without requiring an Internet connection.

# Consumer app requirements

* MUST support offline single sign on such that a user can enter the app without requiring Internet access or creation of a separate account.
* MUST use the OneRoster API to retrieve information defined using the OneRoster standard (e.g. grade level, enrolments etc).
* MUST store all profile information required for the user to resume subsequent sessions using the OneRoster and/or Experience API (e.g. to allow the user to resume a session on a different device).
* MUST store learner progress data using the OneRoster Result API.
* SHOULD store granular learner activity data using Experience API statements (e.g. when a user answers a question).
* MAY ask the user additional information that is not part of the OneRoster standard (e.g. "How much do you like math?").
* MUST use Resiliant Asset Delivery (RAD) to load any assets (e.g. images, textures, audio, video, etc) required that are not contained within the installed app itself.







