---
title: Amberscript Transcription API

language_tabs: # must be one of https://git.io/vQNgJ

- javascript: NodeJS
- java: Java
- python: Python
- shell: cURL

toc_footers:

- <a href='https://www.amberscript.com/en/speech-to-text-api'>Sign Up for our Transcription API</a>

includes:

- errors

search: true

code_clipboard: true
---

# Introduction

Welcome to Amberscript's Transcription API

We will provide you with an `API_KEY` required to access our endpoints.

When you [upload](#uploading-a-file) a file, our transcribers or automatic speech recognition engines will get to work
and create a transcript or subtitles.

* Automatic transcripts and subtitles are usually ready within an hour, manually perfected ones take more time,
  dependent on the delivery time chosen.
* Indicate jobType `direct` for automatic and jobType `perfect` for manual jobs
* You can check the status of a job using the [status](#getting-the-status-of-a-transcription) endpoint.
* When the status is `DONE`, a job is finished, and you can download the file using our [export](#export-to-stl)
  endpoints.

<aside class="notice">
    Valid job status values: OPEN, ERROR, DONE
</aside>

## Authentication

<aside class="notice">
    Remember to authenticate requests by providing the <code>apiKey</code> as a query parameter.
</aside>

## Language Codes

| Language                 | Code       |
|--------------------------|------------|
| Afrikaans                | af-za      |
| Albanian                 | sq-al      |
| Amharic                  | am-et      |
| Arabic                   | ar         |
| Armenian                 | hy-am      |
| Azerbaijani              | az-az      |
| Bahasa Indonesia         | id-id      |
| Basque                   | eu-es      |
| Bengali (Bangladesh)     | bn-bd      |
| Bengali (India)          | bn-in      |
| Bosnian                  | bs-ba      |
| Bulgarian                | bg         |
| Burmese                  | my-mm      |
| Catalan                  | ca         |
| Chinese (Mandarin)       | cmn        |
| Croatian                 | hr         |
| Czech                    | cs         |
| Danish                   | da         |
| Dutch                    | nl         |
| English (Australia)      | en-au      |
| English (United Kingdom) | en-uk      |
| English (all accents)    | en         |
| Estonian                 | et-ee      |
| Farsi (Iran)             | fa-ir      |
| Filipino                 | fil-ph     |
| Finnish                  | fi         |
| Flemish                  | nl-be      |
| French (Canada)          | fr-ca      |
| French                   | fr         |
| Galician                 | gl-es      |
| Georgian                 | ka-ge      |
| German (Austria)         | de-at      |
| German (Switzerland)     | de-ch      |
| German                   | de         |
| Greek                    | el         |
| Gujarati                 | gu-in      |
| Hebrew                   | iw-il      |
| Hindi                    | hi         |
| Hungarian                | hu         |
| Icelandic                | is-is      |
| Italian                  | it         |
| Japanese                 | ja         |
| Javanese                 | jv-id      |
| Kannada                  | kn-in      |
| Khmer                    | km-kh      |
| Korean                   | ko         |
| Lao                      | lo-la      |
| Latvian                  | lv         |
| Lithuanian               | lt         |
| Macedonian               | mk-mk      |
| Malay                    | ms         |
| Malayalam                | ml-in      |
| Marathi                  | mr-in      |
| Mongolian                | mn-mn      |
| Nepali (Nepal)           | ne-np      |
| Norwegian                | no         |
| Polish                   | pl         |
| Portuguese (Brazil)      | pt-br      |
| Portuguese               | pt         |
| Punjabi                  | pa-guru-in |
| Romanian                 | ro         |
| Russian                  | ru         |
| Serbian                  | sr-rs      |
| Sinhala                  | si-lk      |
| Slovakian                | sk         |
| Slovenian                | sl         |
| Spanish                  | es         |
| Sundanese                | su-id      |
| Swahili (Kenya)          | sw-ke      |
| Swahili (Tanzania)       | sw-tz      |
| Swedish                  | sv         |
| Tamil (India)            | ta-in      |
| Tamil (Malaysia)         | ta-my      |
| Tamil (Singapore)        | ta-sg      |
| Tamil (Sri Lanka)        | ta-lk      |
| Telugu                   | te-in      |
| Thai                     | th-th      |
| Turkish                  | tr         |
| Ukrainian                | uk-ua      |
| Urdu (India)             | ur-in      |
| Urdu (Pakistan)          | ur-pk      |
| Uzbek                    | uz-uz      |
| Vietnamese               | vi-vn      |
| Zulu                     | zu-za      |

## Uploading A File

```java
HttpResponse<String> response = Unirest.post("https://api.amberscript.com/api/jobs/upload-media?transcriptionType=transcription&jobType=direct&language=nl&apiKey={{YOUR_API_KEY}}")
  .asString();
```

```javascript
const request = require('request');
const fs = require('fs');

let options = {
  method: 'POST',
  url: 'https://api.amberscript.com/api/jobs/upload-media',
  qs: {
    apiKey: 'YOUR_API_KEY',
    transcriptionType: 'transcription',
    jobType: 'direct',
    language: 'nl',
    numberOfSpeakers: '2'
  },
  headers: {'content-type': 'multipart/form-data'},
  formData: {file: fs.createReadStream("./path/to/my_file.mp3")}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = 'https://api.amberscript.com/api/jobs/upload-media'
filepath = '/Users/userA/Downloads/my-file.mp3'
querystring = {"jobType":"direct","language":"nl","transcriptionType":"transcription","apiKey":"YOUR_API_KEY"}
files = {'file': open(filepath, 'rb')}

response = requests.post(url, files=files, verify=False, params=querystring)

print(response.status_code)
print(response.text)
```

```shell
curl --request POST --url 'https://api.amberscript.com/api/jobs/upload-media?transcriptionType=transcription&jobType=direct&language=nl&apiKey={{YOUR_API_KEY}}' --form file=@./my-file.mp3
```

> Example: Response to a POST request:

```json
{
  "jobStatus": {
    "jobId": "JOB_ID",
    "created": 82800000,
    "language": "en",
    "notes": null,
    "status": "OPEN",
    "nrAudioSeconds": 45,
    "transcriptionType": "translatedSubtitles",
    "filename": "file_name.mp3",
    "jobType": "perfect",
    "sourceJobId": "SOURCE_JOB_ID", // only present for translated subtitles jobs.
    "targetLanguage": "nl" // OPTIONAL, translatedSubtitles only
  }
}
```

Upload a file for transcription.

### HTTP Request

`POST /jobs/upload-media`

### Query Parameters

| Parameter                     | Default                                                                        | Description/Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|-------------------------------|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| language                      | `en`                                                                           | `af-za`, `sq-al`, `am-et`, `ar`, `hy-am`, `az-az`, `id-id`, `eu-es`, `bn-bd`, `bn-in`, `bs-ba`, `bg`, `my-mm`, `ca`, `cmn`, `hr`, `cs`, `da`, `nl`, `en-au`, `en-uk`, `en`, `et-ee`, `fa-ir`, `fil-ph`, `fi`, `nl-be`, `fr-ca`, `fr`, `gl-es`, `ka-ge`, `de-at`, `de-ch`, `de`, `el`, `gu-in`, `iw-il`, `hi`, `hu`, `is-is`, `it`, `ja`, `jv-id`, `kn-in`, `km-kh`, `ko`, `lo-la`, `lv`, `lt`, `mk-mk`, `ms`, `ml-in`, `mr-in`, `mn-mn`, `ne-np`, `no`, `pl`, `pt-br`, `pt`, `pa-guru-in`, `ro`, `ru`, `sr-rs`, `si-lk`, `sk`, `sl`, `es`, `su-id`, `sw-ke`, `sw-tz`, `sv`, `ta-in`, `ta-my`, `ta-sg`, `ta-lk`, `te-in`, `th-th`, `tr`, `uk-ua`, `ur-in`, `ur-pk`, `uz-uz`, `vi-vn`, `zu-za` |
| transcriptionType             | `transcription`                                                                | `transcription`, `captions`, `translatedSubtitles`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| jobType                       | `direct`                                                                       | `perfect`, `direct`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| numberOfSpeakers              | `2`                                                                            | `1`, `2`, `3`, `4`, `5`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| callbackUrl (OPTIONAL)        | NONE                                                                           | `YOUR_CALLBACK_URL`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| transcriptionStyle (OPTIONAL) | `cleanread`                                                                    | `cleanread`, `verbatim`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| turnaroundTime (OPTIONAL)     | `FIVE_DAYS` - `transcription`/`captions`, `SEVEN_DAYS` - `translatedSubtitles` | Hint: Get in touch if you need a turnaround time other than the default one.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| targetLanguage (OPTIONAL)     | NONE                                                                           | `pl `, `en`, `ru`, `fr-ca`, `ca`, `zh`, `ga`, `hu`, `pt`, `da`, `de-at`, `fr`, `nl`, `en-au`, `ko`, `it`, `de`, `fi`, `cmn`, `ja`, `de-ch`, `en-us`, `ro`, `pt-br`, `nl-be`, `cs`, `no`, `sv`, `en-uk`, `es`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### Uploading With `callbackUrl`

1. When you make a request with a `callbackUrl`, we send the final status of your upload to this url.
2. When processing is complete, this status is sent via a `POST` request (see below).

- `status` can either be `DONE` or `ERROR`.

3. Your `callbackUrl` endpoint should respond with any `2xx` if you successfully receive the status.

<aside class="notice">
   What if a callback fails?
    <ul>
      <li>When a callback fails, we retry sending the status update every hour.</li>
      <li>The maximum number of retry attempts is 10.</li>
    </ul>
</aside>

There are times when our system can encounter issues with your job. When that is the case, the callback will be sent
with an ERROR status, and the content of the callback will look like this:

```json
{
  "jobId": "JOB_ID",
  "created": 82800000,
  "language": "nl",
  "status": "ERROR",
  "nrAudioSeconds": 45,
  "transcriptionType": "transcription",
  "filename": "file_name.mp3",
  "jobType": "direct",
  "errorMsg": "No speech found"
}
```

Otherwise, you will get a callback informing you that your job was successfully transcribed.

```json
{
  "jobId": "JOB_ID",
  "created": 82800000,
  "language": "nl",
  "status": "DONE",
  "nrAudioSeconds": 45,
  "transcriptionType": "captions",
  "filename": "file_name.mp3",
  "jobType": "direct"
}
```

### Uploading Without `callbackUrl`

1. When you make a request without the `callbackUrl`, store the value of the `jobId` returned upon a successful call (
   see `Example` in right pane).
2. Use the `jobId` to periodically check the [status](#getting-the-status-of-a-transcription) of the upload request (
   e.g. every 5 mins).

- `status` can either be `OPEN`, `DONE` or `ERROR`.

### File requirements:

- max 6GB
- audio or video file
- supported formats: wav, mp3, m4a, aac, wma, mov, m4v, mp4, opus, ogg, flac

_If you need support for a different file format, please get in touch with us: info (at) amberscript (dot) com_

## Request for translated subtitles for an existing manual captions job

```java
HttpResponse<String> response = Unirest.post("https://api.amberscript.com/api/jobs/translatedSubtitles?sourceJobId=SOURCE_JOB_ID&targetLanguage=nl&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
var request = require("request");

var options = {
  method: 'POST',
  url: 'https://api.amberscript.com/api/jobs/translatedSubtitles',
  qs: {sourceJobId: 'SOURCE_JOB_ID', targetLanguage: 'nl', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/translatedSubtitles"

querystring = {"sourceJobId":"SOURCE_JOB_ID","targetLanguage":"nl","apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("POST", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://api.amberscript.com/api/jobs/translatedSubtitles?sourceJobId=SOURCE_JOB_ID&targetLanguage=nl&apiKey=YOUR_API_KEY'
 ```

> The command returns JSON structured like this:

```json
{
  "jobStatus": {
    "jobId": "JOB_ID",
    "created": 82800000,
    "language": "en",
    "status": "BUSY",
    "nrAudioSeconds": 45,
    "transcriptionType": "translatedSubtitles",
    "filename": "file_name.mp3",
    "jobType": "perfect",
    "sourceJobId": "SOURCE_JOB_ID",
    "targetLanguage": "nl",
    "notes": null
  }
}
```

Request for translated subtitles with uploaded file.

### HTTP Request

`POST /jobs/translatedSubtitles`

### Query Parameters

| Parameter                 | Default      | Description/Example                                                                                                                                                                                          |
|---------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sourceJobId               | NONE         | `SOURCE_JOB_ID`                                                                                                                                                                                              |
| targetLanguage            |              | `pl `, `en`, `ru`, `fr-ca`, `ca`, `zh`, `ga`, `hu`, `pt`, `da`, `de-at`, `fr`, `nl`, `en-au`, `ko`, `it`, `de`, `fi`, `cmn`, `ja`, `de-ch`, `en-us`, `ro`, `pt-br`, `nl-be`, `cs`, `no`, `sv`, `en-uk`, `es` |
| turnaroundTime (OPTIONAL) | `SEVEN_DAYS` | Hint: Get in touch if you need a turnaround time other than the default one.                                                                                                                                 |
| callbackUrl (OPTIONAL)    | NONE         | `YOUR_CALLBACK_URL`                                                                                                                                                                                          |

## Getting The Status Of A Transcription

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/status?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/status',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/status"

querystring = {"jobId":"JOB_ID","apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/status?jobId=JOB_ID&apiKey=YOUR_API_KEY'
```

> The above command returns JSON structured like this:

```json
{
  "jobStatus": {
    "jobId": "JOB_ID",
    "created": 82800000,
    "language": "en",
    "status": "OPEN",
    "jobOptions": {
      "transcriptionStyle": "verbatim" // job options are only present if explicitly requests
    },
    "nrAudioSeconds": 45,
    "transcriptionType": "transcription",
    "filename": "file_name.mp3",
    "jobType": "direct",
    "sourceJobId": "SOURCE_JOB_ID",    // OPTIONAL, translatedSubtitles only
    "targetLanguage": "nl" // OPTIONAL, translatedSubtitles only
    "notes": null
  }
}
```

Retrieve the status of a specific job.

### HTTP Request

`GET /jobs/status`

### Query Parameters

| Parameter | Default | Description/Example |
|-----------|---------|---------------------|
| jobId     | NONE    | YOUR_JOB_ID         |

## Exporting A Finished File

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/status?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY', format: 'json'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export"

querystring = {"jobId":"JOB_ID","apiKey":"YOUR_API_KEY","format":"json"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export?jobId=JOB_ID&apiKey=YOUR_API_KEY&format=json'
 ```

> The above command returns JSON structured like this:

```json
{
  "id": "5c9e45057103e464a4c6f477",
  "recordId": "RECORD_ID",
  "speakers": [
    {
      "spkid": "spk1",
      "name": "Speaker 1"
    },
    {
      "spkid": "spk2",
      "name": "Speaker 2"
    },
    {
      "spkid": "spk3",
      "name": "Speaker 3"
    }
  ],
  "segments": [
    {
      "speaker": "spk1",
      "words": [
        {
          "text": "Goedemiddag",
          "start": 2.11,
          "end": 2.68,
          "duration": null,
          "conf": null
        },
        {
          "text": "welkom",
          "start": 2.68,
          "end": 3.01,
          "duration": null,
          "conf": null
        }
      ]
    },
    {
      "speaker": "spk2",
      "words": [
        {
          "text": "Vragen",
          "start": 4.14,
          "end": 4.9,
          "duration": null,
          "conf": null
        },
        {
          "text": "uur.",
          "start": 4.98,
          "end": 5.4,
          "duration": null,
          "conf": null
        }
      ]
    }
  ]
}
```

<aside class="warning">DEPRECATED</aside>

Export a finished file to several formats.

### HTTP Request

`GET /jobs/export`

### Query Parameters

| Parameter                      | Default | Description/Example                                                                                        |
|--------------------------------|---------|------------------------------------------------------------------------------------------------------------|
| jobId                          | NONE    | YOUR_JOB_ID                                                                                                |
| format                         | `json`  | `xml`, `json`, `srt`                                                                                       |
| maxCharsPerSubtitle (OPTIONAL) | 50      | Determines the maximum number of characters per subtitle frame. A subtitle frame has two lines. (srt only) |
| subtitleDurationMax (OPTIONAL) | 4500    | Determines the max duration (in milliseconds) a single subtitle frame should be shown. (srt only)          |

## Export To STL

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/export-stl?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export-stl',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export-stl"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export-stl?jobId=JOB_ID&apiKey=YOUR_API_KEY'
 ```

> The above command returns a JSON downloadUrl structured like this:

```json
{
  "downloadUrl": "https://PROD-SERVER/ebu-stl/FILENAME?X-Amz-Algorithm=HASH_ALGORITHM&X-Amz-Date=CURRENT_DATE&X-Amz-SignedHeaders=host&X-Amz-Expires=TIME_TO_EXPIRATION&X-Amz-Credential=AMZ_CREDENTIAL&X-Amz-Signature=AMZ_SIGNATURE"
}
```

Export a finished file to STL.

### HTTP Request

`GET /jobs/export-stl`

### Query Parameters

| Parameter                             | Default | Description/Example                                                                  |
|---------------------------------------|---------|--------------------------------------------------------------------------------------|
| jobId                                 | NONE    | YOUR_JOB_ID                                                                          |
| maxNumberOfRows (OPTIONAL)            | 2       | Integer between `1` and `2` which sets the maximum number of rows for each subtitle. |
| maxScreenTimePerRowSeconds (OPTIONAL) | 2       | Float which sets the maximum number of seconds given for each row of subtitles.      |

## Export To SRT

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/export-srt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export-srt',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export-srt"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export-srt?jobId=JOB_ID&apiKey=YOUR_API_KEY'
 ```

> The command returns SRT structured like this:

```
1
00:00:00,449 --> 00:00:03,769
Hi, welcome to Amber script, in this short

2
00:00:03,869 --> 00:00:07,400
video, we would like to show you how
you can use Amber script at its full

3
00:00:07,500 --> 00:00:10,189
potential. The first
function we would like to show

4
00:00:10,289 --> 00:00:13,639
you is the edit function.
This allows you to edit errors
```

Export a finished file to SRT.

### HTTP Request

`GET /jobs/export-srt`

### Query Parameters

| Parameter                             | Default | Description/Example                                                                  |
|---------------------------------------|---------|--------------------------------------------------------------------------------------|
| jobId                                 | NONE    | YOUR_JOB_ID                                                                          |
| maxCharsPerRow (OPTIONAL)             | 42      | Integer between `30` and `45` which sets the maximum number of characters per row.   |
| maxNumberOfRows (OPTIONAL)            | 2       | Integer between `1` and `2` which sets the maximum number of rows for each subtitle. |
| maxScreenTimePerRowSeconds (OPTIONAL) | 2       | Float which sets the maximum number of seconds given for each row of subtitles.      |

## Export To VTT

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/export-vtt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export-vtt',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export-vtt"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export-vtt?jobId=JOB_ID&apiKey=YOUR_API_KEY'
```

> The command returns VTT structured like this:

```
WEBVTT - created by www.amberscript.com

1
00:00:00.449 --> 00:00:03.769
Hi, welcome to Amber script, in this short

2
00:00:03.869 --> 00:00:07.400
video, we would like to show you how
you can use Amber script at its full

3
00:00:07.500 --> 00:00:10.189
potential. The first
function we would like to show

4
00:00:10.289 --> 00:00:13.639
you is the edit function.
This allows you to edit errors
```

Export a finished file to VTT.

### HTTP Request

`GET /jobs/export-vtt`

### Query Parameters

| Parameter                             | Default | Description/Example                                                                  |
|---------------------------------------|---------|--------------------------------------------------------------------------------------|
| jobId                                 | NONE    | YOUR_JOB_ID                                                                          |
| maxCharsPerRow (OPTIONAL)             | 42      | Integer between `30` and `45` which sets the maximum number of characters per row.   |
| maxNumberOfRows (OPTIONAL)            | 2       | Integer between `1` and `2` which sets the maximum number of rows for each subtitle. |
| maxScreenTimePerRowSeconds (OPTIONAL) | 2       | Float which sets the maximum number of seconds given for each row of subtitles.      |

## Export To TEXT

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/export-txt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export-txt',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export-txt"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export-txt?jobId=JOB_ID&apiKey=YOUR_API_KEY'
 ```

> The command returns TXT structured like this:

```
00:00:00
Speaker 1: Hi, welcome to Amber script, in this short video, we would like to show you how you can use Amber script at its full potential. The first function we would like to show you is the edit function. This allows you to edit errors
```

Export a finished file to TEXT.

### HTTP Request

`GET /jobs/export-txt`

### Query Parameters

| Parameter                    | Default | Description/Example                                          |
|------------------------------|---------|--------------------------------------------------------------|
| jobId                        | NONE    | YOUR_JOB_ID                                                  |
| includeTimestamps (OPTIONAL) | `true`  | Boolean.                                                     |
| includeSpeakers (OPTIONAL)   | `true`  | Boolean.                                                     |
| highlightsOnly (OPTIONAL)    | `false` | Boolean.                                                     |
| maxCharsPerRow (OPTIONAL)    | NONE    | Integer which sets the maximum number of characters per row. |

## Export To JSON

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs/export-json?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs/export-json',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs/export-json"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs/export-json?jobId=JOB_ID&apiKey=YOUR_API_KEY'
```

> The command returns JSON structured like this:

```json
{
  "id": "5c9e45057103e464a4c6f477",
  "recordId": "RECORD_ID",
  "filename": "FILENAME",
  "startTimeOffset": 0.0,
  "speakers": [
    {
      "spkid": "spk1",
      "name": "Speaker 1"
    }
  ],
  "segments": [
    {
      "speaker": "spk1",
      "words": [
        {
          "start": 0.45,
          "end": 1.08,
          "duration": 0.63000005,
          "text": "Hi,",
          "conf": 1.0,
          "pristine": true
        },
        {
          "start": 1.11,
          "end": 1.65,
          "duration": 0.53999996,
          "text": "welcome",
          "conf": 1.0,
          "pristine": true
        },
        {
          "start": 1.65,
          "end": 1.8,
          "duration": 0.14999998,
          "text": "to",
          "conf": 1.0,
          "pristine": true
        },
        {
          "start": 1.8,
          "end": 2.1,
          "duration": 0.29999995,
          "text": "Amberscript",
          "conf": 0.65,
          "pristine": true
        }
      ]
    }
  ],
  "highlights": []
}
```

Export a finished file to JSON.

### HTTP Request

`GET /jobs/export-json`

### Query Parameters

| Parameter | Default | Description/Example |
|-----------|---------|---------------------|
| jobId     | NONE    | YOUR_JOB_ID         |

## Delete A Job

```java
HttpResponse<String> response = Unirest.delete("https://api.amberscript.com/api/jobs?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'DELETE',
  url: 'https://api.amberscript.com/api/jobs',
  qs: {jobId: 'JOB_ID', apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs"

querystring = {"jobId":"JOB_ID" "apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("DELETE", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request DELETE --url 'https://api.amberscript.com/api/jobs?jobId=JOB_ID&apiKey=YOUR_API_KEY'
```

> The command returns JSON structured like this:

```json
{
  "message": "Job deleted"
}
```

Delete a specific job.

### HTTP Request

`DELETE /jobs`

### Query Parameters

| Parameter | Default | Description/Example |
|-----------|---------|---------------------|
| jobId     | NONE    | YOUR_JOB_ID         |

## Get List Of Jobs

```java
HttpResponse<String> response = Unirest.get("https://api.amberscript.com/api/jobs?apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
 var request = require("request");

var options = {
  method: 'GET',
  url: 'https://api.amberscript.com/api/jobs',
  qs: {apiKey: 'YOUR_API_KEY'}
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```python
import requests

url = "https://api.amberscript.com/api/jobs"

querystring = {"apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/jobs?apiKey=YOUR_API_KEY'
 ```

> The command returns JSON structured like this:

```json
[
  {
    "jobId": "5f686d2d8c996402a02bbb92",
    "created": 1600679213751,
    "language": "nl",
    "status": "DONE",
    "nrAudioSeconds": 59,
    "transcriptionType": "transcription",
    "filename": "test.mp4",
    "jobType": "perfect",
    "jobOptions": {
      "transcriptionStyle": "cleanread" 
    },
    "notes": null
  },
  {
    "jobId": "5f686d068c996402a02bbb85",
    "created": 1600679174451,
    "language": "nl",
    "status": "DONE",
    "nrAudioSeconds": 191,
    "transcriptionType": "transcription",
    "filename": "test.mp4",
    "jobType": "perfect",
    "jobOptions": {
      "transcriptionStyle": "cleanread"
    },
    "notes": null
  },
  {
    "jobId": "5f686d068c996402a02bbb82",
    "created": 1600679174415,
    "language": "nl",
    "status": "DONE",
    "nrAudioSeconds": 59,
    "transcriptionType": "transcription",
    "filename": "test.mp4",
    "jobType": "perfect",
    "jobOptions": {
      "transcriptionStyle": "cleanread"
    },
    "notes": null
  }
]
```

Get a list of jobs.

### HTTP Request

`GET /jobs`

### Query Parameters

| Parameter                    | Default | Description/Example                                               |
|------------------------------|---------|-------------------------------------------------------------------|
| jobId (OPTIONAL)             | NONE    | YOUR_JOB_ID                                                       |
| jobType (OPTIONAL)           | NONE    | Type of the job e.g. `perfect`.                                   |
| status (OPTIONAL)            | NONE    | `OPEN`, `ERROR` or `DONE`.                                        |
| transcriptionType (OPTIONAL) | NONE    | Type of transcription e.g. `transcription`.                       |
| page (OPTIONAL)              | `0`     | Page to be retrieved.                                             |
| pageSize (OPTIONAL)          | `20`    | Number of records to be retrieved for each page (maximum: `100`). |

## Support

If you need any technical assistance, feel free to contact `info (at) amberscript (dot) com`
