# EduChip-Recipe

The EduChip recipe is a methodology for using existing edtech standards to create apps and content that can work in low resource environments (e.g. limited/no connectivity) AND interoperate so everything *just works* together instead of educators having to re-enter class lists and check multiple dashboards.

Scenario: An user (e.g. teacher, student, etc) installs their Learning Management System (LMS) app and a third-party API consumer app (e.g. a math app etc). The consumer app can request a token from the LMS app which can grant permission to given data scopes (e.g. similar to what would happen using [OAuth](https://oauth.net/2/)).

Terminology:

* Learning Management System (LMS) app: the primary app used to manage course enrolments, student information, etc e.g. [Moodle](https://moodle.org), [Ustad Mobile](https://www.ustadmobile.com), etc.
* Consumer (third party) app: an education app (e.g. a math app, other subject app, library app, etc) that will consume information provided from the LMS app (e.g. student enrolment information, student progress, etc).

# Components

* [Offline single sign on](SINGLE-SIGN-ON.md) - Allows a consumer (third party) app to request a token from the LMS without requiring Internet access.
* [Offline REST API access using Interprocess Communication (IPC)](REST-OVER-IPC.md) - Allows a consumer (third party) app to make [OneRoster](https://www.1edtech.org/standards/oneroster) and [xAPI](https://www.xapi.com) calls to the LMS app without requiring Internet access.
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

# EduBRICK requirements
* MUST implement all of the above consumer app requirements
* MUST NOT have minimum hardware requirements that exceed the latest version of [Android GO edition](https://developer.android.com/guide/topics/androidgo#hardware-reqs)
* Android app size (not including assets delivered via RAD) MUST NOT exceed *60MB-ish*
* Android app must start within *15ish* seconds (cold start) of launching on a device with the latest minimum Android Go hardware requirements.
* MAY support other operating systems or platforms (e.g. iOS, Windows, Linux, Web)
* MUST be licensed under an [OSI-approved license](https://opensource.org/licenses)
* MUST be able to function without any dependency not available under an OSI approved license.
* MUST be able to function without requiring any Internet connectivity (requiring assets to be loaded using RAD is allowed).
* MAY use libraries and content not available under an OSI license to enhance the user experience where available (e.g. Google Play Services, on-device generative AI, etc)
* MAY use connectivity if/when available to enhance the user experience (e.g. where a cloud backend compares results to student average results to provide better feedback/recommendations)
* MUST fulfill the [Digital Public Goods requirements](https://digitalpublicgoods.net/submission-guide/) including the [documentation](https://github.com/DPGAlliance/publicgoods-candidates/blob/main/help-center/documentation.md) and privacy requirements (does not need to be certified at the point of submission, but should meet all criteria).
* Android version MUST comply with Google Play policies
* iOS version (if any) MUST comply with Apple App Store policies
* MUST use up to date dependency versions (within 3 months of the latest release) 
* MUST be localizable: at a minimum the procedure to add a new language MUST be documented. The app SHOULD use a widely used format for localized assets (e.g. Android strings.xml, .po files, etc).











