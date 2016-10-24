# Curator portal

## The Tekuma artwork database

The ambition of this section is to document current and proposed designs of the
Tekuma artwork database. At the time of writing, the overall system is simple: a
single CloudSQL database (from provider cloud.google.com) that presents as MySQL
and thus is a relational database. The columns are described below.

The basic workflow for changing the database is to, before anything else, make
replica and create back-ups off-site from Google Cloud. Then, start a new
CloudSQL instance, and create a table that has columns as desired. Populate the
database, e.g., using an import program that maps rows from previous databases.
Next, deploy the back-end code with configuration to point to a read-only
replica of the previous database. Move the new database to the production
location, and finally update the configuration of the back-end to use it.

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
