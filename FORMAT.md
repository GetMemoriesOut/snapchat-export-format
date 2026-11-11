# How a Snapchat Memories export is put together

This page describes the export Snapchat sends when you ask for your Memories under My Data: the ZIP files, the folders, the file names and `memories_history.json`, field by field.

Last checked against one real export received in early October 2026, opened on a Mac. Snapchat changed this format twice in 2025 and 2026, so yours may differ in the details. If it does, [open an issue](CONTRIBUTING.md).

## Contents

1. [Asking for the export](#asking-for-the-export)
2. [The ZIP files](#the-zip-files)
3. [What the first ZIP holds](#what-the-first-zip-holds)
4. [The media folders](#the-media-folders)
5. [File names](#file-names)
6. [Videos in 10-second clips](#videos-in-10-second-clips)
7. [memories_history.json](#memories_historyjson)
8. [What links a line to a file](#what-links-a-line-to-a-file)
9. [What the files don't hold](#what-the-files-dont-hold)
10. [Not checked](#not-checked)

## Asking for the export

- In the Snapchat app: Settings, then My Data. On a computer: accounts.snapchat.com, then My Data.
- Turn on "Export your Memories".
- When the export is ready, it is on the My Data page, under "See exports", with a Download button.

## The ZIP files

Snapchat splits a large export into ZIP files of about 2 GB each, with the last one smaller. The first one has the base name, and the next ones add a number:

```
mydata~1696345678.zip
mydata~1696345678-2.zip
mydata~1696345678-3.zip
...
```

The number after `~` changes from one export to the next.

## What the first ZIP holds

```
index.html                      opens in a browser and shows the export
html/
  memories_history.html         your Memories, as a web page
  ...
json/
  memories_history.json         the list of your Memories (see below)
  ...
memories/                       photos and videos, if this ZIP holds some
chat_media/                     chat photos and videos, if this ZIP holds some
```

`memories_history.json` is in the first ZIP only. No other ZIP has a `json/` folder with it.

## The media folders

Photos and videos sit in two folders, spread across the ZIPs:

- `memories/` holds the Memories, the ones in the app's Memories tab.
- `chat_media/` holds photos and videos sent in chats. These are not Memories.

`chat_media/` can be much larger. In the export we checked, it held more than four files for every file in `memories/`, and it filled the first ZIPs. The Memories came after.

## File names

A Memory's file is named after its day, an identifier, and a suffix:

```
memories/2019-07-14_8F3C2A9E-1B4D-4E7A-9C21-5A6F0D3E7B12-main.jpg
memories/2019-07-14_8F3C2A9E-1B4D-4E7A-9C21-5A6F0D3E7B12-overlay.png
```

- `-main.jpg` is a photo, `-main.mp4` a video.
- `-overlay.png` holds what was drawn on top of the Memory: caption, stickers, drawing. It is a separate image. A photo opened on its own shows none of it.
- A Memory and its overlay share the same name up to `-main` or `-overlay`. That shared part is the only thing that ties them together.
- In the export we checked, about 1 Memory in 7 had an overlay.
- The name gives the day, not the time.

In the export we checked, every file had its extension. Some people report files without one in other exports. Adding `.jpg` or `.mp4` to the name usually lets them open.

## Videos in 10-second clips

A video longer than 10 seconds comes as several `-main.mp4` files, one per clip of about 10 seconds. Each clip has its own name and identifier, so a gallery shows them as separate videos.

Neither the file names nor `memories_history.json` say which clips belong together. In the export we checked:

- **Length**: every clip but the last lasts 9.5 to 10.6 seconds.
- **Creation time**: each MP4 file stores a creation time in its header. The clips of one video were created within 3 seconds of one another, with the same video settings.
- **Order**: up to autumn 2020, the creation time gave the order. After that, it was wrong about one time in two. The order of the files inside the ZIP is random.
- **Copies**: a clip can be there twice, byte for byte.

Only the image proves the order. The last frame of a clip and the first frame of the next show the same moment.

Some descriptions of the export name clips `-main-001.mp4`, `-main-002.mp4`. We did not see these names in the real export.

## memories_history.json

The file holds one list, `Saved Media`, with one entry per Memory. Here is an invented entry:

```json
{
  "Saved Media": [
    {
      "Date": "2026-01-17 19:30:32 UTC",
      "Media Type": "Image",
      "Location": "Latitude, Longitude: 48.8584, 2.2945",
      "Download Link": "",
      "Media Download Url": ""
    }
  ]
}
```

Every entry has the same five fields:

| Field | What it holds |
|---|---|
| `Date` | Date and time of the Memory, in UTC: `2026-01-17 19:30:32 UTC`. In older exports the `UTC` label is missing, as in `2016-03-03 18:07:12`. It is UTC all the same. |
| `Media Type` | `Image` for a photo, `Video` for a video. |
| `Location` | `Latitude, Longitude: 48.8584, 2.2945`. When Snapchat has no place, both numbers are 0. |
| `Download Link` | Present in every entry. Not needed to read the files in the ZIPs. |
| `Media Download Url` | Present in every entry. Not needed to read the files in the ZIPs. |

The `Download Link` and `Media Download Url` values above are left empty. This page doesn't describe what they hold.

- The number of entries is the number of Memories Snapchat listed.
- `Location` is the only place in the whole export where a Memory's location is written.
- An entry can have no file in the ZIPs, for example when a ZIP wasn't downloaded.

A longer invented file is in [examples/memories_history.json](examples/memories_history.json).

This file holds the date and the place of every Memory. Don't paste it or attach it anywhere public, including issues on this repository.

## What links a line to a file

None of the five fields gives a file name. The export doesn't say which line goes with which file.

The only clue is the file's time inside the ZIP. A ZIP stores that time to 2 seconds, without a time zone. It can be several hours off from the `Date` of the matching line. To pair them, you have to compare times. Two Memories taken within the same 2 seconds can't be told apart this way.

## What the files don't hold

- **Capture date**: in the export we checked, no photo had a capture date written inside it. The date is only the file's own date in the ZIP. Copy or upload the file in a way that rewrites it, and that date becomes today's.
- **Location**: no file in the export held one. It is only in `memories_history.json`.
- **Overlays**: captions and stickers are in the `-overlay` file. The photo or video on its own doesn't show them.
- **Time on a Mac**: when you unzip with a double-click, Finder shifts the files' time by the Mac's offset from UTC. In the export we checked, on a Mac set to Paris time, 1 Memory in 20 moved to another day.

## Not checked

- **Windows**: we opened the export on a Mac only.
- **Older exports**: people report that earlier exports held links to Snapchat's servers instead of the photos and videos. We have not checked one ourselves.
- **Other options**: we describe an export with Memories turned on. Other choices on the My Data page may add folders not listed here.

---

Text under [CC BY 4.0](LICENSE). You can copy and adapt it, including commercially, as long as you credit it: "How a Snapchat Memories export is put together, GetMemoriesOut, https://github.com/GetMemoriesOut/snapchat-export-format".
