# Automated Bucks Cal

Scrape Starbucks Shifts and Parse into Google Calendar

## Setup

[NodeJs Calendar Quickstart](https://developers.google.com/calendar/quickstart/nodejs)
[Create Package.json](https://docs.npmjs.com/cli/init)

```bash
npm init
npm install googleapis --save
npm install superagent --save
npm install puppeteer --save
```

[Create API Credentials](https://console.developers.google.com/apis/credentials?project=automatecalendar) and download `credentials.json`

#### Missing Files

The following files are removed from source control via the gitingore

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

## Scraper

```js
https://mysite.starbucks.com/MySchedule/Schedule.aspx

var shifts = $(".scheduleShift").map(function() {
   var $this = $(this)
   var $store = $this.find(".scheduleShiftStore");
   var $time = $this.find(".scheduleShiftTime");
   var day = $this.closest(".scheduleDayRight").find(".scheduleDayTitle").text()
   var storeLink = $store.attr("href")
   var storeText = $store.text()
   var storeNumber = storeText.split(",")[0].split("#")[1].trim()
   var storeName = storeText.split(",").slice(1).join(",").trim()
   var shiftText = $time.text().trim();
   var shiftStart = shiftText.split("-")[0].trim()
   var shiftEnd = shiftText.split("-")[1].trim()
   return {
      storeNumber,
      storeName,
      day,
      shiftStart,
      shiftEnd
   }
}).get();
```

## Help / Docs / Resources

### Node APIs

* [`fs.ReadFile()`](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback)
* [`fs.WriteFile()`](https://nodejs.org/api/fs.html#fs_fs_writefile_file_data_options_callback)
* [`readline.createInterface(options)`](https://nodejs.org/api/readline.html#readline_readline_createinterface_options)
* [`readline.question()`](https://nodejs.org/api/readline.html#readline_rl_question_query_callback)
* [`readline.close()`](https://nodejs.org/api/readline.html#readline_rl_close)

### Google APIs

* [Google API Node Client](https://github.com/googleapis/google-api-nodejs-client)
* [Callbacks -> Promises -> Async/Await](https://github.com/googleapis/google-api-nodejs-client#first-example)
* [Request Body](https://github.com/googleapis/google-api-nodejs-client#specifying-request-body)

### Visual Studio Code

* [Debugging in VS Code](https://code.visualstudio.com/docs/editor/debugging)
* [Debug Node in VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

### Calendar API

* [Create Events](https://developers.google.com/calendar/create-events)
* [Calendar Auth](https://developers.google.com/calendar/auth)
* [Calendar Overview](https://developers.google.com/calendar/v3/reference/)
* [`CalendarList.List`](https://developers.google.com/calendar/v3/reference/calendarList/list)
* [Events Overview](https://developers.google.com/calendar/v3/reference/events)
* [`Events.List`](https://developers.google.com/calendar/v3/reference/events/list)
* [`Events.Insert`](https://developers.google.com/calendar/v3/reference/events/insert)
* [`Events.Delete`](https://developers.google.com/calendar/v3/reference/events/delete)

### Node

* [Update Package Version](https://stackoverflow.com/q/16073603/1366033)
* [Node Config Files](https://stackoverflow.com/a/14678694/1366033)

### Puppetteer

* [Delay / Sleep / Wait](https://stackoverflow.com/a/46965281/1366033)

### Promises Async

* [How do I convert an existing callback API to promises?](https://stackoverflow.com/q/22519784/1366033)
* [JS Callbacks to Promises](https://medium.com/@samthor/js-callbacks-to-promises-541adc46c07c)
* [How to make a Promise out of a Callback function in JavaScript](https://medium.freecodecamp.org/how-to-make-a-promise-out-of-a-callback-function-in-javascript-d8ec35d1f981)
* [Easier Error Handling Using Async/Await](https://medium.com/@jesterxl/easier-error-handling-using-async-await-b9ab0cb938e)