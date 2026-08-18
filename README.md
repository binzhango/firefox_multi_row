# Firefox Multi-Row Tabs

A small collection of versioned `userChrome.css` stylesheets that brings a
multi-row tab bar to Firefox desktop. Tabs wrap onto additional rows instead of
being compressed into a single horizontally scrolling strip.

The current stylesheet targets **Firefox 154+** and is configured for three
visible rows of fixed-width tabs. Older snapshots are kept in the repository
for people running earlier Firefox versions.

> [!IMPORTANT]
> `userChrome.css` customizes Firefox's internal interface. It is not a stable
> Web standard, so a future Firefox update may require a new stylesheet.

## What the Firefox 154 stylesheet provides

- Up to three visible tab rows, followed by vertical scrolling
- Fixed 210 px normal tabs by default
- Pinned tabs placed in the multi-row flow
- Support for collapsed tab groups
- Correct sizing for split-view tab wrappers
- Row-aligned scrolling and a thin scrollbar
- Optional full-width tabs and a draggable scrollbar handle
- Compatibility with Firefox 154's `--tab-margin-block` tab-spacing variable

## Choose a stylesheet

| Firefox version | File | Status |
| --- | --- | --- |
| 154+ | [`userChrome_ff154.css`](userChrome_ff154.css) | Current |
| 131 | [`userChrome_ff131.css`](userChrome_ff131.css) | Legacy snapshot |
| 81 | [`userChrome_v81.css`](userChrome_v81.css) | Legacy snapshot |
| 71-era | [`ff71+.css`](ff71+.css), [`userChrome_ff71.css`](userChrome_ff71.css) | Legacy snapshots |
| 65 | [`userChrome_ff65.css`](userChrome_ff65.css) | Legacy snapshot |

Use the file that matches your Firefox version. Do not combine multiple
versioned stylesheets.

## Install

1. Open `about:config` in Firefox.
2. Search for `toolkit.legacyUserProfileCustomizations.stylesheets` and set it
   to `true`.
3. Open `about:support`.
4. Find **Profile Folder** (called **Profile Directory** on some platforms) and
   select **Open Folder** or **Show in Finder**.
5. Create a directory named `chrome` inside that profile directory if it does
   not already exist.
6. Copy the stylesheet for your Firefox version into `chrome` and rename the
   copy to exactly `userChrome.css`.
7. Fully quit Firefox and open it again.

For Firefox 154+, the resulting path should look like this:

```text
<your Firefox profile>/chrome/userChrome.css
```

The file at that path should contain the contents of
[`userChrome_ff154.css`](userChrome_ff154.css).

## Configure Firefox 154+

Edit these variables near the top of `userChrome.css`:

```css
:root {
  --multirow-n-rows: 3;
  --multirow-tab-min-width: 210px;
  --multirow-tab-dynamic-width: 0;
}
```

| Variable | Default | Purpose |
| --- | ---: | --- |
| `--multirow-n-rows` | `3` | Maximum visible rows before vertical scrolling starts |
| `--multirow-tab-min-width` | `210px` | Minimum width of each normal tab |
| `--multirow-tab-dynamic-width` | `0` | `0` keeps fixed-width tabs; `1` lets tabs grow into available space |

After changing the file, fully quit and restart Firefox.

### Optional preferences

The Firefox 154 stylesheet recognizes two optional Boolean preferences. Create
either preference in `about:config` and set it to `true` only if you want the
behavior.

| Preference | Effect |
| --- | --- |
| `userchrome.multirowtabs.full-width-tabs.enabled` | Allows tabs to expand across the available row width |
| `userchrome.multirowtabs.scrollbar-handle.enabled` | Makes the scrollbar directly draggable, but prevents window dragging from empty tab-bar space |

## Firefox 154 compatibility notes

Firefox 154 changed the internal spacing token used to calculate tab-row
height. The current stylesheet uses `--tab-margin-block` and falls back to the
older `--tab-block-margin` name:

```css
max-height: calc(
  (var(--tab-min-height) + 2 * var(--tab-margin-block, var(--tab-block-margin, 0px)))
    * var(--multirow-n-rows)
);
```

The stylesheet intentionally does not set `height: fit-content` on the tab
container. That workaround can produce oversized gaps between rows in Firefox
154.

## Troubleshooting

If the tab bar does not change:

1. Confirm the installed file is named exactly `userChrome.css`, including
   capitalization.
2. Confirm it is inside the active profile's `chrome` directory, not the
   Firefox installation directory.
3. Confirm `toolkit.legacyUserProfileCustomizations.stylesheets` is `true`.
4. Fully quit Firefox; closing only the current window may leave the process
   running.
5. Open `about:support`, use **Clear startup cache**, and restart Firefox.
6. Temporarily remove other interface CSS to check for conflicting selectors.

If an update breaks the layout, keep a backup of the working file and check
this repository for a stylesheet matching the new Firefox version.

## Project layout

```text
firefox_multi_row/
├── README.md
├── userChrome_ff154.css   # Current Firefox 154+ stylesheet
├── userChrome_ff131.css   # Firefox 131 snapshot
├── userChrome_v81.css     # Firefox 81 snapshot
├── ff71+.css              # Firefox 71-era snapshot
├── userChrome_ff71.css    # Firefox 71 snapshot
└── userChrome_ff65.css    # Firefox 65 snapshot
```

## Credits

The multi-row approach is based on
[MrOtherGuy/firefox-csshacks](https://github.com/MrOtherGuy/firefox-csshacks),
particularly its
[`multi-row_tabs.css`](https://github.com/MrOtherGuy/firefox-csshacks/blob/master/chrome/multi-row_tabs.css)
stylesheet.
