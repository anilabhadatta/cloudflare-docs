---
title: Download videos
weight: 6
---

# Download videos

When you upload a video to Stream, it can be streamed using HLS/DASH. However, for certain use-cases (such as offline viewing), you may want to download the MP4. You can enable MP4 support on a per video basis by following the steps below:

1.  Enable MP4 support by making a POST request to the /downloads endpoint (example below)
2.  Save the MP4 URL provided by the response to the /downloads endpoint. This MP4 URL will become functional when the mp4 is ready (see step 3).
3.  Poll the /downloads endpoint until the `status` field is set to `ready` to inform you when the MP4 is available. You can now use the MP4 URL from step 2.

## Generating downloadable MP4 files for a video

You can enable downloads for an uploaded video once it is ready to view by making an HTTP request to the /downloads endpoint.

To get notified when a video is ready to view, please see the [webhook documentation](/stream/uploading-videos/using-webhooks/#notifications).

The downloads API response will include all available download types for the video, the download URL for each type, and the processing status of the download file.

### cURL example

```bash
curl -X POST \
-H "Authorization: Bearer $TOKEN" \
https://api.cloudflare.com/client/v4/accounts/$ACCOUNT/stream/$VIDEOID/downloads
```

#### Example response

```bash
{
  "result": {
    "default": {
      "status": "inprogress",
      "url": "https://videodelivery.net/$VIDEOID/downloads/default.mp4",
      "percentComplete": 75.0
    }
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

## Getting links to download files

You can view all available downloads for a video by making an `GET` HTTP request to the downloads API. The response for creating and fetching downloads are the same.

### cURL example

```bash
curl -X GET \
-H "Authorization: Bearer $TOKEN" \
https://api.cloudflare.com/client/v4/accounts/$ACCOUNT/stream/$VIDEOID/downloads
```

#### Example response

```bash
{
  "result": {
    "default": {
      "status": "ready",
      "url": "https://videodelivery.net/$VIDEOID/downloads/default.mp4",
      "percentComplete": 100.0
    }
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

## Retrieving the downloads

The generated MP4 download files can be retrieved via the link in the download API response.

### cURL example

```bash
curl -L https://videodelivery.net/$VIDEOID/downloads/default.mp4 > download.mp4
```

## Securing video downloads

If your video is public, the mp4 will also be publicly accessible. If your video is private and requires a signed URL for viewing, the mp4 will not be publicly accessible. To access the mp4 for a private video, you can generate a signed URL just as you would for regular viewing with an additional flag called `downloadable` set to `true`.

Download links will not work for videos which already require signed URLs if the `downloadable` flag is not present in the token.

For more details about using signed URLs with videos, please see [the documentation](/stream/viewing-videos/securing-your-stream/).

### Example token payload

```json
---
highlight: [6]
---
{
    "sub": $VIDEOID,
    "kid": $KEYID,
    "exp": 1537460365,
    "nbf": 1537453165,
    "downloadable": true,
    "accessRules": [
      {
        "type": "ip.geoip.country",
        "action": "allow",
        "country": [
          "GB"
        ]
      },
      {
        "type": "any",
        "action": "block"
      }
    ]
  }
```

## Billing for MP4 Downloads

MP4 *downloads* are billed in the same way as streaming of the video. You will be billed for the duration of the video each time the MP4 for the video is downloaded. For example, if you have a 10 minute video that is downloaded 100 times during the month, the downloads will count as 1000 minutes of minutes served.

You will not incur any additional cost for storage when you enable MP4s.
