# MTGATracker

[![Discord](https://img.shields.io/discord/425145310684250112.svg)](https://discordapp.com/channels/425145310684250112/425145310684250114)
[![Build Status](https://travis-ci.org/shawkinsl/mtga-tracker.svg?branch=master)](https://travis-ci.org/shawkinsl/mtga-tracker)
[![Github All Releases](https://img.shields.io/github/downloads/shawkinsl/mtga-tracker/total.svg)](https://github.com/shawkinsl/mtga-tracker/releases)
[![Github All Releases](https://wt-bd90f3fae00b1572ed028d0340861e6a-0.run.webtask.io/mtga-tracker-game/games/count?badge=true&fixcache)](https://github.com/shawkinsl/mtga-tracker/releases)
[![Unique Users](https://wt-bd90f3fae00b1572ed028d0340861e6a-0.run.webtask.io/mtga-tracker-game/users/count?badge=true&fixcache)](https://github.com/shawkinsl/mtga-tracker/releases)
[![Beerpay](https://img.shields.io/beerpay/shawkinsl/mtga-tracker.svg)](https://beerpay.io/shawkinsl/mtga-tracker)
![GitHub repo size in bytes](https://img.shields.io/github/repo-size/shawkinsl/mtga-tracker.svg)
[![serverless](http://public.serverless.com/badges/v3.svg)](https://webtask.io/)

<!-- [![Coveralls github](https://img.shields.io/coveralls/github/shawkinsl/mtga-tracker.svg)](https://coveralls.io) <!-- TODO: this -->
MTGA Tracker is an electron-based app that helps you track your MTGA decks!

![Tracker Features](https://i.imgur.com/drjM4D7.png)

<!-- [![Travis](https://img.shields.io/travis/shawkinsl/mtga-tracker.svg)](https://travis-ci.org/shawkinsl/mtga-tracker) --><!-- TODO: this -->
<!-- [![Code Climate](https://img.shields.io/codeclimate/shawkinsl/mtga-tracker.svg)](https://codeclimate.com/github/shawkinsl/mtga-tracker) --><!-- TODO: this -->

## I'm not reading anything else until you tell me how to use it

I figured as much. I know this doc is long, please at least take a moment and scroll to [disclaimers](#disclaimers)
before continuing, though.

### Option 1 (recommended): Run from a release

Download the latest release from the "releases" link above (or
[right here](https://github.com/shawkinsl/mtga-tracker/releases)). Use your favorite md5 tool to verify what you
downloaded is legitimate.

![Selecting the correct download link](https://raw.githubusercontent.com/shawkinsl/mtga-tracker/master/.readme_data/readme_1.png)

Note that you may have to tell Windows Defender to allow MTGA Tracker to run, as MTGA Tracker is currently
unsigned (did you know that code-signing keys & certs cost over $100 per year?)
Your antivirus will likely also want to scan it the first few times you run it.
See more in the [Signing section](https://github.com/shawkinsl/mtga-tracker#signing-code) below.

![Getting past windows defender](https://raw.githubusercontent.com/shawkinsl/mtga-tracker/master/.readme_data/readme_2.png)

### OR, Option 2: Run from source

_This option is not required or recommended unless you intend to contribute code changes to MTGATracker_

Make sure you have all of the following installed:
- [python3.6+](https://www.python.org/downloads/)
- [node.js / npm](https://nodejs.org/en/download/)
- (optional, but recommended) [Git Bash](https://git-scm.com/downloads)

Note that if you choose to skip Git Bash, you're on your own w.r.t. formatting shell commands.

1. Open Git Bash
1. Make sure python is installed by typing: `python --version` . You should see:
    ```bash
    you@yourmachine MINGW64 ~
    $ python --version
    Python 3.6.4
    ```
1. Make sure npm is installed by typing: `npm --version` .  You should see:
    ```bash
    you@yourmachine MINGW64 ~
    $ npm --version
    5.6.0
    ```
1. Run these commands
    ```bash
    cd /c/path/where/you/want/the/code
    git clone https://github.com/shawkinsl/mtga-tracker.git
    cd mtga-tracker
    # next 4 lines """optional"""
    python -m ensurepip
    pip install virtualenv --user
    python -m virtualenv venv
    source venv/Scripts/activate
    # read more about virtualenv here: http://www.pythonforbeginners.com/basics/how-to-use-python-virtualenv
    pip install -r requirements.txt
    cd electron
    npm install .
    ```
1. If all went well, you should now be able to:
    ```bash
    npm start
    ```
    And the decktracker UI should launch!
    
    Note: check out debug flags, which can be set directly in
    [main.js](https://github.com/shawkinsl/mtga-tracker/blob/master/electron/main.js#L22)
    
### Building

Building from windows? Try `sh build.sh` from git bash. If that doesn't work, get in touch with @shawkinsl to figure it
out. There's an open issue to improve this process--help us get CI/CD set up? See
[contributing](https://github.com/mtgatracker/mtgatracker/blob/master/CONTRIBUTING.md) ;)

Output will end up in `MTGATracker-<os>-<arch>` .

Building is probably not supported on OSX yet. (But then again, neither is MTGA, so what are you even doing?)

### Native App
**Backend**

The back end is at it's core a python log parser project. There's a websockets app tacked on top to communicate state
with the frontend.

**Front end**

The front end of MTGA Tracker is an electron app that uses node / websocket to communicate with the backend

### Project layout

`app` - python backend for watching MTGA logs

`electron` - native electron app

`scripts` - various scripts and utilities. either one-time use, or just don't really fit anywhere

`webtasks` - node.js / webtask.io code for webapp and app to talk to

`docs` - code for mtga-tracker.com (called docs for github pages)

### Signing code

Currently MTGATracker ~~is self-signed.~~ (Edit: since this doesn't do anything, we won't bother until
we get a real cert. Did you know that code-signing certs cost over $100 a year?! We didn't.)
So MTGATracker will likely go unsigned unless / until it gets at least a few hundred
[in donations](https://github.com/shawkinsl/mtga-tracker/blob/master/contributors/fundraising.md) ;)
 
Regardless, under the guidance of this [stackoverflow question](https://stackoverflow.com/questions/84847/how-do-i-create-a-self-signed-certificate-for-code-signing-on-windows),
use the following commands to self-sign binaries (not really that useful, but good practice in case MTGATracker ever
does get a real cert).

```powershell
# generate a pfx certificate
PS C:\Users\Spencatro> $cert = New-SelfSignedCertificate -DnsName mtgatracker.com -Type CodeSigning -CertStoreLocation Cert:\CurrentUser\My
PS C:\Users\Spencatro> $CertPassword = ConvertTo-SecureString -String "my_passowrd" -Force –AsPlainText
PS C:\Users\Spencatro> Export-PfxCertificate -Cert "cert:\CurrentUser\My\$($cert.Thumbprint)" -FilePath "c:\t.pfx" -Password $CertPassword

# use windows SDK to sign with the certificate
PS C:\Program Files (x86)\Windows Kits\10\bin\x64> ./signtool.exe sign /v /f C:\Users\Spencatro\t.pfx /t http://timestamp.comodoca.com/authenticode /p my_password C:\Users\Spencatro\PycharmProjects\mtga-tools\MTGATracker-win32-x64\MTGATracker.exe
The following certificate was selected:
    Issued to: mtgatracker.com
    Issued by: mtgatracker.com
    Expires:   Sun Mar 17 03:15:34 2019
    SHA1 hash: E4200EC8E8C6DECAC50444CC1F621BA8557F840E

Done Adding Additional Store
Successfully signed: C:\Users\Spencatro\PycharmProjects\mtga-tools\MTGATracker-win32-x64\MTGATracker.exe

Number of files successfully Signed: 1
Number of warnings: 0
Number of errors: 0
```

## Contributing

All changes by non core-contributors must go through a code-review process. For non core-contributors, this is accomplished
by forking, making changes in the fork, and submitting PR's.

For more information, see [contributing](https://github.com/mtgatracker/mtgatracker/blob/master/CONTRIBUTING.md)

## Disclaimers

Use MTGA Tracker at your own risk. While it does not seem likely given Wizard's [positive, but unofficial](https://twitter.com/MTGATrackerDevs/status/979112260911034368), comments on MTGATracker, we feel inclined to continue warning users that your account may be banned for using this software. In this unlikely event, MTGA Tracker and its developers
are not responsible.

It is entirely possible that Wizards of the Coast will disable all logging and completely wreck this project. While 
we certainly do not hope this will happen, it is worth noting that this project may stop working at any time. In the
event that this happens, please get in touch with us to verify that what you are experiencing is not a software issue.

In accordance with the [MTG Fan Art policy](http://company.wizards.com/fancontentpolicy): MTGA Tracker is unofficial
Fan Content permitted under the Fan Content Policy. Not approved/endorsed by Wizards. Portions of the materials used
are property of Wizards of the Coast. ©Wizards of the Coast LLC.

Please take a moment to read our [privacy policy](https://github.com/shawkinsl/mtga-tracker/blob/master/legal/privacy.md).
The use of MTGATracker is considered an implicit agreement to this policy.

### Credits, License

MTGA Tracker is built with many free / oss libraries, in general listed in the various manifest files.
MTGA Tracker is mainly built using [JetBrains' PyCharm](https://www.jetbrains.com/pycharm/), [cmder](http://cmder.net/),
[Electron](https://electronjs.org/), [Python](https://www.python.org/), and many other libraries and tools.

Thanks to Auth0 for both the [webtask.io](https://webtask.io) serverless platform, which drives any web-based
interactions MTGA Tracker has, as well as [repo-supervisor](https://github.com/auth0/repo-supervisor), which helps keep
secrets safe, even in an open-source project.

Special shoutout to [fyears/electron-python-example](https://github.com/fyears/electron-python-example) for providing
a truly excellent tutotial to launchpad off of!

MTGA Tracker is licensed under the [MIT License](https://opensource.org/licenses/MIT).

#### Core contributors

If this list gets too heavy, please refer to [contributors/core.yml](https://github.com/shawkinsl/mtga-tracker/blob/master/contributors/core.yml)

- [shawkinsl](https://github.com/shawkinsl)
- MatteyRitch

#### Collaborators

If this list gets too heavy, please refer to [contributors/collab.yml](https://github.com/shawkinsl/mtga-tracker/blob/master/contributors/core.yml)

- emerald000
- isharacomix
- windbagy
