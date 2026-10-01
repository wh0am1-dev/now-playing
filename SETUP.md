# setup

## spotify

- create a [Spotify Application](https://developer.spotify.com/dashboard/applications)
- take note of:
  - `client id`
  - `client secret`
- click on **edit settings**
- in **redirect uris**:
  - add `http://localhost/callback/`
- navigate to the following url:

<details>

<summary>⚠️ localhost is no longer allowed !</summary>

```
https://accounts.spotify.com/authorize?client_id={SPOTIFY_CLIENT_ID}&response_type=code&scope=user-read-currently-playing,user-read-recently-played&redirect_uri=http://localhost/callback/
```

</details>

```
https://accounts.spotify.com/authorize?client_id={SPOTIFY_CLIENT_ID}&response_type=code&scope=user-read-currently-playing,user-read-recently-played&redirect_uri=http://127.0.0.1:80/callback/
```

- after logging in, save the `{CODE}` portion of: `http://localhost/callback/?code={CODE}`
- create a string combining `{SPOTIFY_CLIENT_ID}:{SPOTIFY_CLIENT_SECRET}` (e.g. `5n7o4v5a3t7o5r2e3m1:5a8n7d3r4e2w5n8o2v3a7c5`) and **encode** into [base64](https://base64.io)
- then run a [curl command](https://httpie.org/run) in the form of:

<details>

<summary>⚠️ localhost is no longer allowed !</summary>

```sh
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Authorization: Basic {BASE64}" -d "grant_type=authorization_code&redirect_uri=http://localhost/callback/&code={CODE}" https://accounts.spotify.com/api/token
```

</details>

```sh
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Authorization: Basic {BASE64}" -d "grant_type=authorization_code&redirect_uri=http://127.0.0.1:80/callback/&code={CODE}" https://accounts.spotify.com/api/token
```

- save the Refresh token

## vercel

- register on [Vercel](https://vercel.com/)
- fork this repo, then create a vercel project linked to it
- add Environment Variables:
  - `https://vercel.com/<YourName>/<ProjectName>/settings/environment-variables`
    - `SPOTIFY_REFRESH_TOKEN`
    - `SPOTIFY_CLIENT_ID`
    - `SPOTIFY_SECRET_ID`
- deploy!

## readme

you can now use the following in your readme:

```[![spotify](https://APP_NAME.vercel.app/api/spotify)](https://open.spotify.com/user/USER_NAME)```

## customization

if you want a distinction between the widget showing your currently playing, and your recently playing:

### hide the eq bar

remove the `#` in front of `contentBar` in [line 81](https://github.com/novatorem/novatorem/blob/98ba4a8489ad86f5f73e95088e620e8859d28e71/api/spotify.py#L81) of current master, then the EQ bar will be hidden when you're in not currently playing anything

### status string

have a string saying either "Vibing to:" or "Last seen playing:"

- change [`height` to `height + 40`](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L1-L2) (or whatever `margin-top` is set to)
- uncomment [**.main**'s `margin-top`](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L10)
- uncomment [currentStatus](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L93)
