[![Xlf2Xlf NPM](https://badge.fury.io/js/xlf2xlf.svg)](https://www.npmjs.com/package/xlf2xlf)

# XLF2XLF (ex. google-translate-xlf)
![XLF2XLF](./logo.png)

A command-line utility to automatically translate .xlf translation files using Google Translate

**DISCLAIMER**

> This library was forked from https://github.com/nfriend/google-translate-xlf

> Some additions to package dependecies were made according to https://www.npmjs.com/package/google-translate-xlf-updated

The main goal to fork and edit original tool was to preserve word and interpolation blocks order while translating original _.xlf_ file, as base library copied this interpolation blocks to the end of _target_ element.

While making this improvements, the original _translate.js_ file eas fully rewritten starting from library we are using to read and build object model of _.xlf_ to ending with how we are parsing object model tree itself and creating queue to translate.

Also most of redundant dependecies were removed, file and folder structure were redefined.

## Installation

This is a command-line utility.  To install this utility, run:

```bash
npm install -g xlf2xlf
```

This will install the utility globally so that it can be run from any location. The executable installed by this module is `xlf2xlf`.
___

You also could run library using **npx** utility.


```bash
npx xlf2xlf [options]
```

## Usage

Before you begin, you will need an `.xlf` file to translate!

If you are using [Angular 2+](https://angular.io/) and the [Angular CLI](https://cli.angular.io/), you will probably extract an `.xlf` file from your app by [running `ng xi18n` inside your Angular project](https://angular.io/guide/i18n#create-a-translation-source-file-with-ng-xi18n).  This produces a file named `messages.xlf` inside your project folder.

Say we have an `.xlf` file named `messages.xlf`.  Let's translate this file from English ("en") to Hindi ("hi") and save the translated file as `messages.hi.xlf`:

```bash
xlf2xlf --in messages.xlf --out messages.hi.xlf --from en --to hi
```

Or, if you don't like typing too much, you can use the abbreviated versions of the arguments:

```bash
xlf2xlf -i messages.xlf -o messages.hi.xlf -f en -t nl
```

For more usage help, run:

```bash
xlf2xlf help
```

> If the utility not installed globally, just use it via npx, e.g. `npx xlf2xlf --in messages.xlf --out messages.hi.xlf --from en --to hi`

## Messages

Tool adds 2 kind of extra messages to final xlf file according to certain criterias:

1. If we are using `skip` option, there will be information messages inside target tags to ease spot of where translation should be placed.

Message:
```
[INFO] Add your translation here
```

2. If there is any errors during quering the translation, there will be warning messages inside problematic target tags to ease spot where things went wrong.

Message:
```
[WARN] Failed to translate
```

## Parameters

| Option          | Default   | Mandatory   | Description                       |
|-----------------|-----------|-------------|-----------------------------------|
| i / in          | -         | true        | The input .xlf file to translate  |
| o / out         | -         | true        | The name of the translated output file  |
| f / from        | -         | true        | The language code of the input file  |
| t / to          | -         | true        | The language code to translate to  |
| r / rate        | 500       | false       | How long to wait after launching a job before launching another one in ms. For more information see [https://github.com/SGrondin/bottleneck#docs](https://github.com/SGrondin/bottleneck)  |
| c / concurrent  | 4         | false       | How many jobs can be executing at the same time. For more information see [https://github.com/SGrondin/bottleneck#docs](https://github.com/SGrondin/bottleneck)   |
| s / skip        | false     | false       | Skips translating and adds only target tag with boilerplate text `[INFO] Add your translation here`  |

## 🤒 Known issues

- The speed of quering / getting response is depends on network quality.

- Sometimes translation could end up with tons of errors like  `{"name":"HTTPError","statusCode":429,"statusMessage":"Too Many Requests"}`, in this case better to wait for some time before next iteration.

In case you are starting to get `{"name":"HTTPError","statusCode":429,"statusMessage":"Too Many Requests"}` you could do the following:

- Wait a bit before Google Translate will stop blocking your IP
- Try to set bigger rate `npx xlf2xlf --in messages.xlf --out messages.hi.xlf --from en --to hi --rate 5000`, where rate is ms between queries. For more information on that property see [https://github.com/SGrondin/bottleneck](https://github.com/SGrondin/bottleneck#docs)
- Try to limit rate and amount of concurrent requests `npx xlf2xlf --in messages.xlf --out messages.hi.xlf --from en --to hi --rate 10000 --concurrent 1`

## Developing

The easiest way to develop on this module is to use test-driven development.

Unit tests can be run with `npm test`.  To automatically re-run the tests when you make changes to the files, run `npm run test:watch`.

## Translation

This utility uses Google Translate's online API for its translations using this NPM package: https://www.npmjs.com/package/@k3rn31p4nic/google-translate-api

## XLIF 1.2 Specification

[http://docs.oasis-open.org/xliff/v1.2/os/xliff-core.html](http://docs.oasis-open.org/xliff/v1.2/os/xliff-core.html)
