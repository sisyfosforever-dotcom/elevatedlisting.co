# Adding a film to the gallery

The gallery is the `<ul class="fgrid">` inside `<section id="films">` in `index.html`.
Order = strength (best first). There is no visible ranking; position is the only signal.

## Steps
1. Get the YouTube ID (the 11 chars after `youtu.be/`).
2. Verify it exists (must return HTTP 200 and the right title):
   `curl -s "https://www.youtube.com/oembed?url=https://youtu.be/ID&format=json"`
3. Check it's not already there: `grep -c 'data-yt="ID"' index.html` (must be 0).
4. Paste this snippet into `<ul class="fgrid">` at the position matching its strength:

```html
          <li><button class="fcard" type="button" data-yt="ID" aria-label="Play SONG – PLACE">
            <span class="fthumb"><img src="https://img.youtube.com/vi/ID/hqdefault.jpg" alt="" loading="lazy" decoding="async" width="480" height="360"><span class="fplay" aria-hidden="true"></span></span>
            <span class="ftitle">SONG</span><span class="fplace">PLACE</span><span class="fcountry">COUNTRY</span>
          </button></li>
```

   - PLACE = venue + town/region (no country; country goes in `fcountry`).
   - HTML-escape: `&` → `&amp;`, `'` → `&#x27;`. Use an en dash ` – ` in aria-label.
   - Click-to-play is handled by existing JS via `data-yt`; no other change needed.
5. `git add index.html && git commit -m "Gallery: add SONG – PLACE" && git push origin main`
6. Verify live (Pages takes ~1 min): `curl -s https://elevatedlisting.co/ | grep -c 'data-yt="ID"'` → 1.
