# Amigos Paradise Tours – website

The website of **Darshi**, a private driver in Sri Lanka: one static page (`index.html`), no framework, no build step, published with GitHub Pages at https://amigosparadisetours.com.

**Its only goal:** a traveller who was recommended Darshi, usually a Spanish traveller from a WhatsApp travel group opening the link on a phone, trusts him and messages him on WhatsApp. Every change must serve that.

The site was designed to run for years without maintenance. Keep it that way.

## Documentation

- `README.md`: the overview for Darshi and his friend (what he can change for free in the panel, what needs Claude Pro, how to invite a developer).
- `docs/Darshi-Your-website-guide.pdf`: his step-by-step guide. If you change how the panel or a section works, tell him the guide needs updating.
- This file: the rules and how the code works.

## Working with Darshi

Darshi owns this repository and asks for changes from the Claude app on his phone, usually in English. He is not technical.

- Reply in short, simple English. No jargon (say "the photo at the top", not "hero image").
- Before editing, say in one line what you will change. If the request is unclear, ask one question first.
- Change only what he asked for. Never rewrite or remove anything else.
- Every change goes in **both languages** (Spanish and English). If he writes the new text in English, write the Spanish version yourself and show it to him.
- When you finish, open a pull request and tell him: "Open the pull request and tap Merge. The website updates in about a minute." If something goes wrong after merging, he can ask you to undo the last change.

## Rules that protect who he is (never break these)

1. **Never invent facts.** No numbers, years of experience, services, guarantees, prices or places he hasn't confirmed. If a text needs a fact, ask Darshi.
2. **Reviews are literal quotes.** Never change their words, spelling or punctuation (for example "super bien" and "Darshik" stay as written). A translation is marked "Translated from Spanish", "Traducida del inglés" or "Traducida del francés". Only add a review if Darshi confirms the person agreed to appear.
3. **Darshi's own quotes are literal too:** the quote in the hero and the "whole heart" quote in "About me". His "I trust your word" about the deposit is a personal gesture for WhatsApp: don't put it on the website.
4. **His motto stays in English in both languages:** "Come as a guest, leave as amigo", with "amigo" underlined in gold.
5. **Voice:** first person (Darshi speaks). Spanish from Spain, addressing the reader as **tú** (never vosotros or usted). Warm, simple, short sentences, like a friend. No marketing words ("best", "unique", "unforgettable"), no comparisons with other drivers, no showing off: the travellers' words do that.
6. **His English is only mentioned in the FAQ** ("My English is very basic..."), never in the hero or the car block.
7. **No prices or amounts anywhere.** The quote, the deposit and the payment method are sent on WhatsApp, trip by trip.
8. **Nothing that needs maintenance:** no embedded Instagram or Google feeds (link to them instead), no counters, no "years of experience", no dates that go out of date, no car model.
9. **Nothing that looks generated or like a template:** no stock photos, no AI images, no icon sets, no emoji. Real photos only. The one exception is the train card photo (`img/tren-nine-arches.jpg`, by llxvisuals on Unsplash, free licence): replace it with Darshi's own photo of the train when he has one. The only icons are the WhatsApp, Instagram and Google logos on the links to those services.

## Structure

One page, in this order (section ids in brackets): header with ES/EN switch and WhatsApp button · hero (`#inicio`) · about me (`#darshi`) · travellers' experiences, dark band (`#opiniones`) · how we'll travel (`#viajar`), with the car (`#coche`) · quote and booking (`#presupuesto`) · real routes (`#viaje-real`) · FAQ (`#faq`) · contact, dark band (`#contacto`).

## How the two languages work

- Every visible text exists twice: `<span class="t-es">…</span><span class="t-en">…</span>` (the class can also sit on a `p`, `ul` or `ol`). The page opens in English, or in Spanish when the visitor's phone or browser is in Spanish (Darshi's choice); `?en` / `?es` and the visitor's last choice win over that. The EN button adds `class="en"` to `body`; the HTML itself is written with Spanish first. Both versions must say the same thing.
- Images carry `data-alt-es` and `data-alt-en`; `alt` holds the Spanish one.
- WhatsApp buttons are links with class `wa`. The script at the bottom fills in the prefilled message in the visitor's language. The number lives in the script (`num`), in the contact block and in the structured data in `<head>`: change all three if it ever changes. The prefilled message is also how Darshi knows a person came from the website, so keep it.

## Content Darshi edits from the panel

The main photos, reviews, photos with travellers, places, routes, the FAQ, the "before / on arrival / during" lists and the car details are **not written in `index.html`**. They live in data files that Darshi edits himself, for free, from the Pages CMS panel (app.pagescms.org, configured in `.pages.yml`):

