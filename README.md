wdio-video-reporter [![Build Status](https://travis-ci.org/presidenten/wdio-video-reporter.svg?branch=master)](https://travis-ci.org/presidenten/wdio-video-reporter) [![Code Coverage](https://codecov.io/gh/presidenten/wdio-video-reporter/branch/master/graph/badge.svg)](https://codecov.io/gh/presidenten/wdio-video-reporter)
===================

![Logo](https://raw.githubusercontent.com/presidenten/wdio-video-reporter-example-report/master/wdio-video-reporter.png)

This is a reporter for [Webdriver IO v5](https://webdriver.io/) that generates videos of your wdio test executions. If you use allure, then the test cases automatically get decorated with the videos as well.

Videos ends up in `wdio.config.outputDir`

Checkout example Allure report with included videos on failed tests here:
https://presidenten.github.io/wdio-video-reporter-example-report/

![example-allure-report](https://media.giphy.com/media/7Fgle7bHGrxR3zY6Gw/giphy.gif)

Pros:
- Nice videos in your allure reports
- Nice human speed videos, even though tests are fast
- Works with Selenium grid
- Works with all webdrivers that support `saveScreenshot`
- Verified on the following Deskop browsers using Selenium 3.141.59:
  - Chrome
  - Firefox
  - Safari
  - Internet Explorer 11
  - Microsoft Edge
- Verified on the following ios and android devices with [Appium](http://appium.io/docs/en/about-appium/getting-started/) 1.13.0-beta3:
  - Iphone 8
  - Ipad Gen 6
  - Samsung galaxy S9
  - Samsung galaxy tab A10

Cons:
- Works by taking screenshots after "actions", which makes the tests a little bit slower. This is mitigated by carefully choosing which [jsonWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) messages that should result in a screenshot
- Selenium drivers doesn't include alert-boxes and popups in screenshots, so they are not visible in the videos


Quick start
===========

Checkout the simple template at [wdio-template](https://github.com/presidenten/wdio-template) or the [boilerplate with docker](https://github.com/presidenten/WebdriverIO-wdio-v5-boilerplate-with-videos-and-docker) to quickly get up to speed.

Clone one of the repositories and install dependencies with `yarn` or `npm install`. Then run `yarn e2e` or `npm run e2e` in demo directory and finally `yarn report` or `npm run report` to see allure report.


Installation
============

Install the reporter
--------------------

`yarn add wdio-video-reporter`
or
`npm install wdio-video-reporter`


Add the reporter to config
--------------------------

At the top of the `wdio.conf.js`-file, require the library:
```
const video = require('wdio-video-reporter');
```

Then add the video reporter to the configuration in the reporters propertu:

```
 reporters: [
    [video, {
      saveAllVideos: false,       // If true, also saves videos for successful test cases
      videoSlowdownMultiplier: 3, // Higher to get slower videos, lower for faster videos [Value 1-100]
    }],
  ],
```


Using with Allure
-----------------

Adding the Allure reporter as well, automatically updates the reports with videos without any need to configure anything :-)

```
 reporters: [
    [video, {
      saveAllVideos: false,       // If true, also saves videos for successful test cases
      videoSlowdownMultiplier: 3, // Higher to get slower videos, lower for faster videos [Value 1-100]
    }],
    ['allure', {
      outputDir: './_results_/allure-raw',
      disableWebdriverStepsReporting: true,
      disableWebdriverScreenshotsReporting: true,
    }],
  ],
```


Configuration
=============

Normal configuration parameters
-------------------------------

Most users may want to set these

- `saveAllVideos` Set to true to save videos for passing tests. `Default: false`
- `videoSlowdownMultiplier` Integer between [1-100]. Increase if videos are playing to quick. `Default: 3`
- `videoRenderTimeout` Max seconds to wait for a video to render. `Default: 5`
- `outputDir` If its not set, it uses wdio.config.outputDir. `Default: undefined`


Advanced configuration parameters
---------------------------------

Advanced users who want to change when the engine makes a screengrab can edit these. These arrays may be populated with the last word of a [jsonWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) message, i.e. /session/:sessionId/`buttondown`.

- `addExcludedActions` Add actions where screenshots are unnecessary. `Default: []`
- `addJsonWireActions` Add actions where screenshots are missing. `Default: []`

To see processed messages, set `wdio.config.logLevel: 'debug'` and check `outputDir/wdio-X-Y-Video-reporter.log`. This will also leave the screenshots output directory intact for review


Appium setup
------------

Since `wdio-video-reporter` v1.2.4 there is support to help Allure differentiate between safari and chrome browsers on desktop and devices.
The reporter uses the custom property `deviceType` to id the different devices.
Recommended values are `phone` and `tablet`.
It is recommended to include `browserVersion` as well for _all_ browsers to avoid a bug in Chrome webdriver when using devices in same Selenium grid as desktop Chrome browsers.

The generated video files will also get `deviceType` added to the browser name.

Example appium configuration:
```
  "capabilities": [
    {
      ...
      "deviceType": "phone",
      "browserVersion": "73.0-phone-1",
      ...
    }
  ],
```

And `wdio-config.json`:
```
  capabilities: [
    {
      ...
      'appium:deviceType': 'phone',
      "browserVersion": "73.0-phone-1",
      ...
    },
  ],
```


Contributing
============

Fork, make changes, write some tests, lint, run tests, build, and verify in the demo that changes work as they should, then make a PR.

The demo folder works with the built version of the library, so make sure to build if you added new features and want to try them out.

