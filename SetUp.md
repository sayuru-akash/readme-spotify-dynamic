# Set Up

This project supports two music services: **Spotify** and **Last.fm**. You only need to configure one of them.

- If **both** are configured, Spotify takes priority.
- If only **Spotify** is configured, Spotify will be used.
- If only **Last.fm** is configured, Last.fm will be used.

## Quick Start

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
2. Fill in the environment variables for your chosen service (see below)
3. Deploy or run locally

---

## Option 1: Spotify API (Recommended)

### Spotify API App

- Create a [Spotify Application](https://developer.spotify.com/dashboard/applications)
- Take note of:
  - `Client ID`
  - `Client Secret`
- Click on **Edit Settings**
- In **Redirect URIs**:
  - Add `https://example.com/callback`

# Refresh Token

### Powershell

<details>

<summary>Script to complete this section</summary>

```powershell
$ClientId = Read-Host "Client ID"
$ClientSecret = Read-Host "Client Secret"

Start-Process "https://accounts.spotify.com/authorize?client_id=$ClientId&response_type=code&scope=user-read-currently-playing,user-read-recently-played&redirect_uri=https://example.com/callback"

$Code = Read-Host "Please insert everything after 'https://example.com/callback?code='"

$ClientBytes = [System.Text.Encoding]::UTF8.GetBytes("${ClientId}:${ClientSecret}")
$EncodedClientInfo =[Convert]::ToBase64String($ClientBytes)

curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Authorization: Basic $EncodedClientInfo" -d "grant_type=authorization_code&redirect_uri=https://example.com/callback&code=$Code" https://accounts.spotify.com/api/token
```

</details>

### Manual

- Navigate to the following URL:

```
https://accounts.spotify.com/authorize?client_id={SPOTIFY_CLIENT_ID}&response_type=code&scope=user-read-currently-playing,user-read-recently-played&redirect_uri=https://example.com/callback
```

- After logging in, save the {CODE} portion of: `https://example.com/callback?code={CODE}`

- Create a string combining `{SPOTIFY_CLIENT_ID}:{SPOTIFY_CLIENT_SECRET}` (e.g. `5n7o4v5a3t7o5r2e3m1:5a8n7d3r4e2w5n8o2v3a7c5`) and **encode** into [Base64](https://base64.io/).

- Then run a [curl command](https://httpie.org/run) in the form of:

```sh
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Authorization: Basic {BASE64}" -d "grant_type=authorization_code&redirect_uri=https://example.com/callback&code={CODE}" https://accounts.spotify.com/api/token
```

- Save the Refresh token

---

## Option 2: Last.fm API

Last.fm is a simpler alternative that doesn't require OAuth setup. It only requires an API key and username.

### Get Last.fm API Key

1. Go to [Last.fm API Account Creation](https://www.last.fm/api/account/create)
2. Fill in the application details:
   - **Application name**: Choose any name (e.g., "Now Playing Widget")
   - **Application description**: Brief description
   - **Application homepage**: Can be your GitHub profile or repo URL
   - **Callback URL**: Leave empty or use any URL (not used for this project)
3. Click **Submit**
4. Take note of:
   - `API Key` (this becomes `LAST_FM_API_KEY`)

### Get Your Last.fm Username

Your username is the one you use to log in to Last.fm. You can find it:
- In your profile URL: `https://www.last.fm/user/{YOUR_USERNAME}`
- This becomes `LAST_FM_USERNAME`

### Environment Variables for Last.fm

You'll need these two environment variables:
- `LAST_FM_API_KEY` - Your Last.fm API key
- `LAST_FM_USERNAME` - Your Last.fm username

---

## Deployment

### Deploy to Vercel

- Register on [Vercel](https://vercel.com/)

- Fork this repo, then create a vercel project linked to it

- Add Environment Variables:

  - `https://vercel.com/<YourName>/<ProjectName>/settings/environment-variables`
  
  **For Spotify:**
    - `SPOTIFY_REFRESH_TOKEN`
    - `SPOTIFY_CLIENT_ID`
    - `SPOTIFY_SECRET_ID`
  
  **For Last.fm:**
    - `LAST_FM_API_KEY`
    - `LAST_FM_USERNAME`

- Deploy!

### Deploy to Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://dashboard.heroku.com/new?template=https%3A%2F%2Fgithub.com%2Fnovatorem%2Fnovatorem)

- Create a Heroku application via the Heroku CLI or via the Heroku Dashboard. Connect the app with your GitHub repository and enable automatic builds <br>
  `PS. automatic build means that everytime you push changes to remote, heroku will rebuild and redeploy the app.`
  - To start the Flask server execute `heroku ps:scale web=1` once the build is completed.
- Or click the `Deploy to Heroku` button above to automatically start the deployment process.

### Run locally with Docker

- You need to have [Docker](https://docs.docker.com/get-docker/) installed.

- Add Environment Variables (choose one service):

  **For Spotify:**
  - `SPOTIFY_REFRESH_TOKEN`
  - `SPOTIFY_CLIENT_ID`
  - `SPOTIFY_SECRET_ID`
  
  **For Last.fm:**
  - `LAST_FM_API_KEY`
  - `LAST_FM_USERNAME`

- To run the service, open a terminal in the root folder of the repo: <br>
  Execute:
  ```
  docker compose up
  ```
- When finished, navigate to [http://localhost:5000/](http://localhost:5000/)
- To stop the service, open a terminal in the root folder of the repo: <br>
  Execute:
  ```
  docker compose down
  ```

## Deploy to Heroku  

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://dashboard.heroku.com/new?template=https%3A%2F%2Fgithub.com%2Fnovatorem%2Fnovatorem)
- Create a Heroku application via the Heroku CLI or via the Heroku Dashboard. Connect the app with your GitHub repository and enable automatic builds <br>
    `PS. automatic build means that everytime you push changes to remote, heroku will rebuild and redeploy the app.`
    - To start the Flask server execute `heroku ps:scale web=1` once the build is completed.
- Or click the `Deploy to Heroku` button above to automatically start the deployment process.

## Run locally with Docker

* You need to have [Docker](https://docs.docker.com/get-docker/) installed.

* Add Environment Variables:
    * `SPOTIFY_REFRESH_TOKEN`
    * `SPOTIFY_CLIENT_ID`
    * `SPOTIFY_SECRET_ID`
  
* To run the service, open a terminal in the root folder of the repo: <br>
    Execute:
    ```
    docker compose up
    ```
* When finished, navigate to [http://localhost:5000/](http://localhost:5000/)
    
* To stop the service, open a terminal in the root folder of the repo: <br>
    Execute:
    ```
    docker compose down
    ```

# Readme

You can now use the following in your readme:

**For Spotify users:**

`[![Spotify](https://USER_NAME.vercel.app/api/orchestrator)](https://open.spotify.com/user/USER_NAME)`

**For Last.fm users:**

`[![Last.fm](https://USER_NAME.vercel.app/api/orchestrator)](https://www.last.fm/user/USER_NAME)`

# Customization

### URL Parameters

You can customize the appearance of your widget using URL query parameters:

| Parameter | Description | Default | Example |
|-----------|-------------|---------|---------|
| `background_color` | Card background color (hex, without `#`) | `181414` | `0d1117` |
| `border_color` | Card border color (hex, without `#`) | `181414` | `ffffff` |
| `background_type` | Background style: `color`, `blur_dark`, or `blur_light` | `color` | `blur_dark` |
| `show_status` | Show "Vibing to:" or "Recently played:" text | `false` | `true` |

#### Background Types

- **`color`** - Solid color background (default)
- **`blur_dark`** - Blurred album art with dark overlay (great for dark themes)
- **`blur_light`** - Blurred album art with light overlay (great for light themes)

#### Examples

Basic dark theme:
```
https://USER_NAME.vercel.app/api/orchestrator
```

Custom colors:
```
https://USER_NAME.vercel.app/api/orchestrator?background_color=0d1117&border_color=30363d
```

**Blurred album art background (dark):**
```
https://USER_NAME.vercel.app/api/orchestrator?background_type=blur_dark
```

**Blurred album art background (light):**
```
https://USER_NAME.vercel.app/api/orchestrator?background_type=blur_light
```

With status text:
```
https://USER_NAME.vercel.app/api/orchestrator?show_status=true
```

Full customization with blur:
```
https://USER_NAME.vercel.app/api/orchestrator?background_type=blur_dark&border_color=333333&show_status=true
```

### Theme Templates

Change the widget theme by editing the `current-theme` property in `api/templates.json`:

```json
{
    "current-theme": "dark",
    "templates": {
        "light": "spotify.html.j2",
        "dark": "spotify-dark.html.j2",
        "base": "base.html.j2"
    }
}
```

Available themes:
- `light` - Transparent background, no border
- `dark` - Customizable background and border colors

### Creating Custom Themes

To create your own theme:

1. Create a new template file in `api/templates/` that extends `base.html.j2`:

```jinja2
{% extends "base.html.j2" %}

{% block container_styles %}
/* Your container styles */
background-color: var(--background-color);
border: 2px solid var(--border-color);
{% endblock %}

{% block theme_styles %}
/* Your custom CSS overrides */
.song {
    font-weight: 700;
}
{% endblock %}
```

2. Add your theme to `templates.json`:

```json
{
    "current-theme": "my-theme",
    "templates": {
        "light": "spotify.html.j2",
        "dark": "spotify-dark.html.j2",
        "my-theme": "my-theme.html.j2"
    }
}
```

### Health Check Endpoint

A `/health` endpoint is available for monitoring:
```
https://USER_NAME.vercel.app/api/orchestrator/health
```

### Audio-Reactive Animations

The equalizer bars automatically sync to the music's characteristics:

**For Spotify users:**
- Bars pulse at the actual **BPM** of the song
- Animation intensity scales with the track's **energy** level
- Animation curves adjust based on **danceability**
- High-energy tracks have more dramatic bar movements and subtle album art pulsing

**For Last.fm users:**
- If you also have Spotify configured, the widget will look up the track on Spotify to get audio features
- Otherwise, sensible defaults are used (120 BPM, medium energy)

This creates a unique visual experience for each track - calm songs have gentler animations, while energetic tracks have more dynamic, punchy visuals.

## Theme Templates

If you want to change the widget theme, you can do so by the changing the `current-theme` property in the `templates.json` file.

Themes:
* `light`
* `dark`

If you wish to customize farther, you can add your own customized `spotify.html.j2` file to the templates folder, and add the theme and file name to the `templates` dictionary in the `templates.json` file.

## Color

You can customize the appearance of your `Card` however you wish with URL params.

### Common Options:

- `background_color` - Card's background color _(hex color)_ without `#`
- `border_color` - Card border color _(hex color)_ without `#`

Use `/?background_color=8b0000&border_color=ffffff` parameter like so:  
&nbsp; <br> [![Spotify](https://novatorem.vercel.app/api/spotify?background_color=0d1117&border_color=ffffff)]()

## Spotify Logo

You can add the spotify logo by removing the commented out code, seen below:
```html
<a href="{{songURI}}" class="spotify-logo">
    <svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>Spotify</title><path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/></svg>
</a>
```

## Requests

Customization requests can be submitted as an issue, like https://github.com/novatorem/novatorem/issues/2

If you want to share your own customization options, open a PR if it's done or open an issue if you want it implemented by someone else.

# Debugging
If you have issues setting up, try following this [guide](https://youtu.be/n6d4KHSKqGk?t=615).

Followed the guide and still having problems?
Try checking out the functions tab in vercel, linked as:
`https://vercel.com/{name}/spotify/{build}/functions`

<details><summary>Which looks like-</summary>

![image](https://user-images.githubusercontent.com/16753077/91338931-b0326680-e7a3-11ea-8178-5499e0e73250.png)

</details><br>

You will see a log there, and most issues can be resolved by ensuring you have the correct variables from setup.
