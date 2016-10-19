# Curator portal

## Possible Developmental Plans

Currently, the front-end interface exists. The back-end query framework still
needs to be developed. Possible ways to do this:

1. The Firebase way: Create a Google Compute Engine server that is always
listening. On this server, execute a function which connects to the Curator
Database. Inside of this script, make a listener which is always listening for
new children to be added to a "requests" node of the curator firebase
database. Then, from the statically served front-end interface, form queries and
push them to this node. The server can then read the request and use a
technology such as ElasticSearch to query some large database and push a
response to a "responses" node in the Firebase Database.

2. Old Fashioned way: Use AJAX or XMLHTTP to send requests to a formal server
and do queries.

3. The too-easy way: Use the firebase database as the art database, and iterate
through the entire DB to do queries.


## The Tekuma artwork database

The ambition of this section is to document current and proposed designs of the
Tekuma artwork database. At the time of writing, the overall system is simple: a
single CloudSQL database (from provider cloud.google.com) that presents as MySQL
and thus is a relational database. The columns are described below. The basic
workflow for changing the database is to, before anything else, make replica and
create back-ups off-site from Google Cloud.

### columns

Each item is of the form

    slug (TYPE): name of my pet slug or my friend's slug.

where "slug" is the column name, TYPE is an SQL datatype that is recognized by
MySQL, and any text after `:` is a description of intended content and usage.

* artist (TEXT):
* title (TEXT):
* description (TEXT):
* date_of_addition (DATETIME): ...to Tekuma db.
* artist_uid (CHAR(255)):
* artwork_uid (CHAR(255)):
* date_of_creation (DATETIME): ...claimed by the artist.
* tags (TEXT):
* thumbnail_url (CHAR(255)): valid URL for obtaining a thumbnail image, or "null" if not available or not known.
* origin (CHAR(32)): provider of the image. This must be one of several known strings:
  - "portal-source" (obtained directly from artist to Tekuma)
  - "art.com"
* reverse_lookup (CHAR(255)): unique identifier from Tekuma's perspective of data from outside providers. E.g., for the origin "art.com", the SKU is used for unique references.
* META (TEXT): data not otherwise specified.
