- [Armory](#armory)
  - [Hardware](#hardware)
    - [Raspberry Pi with camera](#raspberry-pi-with-camera)
  - [Linux Commands and Services](#linux-commands-and-services)
    - [raspistill](#raspistill)
    - [cron](#cron)
    - [samba](#samba)
  - [Software](#software)
    - [Visual Studio Code](#visual-studio-code)
    - [Markdown](#markdown)
  - [AWS](#aws)
    - [S3](#s3)
  - [Other Online Services](#other-online-services)
    - [GitHub](#github)

# Armory

Welcome to a listing of the tools in my arsenal. This page will explain all the moving parts that make my system work. This includes physical hardware, like the Raspberry Pi that's taking my photos, software, Linux commands, phone apps, and anything else that I use to get this thing going. In most cases, discussion of any tool will have two parts. The first part will be a brief explanation of what the tool is used for. This section will be written for people who are familiar with the internet, but may not have much of an understanding of what's happening on the other end of the series of tubes. The second part will be a more in-depth explanation of how I am using that item in this specific case. The target audience here is essentially myself and nerds like me who may need a quick reminder of what does what.

## Hardware

### Raspberry Pi with camera

The Raspberry Pi is a very low-cost computer. It runs a free operating system based on Linux, and is extremely good for use cases where you don't need a full-blown Windows or Mac OS computer. It's small, quiet, and cheap. It can also be tailored for very specific functions. The Raspberry Pi that I'm using is about the size of a deck of cards and can be used as a desktop computer. There are other models that are the size of a pack of gum that are more limited in functionality, but may make sense for my humble needs here.

I have a Raspberry Pi 3 B+ with the Raspberry Pi Camera 1.3 attached to my study window. Right now all it's doing is taking a photo every 5 minutes 24 hours a day. After I've gathered some more photos and determined the parameters of the AI/ML portion of this project, I'll adjust the script to take photos more or less frequently, add options to temporarily increase frequency on demand when more interesting construction is happening, create animated gifs, and to automatically upload the photos.

## Linux Commands and Services

Linux is a free operating system that's popular among developers and system administrators. Like the Raspberry Pi, it's designed to be extremely flexible and can be customized for specific uses. For example, I only need my Raspberry Pi to take photos and upload them to the internet. This is a simple use, and it doesn't require a graphical operating system. Therefore, I don't have one installed. This means my Pi has all its resources dedicated to its assigned tasks. It also means there are fewer things that can go wrong, which is important when the whole rest of my project relies on this part of the system. Linux is very security and privacy conscious, so I feel more comfortable running this in my home 24/7 than I would be with other devices.

I run Linux primarily in two places. It runs on my Raspberry Pi, and it'll also run on all of my AWS instances. Mac OS is also based on a Linux-like operating system and has most of the same toolkit. I may do some processing of photos on my Mac Mini before uploading to AWS.

### raspistill

Raspistill takes photos using the Raspberry Pi camera.

By default, raspistill simply takes a photo. It has options to change the resolution, orientation, and various other parameters of the photo.
The specific command I use is:

```
raspistill -o /home/pi/photos/$DATE.jpg
```

The -o /... part is telling the command where to save the photo.
$DATE makes the filename the date in a specific format. I use YYYY-MM-DD_HHMM. This makes it easy to sort and organize the images, which will be important once I start uploading and analyzing these.

### cron

cron is how Linux schedules tasks to be run at specific times. It's extremely flexible. Currently I use it to take a photo once every 5 minutes, 24 hours a day. Eventually I'll get a little fancier and move yesterday's photos into a different directory, create an animated .gif of them, and then upload that to AWS.

Here's the entry for this in crontab, the file that dictates what cron runs

```
*/5 * * * * /usr/local/bin/take-photo
```

_/5 tells it to take a photo every 5 mins. _/1 would take a photo every minute, and \*/30 would take one every half an hour.
The remaining asterisks are for hour, day of the month, month, and day of the week, respectively.
The final portion of the line tells cron the path to the file that I want it to run every five minutes.

### samba

Samba is a way to share files and printers on a network. It's a free and open source implementation of Microsoft's original networking technology. When configured properly it's virtually invisible to the end user, so it's likely that you've used it without knowing it. If you've ever shared a printer from your computer to someone else on your network, that was likely using Samba.

I run samba on the raspberry pi so I can check in on my recent photos from any of the computers on my home network. It's currently configured to only allow local network access. I will probably keep it that way, and change it so that files are available from outside my network when I start uploading them to AWS. I used [this](https://pimylifeup.com/raspberry-pi-samba/) tutorial to configure my setup.

## Software

### Visual Studio Code

Visual Studio Code is Microsoft's free code editor. It's very flexible and powerful, and can be heavily customized for your specific use case. This is analogous to Microsoft Word, but for code. It has features equivalent to spell-checking and formatting, but for coders. It's available for Windows, Mac OS, and Linux.

There are a lot of options out of the box, but one of the best features of VSC is its extensibility. You can install extensions to specialize your VSC for whichever programming languages and online services are important to you. For example, I use extensions for Markdown, Python and GitHub that provide helpful previews, highlighting, and symbols so I can write faster. I also use an extension to upload all my changes to GitHub, which makes it very quick to make changes and post them to the website.

### Markdown

Markdown is sort of a shorthand way to write simple content for the web. I write in Markdown and then use a Visual Studio Code plugin that converts my writing into HTML, which is then uploaded to my site. It's probably best to show the difference. In HTML a link looks like this:
```
 <a href="https://www.w3schools.com/">Visit W3Schools.com!</a>
 ```
 That's a lot of arbitrary stuff to remember! Markdown is a lot simpler:
 ```
 [Visit W3Schools.com](https://w3schools.com)
 ```

My favorite resources for Markdown are [this](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) Markdown cheatsheet on Github, and the Markdown All in One extension for Visual Studio Code by Yu Zhang.

## AWS

### S3

S3 is Amazon's Simple Storage Service. It's how an absolutely staggering number of companies, organizations, and individuals host and store files online. A huge percentage of overall internet traffic is simply transferring something from S3 to users in various forms. For example, Netflix hosts all of its video on S3. When you watch a show on your phone, tablet, or TV, the Netflix app is securely streaming the video file to you from a nearby AWS server. S3 is cost-efficient and it's easy to replicate your data across the globe so you're safe in case of local failure.

S3 is where I will store my photos, at least initially (I don't know how SageMaker works yet, so I may eventually need to transfer from S3 into that).

## Other Online Services

### GitHub

[GitHub](https://github.com/) is a Microsoft-owned service to upload and share code. Think of it as Google Docs specialized for programmers. It has powerful collaboration tools that help teams stay in sync wherever they are and whatever they're writing.

GitHub is also how I'm hosting the site initially. As my needs grow I will probably migrate to hosting my website on an AWS server, but that won't be for some time.
