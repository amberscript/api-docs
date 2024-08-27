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

Welcome to Amberscript's Transcription API!

The `API_KEY` can be generated on the [Account Page](https://app.amberscript.com/account).
Please contact us if you need to update your balance for processing manual jobs.

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
| English (United States)  | en-us      |
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

# Uploading and status

## Uploading A File

```java
HttpResponse<String> response=Unirest.post("https://api.amberscript.com/api/jobs/upload-media?transcriptionType=transcription&jobType=direct&language=nl&apiKey={{YOUR_API_KEY}}")
  .asString();
```

```javascript
import axios from 'axios';
import FormData from 'form-data';
import fs from 'fs';

async function uploadMedia() {
    const apiKey = 'YOUR_API_KEY';
    const transcriptionType = 'transcription';
    const jobType = 'perfect';
    const language = 'nl';
    const numberOfSpeakers = '2';

    const formData = new FormData();
    formData.append('file', fs.createReadStream('./path/to/my_file.mp3'));

    const url = 'https://api.amberscript.com/api/jobs/upload-media';
    const queryString = new URLSearchParams({
        apiKey,
        transcriptionType,
        jobType,
        language,
        numberOfSpeakers
    }).toString();

    const options = {
        headers: {
            ...formData.getHeaders()
        }
    };

    try {
        const response = await axios.post(`${url}?${queryString}`, formData, options);
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
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
    "sourceJobId": "SOURCE_JOB_ID",
    // only present for translated subtitles jobs.
    "targetLanguage": "nl"
    // OPTIONAL, translatedSubtitles only
  }
}
```

Upload a file for transcription.

### HTTP Request

`POST /jobs/upload-media`

### Query Parameters

| Parameter                     | Default                                                                        | Description/Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-------------------------------|--------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| language                      | `en`                                                                           | `af-za`, `sq-al`, `am-et`, `ar`, `hy-am`, `az-az`, `id-id`, `eu-es`, `bn-bd`, `bn-in`, `bs-ba`, `bg`, `my-mm`, `ca`, `cmn`, `hr`, `cs`, `da`, `nl`, `en-au`, `en-uk`, `en`, `et-ee`, `fa-ir`, `fil-ph`, `fi`, `nl-be`, `fr-ca`, `fr`, `gl-es`, `ka-ge`, `de-at`, `de-ch`, `de`, `el`, `gu-in`, `iw-il`, `hi`, `hu`, `is-is`, `it`, `ja`, `jv-id`, `kn-in`, `km-kh`, `ko`, `lo-la`, `lv`, `lt`, `mk-mk`, `ms`, `ml-in`, `mr-in`, `mn-mn`, `ne-np`, `no`, `pl`, `pt-br`, `pt`, `pa-guru-in`, `ro`, `ru`, `sr-rs`, `si-lk`, `sk`, `sl`, `es`, `su-id`, `sw-ke`, `sw-tz`, `sv`, `ta-in`, `ta-my`, `ta-sg`, `ta-lk`, `te-in`, `th-th`, `tr`, `uk-ua`, `ur-in`, `ur-pk`, `uz-uz`, `vi-vn`, `zu-za`, `auto` |
| transcriptionType             | `transcription`                                                                | `transcription`, `captions`, `translatedSubtitles`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| jobType                       | `direct`                                                                       | `perfect`, `direct`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| numberOfSpeakers              | `2`                                                                            | `-1`, `0`, `1`, `2`, `3`, `4`, `5`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| callbackUrl (OPTIONAL)        | NONE                                                                           | `YOUR_CALLBACK_URL`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| glossaryId (OPTIONAL)         | NONE                                                                           | `YOUR_GLOSSARY_ID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| transcriptionStyle (OPTIONAL - Transcript only) | `cleanread`                                                                    | `cleanread`, `verbatim`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| turnaroundTime (OPTIONAL - Perfect only)     | `FIVE_DAYS` - `transcription`/`captions`, `SEVEN_DAYS` - `translatedSubtitles` | Hint: Get in touch if you need a turnaround time other than the default one.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| targetLanguage (OPTIONAL- TranslatedSubtitle only)     | NONE                                                                           | Automatic languages ('direct')': `da`, `en`, `es`, `fi`, `fr`, `it`, `nl`, `no`, `pt`, `ro`, `ru`, `sv`, `uk`   <br>Manual languages ('perfect'): `pl`, `en`, `ru`, `fr-ca`, `ca`, `zh`, `ga`, `hu`, `pt`, `da`, `de-at`, `fr`, `nl`, `en-au`, `ko`, `it`, `de`, `fi`, `cmn`, `ja`, `de-ch`, `en-us`, `ro`, `pt-br`, `nl-be`, `cs`, `no`, `sv`, `en-uk`, `es`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

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

### Uploading With `glossaryId`

When using a `glossaryId` with your request, you first need to have created a glossary to be
used. [Read here](#glossary) for more information on how to work with glossaries. You can either:

- [Create a new glossary](#create-a-glossary) and use the `id` of the glossary as `glossaryId` in your upload request.

or

- [List your previously created glossaries](#get-a-list-of-glossaries) and pick an `id` of the glossary that you want to
  use as `glossaryId` in your upload request.

### Number of speakers

- If you know the number of speakers present in your audio file, you can define it by using a number between 1 and 5. It is the most accurate approach to recognize each speaker;
- If you do not know the number of speakers, please use the value '0';
- If your file contains several channels and each speaker is assigned to a specific channel, you can use the value '-1'.

### Automatic language identification

You can let the automatic speech recognition engine determine the language of uploaded media automatically. To do so,
you need to set the value of `language` request parameter to `auto`.

- Supported languages: English, Dutch, German, French and Italian

> **Note**: Automatic language predictions can be wrong and manual specification is recommended. If you need support or
> have an issue, please get in touch with us via info@amberscript.com.

### File requirements:

- max 6GB
- audio or video file
- supported formats: `wav`, `mp3`, `m4a`, `aac`, `wma`, `mov`, `m4v`, `mp4`, `opus`, `ogg`, `flac`

_If you need support for a different file format, please get in touch with us: info (at) amberscript (dot) com_

## Uploading A File by URL

Upload a file from a remote location - for example a file stored on AWS S3. The file has to be publicly accessible from
a URL or it has to be presigned download URL.

> Please note that this endpoint uses parameters in the body of the request.

```java
HttpResponse<String> response=Unirest.post("https://api.amberscript.com/api/jobs/upload-media-from-url?apiKey={{YOUR_API_KEY}}")
  .body("{\"sourceUrl\":\"https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4\", \"language\":\"en\", \"transcriptionType\":\"transcription\"}", \"jobType\":\"direct\", \"numberOfSpeakers	\":\"2\")
  .asJson();
```

```javascript
import axios from 'axios';

async function uploadMediaFromUrl() {
    const apiKey = 'YOUR_API_KEY';

    const url = 'https://api.amberscript.com/api/jobs/upload-media-from-url';
    const queryString = new URLSearchParams({
        apiKey
    }).toString();

    const body = {
        sourceUrl: 'https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4',
        transcriptionType: 'transcription',
        jobType: 'direct',
        language: 'nl',
        numberOfSpeakers: '2'
    };

    const options = {
        headers: {
            'content-type': 'application/json'
        }
    };

    try {
        const response = await axios.post(`${url}?${queryString}`, body, options);
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
```

```python
import requests

url = 'https://api.amberscript.com/api/jobs/upload-media-from-url'
query_string = {"apiKey": "YOUR_API_KEY"}
body = {"sourceUrl": "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4", "jobType": "direct",
        "language": "nl", "transcriptionType": "transcription", "numberOfSpeakers": "2"}

response = requests.post(url, verify=False, params=query_string, json=body)

print(response.status_code)
print(response.text)
```

```shell
curl --request POST --url 'https://api.amberscript.com/api/jobs/upload-media-from-url?apiKey={{YOUR_API_KEY}}' \
-H "Content-Type: application/json" \
--data '{"sourceUrl" : "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4","transcriptionType":"transcription","jobType":"direct","language":"nl"}' \
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
    "sourceJobId": "SOURCE_JOB_ID",
    // only present for translated subtitles jobs.
    "targetLanguage": "nl"
    // OPTIONAL, translatedSubtitles only
  }
}
```

### HTTP Request

`POST /jobs/upload-media-from-url`

### Request Body Parameters

| Parameter                     | Default                                                                        | Description/Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-------------------------------|--------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| language                      | `en`                                                                           | `af-za`, `sq-al`, `am-et`, `ar`, `hy-am`, `az-az`, `id-id`, `eu-es`, `bn-bd`, `bn-in`, `bs-ba`, `bg`, `my-mm`, `ca`, `cmn`, `hr`, `cs`, `da`, `nl`, `en-au`, `en-uk`, `en`, `et-ee`, `fa-ir`, `fil-ph`, `fi`, `nl-be`, `fr-ca`, `fr`, `gl-es`, `ka-ge`, `de-at`, `de-ch`, `de`, `el`, `gu-in`, `iw-il`, `hi`, `hu`, `is-is`, `it`, `ja`, `jv-id`, `kn-in`, `km-kh`, `ko`, `lo-la`, `lv`, `lt`, `mk-mk`, `ms`, `ml-in`, `mr-in`, `mn-mn`, `ne-np`, `no`, `pl`, `pt-br`, `pt`, `pa-guru-in`, `ro`, `ru`, `sr-rs`, `si-lk`, `sk`, `sl`, `es`, `su-id`, `sw-ke`, `sw-tz`, `sv`, `ta-in`, `ta-my`, `ta-sg`, `ta-lk`, `te-in`, `th-th`, `tr`, `uk-ua`, `ur-in`, `ur-pk`, `uz-uz`, `vi-vn`, `zu-za`, `auto` |
| transcriptionType             | `transcription`                                                                | `transcription`, `captions`, `translatedSubtitles`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| jobType                       | `direct`                                                                       | `perfect`, `direct`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| numberOfSpeakers              | `2`                                                                            | `-1`, `0`, `1`, `2`, `3`, `4`, `5`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sourceUrl                     | ``                                                                             | "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| callbackUrl (OPTIONAL)        | NONE                                                                           | `YOUR_CALLBACK_URL`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| glossaryId (OPTIONAL)         | NONE                                                                           | `YOUR_GLOSSARY_ID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| transcriptionStyle (OPTIONAL - Transcript only) | `cleanread`                                                                    | `cleanread`, `verbatim`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| turnaroundTime (OPTIONAL - Perfect only)     | `FIVE_DAYS` - `transcription`/`captions`, `SEVEN_DAYS` - `translatedSubtitles` | Hint: Get in touch if you need a turnaround time other than the default one.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| targetLanguage (OPTIONAL- TranslatedSubtitle only)     | NONE                                                                           | Automatic languages ('direct')': `da`, `en`, `es`, `fi`, `fr`, `it`, `nl`, `no`, `pt`, `ro`, `ru`, `sv`, `uk`   <br>Manual languages ('perfect'): `pl`, `en`, `ru`, `fr-ca`, `ca`, `zh`, `ga`, `hu`, `pt`, `da`, `de-at`, `fr`, `nl`, `en-au`, `ko`, `it`, `de`, `fi`, `cmn`, `ja`, `de-ch`, `en-us`, `ro`, `pt-br`, `nl-be`, `cs`, `no`, `sv`, `en-uk`, `es`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


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

### Number of speakers

- If you know the number of speakers present in your audio file, you can define it by using a number between 1 and 5. It is the most accurate approach to recognize each speaker;
- If you do not know the number of speakers, please use the value '0';
- If your file contains several channels and each speaker is assigned to a specific channel, you can use the value '-1'.

### Automatic language identification

You can let the automatic speech recognition engine determine the language of uploaded media automatically. To do so,
you need to set the value of `language` request parameter to `auto`.

- Supported languages: English, Dutch, German, French and Italian

> **Note**: Automatic language predictions can be wrong and manual specification is recommended. If you need support or
> have an issue, please get in touch with us via info@amberscript.com.

### File requirements:

- max 16GB
- audio or video file
- supported
  formats: `wav`, `mp3`, `mp4`, `aac`, `m4a`, `wma`, `opus`, `ogg`, `flac`, `dss`, `avi`, `wmv`, `mov`, `m4v`, `mpg`, `vob`

_If you need support for a different file format, please get in touch with us: info (at) amberscript (dot) com_

## Request translated subtitles for an existing manual ('perfect') captions job

```java
HttpResponse<String> response=Unirest.post("https://api.amberscript.com/api/jobs/translatedSubtitles?sourceJobId=SOURCE_JOB_ID&targetLanguage=nl&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function createTranslatedSubtitles() {
    const sourceJobId = 'SOURCE_JOB_ID';
    const targetLanguage = 'nl';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/translatedSubtitles?sourceJobId=${sourceJobId}&targetLanguage=${targetLanguage}&apiKey=${apiKey}`;

    try {
        const response = await axios.post(url, {}, { headers: { 'content-type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
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
    "notes": "job_notes"
  }
}
```

Request for translated subtitles with uploaded file. This endpoint only work for manual ('perfect') job. 
Translation for Automatic ('direct') captions must be requested during the initial file submission.

### HTTP Request

`POST /jobs/translatedSubtitles`

### Query Parameters

| Parameter                 | Default      | Description/Example                                                                                                                                                                                          |
|---------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sourceJobId               | NONE         | `SOURCE_JOB_ID`                                                                                                                                                                                              |
| targetLanguage            |              | `pl `, `en`, `ru`, `fr-ca`, `ca`, `zh`, `ga`, `hu`, `pt`, `da`, `de-at`, `fr`, `nl`, `en-au`, `ko`, `it`, `de`, `fi`, `cmn`, `ja`, `de-ch`, `en-us`, `ro`, `pt-br`, `nl-be`, `cs`, `no`, `sv`, `en-uk`, `es` |
| turnaroundTime (OPTIONAL) | `SEVEN_DAYS` | Hint: Get in touch if you need a turnaround time other than the default one.                                                                                                                                 |
| callbackUrl (OPTIONAL)    | NONE         | `YOUR_CALLBACK_URL`                                                                                                                                                                                          |
| notes (OPTIONAL)          | NONE         | Job notes                                                                                                                                                                                                    |

## Getting The Status Of A Transcription

```java
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/status?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function fetchJobStatus() {
    const jobId = '6638de0ba17717297470040c';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/status?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
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
      "transcriptionStyle": "verbatim"
      // job options are only present if explicitly requests
    },
    "nrAudioSeconds": 45,
    "transcriptionType": "transcription",
    "filename": "file_name.mp3",
    "jobType": "direct",
    "sourceJobId": "SOURCE_JOB_ID",
    // OPTIONAL, translatedSubtitles only
    "targetLanguage": "nl"
    // OPTIONAL, translatedSubtitles only
    "notes": "job_notes"
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

# Exporting

## Export To STL

<aside class="warning">DEPRECATED</aside>

```java
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/export-stl?jobId=JOB_ID&apiKey=YOUR_API_KEY")
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
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/export-srt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function exportSRT() {
    const jobId = 'JOB_ID';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/export-srt?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
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
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/export-vtt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function exportVTT() {
    const jobId = 'JOB_ID';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/export-vtt?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
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
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/export-txt?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function exportTXT() {
    const jobId = 'JOB_ID';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/export-txt?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
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
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs/export-json?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function exportJSON() {
    const jobId = 'JOB_ID';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs/export-json?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
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

# Listing and deleting jobs

## Delete A Job

```java
HttpResponse<String> response=Unirest.delete("https://api.amberscript.com/api/jobs?jobId=JOB_ID&apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function deleteJob() {
    const jobId = 'JOB_ID';
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs?jobId=${jobId}&apiKey=${apiKey}`;

    try {
        const response = await axios.delete(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
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
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/jobs?apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function listJobs() {
    const apiKey = 'YOUR_API_KEY';

    const url = `https://api.amberscript.com/api/jobs?apiKey=${apiKey}`;

    try {
        const response = await axios.get(url, { headers: { 'Content-Type': 'application/json' } });
        console.log(response.data);
    } catch (error) {
        console.error('Error:', error.response.data)
    }
}
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

# Glossary

You can make use of the glossary feature to help improve the quality of your transcription, captions, and translated
subtitles.
If you already know the names of people/places/organizations, technical terms, or domain-specific terms that are spoken
in the media file, these can be added to the glossary to improve the automatic recognition of these terms. Furthermore,
for manual services, the glossary is used to aid transcribers in perfecting the file.

- The “names” attribute is used to specify names of people, places, organizations etc.
- The “items” attribute is used to add technical or domain-specific terms along with their descriptions. By including
  the description of these terms, you provide more clarity and further help our transcribers in making your
  transcription, captions, or translated subtitles perfect.

Best practices for creating a glossary:

- Add the exact words that should be in the transcription to the glossary. Stems and various forms of words are not
  supported. For example, if the term “subtitling” is spoken in the media file, specify the term “subtitling” in the
  glossary, not the term “subtitle”.
- Add the names in the same casing (lowercase/uppercase) that should appear in the transcription. For example, for a
  name like “Amberscript”, add the name “Amberscript” to the glossary instead of “amberscript”. Similarly, for an
  acronym like “RSVP”, add the name “RSVP” to the glossary, not “rsvp” or “Rsvp”.
- Avoid including regular words in the glossary such as “the”, “of”, “and” etc., unless they are part of a name such as
  “University of Amsterdam”. You can add multiple words as a single entry if they belong together, e.g., “University of
  Amsterdam”.
- Only add uncommon words such as names or technical terms. Common words such as “climate”, “forest” etc. can be
  avoided.
- The same name or term should not be included more than once.

## Get a list of glossaries

```java
HttpResponse<String> response=Unirest.get("https://api.amberscript.com/api/glossary?apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
import fetch from 'node-fetch';

async function getGlossary() {
    const url = 'https://api.amberscript.com/api/glossary';
    const queryParams = new URLSearchParams({
        apiKey: 'YOUR_API_KEY'
    });

    const options = {
        method: 'GET',
        headers: {'Content-Type': 'application/json'},
    };

    try {
        const response = await fetch(`${url}?${queryParams}`, options);
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}

```

```python
import requests

url = "https://api.amberscript.com/api/glossary"

querystring = {"apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("GET", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://api.amberscript.com/api/glossary?apiKey=YOUR_API_KEY'
 ```

> The command returns JSON structured like this:

```json
[
  {
    "id": "5f686d068c996402a02bbb85",
    "userName": "YOUR_USERNAME",
    "name": "Name of first glossary",
    "names": [
      "Test name one",
      "Test name two"
    ],
    "items": [
      {
        "name": "Item one name",
        "description": "description of item one"
      },
      {
        "name": "Item two name",
        "description": "description of item two"
      }
    ],
    "created": 1600679174451
  },
  {
    "id": "5f686d068c996402a02bbb86",
    "userName": "YOUR_USERNAME",
    "name": "Name of second glossary",
    "names": [
      "Test name one",
      "Test name two"
    ],
    "items": [
      {
        "name": "Item one name",
        "description": "description of item one"
      },
      {
        "name": "Item two name",
        "description": "description of item two"
      }
    ],
    "created": 1600679175000
  }
]
```

Get a list of glossaries.

### HTTP Request

`GET /glossary`

### Query Parameters

| Parameter                | Default   | Description/Example                                      |
|--------------------------|-----------|----------------------------------------------------------|
| sortBy (OPTIONAL)        | "created" | Field by which to sort the resulting list of glossaries. |
| sortDirection (OPTIONAL) | `DESC`    | `DESC` or `ASC`                                          |

## Create a glossary

```java
String body="{\n"+
  "  \"name\": \"Name of first glossary\",\n"+
  "  \"names\": [\n"+
  "    \"Test name one\",\n"+
  "    \"Test name two\"\n"+
  "  ],\n"+
  "  \"items\": [\n"+
  "    {\n"+
  "      \"name\": \"Item one name\",\n"+
  "      \"description\": \"description of item one\"\n"+
  "    },\n"+
  "    {\n"+
  "      \"name\": \"Item two name\",\n"+
  "      \"description\": \"description of item two\"\n"+
  "    }\n"+
  "  ]\n"+
  "}";
  HttpResponse<String> response=Unirest.post("https://api.amberscript.com/api/glossary?apiKey=YOUR_API_KEY")
  .header("Content-Type","application/json")
  .body(body)
  .asString();
```

```javascript
async function createGlossary() {
    const url = 'https://api.amberscript.com/api/glossary';
    const body = {
        "name": "Name of first glossary",
        "names": [
            "Test name one",
            "Test name two"
        ],
        "items": [
            {
                "name": "Item one name",
                "description": "description of item one"
            },
            {
                "name": "Item two name",
                "description": "description of item two"
            }
        ]
    };

    const options = {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(body)
    };

    const queryParams = new URLSearchParams({
        apiKey: 'YOUR_API_KEY'
    });

    try {
        const response = await fetch(`${url}?${queryParams}`, options);
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
```

```python
import requests

url = "https://api.amberscript.com/api/glossary"

querystring = {"apiKey":"YOUR_API_KEY"}

payload = "{\n" \
          "  \"name\": \"Name of first glossary\",\n" \
          "  \"names\": [\n" \
          "    \"Test name one\",\n" \
          "    \"Test name two\"\n" \
          "  ],\n" \
          "  \"items\": [\n" \
          "    {\n" \
          "      \"name\": \"Item one name\",\n" \
          "      \"description\": \"description of item one\"\n" \
          "    },\n" \
          "    {\n" \
          "      \"name\": \"Item two name\",\n" \
          "      \"description\": \"description of item two\"\n" \
          "    }\n" \
          "  ]\n" \
          "}"

headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, data=payload, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://api.amberscript.com/api/glossary?apiKey=YOUR_API_KEY' --header 'Content-Type: application/json' --data-raw '{
    "name":"Name of first glossary",
    "names": [
      "Test name one",
      "Test name two"
    ],
    "items":[
        {
            "name":{{glossaryItem1Name}},
            "description":{{glossaryItem1Description}}
        },
        {
            "name":"{{glossaryItem2Name}}",
            "description":{{glossaryItem2Description}}
        }
    ]
}'
 ```

> The command returns JSON structured like this:

```json
{
  "id": "5f686d068c996402a02bbb85",
  "userName": "YOUR_USERNAME",
  "name": "Name of first glossary",
  "names": [
    "Test name one",
    "Test name two"
  ],
  "items": [
    {
      "name": "Item one name",
      "description": "description of item one"
    },
    {
      "name": "Item two name",
      "description": "description of item two"
    }
  ],
  "created": 1600679174451
}
```

Create a glossary.

### HTTP Request

`POST /glossary`

### Body JSON

### UserGlossary

| Attribute | Type                                                                           | Description                                                                       | Required |
|-----------|--------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|----------|
| name      | string <br><small>maxLength: 60</small>                                        | Name of the glossary.                                                             | Yes      |
| names     | [string] <br><small>maxItems: 250</small> <br><small>maxItemLength: 20</small> | Array of names.                                                                   | No       |
| items     | [object] <br><small>maxItems: 750</small>                                      | Array of glossary items. **GlossaryItem** format is described in the table below. | No       |

### GlossaryItem

| Attribute        | Type                                     | Description                    | Required |
|------------------|------------------------------------------|--------------------------------|----------|
| item.name        | string <br><small>maxLength: 60</small>  | Term which is being described. | Yes      |
| item.description | string <br><small>maxLength: 200</small> | Description of the term.       | No       |

## Update a glossary

```java
String body="{\n"+
  "  \"name\": \"Name of first glossary\",\n"+
  "  \"names\": [\n"+
  "    \"Test name one\",\n"+
  "    \"Test name two\"\n"+
  "  ],\n"+
  "  \"items\": [\n"+
  "    {\n"+
  "      \"name\": \"Item one name\",\n"+
  "      \"description\": \"description of item one\"\n"+
  "    },\n"+
  "    {\n"+
  "      \"name\": \"Item two name\",\n"+
  "      \"description\": \"description of item two\"\n"+
  "    }\n"+
  "  ]\n"+
  "}";
  HttpResponse<String> response=Unirest.put("https://api.amberscript.com/api/glossary/GLOSSARY_ID?apiKey=YOUR_API_KEY")
  .header("Content-Type","application/json")
  .body(body)
  .asString();
```

```javascript
async function updateGlossary() {
    const url = 'https://api.amberscript.com/api/glossary/GLOSSARY_ID';
    const body = {
        "name": "Name of first glossary",
        "names": [
            "Test name one",
            "Test name two"
        ],
        "items": [
            {
                "name": "Item one name",
                "description": "description of item one"
            },
            {
                "name": "Item two name",
                "description": "description of item two"
            }
        ]
    };

    const queryParams = new URLSearchParams({
        apiKey: 'YOUR_API_KEY'
    });

    const options = {
        method: 'PUT',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(body)
    };

    try {
        const response = await fetch(`${url}?${queryParams}`, options);
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
```

```python
import requests

url = "https://api.amberscript.com/api/glossary/GLOSSARY_ID"

querystring = {"apiKey":"YOUR_API_KEY"}

payload = "{\n" \
          "  \"name\": \"Name of first glossary\",\n" \
          "  \"names\": [\n" \
          "    \"Test name one\",\n" \
          "    \"Test name two\"\n" \
          "  ],\n" \
          "  \"items\": [\n" \
          "    {\n" \
          "      \"name\": \"Item one name\",\n" \
          "      \"description\": \"description of item one\"\n" \
          "    },\n" \
          "    {\n" \
          "      \"name\": \"Item two name\",\n" \
          "      \"description\": \"description of item two\"\n" \
          "    }\n" \
          "  ]\n" \
          "}"

headers = {
  'Content-Type': 'application/json'
}

response = requests.request("PUT", url, data=payload, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request PUT --url 'https://api.amberscript.com/api/glossary/GLOSSARY_ID?apiKey=YOUR_API_KEY' --header 'Content-Type: application/json' --data-raw '{
    "name":"Name of first glossary",
    "names": [
      "Test name one",
      "Test name two"
    ],
    "items":[
        {
            "name":{{glossaryItem1Name}},
            "description":{{glossaryItem1Description}}
        },
        {
            "name":"{{glossaryItem2Name}}",
            "description":{{glossaryItem2Description}}
        }
    ]
}'
 ```

> The command returns JSON structured like this:

```json
{
  "id": "5f686d068c996402a02bbb85",
  "userName": "YOUR_USERNAME",
  "name": "Name of first glossary",
  "names": [
    "Test name one",
    "Test name two"
  ],
  "items": [
    {
      "name": "Item one name",
      "description": "description of item one"
    },
    {
      "name": "Item two name",
      "description": "description of item two"
    }
  ],
  "created": 1600679174451
}
```

Update a specific glossary.

### HTTP Request

`PUT /glossary/GLOSSARY_ID`

### Body JSON

### UserGlossary

| Attribute | Type                                                                           | Description                                                                       | Required |
|-----------|--------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|----------|
| name      | string <br><small>maxLength: 60</small>                                        | Name of the glossary.                                                             | Yes      |
| names     | [string] <br><small>maxItems: 250</small> <br><small>maxItemLength: 20</small> | Array of names.                                                                   | No       |
| items     | [object] <br><small>maxItems: 750</small>                                      | Array of glossary items. **GlossaryItem** format is described in the table below. | No       |

### GlossaryItem

| Attribute        | Type                                     | Description                    | Required |
|------------------|------------------------------------------|--------------------------------|----------|
| item.name        | string <br><small>maxLength: 60</small>  | Term which is being described. | Yes      |
| item.description | string <br><small>maxLength: 200</small> | Description of the term.       | No       |

## Delete a glossary

```java
HttpResponse<String> response=Unirest.delete("https://api.amberscript.com/api/glossary/GLOSSARY_ID?apiKey=YOUR_API_KEY")
  .asString();
```

```javascript
async function deleteGlossary() {
    const url = 'https://api.amberscript.com/api/glossary/6638e42835d0580edd5e35c2';
    const queryParams = new URLSearchParams({
        apiKey: 'YOUR_API_KEY'
    });

    const options = {
        method: 'DELETE',
        headers: {'Content-Type': 'application/json'},
    };

    try {
        const response = await fetch(`${url}?${queryParams}`, options);
        const data = await response.text();
        console.log(data);
    } catch (error) {
        console.error('Error:', error.response.data);
    }
}
```

```python
import requests

url = "https://api.amberscript.com/api/glossary/GLOSSARY_ID"

querystring = {"apiKey":"YOUR_API_KEY"}

payload = ""
response = requests.request("DELETE", url, data=payload, params=querystring)

print(response.text)
```

```shell
curl --request DELETE --url 'https://api.amberscript.com/api/glossary/GLOSSARY_ID?apiKey=YOUR_API_KEY'
 ```

Delete a specific glossary.

### HTTP Request

`DELETE /glossary/GLOSSARY_ID`

# Support

If you need any technical assistance, feel free to contact `info (at) amberscript (dot) com`
