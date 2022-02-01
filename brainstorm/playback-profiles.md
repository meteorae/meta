# Playback Profiles

Version: 1.0  
Last change: 2022/02/01

## Changelog

### v1.0 (2022/02/01)

* Initial version

## Existing implementations

### Plex

Plex provides DLNA profiles in the form of XML files.

It is unclear how non-DLNA clients declare their compatibilities, if at all.

#### Format

XML files containing the following main categories:

* **Name:** how Plex Media Server refers to the profile. Must be identical to the profile’s file name.
* **Identification:** how Plex Media Server recognizes a device, given an incoming HTTP request.
* **Protocol Info:** how Plex Media Server reports its own DLNA protocol support to devices.
* **Device Description:** how Plex Media Server identifies itself to other devices during DLNA network discovery.
* **Settings:** configuration options that change the DLNA server’s behavior when talking to a particular device.
* **Transcode Targets:** the video, music and photo media formats to which PMS will transcode content devices can’t play natively.
* **DirectPlay Profiles:** the set of video, music and photo formats the device can play natively.
* **Codec Profiles:** settings and limitations on the video, music and photo formats that the device can play natively.
* **Container Profiles:** settings and limitations on the container formats that the device can play natively.
* **Transcode Target Profiles:** settings for transcode targets.
  DLNA Media Profiles: customization of media format representations for very picky DLNA devices.

#### Handling of unknown devices

Plex maps unknown devices to the "Generic" profile, which does not specify any limitation. As such, unknown clients will simply play any files directly.

#### Codec names

Mention is made of the following codecs (non-exhaustive list):

```
container = { asf, avi, mov, mp4, mkv, mpegts, wtv, mpeg, m4v }
video = { h264, mpeg1video, mpeg2video, mpeg4, msmpeg4, mjpeg, wmv2, wmv3, vc1, cinepak, h263 }
audio = { aac, ac3, truehd, eac3, dca, mp3, mp2, pcm, wmapro, wmav2, wmavoice, wmalossless }
```

It seems fairly similar to the formats used by ffmpeg.

#### DLNA-specifics

It is possible to refine matching with the properties they return after a search request.

#### Supported protocols

Built-in profiles provided by Plex as of 2020-02-01 are: http, hls and dash.

* **http:** refers to serving the file directly via HTTP for directly playback
* **hls:** refers to serving the file via [HTTP Live Streaming](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)
* **dash:** refers to serving the file via [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)

#### References

[Writing profiles for DLNA devices (Plex Forum)](https://forums.plex.tv/t/writing-profiles-for-dlna-devices/38060)

### Jellyfin/Emby

Jellyfin and Emby feature DLNA profiles derived from Plex's profiles, with a similar format (Some changes have been made by Emby for unknown reasons).

Most (if not all) of the fields and values are identical.

Non-DLNA clients declare their capabilities by sending a large JSON object containing their playback profiles, made of similar sections to the Plex DLNA profiles described above.

### Olaris

Olaris does client-side detection. On the web, it seems to only support mp4-based video formats.

It does not seem to have any server-side profiles or DLNA support.

### Dim

Does not seem to do any format detection at this point in time.

## Driving-forces

### Profiles should be easy to write and well-documented

Having to generate a complex object like Emby/Jellyfin leads to frustration in client developers. It's difficult to debug, annoying to write and can't be reused across clients.

The profile format needs to be human-readable, well-documented and should dieally support automated validation.

### Profiles should support dedicated clients and DLNA

While we likely won't support DLNA for a while, we should still plan for it.

Profiles should either be divided into DLNA and clients, or have fields for both.
