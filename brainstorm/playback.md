# Playback

Version: 1.0  
Last change: 2022/02/01

## Changelog

### v1.0 (2022/02/01)

* Initial version

## Driving forces

### Clients should be as dumb as possible

The playback logic in clients should be straightforward and easy to implement. In the interest of consistency across various platforms, they should not be making decisions about which files to play or the order to play them in.

### Playback should scale

For performance and scalability, queues should not be handled by the client. Storing very large queues can consume a lot of memory in memory-restricted environments (Smart TVs or low-powered devices like the Raspberry Pi).

### The queue should persist

It can be annoying to close a client in the middle of a show or during a music playlist, and have to find where you were, or start an entire new music playlist and hear the same songs again.

To alleviate this, the playlist should persist somewhere, across client and server restarts.

## General playback flow

Considering all of the above, a sensible playback flow could be visualized as such:

![Playback flow](playback-flow.svg#gh-light-mode-only)
![Playback flow](playback-flow-dark.svg#gh-dark-mode-only)

In order to accommodate for persistence and very large queues, the server would store the clients' queues in the database, along with the parameters used to generate said queue (In order to allow for dynamically adding items to the queue when a client reaches the end of it and the playback should be infinite).

## Playback profiles

In order to determine compatibility of a file with a client, the server needs to know which formats the client can play, and with which limitations (For example, not all clients might be able to handle interlaced video or 24-bit audio).

In keeping with the requirement of clients being as simple as possible, the server should handle all profile detection. This avoids client authors having to (dynamically or statically) build complex (and potentially large) JSON objects to represent their client's capabilities, and potentially long operations (Depending on the format and platform, test files might need to be played in order to confirm compatibility, which is not great).

As we would be storing profiles for DLNA players, this approach also leads to a reduction in code, since we won't need two different paths for determining compatibility.

Ideally, profiles should be provided in two different places:

* Built-in profiles provided with Meteorae
* User-provided profiles

The user-provided profiles should live in a separate directory (to avoid overwrites due to updates) and should override any build-in profiles.

Of course, we should make it easy to submit new profiles and encourage client developers to push their profiles to the server.

For details about the playback profile format, see [playback-profiles.md](playback-profiles.md).
