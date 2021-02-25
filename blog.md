# Blog
This blog will be a play-by-play of the process of building this project. This will be written basically stream of consciousness at unpredictable intervals, so don't expect to find anything too useful here. Also, more than any other section of the website, this one is written with an audience of one in mind: myself.

- [Blog](#blog)
  - [Thinking about thinking about things - February 2, 2021](#thinking-about-thinking-about-things---february-2-2021)
  - [First steps - February 9, 2021](#first-steps---february-9-2021)
  - [Observations - February 16, 2021](#observations---february-16-2021)
  - [More Observations - February 25, 2021](#more-observations---february-25-2021)

## Thinking about thinking about things - February 2, 2021
For the last year or so, we've endured a barrage of heavy machinery noise. It's due to a large development project happening behind our apartment. I finally decided to make lemonade with these lemons. Since I've got such a great view of the development, I want to use AI/ML to analyse progress as it occurs.

The basic idea is that I'll frequently take photos of the development and then use various AWS tools to distribute, store, and analyse progress. A lot will go into it, but a few things that I can see needing:
* A Raspberry Pi to take photos and upload them
* Some way to access the Pi. Since it'll be at a window, it'll be awkward to connect a monitor
* S3 for storing photos online
* Rekognition for identifying things in the photos
* SageMaker for identifying changes over time
* EC2 for hosting the website
* Route53 for DNS resolution
* Cost Explorer for figuring out how much this is all costing me

I'm sure I'm missing a lot of things, but this is the start.

## First steps - February 9, 2021
I dusted off all my Raspberry Pis, found the most recent and powerful one, and created a fresh Linux install for it. I'm running basic Raspbian. I temporarily connected it to a monitor, did the usual config things, like changing the root user password, updated everything, configured SSH, and then stuck it in my window with the camera pointing out. After fiddling around with camera angles to find one that showed as much of the construction site as possible without any weird occlusions from my building, I wrote a very simple script to take photos and then set a cron task to run that every 5 minutes all day long. I'm going to let this run for awhile to see how consistent file sizes are and make sure my camera setup is sufficiently stable.

Things to work on:
* I don't have a good way to access the photos remotely. I can manually copy them through the command line, but that's pretty burdensome. Eventually I'll upload directly to S3, but I'm not there yet.
* Photos vary in size from between 2 megabytes and 5 megabytes. Generally night photos are smaller, and ones taken when it's very sunny are larger.
* Taking a photo every 5 mins means each day has 288 photos.
* I should think about some ways to add some metadata to photos. Things like wind, weather, and temperature might be interesting to compare against photos. Need to decide between storing them in some sort of database or just scraping the data using Python and adding it to the metadata of the file somehow

## Observations - February 16, 2021
The script has been running for a week. I added a samba server config to make it easier to view photos. It's pretty neat to page through them to see the progress throughout the day, and then compare photos from the same time on different days. The camera seems stable, and the script/crontab combo is working great so far. I did screw up the filenames of a small selection of files when I fiddled with the script a little, but I didn't lose any photos and I can derive the correct filename from the date on each photo. I'll fix this later, before I start offloading the photos elsewhere. The problem photos are all on February 11th.

Things to work on:
* I have six full days' worth of photos. Each day's total haul is around a gigabyte of photos, plus or minus about 10%.
* Consider using Python to rename those files. I know I could hack something together using the command line, but this sounds like a good exercise for Python.
* Look into animated gifs, maybe one for each day, one for each morning, and one for the end of the day.
* Could I use AI to compare expected weather with observed weather? Maybe I should also hook up a temperature sensor and track my measured temp, some weather app or site's expected temp, and compare those against the brightness of the photo or something.

## More Observations - February 25, 2021
Over two weeks of photos now. My directory structure is too flat and it's getting annoying to look through them, so I manually created a directory for each day and moved photos in there. The smallest day so far is just under a gigabyte and the largest one is just over 1.1, so they're staying pretty consistent. I looked through a lot of the night photos; I don't think it's worth keeping most of them. It's kind of neat to occasionally see the night watchman's flashlight winding through the site or spot an occasional cleanup crew, but I doubt these photos are high quality enough to glean any insight from programmatically.

Things to work on:
* imagemagick is what I need to create my animated .gifs
* I should write a new script that creates a new folder at the end of every day and moves current photos in there.
* Maybe I can keep taking night photos, but either do it less frequently or not upload them to S3 or include them in .gifs?
* The forementioned script should also create a .gif of each day's photos.
* Need to make some decisions about what other .gifs to create. 
* I know S3 can only be used to host static web pages, but is there some hacky way to show the first photo (easy) and the latest one (hard)?