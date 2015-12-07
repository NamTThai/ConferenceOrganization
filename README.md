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
1. Session entities are required to have a name. Each of them has a conference entity as ancestor.
1. Session speaker entities have no ancestor. They are uniquely identified by their speakers' email address

## Task 2: Add Sessions to User wishlist
1. User wishlist is stored in his/her Profile entity. This wishlist consists list of URL safe Session IDs
1. User can add, remove and view his/her wishlist

## Task 3: Work on indexes and queries

#### 3.1 Come up with 3 additional queries that can be useful in this application
1. `getConferenceSessionBySpeaker`: Get all sessions by a specified speaker in a specified conference. The application already had a function to filter session by speaker email. It would be useful to filter by conference as well.
1. `getConferenceSessionByDuration`: Get all sessions in the specified conference and shorter than a specified amount of time.

#### 3.2 Get sessions with 2 inequality filters
* Name of function is `getSessionByComplexInequality`
* In this task, the app let users specified `SESSION TYPE` and `HOUR`. It will then queries all sessions that are different from `SESSION TYPE` and starts before `HOUR`
* This is a tricky problem, given that datastore queries only allow 1 inequality in each query
* This problem can be solved by performing the second inequality filter on local server after fetching data using the first inequality filter.

## Task 4: Feature Speaker
1. Featured speakers are added to Memcache using TaskQueue


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
