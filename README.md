# Google Analytics Hit Counter

A page views counter that pulls data from Google Analytics API.

## Pre-requisites

To start, you need to first enable [Google Analytics Reporting API](https://developers.google.com/analytics/devguides/reporting/core/v4), and create corresponding credentials.

Follow [this guide](https://developers.google.com/analytics/devguides/reporting/core/v4/quickstart/service-py#1_enable_the_api) to create a service account and get your keys in JSON format. You should find fileds `project_id`, `private_key` and `client_email` in the JSON file downloaded from Developer Console.

## Setup

Install project dependencies:

```sh
$ npm install
```

Copy the config file `config.sample.json` to `config.json` and replace the values inside:

```js
{
  "listenPort": 8000,
  // Maximum amount of queries in a single request
  "maxQueryAmount": 10,
  // TTL of cache for remote API response (in seconds)
  "apiCacheTtl": 3600,
  // Credentials of service account (you can find this in your JSON key file)
  "authorization": {
    "projectId": "<REPLACE_WITH_PROJECT_ID>",
    "privateKey": "-----BEGIN PRIVATE KEY-----\n(...)",
    "clientEmail": "quickstart@PROJECT-ID.iam.gserviceaccount.com"
  },
  "analytics": {
    // View ID of Analytics
    "viewId": "<REPLACE_WITH_VIEW_ID>",
    // To count total pageviews, set an early enough value
    "startDate": "2010-01-01",
    "endDate": "today"
  }
}
```

To find your Google Analytics View ID, navigate to Admin > View > View Settings.

## Usage

```sh
$ npm start
```

The server will listen on port specified in config file.

Make a HTTP GET request to get pageviews (multiple identifiers can be separated by commas):

```sh
$ curl -X GET -H 'Content-Type: application/json' \
  'http://localhost:8000/api/pageviews?pages=foo-bar,test-page'
```

```json
{
  "data": {
    "foo-bar": 114,
    "test-page": 514
  }
}
```

## License

MIT