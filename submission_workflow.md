## Artwork Submission Workflow
##### Connection between  Arist <-> Curator

### Terminology

Databases:
- Curator Firebase DB
- Arist   Firebase DB
- Curator SQL central DB

Curator Firebase DB Branches:
- submissions
- approved
- declined 

Daemons:
- artist-daemon (handles image resizing, and adding submissions to the curator DB)
- curator-daemon (handles moving data into the SQL database, and unlocking artist's artworks)

--------------------------------------------

1. An artist submits an artwork to tekuma
 - A copy of the artwork's JSON is copied from the artist DB to the curator DB's submissions branch
 - The artwork that appears in the `studio` is locked, and prevents the artist from submitting it again. 

2. A curator reviews the artwork from the curator portal.
	a) the curator sets the artwork to `held`, which then notifies the artist (via a message icon) that they have a new message. The curator should leave a summary of why the artwork was held, and what can be done so it can be accepted. When a submission is marked as held, the curator-daemon unlocks the artwork in the artist's `studio` interface.

	b) Curator sets the artwork as `declined`. The artwork's json is moved from the submissions branch to the declined branch. The artist is then notified of a change in artwork status. 

	c) Curator sets the artwork as `accepted`. The artwork's JSON is moved from the submissions branch to the approved branch. This then triggers the curator-daemon to copy the artwork's details into the SQL database. The artist is notified of a change in artwork status.

3. If the artwork was set as 'held', the artwork is then editable by the artist again. They can make any changes to the artwork they need to the artwork's information, and can then re-submit the artwork. This will overwrite the prior submission that was marked as held, as the submission branch is keyed by artwork_uid, and the artwork_uid is not changed. 

4. If the artwork was rejected, an immutable artwork remains in the artist's studio interface. The artist can also view the message from the curator on why their artwork was rejected, and the artist can re-upload and try again.

NOTE: When an artwork is held, this means that there is something wrong with its data. E.g., it has no story, its artist information is wrong, it has no tags or colors, etc. 
BUT, if there is a problem with the artworks image file (resolution, quality, aspect ratio,etc), the artwork should be declined. There is no way for the artist to upload a new image inside of an existing artwork. 
