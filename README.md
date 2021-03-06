# Amazon Warehouse Deals Alert

Console utility that scans for new warehouse deals and notifies you about their existence. Even via speech output on Mac OS ;-)

[![License](https://poser.pugx.org/dstuecken/Amazon-WD-Alerts/license)](https://packagist.org/packages/dstuecken/Amazon-WD-Alerts)

## Requirements

* PHP 5.4

## Installation from source 

Clone

```shell
git clone https://github.com/dstuecken/Amazon-WD-Alerts.git
cd Amazon-WD-Alerts
```

Install all dependencies via composer:

```shell
composer install
```

Check https://getcomposer.org/download/ if you don't have composer installed.

## Download binary

Or just use this binary with all dependencies included:

https://github.com/dstuecken/Amazon-WD-Alerts/releases/download/1.0.4/amazon-wd-alerts.zip

# Usage

Mac OS & Linux

```shell
./check.sh B01BWAHNH4 299,99
```

Windows

```shell
cd Amazon-WD-Alerts
c:/path/to/php.exe check.php B01BWAHNH4 299,99
```

Where **B01BWAHNH4** is the article number from Amazon and **299,99** is the maximum price you want to pay.

The price is additional. Leave it blank to just check for the deal itself. So for instance just do:

```shell
./check.sh B01BWAHNH4
```

There is a config called .config.yml where you can define some options like smtp mail notifications or speech output on Mac OS.

### How does it look like?

#### Console notification
![consolenotification](https://cloud.githubusercontent.com/assets/493399/20566057/1fa6ecf6-b194-11e6-8b5c-f9c8d47b1a52.png)
![consolenotification](https://cloud.githubusercontent.com/assets/493399/20594653/5c144f6e-b237-11e6-84ad-175911cf790a.png)

#### Default interval is 30 seconds
![interval](https://cloud.githubusercontent.com/assets/493399/20594652/5c13c756-b237-11e6-9061-5885b1957bcb.png)

#### Mac OS alert notification
![macosnotification](https://cloud.githubusercontent.com/assets/493399/20566058/1fc07338-b194-11e6-90db-40fc23a75942.png)

### .config.yml

There is a configuration file called .config.yml that allows you to set some flexible options:

```shell
mail:                    # mails are sent using swift mailer
  enabled: 0             # 1/0 smtp mailing enabled?
  username: user         # smtp username
  password: pass         # smtp password
  from: user@example.com # from email address
  to: user@example.com   # to email address (recipient)
  subjectPrefix: Warehouse Deal Found # prefix mail subjects with this
  smtp: smtp.gmail.com   # smtp hostname
  port: 25               # smtp port
  security: ssl          # security, blank for unsecure, ssl/tls for secure connections, check swiftmailer documentation for more details
log:
  enabled: 1             # 1/0 logging enabled?
  file: log/system       # log file name
hooks:                   # hooks - leave blank to disable
  shellStart:            # shell command that is executed on starting the script
  shellDeal:             # shell command that is executed if a deal was found | '%TITLE%', '%CONDITION%', '%PRICE%' is replaced by the findings
  shellNoDeal:           # shell command that is executed if there is no deal
options:
  engine: Amazon                    # Name of the Engine to use, e.g. Amazon results in \dstuecken\WdAlerts\Crawler\Amazon\Engine
  updateInterval: 30                # interval in seconds for new requests to amazon
  enableMacOsNotificationHandler: 1 # show growl like notifications in mac os x
  enableMacOsSpeechOutput: 1        # shell_exec say command to enable speech output on mac os
  enableConsoleTextOutput: 1        # write status to console
  startBrowserIfDealFound: 1        # opens your favorite browser and opens your deal if one was found
  useRefLink: 1                     # set this to 0 if you don't want to credit the author :-(
  userAgent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36 # user agent that is used for retrieving the pages
  acceptLanguage: de-DE,de;q=0.8,en-US;q=0.6,en;q=0.4 # Accept-Language header for requests
  proxy:                            # https proxy, e.g. tcp://85.26.146.169:80

```

### FAQ

#### How can i switch from amazon.de to amazon.com?

Check out src/WdAlerts/Crawler/Amazon/config.yml to easily change it from amazon.de to amazon.com so that it looks like this:

```shell
definitions:
  offersUrl: https://www.amazon.com/gp/offer-listing/                                   # Base URL for Amazons new and used items
  searchFor: "#olpOfferList img[alt='Amazon Warehouse Deals']"                          # Search for this css selector within html of offersUrl
  xPathTitle: //*[@id="olpProductDetails"]/div[1]/h1                                    # Search for this XPath to find the title
  xPathPrice: //*[@id="olpOfferList"]/div/div/div[2]/div[1]/span[1]                     # Search for this XPath to find the price
  xPathCondition: //*[@id="olpOfferList"]/div/div/div[2]/div[2]/div[1]/span             # Search for this XPath to find the condition
  xPathDescription: //*[@id="olpOfferList"]/div/div/div[2]/div[2]/div[2]/div[1]/text()  # Search for this XPath to find the condition's description
  locale: de_DE                                                                         # Locale of amazon.de
```

You could also copy the folder `src/WdAlerts/Crawler/Amazon` to `src/WdAlerts/Crawler/AmazonCom`, change it within there and set your Engine to `engine: AmazonCom` in the main configuration file (.config.yml).

#### How do I enable notification under Windows?

Edit the "shellDeal:" Option in .config.yml to something like:

```shell
shellDeal: msg %username% "Your deal %TITLE% is available for %PRICE%! Go get it!"
```