- `data/reviews.json`: each review has `name`, `from_es`/`from_en`, `trip_es`/`trip_en`, `original_language`, `original` (literal text), `spanish`/`english` (translations), `photo` and `featured` (shown among the first ones; the first featured review is shown bigger).
- `data/travellers.json`: `photo` and `description` (English).
- `data/places.json`: the strip "Algunos lugares del camino / Some places along the way" under the routes. Each place has `photo`, `name_es` and `name_en` (the caption, also its alt text) and `by` (the traveller who took it, shown as "Foto: …"; empty if Darshi took it). The line under the title says they are real photos taken by his travellers. Only Darshi's or his travellers' own photos, never photos from the internet, and only names of places he confirms.
- `data/faq.json`: `question_es`/`question_en`, `answer_es`/`answer_en` (paragraphs separated by an empty line). A question without an answer is not shown.
- `data/help.json`: `before`, `arrival`, `during`, each a list of `{es, en}` lines (the "Antes del viaje / Al llegar / Durante el viaje" block in `#viajar`).
- `data/car.json`: `specs` (list of `label_es`/`label_en`/`value_es`/`value_en`) and `note_es`/`note_en` (the italic sentence under them). No car model, ever.
- `data/links.json`: `google_reviews`, the link to his Google reviews. While it is empty, the "Ver todas mis reseñas en Google" button in `#opiniones` stays hidden; with an `https://` link it appears.
- `data/routes.json`: `name_es`/`name_en` (the button), `who`, `days`, `type_es`/`type_en` (a short note), `stops` (separated by "·"; "A / B" means either). A route with `who` is a real trip with those travellers; with an empty `who` it is "Ruta que te propongo / A route I suggest". The script draws every route on the map from the `PLACES` list (latitude/longitude) near the end of `index.html`: a stop missing from that list is shown in the list but not on the map, so add it there. One button per route switches the map (no buttons when there is only one route). All routes follow the same pattern.
- `data/photos.json`: the main photos (top, About me, the three "How we'll travel" cards, the car), each with `photo` and an English `description`. They match the `data-slot` attributes in `index.html`. A photo that differs from the one written in the HTML replaces it when the page loads.

The script at the bottom of `index.html` reads these files and builds the markup (with the same classes as the rest of the page). To change any of this content, edit the JSON files, not the HTML. Keep the field names: the panel depends on them. If you add a field, add it to `.pages.yml` too.

## Search engines

- The `h1` holds "Darshi · Conductor privado en Sri Lanka" before the motto: keep those words in it.
- The `<script type="application/ld+json">` in `<head>` tells Google what the business is. Keep it true; never add ratings or reviews to it (Google doesn't allow a business to mark up its own reviews).
- `robots.txt` and `sitemap.xml` are at the root. The site is one page, so the sitemap never needs changes.

## Pending items and review mode

Things that are not confirmed yet stay in the code but are **never shown to the public**:

- `.tag`: a yellow note about something to confirm.
- `.prov`: a note on a provisional photo, saying what the ideal photo is.
- `.slot`: an empty box where a photo will go.
- `.pend`: a whole element (an FAQ answer, a route card, a car detail) that only appears once it's confirmed.

The script in `<head>` turns on **review mode** (class `revisar` on `html`), which shows all of the above in yellow. It's off on amigosparadisetours.com and github.io, and on everywhere else. `?revisar` forces it on, `?publico` forces it off. To list what's pending, search the file for `class="tag`, `prov`, `slot` and `pend`.

When something is confirmed: write the real content in both languages, then remove its `.tag` or `.prov` and the `pend` class. Never remove `pend` without the confirmed content.

## Common tasks

**Replace a photo.** The main photos can be changed by Darshi from the panel ("Main photos"). If he asks you instead: put the photo in `img/`, update both `data/photos.json` and the `src`/`data-alt-es`/`data-alt-en`/`alt` of the matching `data-slot` image in `index.html` (keep them the same), and remove its `.prov` caption if it is the final photo. Photos should be about 1,600 px on the long side at most and under about 400 KB (the "Optimize photos" GitHub Action shrinks bigger ones). The image WhatsApp shows when the link is shared is set apart, in `og:image` (an absolute URL, with `og:image:width` and `og:image:height`): it is the close-up `img/portada-darshi.jpg`, because a face reads better in a small preview than the full photo at the top. Don't delete old photos unless he asks.

**Add a review, a photo with travellers or a route.** Darshi can do it himself from the panel. If he asks you, add an entry to the matching file in `data/`. Reviews: literal text in `original`, the translations in `spanish`/`english`, the type of trip only if the traveller said it, and a photo only with their permission. Only `featured` reviews are visible at first (keep about 4); the rest appear behind "Ver más opiniones". Routes are drawn on the map automatically from their stops (see above).

**Change or add an FAQ.** Darshi can do it in the panel ("Questions (FAQ)"). If he asks you, edit `data/faq.json` in his words, in both languages.

**Add the video.** Follow the comment above `.video` in `#opiniones`: a vertical MP4 (H.264, ideally under 15 MB) and a JPG poster in `img/`, then remove `pend`.

**Add the Google reviews link.** Darshi can do it in the panel ("Links"). If he asks you, put the link in `data/links.json`.

## Technical rules

- Keep it one static `index.html` plus the `data/` files. No frameworks, build tools, external scripts, trackers, analytics or cookies. Fonts are local in `fonts/` (SIL Open Font License, see `fonts/LICENSE.txt`). The only browser storage is the remembered language.
- Colours are CSS variables in `:root`, with a dark-mode version. Titles use Libre Caslon Display, quotes Libre Caslon Text italic, body text Figtree.
- Most visitors use a phone: check every change at 375 px wide, in both languages.
- The map is "Sri Lanka relief location map" by Uwe Dedering (Wikimedia Commons, CC BY-SA 3.0). Its credit under the map must stay.
- The icons (`icon-32.png`, `icon-180.png`) are the "D" from his logo. His full logo is `img/logo-amigos.png`.
- Don't remove `.nojekyll` or `CNAME`: GitHub Pages needs them.
