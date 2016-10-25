# Organization and relationship of tables and databases

## Terminology

Two types of database are currently employed:

* Firebase (https://firebase.google.com/docs/database/) JSON-oriented, and

* Google CloudSQL (https://cloud.google.com/sql/docs/),
  which presents as a MySQL database.


## Summary of the main database

The main database that is used in production is named `Tekuma_artworkdb`. It is
relational and provided as a CloudSQL instance. The tables are as follows:

1. artworks - there is precisely one row per artwork, which has a globally
   unique identifier (UID). Here, being globally unique implies that artwork
   UIDs can be used in any context throughout the Tekuma data-space (e.g., other
   tables in this database), and this table can be queried to obtain more
   information using the UID. Note that an artwork may have associated with it
   multiple distinct image files, e.g., of different sizes or file types.

2. artists - there is precisely one row per artist, where "artist" is
   interpreted as a legal entity. In the case of third-party brokers like
   art.com, it may be difficult to ascertain the legal entities behind the
   provided artists' names, but anyway it might not be necessary to do so from
   the perspective of Tekuma as a company. However, any artist with which Tekuma
   engages in contract should have a dedicated row in the `artists` table.

3. labels - labels (also known as "tags" or "keywords") used to describe
   artworks or artists. Note that labels in this table are separate from
   particular artworks. Relationships among labels and entities that are labeled
   are expressed in the `associations` table.

4. associations - pairs of labels and artworks, or labels and artists, together
   with annotation about the claimed relationship, e.g., when it was decided and
   whether some third-party service (e.g., Clarif.ai) determined it.
