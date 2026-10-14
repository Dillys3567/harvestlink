# HarvestLink Marketplace (prototype)

A simple marketplace where farmers list tomato harvests and buyers browse, compare prices and make contact.
It uses sample data. There is no server: everything runs in the browser.

## Run it on your computer
1. Unzip the folder.
2. Open `index.html` in a browser (double-click it). No install or build step is needed.
3. For automatic reload while you edit, open the folder in VS Code and use the "Live Server" extension.

## Files
| File | What it holds |
|---|---|
| `index.html` | Page structure: header, main area, footer |
| `styles.css` | All colours and layout. Change the colours in the `:root` block at the top |
| `data.js` | Sample farmers, harvests and requests. Edit this to change what the site shows |
| `i18n.js` | All the words on the site, in English (`en`) and Twi (`tw`). Edit this to fix or add wording |
| `app.js` | All the behaviour: browse, filter, compare, listing page, farmer page, photo upload, demo phone |

## Things you can change quickly
- Colours: the variables at the top of `styles.css` (a dark version is just below them).
- Crate size used for the price per kg: `CRATE_KG` at the top of `app.js`.
- Tomato varieties: the `varieties` list in `data.js`.
- The farmer shown on "My farm": `me` in `data.js`.

## English and Twi
The switch at the top right changes every label, button and message. The choice is remembered in the browser.
**The Twi is a first draft written without a native speaker. Have a Twi speaker (ideally a farmer or trader from your field area) read it and correct it before anyone outside the team sees it.**
- To fix a word: open `i18n.js`, find the key in the `tw` block, and change the text. The `en` block shows what each key means.
- Placeholders such as `{n}`, `{name}` and `{buyer}` are filled in by the site. Keep them exactly as they are.
- If a Twi key is missing, the site shows the English text instead, so nothing breaks.
- To add a language (for example Ga or Ewe): copy the `tw` block in `i18n.js`, rename it, translate it, then add one more button with `data-lang="..."` to the `langSeg` group in `index.html`.
- Names of farmers, towns and tomato varieties come from `data.js` and are not translated.
- Prices are the same in both languages. Dates show Twi day names with day/month numbers (see `days` in `i18n.js`).

## Demo phone (USSD, voice line, SMS)
The "Phone (USSD)" page shows how a farmer with a basic phone could use HarvestLink. The phone belongs to the demo farmer and works on the same data as the website, so a harvest posted from the phone appears under "Browse harvests" and "My farm".
**It is a simulation. Nothing is dialled or sent, and the short code and voice number are made up.**
- **USSD menu:** dial `*920*55#` and press Call/OK(This is made up). Post a harvest, see your harvests (mark sold, change price), see buyer requests, check listed prices.
- **Voice line (IVR):** dial `5050` and press Call/OK. The menu is read aloud by the browser's own voice, for farmers who do not read. A written transcript appears beside the phone, and "Sound" turns the voice off. The voice depends on the browser and computer, so it will not sound like a real phone line.
- **SMS:** the phone gets a message when a harvest is posted from it, and when a buyer sends a request on one of the demo farmer's listings.
- The phone is in English only, whatever language the site is in. Its words are at the end of the `en` block in `i18n.js`. Translate them to Twi only with a native speaker, and keep every USSD screen under 160 characters (the phone screen shows the count).
- A USSD session closes after 60 seconds without an answer, as a real one would. Change `USSD_TIMEOUT_S` in `app.js` if that is too short for a presentation. The short code and voice number are `USSD_CODE` and `IVR_NUMBER` in the same place.
- A harvest posted from the phone has no photo or notes.

## What is saved
Changes (new harvests, edits, photos, requests) and your language choice are kept in your browser's localStorage so a refresh does not lose them.
Use "Restore sample data" in the footer to reset. Nothing is sent anywhere.

## Connecting a backend in the build stage
Look for the comments marked `BACKEND` in `app.js`:
- `load()` should fetch farmers, harvests and requests from your API.
- `save()` and the add, edit, remove and request handlers should call your API instead of writing locally.
- Profile photos are shrunk to 240 px and stored as a data URL. With a backend, upload the image file to storage and save its address instead.
- Harvest photos work the same way: shrunk to 640 x 360 px and stored on the listing as `photo`. A listing with no `photo` shows the dummy image drawn by `art()` in `app.js`. Browser storage holds roughly 5 MB, so a real launch needs file storage.
- The demo phone needs a USSD, voice and SMS provider (for example Africa's Talking, Hubtel or Arkesel). The provider calls your server with each key press, and the server answers with the text that `ussdText()` and `ivrPrompt()` build in `app.js`. `sms()` should send a real message through the provider.
- Phone numbers are shown to every visitor. Decide before launch whether to hide them until a buyer is signed in.

## Notes
- Fonts load from Google Fonts. Without internet the page falls back to system fonts.
- The sample prices, names and numbers are made up.
