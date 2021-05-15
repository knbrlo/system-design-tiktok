# tiktok

- [x] https://www.youtube.com/watch?v=Z-0g_aJL5Fw&fbclid=IwAR07JfrhcYD0O8Jue-GmfZ1nmum9vD1hOLLr9bZFph3WzPyNn2B7WnPaBBk

## Summary
- Mobile app
- Upload a video
- View a feed of videos
    - One at a time

- Follow users

- Perform actions on the videos
    - Fav
    - Comment

-------

## Notes
- 1 - Upload Videos
    -   Assume that the videos are 1 minute long
    - If uploading videos, are there text asociated with that?
        - Yes assuming you can add a comment or caption to it.
        - Tagging

- 2 - View Feed
    - Aggregating videos from people
        - People I follow?
            - Focus on this first.

        - Recommended videos?
        - Is the aggregation a mixture of those two things

- 3 - Favortieing, Following, COmmenting,
    - Video interaction endpoint 


- 4 - Availability
    - Assuming that it needs to be a highly available system, serving a lot of users at once 99.999% 
    - If it doesn't need to be highly available, then we can talk about balancing our budget     

- 5 - Latencty
    - Since it's a mobile device then we probably cache a lot of things on the device itself to start.
    - Then new content we pull more.
    - Upload
        - TBD
    - Download
        - TBD

- 6 - Scale
    - What's a rough number of users per day.
        - 1 Million active users a day.
    
- 7 - Storage
    - Video Size Estimate
        - Videos 5MB / 2 Videos Per day Upload
    - User Metadata 1-5kb
    - Once we upload the video we want to store it in blob storage maybe in S3.
    - Video itself would live in the blob store in S3 and the url to that would live in a video table.
    - Once we hit the API with the video we'd return with a 200 saying that the video uploaded correctly.

- 8 - DataBase
    - Use a relational database, whatever flavor of relational database that you choose.
    - What are the differences between a relational database and a different type of database and why would you want to use a relational database?
        - Relational Database vs SQL Database
            - Relational
                - Typically more structured data
                - User data objects
                - Linking tables together.
                - Such as a Single User that has many videos and those could be two different tables.

            - NoSQL 
                - Really good for unstructured data, blob data more free form in nature
                - Not as structured you'lll be looking for keys and values in that data

        - In this case a relational database is going to be more space and speed efficient

- 9 - Using the app
    - When we open the app, assuming that we'd like to start preloading as much as we can so that the user isn't waiting too much for the app to load.
        - Would want to load the first 3 videos as quick as possible.
        - Would be nice to have some sort of redis cache or cache where we pre-load the top 10 before they
    - We could run a precache_service in the background on our severs where it pre-loads suggestions for a certain uuid and pre-caches them.
        - Could be done when they do the get request or we could do it in the background.
        - Trying to get away from
            - System is going to be really read heavy, so we'd also want a read-only database where we don't create too much load on one database.
            - We'd want to have something that manages the reads.

- 10 - What would be the bottlenecks of this system if we 10x our traffic?
    - First thing I think about is regions, regional data centers geolocating.
    - We're gonig to put the user behind some, the APIS behind some CDN
    - Say we get a famous person that sends out a video
        - The first person to get that video in that regions we cache that video in that region, then all the users are getting it after that are getting a cached version then we route it to a local node so the users that are getting it aren't getting it from my blob storage it's only hitting the CDN.
    - Having a load balancer behind the API endpoints is a good idea too which allows us ot have almost no down time when we make updates to services then load balancer could switch between available systems to process requests.
    - Also we'd want to setup auto-scaling groups for our LB, Write DB, Read DB, pre_cache service.
    - We'd also want to look at Datbase Sharding some service before we write to the DB where it picks what DB to write to. There are a lot of different sharding algorithms.
        - We could shard based on user location etc.

- What would we add to tnhis before wrapping up?
    - If we had more time we'd get into the pre_cache service as it might have it's own DB structure.

