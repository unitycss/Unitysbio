# unity.cs — bio site

A single-page bio card: full-bleed looping video background, a glass panel with
your avatar, name/username, and a 3-line bio, plus a mini music player pinned
to the bottom-right corner that spins your track's artwork.

## Structure

```
unity-bio/
├── package.json
├── vercel.json
└── public/
    ├── index.html        ← everything (markup/CSS/JS) lives in this one file
    ├── avatar.jpg        ✅ your cat photo (profile + DVD screensaver sprite)
    ├── rainy-forest.mp4  ✅ your background video
    ├── song1.mp3         ✅ Hate Me — Ellie Goulding ft. Juice WRLD
    ├── song2.mp3         ✅ Righteous — Juice WRLD (plays first, on load)
    ├── song3.mp3         ✅ Wishing Well — Juice WRLD
    └── song4.mp3         ✅ "Nope, You're Too Late, I Already Died"
```

All five assets are in and wired up. The page's color palette (deep pine
`#0b1712`, glass panel tint `#15291a`, leaf-green accent `#9fd88c`, misty teal
`#3c6155`) was sampled directly from frames of `rainy-forest.mp4`, so the
panel, avatar ring, and player sit inside the same color world as the video
instead of looking pasted on top of it.

## Running locally

```bash
cd unity-bio
npx serve public
```

## Deploying to Vercel

```bash
npm i -g vercel
cd unity-bio
vercel
```

When prompted for the output directory, point it at `public` (or just accept
the defaults — `vercel.json` is already configured for a static site). Every
asset (`avatar.jpg`, `rainy-forest.mp4`, `album.jpg`, `track.mp3`) needs to sit
inside `public/` since that's what gets deployed.

## Notes on behavior

- **Video background** runs as two stacked `<video>` elements. The active one
  plays normally; in the last 1.4s before it ends, the second copy starts
  playing from frame 0 underneath and the two cross-fade via
  `requestAnimationFrame`. That blend hides the hard cut — it's not a true
  motion-matched loop (the source clip isn't edited to loop), but the fade
  reads as smooth instead of an abrupt jump.
- **Playlist** now has four tracks (`song1.mp3` – `song4.mp3`, in order:
  Hate Me → Righteous → Wishing Well → "Nope, You're Too Late, I Already
  Died"). Playback **starts on the second track (Righteous)**. It
  auto-advances when one ends and loops back to the top after the last one.
  Prev/next buttons sit next to the track name.
- **Disc icon** is now a generic vinyl-record SVG (grooves, label, a little
  music-note mark) instead of per-song artwork — it works for every track
  without needing individual album art, and it's the thing that spins/pauses
  with playback.
- **Volume** starts at 40% and is adjustable via the slider behind the
  speaker icon.
- **Cursor** is a small hand-drawn tree-branch SVG (twig + leaves) in place of
  the system arrow.
- **Avatar** effects: a pulsing soft-glow halo, a slowly rotating wreath of
  six SVG tree branches, and the spinning gradient ring — plus the photo
  itself rotates slowly inside its circular mask.
- **Tabs**: Bio (short tagline — Unity 5+ yrs, Blender 1+ yr, C# 2+ yrs, XR /
  PC / UI / particles & VFX, with a pointer to the Professions tab),
  Professions (the same experience broken into rows), and Links (Dawnworks
  Games Discord button). Switching tabs swaps content with a single fade.
- **DVD screensaver** — the new monitor-icon button in the player toggles a
  bouncing copy of your avatar photo that ping-pongs around the browser
  window and shifts hue on every wall bounce, classic DVD-logo style. It's
  a pure CSS-transform + `requestAnimationFrame` loop, purely decorative,
  and stays off until you click the button.
- **Disc art** spins continuously while playing and freezes mid-rotation when
  paused. Click it again to resume.
- **Close (✕)** collapses the player down to just the spinning disc; clicking
  the disc again reopens the full player and resumes normal controls.
- All motion respects `prefers-reduced-motion` and is disabled for users who
  have that OS setting on (the DVD screensaver isn't auto-started regardless,
  so this mainly affects the ambient effects).
