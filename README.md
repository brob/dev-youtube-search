# Netlify function to index YouTube videos on Algolia

This is a quick script to index (or just grab JSON) for youtube videos.

## Requirements

* [Algolia account](https://algolia.com/signup?utm_source=social&utm_medium=github&utm_campaign=devrel_youtube&utm_id=integrations) with an app and Index set up
* [YouTube Developers API Key](https://developers.google.com/youtube/v3)

## Deploy immediately on Netlify

Don't want to worry about setup? Use the deploy button below, fill in the Environment variables you need, and use the functions.

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/brob/netlify-youtube-ingest)

## Installation

In your Netlify project, create (or use) your Netlify functions directory. If you don't have one, create a new directory:

```sh
mkdir /netlify/functions
```

Put the contents of `netlify/functions/getYoutube.js` into this directory.

Install the dependencies:

```sh
npm install algoliasearch axios dotenv
```

To run locally, use [the Netlify Dev CLI](https://www.netlify.com/products/dev/).

```sh
netlify dev
```

### Environment variables

The function requires environment variables to connect to the YouTube API and an Algolia index.

|variable|use|
|--------|---|
|`YOUTUBE_KEY`| Your API Key for [the YouTube API](https://developers.google.com/youtube/v3)|
|`ALGOLIA_APP_ID`| The Algolia app ID |
|`ALGOLIA_API_KEY`| Your Algolia API key. This needs write permissions to write to the index |
|`VIDEO_INDEX`| The Index name where your search index will be stored in Algolia |
 
## Usage

After installation, you'll have access to the following endpoints on your Netlify site:

`/.netlify/functions/getYoutubeJson`
`/.netlify/functions/getYoutubeByUrl`
## Parameters 

### `getYoutubeJson

```sh
/.netlify/functions/getYoutubeJson
        ?channelID=<your channel id>
        
        &maxResults=<number of results>
        
        &index=<true/false to index>
```

|parameter|type|what it does|
|---|----|---|
| `channelId` | `string` | The ID of the YouTube channel to scrape |
| `maxResults` | `string` | How many results to return (this is ordered by date, so it will get "latest" `n` videos) |
| `index` | `boolean` | if `true` will submit the videos in the list to Algolia (using env variables) if `false` will just display JSON in the body of the response

### `getYoutubeByUrl`

This endpoint is mostly intended to be used with an IFTTT Applet. IFTTT has a Trigger action that is for new YouTube video on a channel. When there's a new video, it can then fire a webhook to this endpoint and provide the URL for the video. The endpoint then gets the video ID from the video and indexes the video content and id.

|parameter|type|what it does|
|-------|----|------|
|`videoUrl` | `string` | The URL to the youtube video (this is the way to get it in IFTTT). This will be used to get the ID of the video|
| `index` | `boolean` | if `true` will submit the videos in the list to Algolia (using env variables) if `false` will just display JSON in the body of the response |
