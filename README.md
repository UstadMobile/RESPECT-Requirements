# EduChip-Recipe

The EduChip recipe is a methodology to combine existing edtech and HTTP standards to create apps and content that can work in low resource environments (e.g. limited/no connectivity) AND interoperate so everything *just works* together instead of educators having to re-enter class lists and check multiple dashboards.

Scenario: An user (e.g. teacher, student, etc) installs their Learning Management System (LMS) app and a third-party API consumer app (e.g. a math app etc). The consumer app can request a token from the LMS app which can grant permission to given data scopes (e.g. similar to what would happen using [OAuth](https://oauth.net/2/)).

Terminology:

* Learning Management System (LMS) app: the primary app used to manage course enrolments, student information, etc e.g. [Moodle](https://moodle.org), [Ustad Mobile](https://www.ustadmobile.com), etc.
* Consumer (third party) app: an education app (e.g. a math app, other subject app, library app, etc) that will consume information provided from the LMS app (e.g. student enrolment information, student progress, etc).

# Components

* [Offline single sign on and REST API access](SINGLE-SIGN-ON-OFFLINE-API.md) - Allows a consumer (third party) app to request a token from the LMS without requiring Internet access and make [OneRoster](https://www.1edtech.org/standards/oneroster) and [xAPI](https://www.xapi.com) calls to the LMS app without requiring Internet access.
* [Resiliant Asset Delivery (RAD)](RAD.md) - Allows a consumer (third party) or LMS app to retrieve assets as flexibly as possible; this includes loading via USB stick, retrieving data from nearby devices instead of the Internet where possible to reduce bandwidth usage (e.g. download once instead of 30 times when there are 30 users), and opting in (with user consent) to using a caching http proxy (e.g. if provided on a school network) to load assets that do not contain personal information.

# LMS Requirements

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







