Conference Organization application, allowing users to create conference, manage sessions and speakers, among other functionalities.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

## Task 1: Add Sessions to a Conference

#### Relationship between entities in the application
1. Each Conference entity has a Profile entity as an ancestor
1. Each Session entity has a Conference entity as an ancestor
1. Each Session entity has a `HAS-A` relationship with a SessionSpeaker entity. In case no speaker is specified, it is defaulted to one with ID/email `NA`
1. SessionSpeaker entities are stored separately from Profile entities, even if they belong to the same person. This allows for modularity, such that modification in one entity does not affect the other.
1. The application stores SessionSpeaker as an entity instead of a simple StringProperty/StructuredProperty of Session entity. This allows for scalability: when additional information about speakers are needed, such as description, resume, those can be added without significant modification to the codebase.
1. SessionSpeaker entities have no ancestor. They are uniquely identified by their speakers' email address because email addresses are unique.

#### Design of Session kind
1. Each Session entities consist of `name`, `highlight`, `speaker` (identified by email), `duration`, `date` and `startTime`.
1. Session name is required due to designer's (me) preference.
1. `name`, `highlight`, `speakerEmail` are `StringProperty`. `highlight` can be changed to `TextProperty` if necessary. All of them are indexed by default. In the future, if memory becomes a problem, `highlight` does not need to be indexed.
1. `duration` is `FloatProperty`. It should specified session duration in terms of hour (i.e. 1h30 -> 1.5). This allows for inequality filters on `duration` property.
1. `date` is `StringProperty`. It should be `DateProperty` instead but the application uses `StringProperty` because of shipping deadline.
1. `startTime` is `TimeProperty`.

## Task 2: Add Sessions to User wishlist
1. User wishlist is stored in his/her Profile entity. This wishlist consists list of URL safe Session IDs
1. User can add, remove and view his/her wishlist

## Task 3: Work on indexes and queries

#### 3.1 Come up with 3 additional queries that can be useful in this application
1. `getConferenceSessionBySpeaker`: Get all sessions by a specified speaker in a specified conference. The application already had a function to filter session by speaker email. It would be useful to filter by conference as well.
1. `getConferenceSessionByDuration`: Get all sessions in the specified conference and shorter than a specified amount of time.

#### 3.2 Get sessions using query that has 2 properties with inequality filter
* Name of function is `getSessionByComplexInequality`
* In this task, the app let users specified `SESSION TYPE` and `HOUR`. It will then queries all sessions that are different from `SESSION TYPE` and starts before `HOUR`
* This is a tricky problem, given that datastore queries only allow 1 property to have inequality filter
* This problem can be solved by performing the second inequality filter on local server after fetching data using the first inequality filter.

## Task 4: Feature Speaker
1. If a Speaker has two or more sessions at any conference, he/she becomes a featured speaker for that conference
1. List of featured speakers are stored in Memcache
1. After a session is created, a background task is added to PushQueue. This task checks if the session's speaker is a featured speaker; if yes, this speaker ID is cached in Memcache.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
