# Curator portal

## Possible Developmental Plans
Currently, the front-end interface exists. The back-end query framework still needs to be developed. Possible ways to do this:

1. The Firebase way: Create a Google Compute Engine server that is always listening. On this server, execute a function which connects to the Curator Database. Inside of this script, make a listener which is always listening for new children to be added to a "requests" node of the curator firebase database. Then, from the statically served front-end interface, form queries and push them to this node. The server can then read the request and use a technology such as ElasticSearch to query some large database and push a response to a "responses" node in the Firebase Database.

2. Old Fashioned way: Use AJAX or XMLHTTP to send requests to a formal server and do queries.

3. The too-easy way: Use the firebase database as the art database, and iterate through the entire DB to do queries.


## The Tekuma artwork database

### columns

Each item is of the form

    slug (TYPE): name of my pet slug or my friend's slug.

where "slug" is the column name, TYPE is an SQL datatype that is recognized by
MySQL, and any text after `:` is a description of intended content and usage.

* artist ():
* title ():
* description ():
* date_of_addition (): ...to Tekuma db.
* artist_uid ():
* artwork_uid ():
* date_of_creation (): ...claimed by the artist.
* tags ():
* thumbnail_url ():
* origin ():
* reverse_lookup ():
* META (TEXT):
