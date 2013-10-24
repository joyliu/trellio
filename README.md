# Trellio

Trellio is a simple Sinatra app that hooks up Twilio with Trello to create cards
on a Trello board for voicemails.

## Features

* Posts voicemail recording to Trello card
* Posts voicemail transcription to Trello card
* Play a custom voicemail message

## Dependencies

* MongoDB

## Setup

```bash
$ bundle
```

### Settings

Settings are managed via environment variables:

* `FORWARD_NUMBER` The phone number to attempt to forward calls to
* `RING_TIMEOUT` Number of seconds to ring before going to voicemail
* `MESSAGE_URL` URL to an audio file to playback as voicemail message
* `RECORD_TIMEOUT` Max number of seconds for caller's recorded message

### Trello Integration

1. Grab the ID of the Trello board you'd like to use and set to ENV var
   `TRELLO_BOARD_ID`
1. Get a Trello developer key here: https://trello.com/1/appKey/generate
1. Set that as ENV var `TRELLO_DEVELOPER_PUBLIC_KEY`
1. Run the Sinatra app: `$ ruby app.rb`
1. Navigate to http://localhost:4567/authorize_trello and follow the
   instructions there

### Twilio Integration

1. Setup the "Voice Request URL" for your number to send a `POST` to "/incoming"

## Known Issues

* For very short messages that are transcribed quickly, we can end up with two
  cards in Trello: one for the recording and another for the transcription. Need
  to use some form of blocking queue for each Message to prevent this race
  condition.