# Conference Central

Conference organization web app using Google App Engine.

## Requirements

- [python][0] >= 2.7
- [Google App engine SDK][1] >= 1.9.28-1

## APIs

- [Google Cloud Endpoints][2]

## Setup
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
2. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
3. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
4. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
5. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting
   your local server's address (by default [localhost:8080][5].)
6. Generate your client library(ies) with [the endpoints tool][6].
7. Deploy your application.

## Design Choices for Udacity Tasks

#### 1. Sessions and Speakers

Sessions are objects and ancestors to conferences to avoid the creation of orphaned sessions as well as to allow ease of querying sessions in a conference.

Sessions have the fallowing properties:
- name: String value, represents the alphanumerical name of the session.
- highlights: String value, alphanumerical text containing a brief description of the session.
- speaker: String value, alphanumerical name of the speaker (more defined below).
- duration: Integer value, length of session in minutes.
- typeOfSession: String value, alphanumerical category name.
- date: Date value, date of session's occurrence in yyy-mm-dd format.
- startTime: Time value, time of session's occurrence in Hours(24):Minutes(60) (%H:%M) format.
- websafeConferenceKey: String value, alphanumerical used as a key to identify conference ancestor.

Speakers are defined as strings in sessions to keep form unneeded complexity and to allow Speakers to be as flexible as possible.

#### 2. Task 3, two addition queries

1. Query for session by date, returns sessions that occur on a given date.
2. Query for session by type, returns sessions that have the given type.

#### 3. Task3, "problematic query"

The difficulty with the query of "time less than 7:00 and type not equal to workshop" is that it has inequalities on two fields, this is not currently allowed in Google App engine. To solve this a query for each property that has an inequality is made and only the keys are fetched, then the queries are put in a set and intersected with each other to leave only the mutually common results. This result will become the desired result.

One may also use a MapReduce algorithm to allow for better scaling and faster performance. Though this may introduce more complexity than is needed currently for this app, so the prior method was implemented instead.

[0]: https://www.python.org
[1]: https://cloud.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python
[2]: https://cloud.google.com/appengine/docs/python/endpoints
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
