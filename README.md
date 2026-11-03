# Gifted

A shared household gift ledger, so nobody gets the same gift twice and everything is bought and wrapped before the day it goes out.

Live site (GitHub Pages): https://tcmorrismsl-web.github.io/Gifted/

## What's on the page

- **Group filter** – chips across the top (e.g. "Morris family", "Teachers") filter the whole page.
- **Upcoming** – gifts with a give-on date, grouped by day. Each day shows how many gifts are ready, one-tap *Mark bought / Mark wrapped / Mark given* buttons, a warning when a day is 3 days out or less with something not yet bought or wrapped, and a "No gift yet for" row listing group members who still need a gift for that day.
- **Before you buy** – type a person and a gift idea to check it against everything they've already gotten.
- **People** – one card per person with groups, next gift, yearly budget and notes.
- **Ledger** – every gift, searchable and filterable by status, year and occasion.
- **On the way** – every gift marked as shipping, soonest arrival first, with a Track button that opens the carrier's page (UPS, USPS, FedEx, Amazon and DHL are recognized from the tracking number) and a one-tap Arrived button. It flags packages expected after their give-on date, packages past their expected arrival, and gifts still on the way within 3 days of being given. The same flags show in Upcoming, where *Mark arrived* comes before *Mark wrapped*.
- **My private list** – a second tab that only the signed-in account can see, for gifts for each other. It has the same Upcoming, People and Ledger views, and Before you buy checks it alongside the household list. If the database rules aren't the private-list version yet, the tab shows a warning instead of claiming to be private.

Gift statuses: Idea → Bought → Wrapped/ready → Given.

## Sign-in and data

- Sign-in is **Google** through Firebase Authentication (project `gifted-46f0f`).
- Data lives in **Cloud Firestore** in that project and syncs live between devices. It also keeps an offline copy, so the page still shows the list without signal.
- Only the Google accounts listed in `firestore.rules` can read or write anything, and each account's private list is readable only by that account. To add or remove someone, edit that list and paste the file into Firebase console → Firestore Database → Rules → Publish.
- The Firebase config in `index.html` is not a secret; the rules are what protect the data.

## Files

- `index.html` – the site (GitHub Pages serves it from `main`, root folder).
- `firestore.rules` – database access rules; paste into the Firebase console whenever they change.
- `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `icon-maskable-*.png`, `apple-touch-icon.png` – lets you add Gifted to a phone's home screen like an app. The current icon is the Christmas gnome snow globe under the northern lights. Phones keep the icon from when the shortcut was added, so after changing it, bump the `?v=` tag in `index.html` and re-add the shortcut.
- `gifted.html` – the earlier claude.ai artifact version (uses claude.ai's built-in storage instead of Firebase). Kept for reference.

## Data model (Firestore)

- `gifts/<id>` – recipient, item, status (`idea` | `bought` | `ready` | `given`), from, occasion, due (give-on date, `YYYY-MM-DD`), date (date bought or given), year, price, store, link, notes, ship (`` | `on_way` | `arrived`), tracking, carrier (blank = detect from the number), arrives (expected arrival), arrivedOn, createdAt, createdBy, updatedAt, updatedBy (Firebase user ids)
- `people/<slug of name>` – name, groups (list), budget (yearly $), notes, updatedAt, updatedBy
- `members/<uid>` – name, email, photo, lastSeen for each signed-in account (used to show who added or changed a gift)
- `meta/settings` – givers (names offered in the "From" field)
- `private/<uid>/gifts/<id>` – same fields as `gifts`, readable and writable only by the account with that uid

## Updating

Edit `index.html`, commit and push to `main`. GitHub Pages republishes within a minute or two.
