# Classroom Schedule Display

A single-page web app that displays a daily classroom schedule on a large
screen. Vanilla JS, no build step, no backend. Deploy by uploading these
files to GitHub Pages.

## Quick Deploy (GitHub Pages)

1. Create a new repo on GitHub.
2. Upload everything in this folder (`index.html`, `images/`, `audio/`)
   to the repo root.
3. In the repo: **Settings → Pages → Source: `main` / `(root)`**.
4. Wait a minute, then visit `https://<your-username>.github.io/<repo-name>/`.

That's it — open the page on the classroom display in fullscreen.

## Day-to-Day Use

- Click **Fullscreen** in the bottom-right of the sidebar.
- The first click anywhere on the page also enables the chime sound
  (browsers require a user gesture before playing audio).
- The schedule advances automatically based on Pacific Time.
- The next block's image and title are always shown at the top of the
  sidebar under **Up Next**.
- 5 minutes before each block ends (when another block follows), a chime
  plays once at 30% volume (set by `chime.volume` in `index.html`).
- In the last 60 seconds the countdown turns red.

## Editing the Schedule

Press **Ctrl+Shift+D** (or click the **Edit** button) to open the editor.

In the editor you can:

- Drag rows by the **⋮⋮** handle to reorder.
- Change titles and start/end times.
- Switch the assigned image (Image 1–13).
- Leave every day unchecked to keep a block in the editor without showing
  it (Snack Time is parked this way).
- Toggle days of the week (M T W T F S S — first M is Monday).
- Add new blocks or delete existing ones.

All changes save automatically to this browser's localStorage. Click
**Reset to Defaults** to restore the original schedule.

> The saved schedule lives under the key `classroomScheduleV2`. When the
> defaults in `index.html` change, bump that key so browsers holding an old
> saved copy pick up the new schedule.

> Schedule edits are stored per-browser. If you switch displays you'll
> need to re-enter them, or copy localStorage between browsers.

## Time Source

The app shows time in **America/Los_Angeles** regardless of the device's
system timezone. On load it tries to sync against:

1. `https://www.cloudflare.com/cdn-cgi/trace` (primary)
2. `https://worldtimeapi.org/api/timezone/Etc/UTC` (fallback)

If both fail (e.g. offline), it falls back to the local clock — still
formatted in PT. The sync indicator at the bottom of the sidebar shows
green when network-synced, amber when on local clock.

## Display Layout

The display is built for a 32" TV that crops the edges of the picture
(overscan). Everything sits inside a ¾" safe margin (`--safe-inset: 2.7vw`),
filled with the lilac panel color. Sizes are in `vw` so the proportions hold
at any resolution. Font: **Lexend**. Colors are the `--panel-*` tokens at
the top of the stylesheet; the schedule editor keeps its own dark theme.

## File Layout

```
index.html            ← everything: HTML, CSS, JS, schedule data
images/block1.jpg     ← Breakfast/SEL
images/block2.jpg     ← Phonics
images/block3.jpg     ← Workshop
images/block4.jpg     ← PE
images/block5.jpg     ← Music
images/block6.jpg     ← Library
images/block7.jpg     ← Lunch/Recess
images/block8.jpg     ← Science/SS
images/block9.jpg     ← Math
images/block10.jpg    ← Plan/Do/Reflect
images/block11.jpg    ← Snack Time (hidden: no days selected)
images/block12.jpg    ← Recess
images/block13.jpg    ← End of Day
images/day-at-glance.jpg ← shown when no block is active
audio/chime.wav       ← 2-note chime played at 5-min warning
```

## Replacing Images

To swap in your own images, just overwrite the JPGs in `images/` keeping
the same filenames. They must be JPEGs; convert PNGs first, e.g.
`magick block1.png -strip -interlace JPEG -quality 85 block1.jpg`.
The image area is about 4:3 and crops wider images at the sides.

## Replacing the Chime

Replace `audio/chime.wav` with any short audio file your browser can play
(WAV, MP3, OGG). If you change the file extension, also update the `<audio>`
tag's `src` attribute in `index.html`.

## Browser Compatibility

Tested against modern Chromium-based browsers (2023+) and Firefox 102+.
Uses `structuredClone`, the Fullscreen API, `Intl.DateTimeFormat` with
IANA timezone, and HTML5 drag-and-drop — all widely supported.
