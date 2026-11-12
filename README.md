# snapchat-export-format

How Snapchat's Memories export is put together: the ZIP files, the folders, the file names, and `memories_history.json` field by field.

This repository is documentation only. There is no code to run.

- [FORMAT.md](FORMAT.md): the full description.
- [examples/memories_history.json](examples/memories_history.json): an invented file with the same shape as the real one.

## The short version

- Snapchat sends the export as one or more ZIP files of about 2 GB. Only the first one holds `json/memories_history.json`.
- Memories are in a `memories/` folder. Chat photos and videos are in `chat_media/`, apart.
- Each Memory is a `-main.jpg` or `-main.mp4` file. A caption or a sticker sits in a separate `-overlay.png` file.
- A video longer than 10 seconds comes as several files, one per clip of about 10 seconds. Nothing in the export says which clips go together.
- `memories_history.json` lists each Memory with its date and time in UTC, its type and its location. No field gives the file name.
- No photo has a capture date written inside it, and no file holds a location.

## Questions

**Where is the list of my Memories?**
In `json/memories_history.json`, in the first ZIP only.

**Why do my photos show the wrong date?**
The date is only the file's own date, not one written inside the photo. Copying or uploading the file can replace it with today's.

**Where did my captions and stickers go?**
Into a separate `-overlay.png` file next to each photo or video.

**Where is the location of each Memory?**
Only in `memories_history.json`, in the `Location` field. Snapchat writes 0 for both numbers when it has none.

**Why is one video split into several files?**
Snapchat delivers long videos as clips of about 10 seconds, each in its own file. [FORMAT.md](FORMAT.md#videos-in-10-second-clips) lists what tells the clips of one video apart.

## What to do with it

Photos from the export have no capture date inside them, and captions sit in separate `-overlay.png` files. Three ways to deal with that:

- A handful of photos: set the date by hand (Windows: Properties > Details; Mac Photos: Image > Adjust Date and Time). The time in `memories_history.json` is UTC.
- Many photos: read `memories_history.json` and write each date into the matching file with ExifTool and a short script. Nothing in the export links a line of the JSON to a file name (see [FORMAT.md](FORMAT.md#what-links-a-line-to-a-file)), so you have to match them yourself.
- All of it at once, in the browser, with nothing uploaded: [GetMemoriesOut](https://getmemoriesout.com/en) does the matching, puts captions back on photos and sorts by year and month. It is a paid tool: the recap and 500 files are free, the rest is one payment. We make it.

## Guides on the same topic

- [What is memories_history.json](https://getmemoriesout.com/en/guides/memories-history-json)
- [Why photos lose their date](https://getmemoriesout.com/en/guides/memories-date-and-location)
- [Missing or duplicated ZIP files](https://getmemoriesout.com/en/guides/missing-memories)
- [Memories tools compared](https://getmemoriesout.com/en/guides/memories-tools-compared)

## Your export is private

`memories_history.json` holds the date and the place of every Memory you saved. Don't paste it or attach it in an issue, a pull request or anywhere public. Issues on this repository are public.

## Something is different in your export

Snapchat changes this format from time to time. If your export doesn't match FORMAT.md, read [CONTRIBUTING.md](CONTRIBUTING.md), then open an issue.

## Who wrote this

The people behind [GetMemoriesOut](https://getmemoriesout.com), a tool that reads this export.

## License

The text is under [CC BY 4.0](LICENSE). You can copy and adapt it, as long as you credit it with a link back here.
