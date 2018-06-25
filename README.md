# Teams MediaWiki

This is a extension for [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki) that sends notifications of actions in your Wiki like editing, adding or removing a page into a Microsoft Teams Channel.  I borrowed the majority of this from  the [Slack](https://github.com/kulttuuri/slack_mediawiki) extension.

> There are also extensions that can send notifications to [HipChat](https://github.com/kulttuuri/hipchat_mediawiki) or [Discord](https://github.com/kulttuuri/discord_mediawiki).

![Screenshot](http://i.imgur.com/4SG64a3.jpg)

## Supported MediaWiki operations to send notifications

* Article is added, removed, moved or edited.
* Article protection settings are changed.
* New user is added.
* User is blocked.
* File is uploaded.
* ... and each notification can be individually enabled or disabled :)

## Requirements

* [cURL](http://curl.haxx.se/). This extension also supports using `file_get_contents` for sending the data. See the configuration parameter `$wgTeamsSendMethod` below to change this.
* PHP-cURL extensions
* MediaWiki 1.8+ (tested with version 1.8, also tested and works with 1.25+)
* Apache should have NE (NoEscape) flag on to prevent issues in URLs. By default you should have this enabled.

## How to install

1) Create a new Teams Incoming Webhook. When setting up the webhook, define channel where you want the notifications to go into.

2) After setting up the Webhook you will get a Webhook URL. Copy that URL as you will need it in step 4.

3) [Download latest release of this extension](https://github.com/gswallow/teams_mediawiki/archive/master.zip), uncompress the archive and move folder `TeamsNotifications` into your `mediawiki_installation/extensions` folder.

4) Add settings listed below in your `localSettings.php`. Note that it is mandatory to set these settings for this extension to work:

```php
require_once("$IP/extensions/TeamsNotifications/TeamsNotifications.php");
// Required. Your Teams incoming webhook URL.
$wgTeamsIncomingWebhookUrl = "";
// Required. Name the message will appear to be sent from. Change this to whatever you wish it to be.
$wgTeamsFromName = $wgSitename;
// URL into your MediaWiki installation with the trailing /.
$wgWikiUrl		= "http://your_wiki_url/";
// Wiki script name. Leave this to default one if you do not have URL rewriting enabled.
$wgWikiUrlEnding = "index.php?title=";
// What method will be used to send the data to Teams server. By default this is "curl" which only works if you have the curl extension enabled. This can be: "curl" or "file_get_contents". Default: "curl".
$wgTeamsSendMethod = "curl";
```

5) Enjoy the notifications in your Teams room!
	
## Additional options

These options can be set after including your plugin in your localSettings.php file.

### Customize room where notifications gets sent to

By default when you create incoming webhook at Teams site you'll define which room notifications go into. You can also override this in MediaWiki by setting the parameter below. Remember to also include # before your room name.

```php
$wgTeamsRoomName = "";
```

### Remove additional links from user and article pages

By default user and article links in the nofication message will get additional links for ex. to block user, view article history etc. You can disable either one of those by setting settings below to false.

```php
// If this is true, pages will get additional links in the notification message (edit | delete | history).
$wgTeamsIncludePageUrls = true;
// If this is true, all minor edits made to articles will not be submitted to Teams.
$wgTeamsIgnoreMinorEdits = false;
```

### Set emoji for notification

By default notification in Teams has the default emoji for notification. You can customize this with the setting below. You can find all available emojis from [here](http://www.webpagefx.com/tools/emoji-cheat-sheet/).

```php
$wgTeamsEmoji = "";
```

### Show edit size

By default we show size of the edit. You can hide this information with the setting below.

```php
$wgTeamsIncludeDiffSize = false;
```

### Disable new user extra information

By default we show full name, email and IP address of newly created user in the notification. You can individually disable each of these using the settings below. This is helpful for example in situation where you do not want to expose this information for users in your Teams channel.

```php
// If this is true, newly created user email address is added to notification.
$wgTeamsShowNewUserEmail = true;
// If this is true, newly created user full name is added to notification.
$wgTeamsShowNewUserFullName = true;
// If this is true, newly created user IP address is added to notification.
$wgTeamsShowNewUserIP = true;
```

### Disable notifications from certain user roles

By default notifications from all users will be sent to your Teams room. If you wish to exclude users in certain group to not send notification of any actions, you can set the group with the setting below.

```php
// If this is set, actions by users with this permission won't cause alerts
$wgExcludedPermission = "";
```

### Disable notifications from certain pages / namespaces

You can exclude notifications from certain namespaces / articles by adding them into this array. Note: This targets all pages starting with the name.

```php
// Actions (add, edit, modify) won't be notified to Teams room from articles starting with these names
$wgTeamsExcludeNotificationsFrom = ["User:", "Weirdgroup"];
```

### Actions to notify of

MediaWiki actions that will be sent notifications of into Teams. Set desired options to false to disable notifications of those actions.

```php
// New user added into MediaWiki
$wgTeamsNotificationNewUser = true;
// Article added to MediaWiki
$wgTeamsNotificationAddedArticle = true;
// Article removed from MediaWiki
$wgTeamsNotificationRemovedArticle = true;
// Article moved under new title in MediaWiki
$wgTeamsNotificationMovedArticle = true;
// Article edited in MediaWiki
$wgTeamsNotificationEditedArticle = true;
```
	
## Additional MediaWiki URL Settings

Should any of these default MediaWiki system page URLs differ in your installation, change them here.

```php
$wgWikiUrlEndingUserRights          = "Special%3AUserRights&user=";
$wgWikiUrlEndingBlockUser           = "Special:Block/";
$wgWikiUrlEndingUserPage            = "User:";
$wgWikiUrlEndingUserTalkPage        = "User_talk:";
$wgWikiUrlEndingUserContributions   = "Special:Contributions/";
$wgWikiUrlEndingBlockList           = "Special:BlockList";
$wgWikiUrlEndingEditArticle         = "action=edit";
$wgWikiUrlEndingDeleteArticle       = "action=delete";
$wgWikiUrlEndingHistory             = "action=history";
$wgWikiUrlEndingDiff                = "diff=prev&oldid=";
```

## Setting proxy

To add proxy for requests, you can use the normal MediaWiki way of setting proxy, as described [here](https://www.mediawiki.org/wiki/Manual:$wgHTTPProxy). Basically this means that you just need to set `$wgHTTPProxy` parameter in your `localSettings.php` file to point to your proxy.

## Contributors

[@jacksga](https://github.com/jacksga) [@Meneth](https://github.com/Meneth)

## License

[MIT License](http://en.wikipedia.org/wiki/MIT_License)
