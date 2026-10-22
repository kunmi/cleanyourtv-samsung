# CleanYourTV — Samsung Edition (personal setup)

My version of the CleanYourTV idea, adapted for the TV I actually own.

> Inspired by [@gathint's tweet](https://x.com/gathint/status/2111379569030939102) and
> [Gathin23/cleanyourtv](https://github.com/Gathin23/cleanyourtv): "Vibe coded my parents' TV…
> removed 25 Xiaomi packages and wrote a new launcher. Looks like an Apple TV now."

## Kunmi fork customisations

This fork is adapted for Kunmi's **Samsung S95F** in Hamburg. Its dashboard now
opens with a default app row:

- **YouTube**
- **Disney+**
- **Netflix**
- **MagentaTV**
- **Moonlight**
- **TizenBrew**
- **Samsung** — returns to Samsung's default Home screen

In the installed Tizen app (`tizen-app/`, shown as **Kunmi Home**), the tiles
use Samsung's Application API to launch the installed native apps. If a known
app ID does not match the TV's regional build, the launcher scans the TV's
installed apps by name before reporting that the app was not found. This
name scan is especially important for MagentaTV, whose Tizen app ID is not
published by Telekom.

When the same page is opened in a normal browser instead of the installed TV
app, YouTube, Disney+, Netflix and MagentaTV fall back to their web players.
Moonlight, TizenBrew and Samsung Home require the installed TV app; the
Samsung tile falls back to exiting the dashboard if Samsung Home cannot be
launched directly.

## Upstream owner's TV

- **Samsung 50" Crystal UHD U8000F (2025)**, model `UN50U8000FFXZC`, Tizen OS

## The honest part: what works on Samsung, and what doesn't

The original project debloats over **ADB** (`pm disable-user`) and installs a custom
launcher. That only works on **Android TV / Google TV**. The original repo's own
supported-TV table says it plainly: Samsung (Tizen) has **no ADB**, so there is no
package disabling and no replacement launcher on this TV.

So the Samsung version of "clean your TV" is:

| Goal from the tweet | Samsung (Tizen) equivalent | Status |
| --- | --- | --- |
| Remove junk apps | Delete / Remove-from-Home the pre-installed apps in the TV UI | ✅ Done |
| Kill ad tracking & telemetry | Viewing Information Services OFF, Interest-Based Advertising OFF | ✅ Done |
| Block ads at network level | Ad-blocking DNS `94.140.14.14` (AdGuard) | ✅ Done |
| Custom home screen | A permanent web dashboard in the TV's Internet browser | ✅ Done — see below |
| Dashboard survives power-off | Auto Power Off OFF, Sleep Timer OFF, Autorun Last App ON | ✅ Done |
| ADB debloat + custom launcher | Not possible on Tizen — scripts included for any future Android TV | 📦 In `android-tv/` |

Full step-by-step for my exact model: [`samsung/U8000F-cleanup.md`](samsung/U8000F-cleanup.md)

## The dashboard (permanent)

A single self-contained page — live clock, Thornhill weather (Open-Meteo,
refreshes every 15 min), XEQT & VFV (TSX) plus NIFTYBEES & INFY (NSE) with a
TradingView live ticker, and a YouTube Music card. Weather is mandatory/always on.

- **Live:** https://kdega77.github.io/cleanyourtv-samsung/
- Also published at: https://kdega77.github.io/home-dashboard/
- Source: [`index.html`](index.html) in this repo

On the TV: open the **Internet** app → go to the live link → **Add to Home** +
bookmark. With the keep-awake settings above, the TV reopens it by itself —
no phone, no mirroring, no sign-in.

## Android TV owners

If a TV or streamer in the house ever runs Android TV / Google TV, the tweet's
full method applies. This repo carries a conservative, reversible version:

- [`android-tv/debloat.sh`](android-tv/debloat.sh) — disables (never uninstalls)
  common bloat/telemetry packages over ADB; every line is commented and
  reversible.
- [`android-tv/restore.sh`](android-tv/restore.sh) — one-shot undo of everything
  the debloat script disabled.
- Or just use the original project: https://github.com/Gathin23/cleanyourtv

## Re-do this on any TV

[`PROMPT.md`](PROMPT.md) is a fill-in prompt for an AI coding assistant — hand it
the TV model and it works out the equivalent cleanup + dashboard for that set,
the same way this repo was made.

## Undo (Samsung)

- DNS back to Automatic: Settings → Connection → Network → Network Status →
  IP Settings → DNS → Get automatically.
- Deleted apps can be reinstalled from the TV's Apps store.
- Privacy toggles can be switched back on in General & Privacy → Terms & Privacy.

---

*Personal project. Not affiliated with Gathin23/cleanyourtv — credit to them for
the original idea and repo.*
