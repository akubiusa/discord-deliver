# discord-deliver

Web server to send messages to Discord for use with Docker

## Configuration

`discord-deliver.env`

- `DISCORD_TOKEN`: Discord Bot Token (required)
- `DISCORD_CHANNEL_ID`: Default Discord Channel Id (optional)
  - This value is used if the channel ID is not specified in the URL.

ref: [example directory](example/)

## Usage

The container listens on port 80. Send a JSON `POST` request:

- `POST /`: sends to the channel in `DISCORD_CHANNEL_ID`
- `POST /{channel_id}`: sends to the channel given in the URL (a single numeric path segment). If the URL is not such a value, `DISCORD_CHANNEL_ID` is used instead

```bash
curl -X POST -H "Content-Type: application/json" -d '{"content":"Hello, world!"}' "http://discord-deliver/{channel_id}"
```

Request body (JSON, `Content-Type` must be exactly `application/json`):

- `content`: plain text message
- `embed`: a single Discord embed object

The response is `{"status": true, "response": ...}` on success. On failure it is `{"status": false, "message": ...}` with one of these HTTP status codes:

- 400: empty request body
- 404: the URL has no valid channel ID and `DISCORD_CHANNEL_ID` is not set
- 405: method other than `POST`
- 415: `Content-Type` is not `application/json`
- 500: `DISCORD_TOKEN` is not set
- Any other status returned by the Discord API (such as 401, 403, 429) is passed through, with the Discord response in `response`.

## Warning / Disclaimer

The developer is not responsible for any problems caused by the user using this project.

## License

The license for this project is [MIT License](LICENSE).
