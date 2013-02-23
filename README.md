Hubot iMessage Adapter
----------------------

Description
===========
This is an adapter for Hubot that lets you communicate with Hubot via Apple iMessage.


Installation and Setup
======================
Since Apple doesn't (currently) offer an API for accessing the iMessage protocol,
the only way Hubot can use iMessage is by communicating with Messages.app
through AppleScript. As such, using hubot-imessage requires Hubot to be running
on a machine with OS X 10.8 or newer.

Download the [latest version of Hubot](https://github.com/github/hubot/archive/master.zip)
(more info at https://github.com/github/hubot). Extract it somewhere, and then
add both `hubot` and `hubot-imessage` to the dependency section of your `package.json`:

    dependencies: {
        // more dependencies here...
        "hubot": ">=1.4.6",
        "hubot-imessage": ">=0.0.1"
    }

You likely also want to add the `hubot-scripts` package as well; see the regular
Hubot documentation for more info.

Run `npm install` to properly install your dependencies.


### Configure Messages.app
At this point, you've got a basic Hubot instance, but the Hubot iMessage adapter
requires some additional setup.

1. You'll need to set up a secondary iMessage account for your Hubot. Sign up for a
new Apple ID at https://appleid.apple.com/cgi-bin/WebObjects/MyAppleId.woa/wa/createAppleId
and sign in to it from your desktop Messages.app.

2. Open $HUBOT_PATH/node_modules/hubot-imessage/src/sendMessage.scpt (here, and all
other times $HUBOT_PATH is mentioned, replace it with the actual path to your Hubot
instance) in AppleScript Editor (located in /Applications/Utilities). On line 6,
 replace "$HUBOT_PATH" with the full path to your base Hubot dir.

3. In order for Messages.app to properly route conversations to Hubot, it needs to
be set up to run a few AppleScripts in response to events. Open up Messages.app,
then its Preferences pane from the title menu. Go to the Alerts tab.

4. Select the event "Text Invitation". Check the box for "Run an AppleScript",
and select "Auto Accept.applescript" from the dropdown menu.

5. Select the event "Message Received". Check the box for "Run an AppleScript".
In the selection dropdown, click "Choose Script". Find and select
"messageReceived.scpt", which is located in $HUBOT_PATH/node_modules/hubot-imessage/src
(and which you should have already edited, as per the above section).

6. Messages.app is now configured to accept iMessages from any user, but Hubot
will only repsond to commands sent from iMessage users in its whitelist.
Hubot reads in a comma-separated list of iMessage IDs from the environment
variable HUBOT_IMESSAGE_HANDLES to know who to trust. iMessage IDs typically 
take the format of `+15551234` or `E:steve@mac.com`. 
You can easily set this from your Terminal with something like:

  export HUBOT_IMESSAGE_HANDLES="+15551234,E:steve@mac.com"


Usage
=====
Run Hubot with the following command:

    $HUBOT_PATH/node_modules/.bin/hubot -a imessage

I'd probably recommend creating an alias for it, or adding $HUBOT_PATH/node_modules/.bin
to your $PATH.

From there, you can treat it just as you would any normal Hubot instance with
regards to installing custom scripts, etc.


Warnings and Miscellanea
========================
Typically, you'd host a Hubot on Heroku or something similar; requiring OS X
makes that impossible. There are a few unfortunate snags I've found from running
Hubot / hubot-imessage on a consumer desktop OS:

* Messages.app doesn't let you sign in to multiple iMessage accounts at the same
time. If you want to be able to send iMessages from your main account on your home
computer, you may want to set up a second machine to act as a dedicated Hubot
server.

* If your computer is in sleep mode, Hubot won't respond immediately. However,
the iMessages you send will queue up; when your computer comes awake, Hubot will
process and respond to messages in the order in which they were received.


Contribute
==========
I gladly accept pull requests!


License
=======
(c) Michael Walker
Licensed under the MIT License. See LICENSE for more info.