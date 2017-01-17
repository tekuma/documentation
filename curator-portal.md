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


## Search structures

This section documents the structure of search queries. API details are provided
in the source code but might be moved here eventually if extraction-from-code
tools for JS API documentation are not found.

There are two main parts of the search query:

1. general search text
2. topic-oriented fields.

The first corresponds to the General search box in the UI accordion. It is
supposed to be free form and potentially interpreted in sophisticated ways that
are not revealed to the user, and that may change with new deployments of the
backend (transparently to the user, except possibly as noticing new search
results from the same query string).

The second provides a way to constrain the search results by declaring
particular requirements of any artwork. Compared with other websites, these act
like fields of the "advanced search". E.g., a user can declare text that must
appear in the title, or the artist's name. At the level of the JavaScript API,
relevant functions that work with search structures have two parameters that
correspond to the parts described above. E.g.,

    query = "generic search string"
    fields = {
	title: "title",
	artist: "artist's name",
	color_list: [0xdeadbe, 0xeff00f, 0x66666]
    }

NOTE: query and fields given are interpreted as conjunction. E.g., if `artist`
is the only defined element of `fields', then all matches from `query` must have
artist name that matches fields.artist. Further note that "match" for TEXT type
columns is currently interpreted as existence of substring, independently of
lowercase or uppercase.


## Other parts of the artwork db API

### detail

This section describes an API through HTTP that provides client-side programs
access to details of artwork, including labels and other items associated with
the artwork (but not necessarily in the artworks table). The basic request is of
the form:

    GET /detail?uid=deadbeef&auth=secretcode

where `deadbeef` is the artwork UID, `secretcode` is a Firebase authentication
token, e.g., as already used for `/search?auth`. The return value is in JSON and
provides details about the requested artwork if one with the given UID is found,
otherwise an object containing only the requested UID and `found: false` (e.g.,
`{uid="deadbeef", found: false}`) is returned. Sketch of the schema:

    {
    uid: String
    found: true  // or false if no artwork with UID found
    artist: String
    title: String
    description: String
    album: String
    date: String of the form YYYY-MM-DD  // date of uploading
    creation_year: Integer
    thumbnail512_url: String
    tags: Array of Objects, each object has schema {
	rgb_colors: Array of RGB integer triples  // each from [0, 255]
	labels: Array of Strings
	}
    }
