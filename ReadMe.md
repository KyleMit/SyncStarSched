# Automated Bucks Cal

Scrape Starbucks Shifts and Sync into Google Calendar

## Setup

[NodeJs Calendar Quickstart](https://developers.google.com/calendar/quickstart/nodejs)

```bash
npm init
npm install googleapis --save
npm install superagent --save
npm install puppeteer --save
npm install moment --save
```

[Create API Credentials](https://console.developers.google.com/apis/credentials?project=automatecalendar) and download `credentials.json`

### Missing Files

The following files are removed from source control via the `.gitingore`

```
credentials.json
token.json
secrets.json
```

**Credentials** can be obtained by downloading form [Github API Console](https://console.developers.google.com/)

**Token** is generated and saved to the local file system by running the app and granting access to the developer credentials for a particular user and scope.

**Secrets** contains the unique information needed to log on to your account and obtain your schedule information and pass along to your calendar.  It should have the following format:

```json
{
    "partnerId": "?",
    "password": "?",
    "securityPrompts": {
        "What was your favorite childhood game?" : "?",
        "What city do you grow up in?" : "?"
    }
}
```

## Run

```bash
node .
```

On first run, node terminal with prompt for OAuth credentials.  Follow the link, authorize the app, copy the auth code, and the token will be saved to `.\token.json`

## Features

1. Login
2. Scrape
3. Parse (moment.js)
4. Google OAuth
5. Retrieve All events and compare
6. Insert New events
7. Delete Removed Events in the future
8. Run on schedule (Azure Functions)
9. Send Email alert when new events added

## Config Settings

* OAuth Google Sign In OR API Key
* Starbucks Sign In & Security Questions
* Calendar ID - One Time Setup  - use SBux Color

## Modules

* [file system (fs)](https://nodejs.org/api/fs.html)
* [node readline](https://nodejs.org/api/readline.html)

## Event Body Format

```js
{  
  'summary': 'Starbucks Shift',
  'location': '49 Church St #2072, Burlington, VT 05401',
  'description': 'Automatically added from https://mysite.starbucks.com/MySchedule/Schedule.aspx.',
  'start': {
    'dateTime': '2018-12-28T12:00:00-05:00',
    'timeZone': 'America/New_York'
  },
  'end': {
    'dateTime': '2018-12-28T17:00:00-05:00',
    'timeZone': 'America/New_York'
  },
  'reminders': {
    'useDefault': false,
    'overrides': [
      {'method': 'popup', 'minutes': 60 * 4}
      {'method': 'popup', 'minutes': 60}
      {'method': 'popup', 'minutes': 15}
    ]
  }
}
```
