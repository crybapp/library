# Cryb beginners guide Janus Edition

Welcome to the beginners guide for setting up Cryb with the Janus SFU. Some really important things to note:

* Janus absolutely requires Linux in order to run. We've packaged this up in a docker container for you so it is able to run everywhere
* We will be setting everything up under the assumption of using the Docker driver. This driver supports automatic creation and deletion so it's a better choice for most people.
* Later versions of this guide will visit some best practices to use for network security. 
* This is not the only way to set it up and this guide will be expanded to accomodate more setup scenarios. 

now that all that stuff is over, let's begin!

# Inital reqirements: 
Cryb needs a couple things in order to run properly. Namely NodeJS, Redis, MongoDB, and Docker.
*Git is optional but reccomended so I will include instructions here*

## NodeJS
NodeJS is a javascript runtime for running JS apps on your computer. It powers the Cryb Code. Don't worry too much about what that means if it's confusing, just know we need it.

### windows

Headover to the [Node download page](https://nodejs.org/en/download/) and pick the correct installer for your system. 

![Imgur](https://i.imgur.com/BVRdfav.png)

1. Once the installer is downloaded run it
2. Leave everything default and click next a couple of times. 
3. It may require administrative permissions. Click yes on the UAC prompt.
4. Click finish then it's done!

Let's check the install by opening up powershell/command prompt, then typing the command ```node -v```

if you see a version number it worked!

### MacOS / Linux 
The installation for Mac and Linux is much the same as above just grab the installer for your systems and let it all be default. 

If you want to use a package manager follow the instructions [here](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions-enterprise-linux-fedora-and-snap-packages)
*******************

## MongoDB
MongoDB is the database Cryb uses to make stuff work. Follow their guide [here](https://docs.mongodb.com/manual/installation/#mongodb-community-edition-installation-tutorials) to get it installed. I reccomend leaving it as a service, but you may not want it on all the time, so that's up to you.
***
## Docker
Docker is our magic cross-platform bullet. And by magic I mean that it just lets us run everything in linux on a windows computer. Please follow their guides [here](https://docs.docker.com/install/) for your operating system.

If you're on windows you'll need to take an extra step after installing docker for windows.

find the docker icon in the system try, right click and click on Settings:
![Imgur](https://i.imgur.com/lOm8X5f.png)

Under general find and check the box for "Expose daemon on tcp://localhost:2375 without TLS"
![Imgur](https://i.imgur.com/WAILYRM.png)

Now you're all setup!

***
## Redis
Redis is a in memory key/value store that cryb uses for temporary data and a Pub/Sub interface. That is, it's super fast and helps out alot. 


Redis is only compiled for *nix machines, as such it is not available in a super easy manner for windows users. Enter: Docker.

open powershell, teminal, or command prompt and run the command:
```docker run --name redis-server -p 6379:6379 redis```
That will setup the redis server with default settings and is enough for Cryb.

If you don't want to keep the command prompt open use this command instead
```docker run -d --name redis-server -p 6379:6379 redis```
This will run redis in detached mode which will keep it on all the time, without you having to keep the command prompt open.

***

## Git
Git is a source control tool but it also makes cryb easy to download and update
Follow these [instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) to get it set up for your OS. After that we are ready to setup Cryb!

***
# Time to setup cryb
Now that we've got all the applicationswe need ready and raring to go. It's time to get cryb and configure it. We're going to focus on an installation for localhost as it's the easiest to understand.

## Let's git down to buisness

The first thing we must do is download the cryb application. Git makes this nice and easy. 
Open command prompt/terminal/powershell and ``cd`` to a good containing directory. I'll be using ``Desktop/Cryb`` for this. 

So from ``Desktop/Cryb`` in our powershell window we're gonna need to run these 5 commands:

```
git clone https://github.com/crybapp/api.git
git clone https://github.com/crybapp/web.git
git clone https://github.com/crybapp/portals.git
git clone https://github.com/crybapp/portal.git
git clone https://github.com/crybapp/janus-docker.git
```

after that if you use the ``ls`` command (or ``dirs`` in command prompt) the directory should look something like this:
```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/11/2020   4:10 PM                api
d-----        1/20/2020   8:59 AM                janus-docker
d-----        2/17/2020  10:24 AM                portal
d-----        2/17/2020   5:39 PM                portals
d-----        2/20/2020   9:46 AM                web
```
(This is powershell output. It might look a little different on your os, but as long as the names are there you're good to go.)

## API
The Api microservice acts as the entrypoint into the backend of cryb. This service handles communication with web, rooms, and chat, as well as authentication with discord.

### The dreaded .env file
Most of crybs configuration happens in a .env file. This is a file to override specific Enviornment variables for an easy configuration.

If you want to copy and paste a working env file, it should end up like this:

```
NODE_ENV=development

PORT=4000

JWT_KEY=bigcrybfan

PORTALS_API_URL=http://localhost:5000
PORTALS_API_KEY=Thinkingwithportals

APERTURE_WS_KEY=unused
APERTURE_WS_URL=ws://localhost:9001

MONGO_URI=mongodb://localhost:27017/Cryb
REDIS_URI=redis://localhost:6379

DISCORD_CLIENT_ID=<YourClientID>
DISCORD_CLIENT_SECRET=<YourClientSecret>
DISCORD_CALLBACK_URL=http://localhost:3000/auth/discord

DISCORD_OAUTH_ORIGINS=http://localhost:3000,http://localhost,http://localhost:4000

MIN_MEMBER_PORTAL_CREATION_COUNT=1
MAX_ROOM_MEMBER_COUNT=100
```

Now that we have a baseline let's break it all down:

#### NODE_ENV
The NODE_ENV variable sets the applications context and enables and disables certain tool depending on it's setting 

for the purposes of Cryb it has 2 values: ``development`` and ``production``
``development``: Tells api to turn on more detailed logging.
``productions``: Turns it off.

#### PORT
The TCP Port to listen on. this should stay as 4000 unless you know what you are doing.

#### JWT_KEY
This is a key used to encrypt the messages between Api and other services. It can be anything you want it to be. It will be used again. 

This should, ideally, be a randomized base64 number of sufficent length for maximum security. but that won't affect whether or not it works.

#### PORTALS_API_URL 
This is the http endpoint for the portals microservice. Leaving it as it appears above should be fine.

#### PORTALS_API_KEY
This is the key set in the .env for portals under the variable ``API_KEY``. It works the same as the JWT_KEY in that it really doesn't matter what you put.

#### APERTURE_WS_KEY and APERTURE_WS_URL
These variables deal with running Cryb using aperture and are not applicable here. They are still enforced by API, however, so we must set them. Leave them how they are above.

#### MONGO_URI
The url for the mongodb database we installed earlier. Leaving it as it appears above should be fine.

#### REDIS_URI
The url for the redis service we setup earlier. Leave it as it is.

#### DISCORD_CLIENT_ID
This variable is used so discord knows who is trying to authenticate with them. We will set it in the next section as it requires some configuration on discord's end.

#### DISCORD_CLIENT_SECRET
This variable is used to authenticate your application with discord. We will set it in the next section for the same reason above.

#### DISCORD_CALLBACK_URL
This variable tells discord where to send responses back to. We need it to go to API/auth/discord. so just leave it alone.

#### DISCORD_OAUTH_ORIGINS
This variable let's discord know what URL's to accept requests from. Leave it alone.

#### MAX_MEMBER_PORTAL_CRATION_COUNT 
This variable sets how many people need to be in the room before a portal starts up. I use 1 for testing and two for production.

#### MAX_ROOM_MEMBER_COUNT 
This variable sets the maximum allowed people in the room. go crazy.


WOOO That's a lot to take in. We got a little bit more to go before API is ready though.
***
## Discord settings
You're going to need to register an application with discord in order to get everything to work nicely. and here's how we do it.

Go to [the discord developer portal](https://discordapp.com/developers/applications) and log in to your discord account

Click the New Application button in the top left.
![Imgur](https://i.imgur.com/wLV4uWy.png)

Give it a name in the pop up then click create

You should see a screen like this
![Imgur](https://i.imgur.com/DKWEb61.png)
Look familiar?

You'll need to copy the client ID and paste it into DISCORD_CLIENT_ID in the .env and do the same to CLIENT_SECRET (Just put it in DISCORD_CLIENT_SECRET)

Yay! Api is full configured... but discord still won't work yet. 

### Oauth settings
oauth is how we get authentication information from discord. in order for that to work discord needs to know so url's to send you to.

In the developer page find the OAuth2 option on the sidebar
![Imgur](https://i.imgur.com/t26nRam.png)
Under the redirects you'll need to add a few urls 
![Imgur](https://i.imgur.com/NdaReq5.png)
The Localhost:4000 is the most important but adding all for good measure doesn't hurt.

Congrats API is setup!

# Portals
Portals manages the VM's for us. It'll create them, tear them down, and make sure they're healthy.
The .env.example file has a lot of options, but it's less than it looks like.

Here's a minimal working version of a .env to go with API above.

## Windows:

```
NODE_ENV=development

PORT=5000

API_URL=http://localhost:4000
API_KEY=thinkingwithportals

PORTAL_KEY=space

MONGO_URI="mongodb://localhost:27017/Cryb"
REDIS_URI=redis://localhost:6379

DRIVER=docker

DOCKER_HOST=http://127.0.0.1
DOCKER_PORT=2375
DOCKER_IMAGE=cryb/portal
DOCKER_SHM_SIZE=1024

ENABLE_JANUS=true
JANUS_HOSTNAME=localhost
JANUS_STREAMING_ADMIN_KEY=testing
JANUS_PORT=8088
```

## *nix:

```
NODE_ENV=development

PORT=5000

API_URL=http://localhost:4000
API_KEY=thinkingwithportals

PORTAL_KEY=space

MONGO_URI="mongodb://localhost:27017/Cryb"
REDIS_URI=redis://localhost:6379

DRIVER=docker

DOCKER_SOCK=/var/run/docker.sock
DOCKER_IMAGE=cryb/portal
DOCKER_SHM_SIZE=1024

ENABLE_JANUS=true
JANUS_HOSTNAME=localhost
JANUS_STREAMING_ADMIN_KEY=testing
JANUS_PORT=8088
```

Let's break this down.

#### NODE_ENV
Same as in API

#### PORT
Same as API. Don't touch unless you know what you're doing.

#### API_URL
The HTTP url for the API service. You should be able to leave as is without problems.

#### API_KEY
This is the key used to authenticate the API microservice. It should be the same value from API's PORTALS_API_KEY

#### PORTAL_KEY 
This is the key used to authenticate the individual VMs and allow them to get configuration information. This will be the same as portal .env in a later section. 

#### MONGO_URI & REDIS_URI
Exact same as in API

#### DRIVER 
The driver is a series of modules that allow cryb to interact with different cloud provders and VM architecture.

Available drivers at the time of writing are:
 * Manual
 * Docker
 * DigitalOcean
 * HetznerCloud
 * GCloud
 * Kubernetes

Each driver has it's own configuration to add to .env that you can find in .env.example.

For now we're using docker driver.

#### DOCKER_SOCK 
The unix socket to communicate with the docker api. leave it as the default for now.

#### DOCKER_HOST & DOCKER_PORT
The address for the local machine's docker API. Leave it as default.

#### DOCKER_IMAGE
This the image to run for the portal. We'll be building it later but leave it as ``Cryb/Portal``

#### DOCKER_SHM_SIZE
This is the size of the shared memory that docker will use. Don't change this unless you know what you're doing.

#### ENABLE_JANUS 
This value tells portals to use Janus for transport instead of aperture.

#### JANUS_HOSTNAME
this is the dns value for the janus server. Odds are leaving it as local host will work.
**DO NOT PUT HTTP:// IN FRONT OF IT OR A PORT NUMBER AT THE BACK. IT WILL FIGURE IT OUT.**

#### JANUS_STREAMING_ADMIN_KEY
This is the key used to let janus know you are authorized to create endpoints. It is configured in janus.plugin.streaming.jcfg

#### JANUS_PORT
This is the port that the JANUS http server listens on 8088 by default.

Luckily after the ENV portals is good to go. 
***
# JANUS

Janus is our SFU or Selective Forwarding Unit. It's job is to make sure only the right traffic gets to users.

Default settings for janus are enough for most users. 

to get this ready all you need to do is run
``docker build . -t cryb/janus``
This will build the docker image so we can run it later.

***
# Portal
Portal is the individual VM that we use to host the browser. 

Here's a working .env file for it 
Docker for windows
```
NODE_ENV=development

PORTALS_WS_URL=ws://host.docker.internal:1337
PORTALS_KEY=space

STREAMING_URL=host.docker.internal
STREAMING_KEY="unused"

# The Display services like Chromium and ffmpeg will use
DISPLAY=:100

VIDEO_WIDTH=1280
VIDEO_HEIGHT=720
VIDEO_FPS=24
VIDEO_BITRATE=2400000

AUDIO_ENABLED=true
AUDIO_BITRATE=128000

IS_CHROMIUM_DARK_MODE=true
STARTUP_URL=https://google.com
```
Docker for everything else 
```
NODE_ENV=development

PORTALS_WS_URL=ws://127.0.0.1:5000
PORTALS_KEY=space

STREAMING_URL=127.0.0.1
STREAMING_KEY=unused

# The Display services like Chromium and ffmpeg will use
DISPLAY=:100

VIDEO_WIDTH=1280
VIDEO_HEIGHT=720
VIDEO_FPS=24
VIDEO_BITRATE=2400000

AUDIO_ENABLED=true
AUDIO_BITRATE=128000

IS_CHROMIUM_DARK_MODE=true
STARTUP_URL=https://google.com
```

#### NODE_ENV
Same as API

#### PORTALS_WS_URL
This is the address of the portals WS server. Default will work fine.

#### PORTALS_KEY
This is the key used to authenticate with the portals microservice

#### STREAMING_URL
This is the host name to send A/V data too. It should be left alone.

#### STREAMING_KEY
This isn't used in the janus implementation

#### DISPLAY
The linux display number to use. Don't change unless you know what you are doing.

#### VIDEO_WIDTH & VIDEO_HEIGHT
The resolution of the output video. This is what will be streamed to you.

#### VIDEO_FPS 
The framerate of the output video. This is what will be streamed to you.

#### VIDEO_BITRATE
How much memory the video can use per second. Higher values create higher quality at the cost of more network bandwidth

#### AUDIO_ENABLED
This enables audio capture and transmit.

#### AUDIO_BITRATE
This is the same as video bitrate. Higher values mean more fidelity.

#### IS_CHROMIUM_DARK_MODE
Darkmode option for the browser

#### STARTUP_URL
Allows you to set the website to open on.

Configuration complete!

# Running the service.  
