# Gifted

A shared household gift ledger, so nobody gets the same gift twice and everything is bought and wrapped before the day it goes out.

The live page is a private claude.ai artifact: https://claude.ai/artifact/4iFyrGXca8sL5PNH2ZC8nU

## What's on the page

- **Group filter** – chips across the top (e.g. "Morris family", "Teachers") filter the whole page.
- **Upcoming** – gifts with a give-on date, grouped by day. Each day shows how many gifts are ready, one-tap *Mark bought / Mark wrapped / Mark given* buttons, a warning when a day is 3 days out or less with something not yet bought or wrapped, and a "No gift yet for" row listing group members who still need a gift for that day.
- **Before you buy** – type a person and a gift idea to check it against everything they've already gotten.
- **People** – one card per person with groups, next gift, yearly budget and notes.
- **Ledger** – every gift, searchable and filterable by status, year and occasion.

## Sign-in and sharing

There is no separate username or password. The page uses each person's claude.ai account: whoever opens it signed in sees the same ledger, edits sync live for everyone, and the header shows who is signed in. Each gift records who added it and who last changed it (shown at the bottom of the gift's edit sheet).

To let someone else add and edit gifts, share the page with them from its Share menu as an **Editor** (invite them by email). People shared as Viewer or Commenter can see the ledger but not change it.

Gift statuses: Idea → Bought → Wrapped/ready → Given.

## Files

- `gifted.html` – the artifact source. It is published to claude.ai, which wraps it in the HTML skeleton and provides the `db` and `user` runtime capabilities. Opened outside claude.ai, the page loads but shows no data.

## Data model (artifact database)

- `gifts/<id>` – recipient, item, status (`idea` | `bought` | `ready` | `given`), from, occasion, due (give-on date, `YYYY-MM-DD`), date (date bought or given), year, price, store, link, notes, createdAt, createdBy, updatedAt, updatedBy (person ids, never names)
- `people/<slug of name>` – name, groups (list), budget (yearly $), notes, updatedAt, updatedBy
- `meta/settings` – givers (names offered in the "From" field)

## Updating

Edit `gifted.html`, then republish it to the same artifact URL from Claude so the link and the stored data stay the same.
