# Docker Container for Logitech Media Server

This is a Docker image for running the Logitech Media Server package
(aka SqueezeboxServer).

Run Directly:

    docker run -p 9000:9000 \
               -p 9090:9090 \
               -p 3483:3483 \
               -p 3483:3483/udp \
               -v /etc/localtime:/etc/localtime:ro \
               -v /etc/timezone:/etc/timezone:ro \
               -v <local-state-dir>:/srv/squeezebox \
               -v <audio-dir>:/srv/music \
               --devices /dev/snd \
               larsks/logitech-media-server


The web interface runs on port 9000.  If you also want this available
on port 80 (so you can use `http://yourserver/` without a port number
as the URL), you can add `-p 80:9000`, but you *must* also include `-p
9000:9000` because the players expect to be able to contact the server
on that port.

## Adding a sound device

Use `aplay -L` to get a list of your devices. Add the card and device ID to get the correct identifier (e.g. plughw:1,0 for a USB sound card). Now, in the LMS server interface, install the `WaveInput` plugin, then add a new favorite. Give it a name and as URL "wavin:\<name:,card,device\>" (e.g. `wavin:plughw:1,0`).

Important changes to the docker file are:
- add squeezeboxserver to the `audio` group
- install packages `alsa-utils` and `alsa-tools`

When running the container, add `-device /dev/snd`.

## Using docker-compose

There is a [docker-compose-logitech-media-server.yml][] included in this repository that
you will let you bring up a Logitech Media Server container using
`docker-compose`.  The compose file includes the following:

    volumes:
      - ${AUDIO_DIR}:/srv/music

To provide a value for `AUDIO_DIR`, create a `.env`
file that points `AUDIO_DIR` at the location of your music library,
for example:

    AUDIO_DIR=/home/USERNAME/Music

[docker-compose-logitech-media-server.yml]: docker-compose-logitech-media-server.yml
