# Bunny CDN "DRM" Video Downloader

This is a simple Python class that I wrote to download Bunny CDN's "[DRM](https://bunny.net/stream/media-cage-video-content-protection/)" videos using [yt-dlp](https://github.com/yt-dlp/yt-dlp).

## Requirements

You only need to install yt-dlp and then you may integrate it within your script and modify it according to your needs:

```bash
pip install yt-dlp
```

or

```bash
python3 -m pip yt-dlp
```
> It is also better to have [FFmpeg](https://ffmpeg.org) installed on your system.

## Usage

If you're willing to use the script as is, simply paste the the iframe embed page URL at the bottom where indicated between the quotes `""` (structure: [`https://iframe.mediadelivery.net/embed/{video_library_id}/{video_id}`](https://docs.bunny.net/docs/stream-embedding-videos)) as well as the webpage referer and run the script:

```bash
python3 b-cdn-drm-vod-dl.py
```

___

Alternatively, you may want to run this command once:

```bash
chmod +x b-cdn-drm-vod-dl.py
```

after that you can run it this way:

```bash
./b-cdn-drm-vod-dl.py
```

## Expected Result

By default:

* the highest resolution video is to be downloaded. You can change this behavior in the `main_playlist` function located under the `prepare_dl` method.
* The video will be downloaded in the `~/Videos/Bunny CDN/` directory. This configuration can be changed by providing the `path` argument when instantiating a new `BunnyVideoDRM` object.
* The video file name will be extracted from the embed page. This can be overridden by providing the `name` argument .

> Please note that the video format will be always `mp4`.

## Explanation

The idea is all about simulating what's happening in a browser.

The program runs a sequence of requests tied to a [session](https://requests.readthedocs.io/en/latest/user/advanced/#session-objects) first of which is the embed page request from which information are extracted such as the video name.

After that, the download link (that is the HLS/M3U8) is ready to be fed to yt-dlp to download the video segments, decrypt them (since Bunny CDN's "DRM" videos are encrypted with the AES-128 algorithm) and merge them into one playable video.
