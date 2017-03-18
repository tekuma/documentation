# Protocol for Artist Removal
When a request is received from an artist to have their profile and artwork removed from our system, do you following.

1. User logs the request here:
[https://goo.gl/forms/Dh8ffMiuHD8mB1HJ3](https://goo.gl/forms/Dh8ffMiuHD8mB1HJ3)

2. Which, inputs the request automatically into:
[https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136](https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136)

3. After getting the user's information from the Sheet, the user's UID needs to be deduced. 

4. After this information is logged, the removal begins. A script __Will__ be created which deletes their account on artist.tekuma.io. The script needs the artist's UID.
    - Remove all user data from `/public/onboarders/{UID}` in firebase
    - Remove all user data from `/private/onboarders/{UID}` in firebase
    - Remove their firebase.auth() user account
    - Remove their images from storage at `art-uploads/portal/{uid}`    

5. A script will also be created which removes the user's data from the curator SQL database.

5. The deleter should mark the date/time of when the deletion occurs at:
[https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136](https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136)
---------------------------------------
### Manual Deletion from artist.tekuma.io
#### If script is un-accessible, a user can be deleted manually. 

1. View log at [https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136](https://docs.google.com/spreadsheets/d/1w956GEiks9RkAu0E_9ObK9iR5CTKJW7di98uU65I7gM/edit#gid=1717827136) 

2. After gathering the user's info, use their Email Address to find their UID at https://console.firebase.google.com/project/artist-tekuma-4a697/authentication/users 

3. In the spreadsheet, record their UID, and when they first made an account. 

4. From the user dashboard, click on the '...' and click 'delete account' 

5. Go to the firebase database:  https://console.firebase.google.com/project/artist-tekuma-4a697/database/data/  and navigate to the data at: https://artist-tekuma-4a697.firebaseio.com/public/onboarders/{UID}  in the box and click 'Go' , then delete the entire node corresponding to the user's UID. 

6. Then navigate the firebase database to  https://artist-tekuma-4a697.firebaseio.com/_private/onboarders/{UID}, and delete the corresponding node. 

At this point the user has been deleted from Artist.tekuma.io, but any of their work that has been reviewed and approved is still in the central curator's database, and is accessible through curator.tekuma.io. 


-------------
NOTE: after deletion, DATA (not images/files) can be recovered from the daily back-ups at 
https://console.firebase.google.com/project/artist-tekuma-4a697/database/backups 

