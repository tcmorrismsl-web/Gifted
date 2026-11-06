# Gifted

A shared household gift ledger, so nobody gets the same gift twice and everything is bought and wrapped before the day it goes out.

Live site (GitHub Pages): https://tcmorrismsl-web.github.io/Gifted/

## What's on the page

Each section is its own tab: Upcoming (opens first), On the way, Before you buy, People and Ledger. On phones the tabs sit in a bar at the bottom of the screen. Pinch and double-tap zoom are turned off. In every pop-up, Cancel/Close and Save stay pinned at the top while you scroll.


- **Group filter** – chips across the top (e.g. "Morris family", "Teachers") filter the whole page.
- **Upcoming** – gifts with a give-on date, grouped by day. Each day shows how many gifts are ready, one-tap *Mark bought / Mark wrapped / Mark given* buttons, a warning when a day is 3 days out or less with something not yet bought or wrapped, and a "No gift yet for" row listing group members who still need a gift for that day.
- **Before you buy** – type a person and a gift idea to check it against everything they've already gotten.
- **People** – tap anyone to open their card, with their gift history and a Show in ledger button. One card per person with groups, birthday, Mother's/Father's Day flags, next gift, yearly budget and notes.
- **Events** – **+ Event** adds a birthday party, holiday, shower or anything else with a date, optional time and place, and the people (or a whole group) who need a gift. It shows in Upcoming right away, listing "No gift yet for…" until gifts are logged. An event with nobody picked is just a dated reminder. A party for someone also stands in for their automatic birthday reminder. Events made on the private tab stay private.
- **Import a list** – next to the People heading. Paste lines like `1/7: Bennet Amburgey` or `8/22: Kayci & Joey Plummer Anniversary`; it previews each line as a new card, an update to an existing card (matched by name), or skip. Couples' anniversaries become a couple card, or go on each spouse's own card for your own anniversary. Updates only change the date.
- **Anniversaries** – a yearly date on a person or couple card, reminded in Upcoming like birthdays. On your spouse's card it shows as "Our anniversary" on your private list.
- **Birthdays and holidays** – a person's next birthday, and Mother's Day (2nd Sunday in May) or Father's Day (3rd Sunday in June) for anyone flagged, show up in Upcoming 60 days ahead until a gift is logged for them. Tick **This is me** on your own card: your own dates never remind you, and your dates show on your spouse's private list instead of Household.
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
- `people/<slug of name>` – name, groups (list), birthday (`MM-DD`), birthYear, anniversary (`MM-DD`), anniversaryYear, mothersDay, fathersDay, account (uid of the person's own sign-in, if it's them), budget (yearly $), notes, updatedAt, updatedBy
- `members/<uid>` – name, email, photo, lastSeen for each signed-in account (used to show who added or changed a gift)
- `events/<id>` – title, kind, date (`YYYY-MM-DD`), time (`HH:MM`), where, people (person keys), group, notes, createdAt, createdBy, updatedAt, updatedBy. Private events live at `private/<uid>/events/<id>`.
- `meta/settings` – givers (names offered in the "From" field)
- `private/<uid>/gifts/<id>` – same fields as `gifts`, readable and writable only by the account with that uid

## Updating

Edit `index.html`, set `APP_VERSION` in it and `v` in `version.json` to the same new value, then commit and push to `main`. GitHub Pages republishes within a minute or two. Phones may hold a saved copy for up to 10 minutes, but the page checks `version.json` whenever it opens or comes back on screen and reloads itself when the version changes.
