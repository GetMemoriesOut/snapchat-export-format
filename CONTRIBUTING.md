# Contributing

Corrections to FORMAT.md are welcome. Snapchat changes its export, and one export can't show every case.

## Never paste your memories_history.json

Don't paste or attach your `memories_history.json`, in whole or in part: it contains your GPS positions. Each line gives the date, the time and the place of one Memory, and together they show where you live and where you go. Issues and pull requests here are public, search engines index them, and deleting them later doesn't remove the copies.

The same goes for screenshots of the file, and for `Location` values copied by hand.

## What to send instead

- When you received your export: month and year.
- What you see, described in words. For example: "the `Date` field has no `UTC` label", or "my videos are named `-main-001.mp4`".
- A made-up line with the same shape as yours, if it helps. Change every date and every number. [examples/memories_history.json](examples/memories_history.json) shows how.
- File names with the identifier replaced by `XXXX`, as in `2019-07-14_XXXX-main.jpg`.

## What this repository covers

It describes what Snapchat sends and how the export is laid out. Tools, scripts and links to products are out of scope, and pull requests that add them will be closed.

## Pull requests

Edit FORMAT.md, and say in the description which export you checked it against (month and year, phone or computer). Keep invented values in every example.
