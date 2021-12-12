# Detecting Puppeteer-Extra Stealth in Headed Mode

## Demo
**Link:** https://serene-benz-a681c4.netlify.app/

**Password:** superstrong999@#$

## Preview
| Regular Browser | Puppeteer with Stealth |
|-----------------|------------------------|
|<img src="https://user-images.githubusercontent.com/11026445/145713931-80eec91d-42be-423a-99d7-5a70a58ac2bd.png" width="200">|<img src="https://user-images.githubusercontent.com/11026445/145714077-35426b3d-552a-4af2-84dd-49134212b2b4.png" width="200">|

## Signals

### Signal 1:
Signal 1 is a low hanging fruit in Puppeteer Extra Stealth which has not been worked on yet. See it [here](https://github.com/berstend/puppeteer-extra/pull/565).

### Signal 2:
Signal 2 is a chromium bug which allows us to skip evaluateOnNewDocument. It's used in [Amazon Bot Detection Script](https://github.com/chris124567/commercial-bot-detectors/blob/master/files/amazon.js) (See **#_injectIframe_** function)

### Signal 3:
Signal 3 is a standard technique used by CreepJS to detect inconsistencies via worker properties.

### Signal 4:
Signal 4 is a POC on sniffing Function & Object references in native functions.

# Testing
| Device / Browser                                  | S1     | S2     | S3     | S4     |
|---------------------------------------------------|--------|--------|--------|--------|
| M1 iMac / Puppeteer 12.0.1 with Stealth 2.9.0     | failed | failed | failed | failed |
| Samsung Galaxy Note 9 / Chrome 96.0.4664.92       | passed | passed | passed | passed   |
| Samsung Galaxy Note 9 / Samsung Browser 16.0.2.19 | passed | passed | passed | passed   |
| M1 iMac / Chrome 96.0.4664.94                     | passed | passed | passed | passed   |
| M1 iMac / Safari 15.1                             | passed | passed | passed | passed   |
| M1 iMac / Firefox 94.02                           | passed | passed | passed | passed   |
| iPad Pro 2020 / Safari 14                         | passed | passed | failed | passed   |
| iPad Pro 2020 / Chrome 96.0.4664.94               | passed | passed | passed | passed   |
| MBP Pro 15" 2018 / Chrome 96.0.4664.93            | passed | passed | passed | passed   |
| MBP Pro 15" 2018 / Brave 1.32.113                 | passed | passed | passed | passed   |

**Note:** _IPad Pro 2020 / Safari 14_ seems to incorrectly fail on S3 because `navigator.platform` is inconsistent. On that note, CreepJS also has a pretty low trust score for the same device / browser. I'll leave this issue to further investigation in the future.

## Methodology
I followed three main branches of approach to find techniques:
1. Testing each evasion script on commercial bot detectors to find a weak evasion.
2. Reverse engineering some commercial bot detectors that still has some readability.
3. Code-reading Puppeteer-extra stealth and back reading github discussions for some unresolved flaws.

## Personal Notes
1. I feel it's a little hard to detect stealth today than last year because they now use Proxy & a clone of Reflect class.
2. Stealth with only webdriver evasion enabled on a non-virtualized machine seems to work on all sites except ones with Shape Security bot detection (e.g. nordstrom.com). So, I speculate that there must be a way to detect non-tampered Puppeteer instance.
