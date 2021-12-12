# Detecting Puppeteer-Extra Stealth in Headed Mode

## Website
https://serene-benz-a681c4.netlify.app/ ( PASSWORD: superstrong999@#$ )

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
| Samsung Galaxy Note 9 / Chrome 96.0.4664.92       | passed | passed | passed | todo   |
| Samsung Galaxy Note 9 / Samsung Browser 16.0.2.19 | passed | passed | passed | todo   |
| M1 iMac / Chrome 96.0.4664.94                     | passed | passed | passed | passed |
| M1 iMac / Safari 15.1                             | passed | passed | passed | todo   |
| M1 iMac / Firefox 94.02                           | passed | passed | passed | todo   |
| iPad Pro 2020 / Safari 14                         | passed | passed | failed | todo   |
| iPad Pro 2020 / Chrome 96.0.4664.94               | passed | passed | passed | todo   |
| MBP Pro 15" 2018 / Chrome 96.0.4664.93            | passed | passed | passed | todo   |
| MBP Pro 15" 2018 / Brave 1.32.113                 | passed | passed | passed | todo   |

**Note:** _IPad Pro 2020 / Safari 14_ seems to incorrectly fail on S3. CreepJS also has a pretty low trust score for the same device / browser. I'll leave it to further investigation.

## Methodology
I followed three main branches of approach:
1. Testing each evasion script on commercial bot detectors to find a weak evasion.
1. Reverse engineering some commercial bot detectors with still some readability.
2. Code-reading Puppeteer-extra stealth and back reading github discussions.
  - I feel it's a little hard to detect now because they now use Proxy & a clone of Reflect class.

## Personal Notes
1. Stealth with only webdriver evasion on a non-virtualized machine seems to work on all sites except Shape Security (nordstrom.com).
