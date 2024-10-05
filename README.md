# Playlist Sorter for YouTube

**UPDATE: Oct 5, 2024**

**This extension is no longer available in the Chrome extension store.**

YouTube has revoked the Chrome extension's access to the YouTube API. YouTube audited the extension and required several changes that I am unable to, or will not, make (including changing the name of the extension and user interface to not use the word "YouTube"). I guess we'd have to call it "Playlist Sorter for That Really Popular Video Sharing Service". No thanks.

I'll leave the code here for now, in case anyone wants to run it locally, or make their own attempt with YouTube's auditors.

---

If you’ve ever wanted to sort your playlist videos by name, then you’ve gotten used to disappointment, since YouTube doesn’t support that!

I recently decided to host all my family videos on YouTube. I love to gather the kids around and watch their funny videos from 5 years ago! Having them on YouTube makes it really easy to stream or beam to the TV.

So, I started the process of uploading everything to YouTube, and creating playlists by year (2016, 2017, etc). All my videos are named like 2016_02_snow_day.

I was getting pretty excited about having all our videos so accessible. But then I realized the videos were not playing in order. I discovered there’s no way to sort by name/title! The closest thing YouTube offers is to order by Date Added.

Unfortunately, when uploading 100s of videos via the upload screen, the “date added” isn’t very predictable (even if you are careful about the order you drop the videos into the upload area). There is a “manual” ordering option, but that’s kinda useless when you’ve got 100s of videos.

To make YouTube work for me, I needed to be able to order by video name. This extension uses the [YouTube API](https://developers.google.com/youtube/v3/) to load your playlists and enable you to sort the videos within the playlist.

## Available on the Chrome app store

- https://chrome.google.com/webstore/detail/playlist-sorter-for-youtu/ameobkgafemjmlgobgbdadbdbgncbecg

## Using it

- Create one or more playlists with some videos in your YouTube account
- Click the Playlist Sorter icon in the Chrome extension toolbar area
- Login with your YouTube credentials
- You playlists will be displayed
- Click a playlist
- Once in a playlist, click the A-Z link to sort alphabetically by video title

## Developing

If you'd like to develop your own extension based on this one, you can probably clone it, start making changes, and run it locally. However, if you plan to publish anything to the Chrome app store, you'll need to change the name to avoid any confusion with this extension.

### OAuth Client ID
You'll need a google oauth client ID. Client-side-only apps (such as this extension) have a challenge. They can't store the oauth client _secret_. How to oauth without it? Google apis support the "implicit grant flow" for authenticating with the google api without a client secret. It uses only the _client ID_. Details [here](https://developers.google.com/youtube/v3/guides/auth/client-side-web-apps).

Once you have your client ID (not _secret_), create a file called src/client-id.js with these contents: 

```
const clientId = "874509873532-9pc5ia873dgtw6fp3sp734uq7h98dfx2.apps.googleusercontent.com"
export default clientId
```

### Install dependencies and build:
- `npm install`
- `npm run watch` (this will build and watch for source changes)

### Loading the extension locally

- Go to chrome://extensions
- Enable the "Developer mode" slider in the top right
- Then click the "Load Unpacked" button and navigate to the `extension` directory in the filesystem

### Reloading

After making changes, you'll need to:

1. Monitor the `npm run watch` output in the terminal to be sure your changes built cleanly
2. Click the reload icon for the extension in chrome://extensions. This will close the Playlist Sorter page.
3. Click the Playlist Sorter icon in the Chrome extension toolbar area to reopen it

### Extension code

The extension code is found in the `src` directory.

- `app.js` App entry point that loads the top-level PlaylistSorter react component.
- `background.js` This code runs whenever the extension is loaded. It listens for our extension button being clicked and creates a new tab for our UI.
- `*.jsx` React components for the app.

### ES Lint
- `npm run lint`

### Debugging

Open devtools on the Playlist Sorter app tab.

### Building a new version

- Be sure to update the version in `manifest.json`
- Run ./build.sh to create a new extension.zip in the current directory
- Upload the new zip to the Chrome store
