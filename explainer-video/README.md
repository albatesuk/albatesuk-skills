# Explainer video

Give Claude your website, and it makes a short video (30 to 60 seconds) that explains what your product does and why someone should care. You can put the video on your homepage, share it on social media, or play it in a presentation.

The video has:

- captions that tell the whole story, so it works with the sound off
- your logo, colours and fonts, taken from your website
- an ending that shows your web address, so viewers know where to go
- your own music, if you have a track you'd like to use

## How to use it

[Install it](../README.md#how-to-install), then tell Claude what you want, for example:

> Make an explainer video for example.com

You can say more if you like: who the video is for, how long it should be, or a line you want in it. Claude can also work from your website's files if you have them on your computer.

## What happens next

Claude works through it with you step by step, and checks with you before each big step:

1. **Research.** Claude reads your website to understand what you offer, who it's for, and what your brand looks like.
2. **Script.** Claude writes a short script: what's on screen and what the captions say, second by second. You approve it or ask for changes. Nothing gets made until you're happy with the script.
3. **Preview.** Claude builds a preview you can play in your web browser. Ask for as many changes as you like.
4. **Video files.** Once you approve the preview, Claude makes the finished files. You get a high-quality version for presentations and big screens, and a smaller version for your website.
5. **Music (optional).** Tell Claude where your music file is, and it fits the track to the video and sets the volume.
6. **Website (optional).** Claude can add the video to your website for you.

## Good to know

- **Making the video files takes a while,** often 20 minutes to an hour on a typical laptop. Claude will tell you roughly how long it expects.
- **Claude may need to install some free software** to make the video files. It will tell you what's needed and ask before installing anything.
- **Check the claims.** Claude only uses what your website says, and lists every number and promise in the script for you to check. You know your business best, so read that list before you approve the script.
- **Music:** use a track you have the rights to.

## What it does on your computer

- **Reads your website.** Claude visits the website you name, or reads the folder of website files you point it to.
- **Saves files in one project folder.** The preview, the script, a few small helper programs Claude writes for this video, and the finished video files all go in the folder you choose.
- **Plays the preview in your browser.** Claude runs a small web server on your computer to show the preview. Only you can see it.
- **Installs free software, if you agree.** To make the video files, Claude may need Node.js, Google Chrome and ffmpeg, plus one Node.js add-on, `playwright-core`, in the project folder. It asks before installing anything and uses your computer's usual installer.
- **Changes your website only if you ask.** Claude adds the video to your site only when you ask it to, and doesn't publish or upload anything.

Nothing is sent anywhere except the normal requests to your website and to download the software above.
