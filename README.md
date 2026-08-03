# Online Study Room

Clickable prototype of the study-room flow. Single self-contained page —
no build step, no dependencies.

**▶️ Live demo: <https://belendali.github.io/study-room-prototype/>**

Design source: [the Figma file](https://www.figma.com/design/HjtuqIuyQFAMiGywZqqU4P/?node-id=57-2381)
— section `07/2026`. Frames worth knowing:

| Frame | Node | What |
| --- | --- | --- |
| `study room` | `57:2496` | Room picker — stats, carousel, dots |
| `Cozy library` | `57:2660` | Duration half-sheet over the blurred room |
| `Cozy library` | `62:2723` | Focusing state — name tags, activity feed |
| `giveup` | `62:2851` | Give-up confirm sheet |
| `cozy library bg` | `803:8806` | **Empty-seat patch positions** |

> This is a design prototype, not production code: one HTML file, positions
> hardcoded from the design, fake data and timers. Treat it as an executable
> spec for layout, motion and interaction — not as a starting codebase.

## Run it

```bash
python3 -m http.server 8642
```

Then open <http://localhost:8642/>.

> Videos are loaded from disk, so opening `index.html` directly with `file://`
> will not play them. Serve the folder over HTTP as above.

## The flow

| Screen | What it does |
| --- | --- |
| **Learning Hub** | Entry tiles; *Online Study Room* opens the picker |
| **Study Room** | Total focus + online count, big carousel of the four rooms |
| **Pick a duration** | Entering a room blurs it and raises a half-sheet: 25 / 45 / 60 min or custom, plus your name |
| **Focusing** | Live countdown, name tags floating over the room, rolling activity feed |
| **Session Complete** | Lottie celebration, focused-vs-goal stats, then an award popup |

## Empty-seat effect

When someone leaves, their name tag fades out and a cut-out of the *empty* desk
fades in over the video, so the seat visibly clears. Seats churn automatically
every 7s while focusing — **tap any name tag** to make that person leave or come
back on demand.

Seat coordinates come from the Figma frame `cozy library bg` (`803:8806`), which
lays the patches over the room's 375×667 still. The room video is displayed at
exactly that geometry (`375×667` at `top: -82px`), so design coordinates map 1:1
onto the screen. In the design each name tag sits ~20px above-left of its own
desk — that offset is what pairs a person to a seat.

The cut-outs are traced from the Cozy Library artwork, so the overlay only
applies to that room; elsewhere a leaver just loses their tag.

## Known gaps

- Name-tag positions are the Cozy Library set for **all four rooms**, so tags do
  not line up with the characters in Galaxy / Rainy / Woodland. The design has
  per-room coordinates (`57:2820`, `57:2880`).
- Empty-seat cut-outs exist for Cozy Library only.
- Who leaves and when is faked on a 7s timer; tapping a name tag toggles that
  person on demand (handy for demoing).

## Layout

```
index.html            the whole prototype
assets/               icons, mascot, Lottie files, blank-*.png seat cut-outs
cover video/          room covers for the picker carousel
study video/          in-room footage + ambient audio
```
