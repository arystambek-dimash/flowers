# 🌸 Open me

A one-page surprise. You press **"Open me"** — flowers come down from the top
as one solid sheet, cover the screen completely, and when the sheet passes,
a puzzle made of your photo is already standing behind it. The screen is fully
covered at that moment, so the swap is never seen — like a magician with
a curtain.

And right there, behind the curtain, the downpour ends for good. A garden grows
in its place: grass along the bottom edge, flowers on stems that open as you
scroll, and butterflies and bees flying above them. From then on flowers only
fall in the rare celebratory seconds — when the puzzle is solved, when the heart
is broken, when she says yes.

Music starts with that very first tap and never stops again. Nothing here is a
recording: the page carries five short pieces of its own — *first light*, *slow
dance*, *the long walk home*, *three in the morning*, *you said yes* — each in a
different key, tempo and instrument colour. They play one after another in a
shuffled order, and when all five have played the order is shuffled again, so
the music neither runs out nor starts repeating itself. What is on right now is
written on the vinyl label and on the strip under the turntable. Drop your own
mp3 files next to the page and they take over as a playlist instead.

Everything leads itself from there. Solve the puzzle and the page praises her
and carries her down to the turntable with your song. Below that, the **story
plays out frame by frame**: how she followed first, how he froze for nine days,
how he finally worked up the nerve, the first date, and how ordinary days became
shared ones. At the end he says: "there's something I want to tell you" —
"what?" — "scroll down".

And below that is **a heart you have to break**. One click: it cracks, bursts
into shards, and behind it the question is waiting: **"Will you be my
girlfriend?"**, where the "no" button jumps away from her finger. After "yes"
flowers and hearts rise, a letter unfolds, and the last line leads out of the
screen into real life — to where you are already standing with an actual bouquet.

It is all one file, `index.html`. No build, no dependencies, no images or sounds
pulled from the internet — the flowers are drawn as vectors right in the browser,
the two characters are embedded artwork, and the music is synthesised on the fly.

In this version the characters were drawn to look like the two of you (him:
wavy dark hair and a maroon-and-white track jacket; her: long dark hair,
sunglasses on top of her head and a white blouse), and the story plays like a
motion-graphics explainer: the actors slide in from opposite edges, their lines
hang in bubbles right above their heads and travel with them, every frame gets
a numbered title chip ("03 · I WENT FOR IT"), the camera eases in between acts,
the narration flies in word by word, a giant counter ticks off the days, and a
progress bar with a "frame N / 12" readout tracks where you are. The scene is
alive: trees sway, birds cross the daytime frames while the sun's rays turn,
fireflies come out in the evening, and a band of foreground grass gives depth.

The characters and the puzzle photo are embedded directly in `index.html`, so
the page works as a single file, without the folder. On reload the page returns
to the top on its own, back to the "Open me" button. Before the step out into
real life there is a pause — "close your eyes… one, two, three" — exactly long
enough to pick up the bouquet. The photo of the two of you is already in
`photos/1.jpg` and set in the puzzle (the second option is `photos/2.jpg`, swap
it in `CONFIG.photo`). The character sources are `boy_recraft.svg` and
`girl_recraft.svg`; the transparent versions used on stage are `boy_char.png`
and `girl_char.png`.

## Your own songs

Without a Premium login in the browser, the Spotify embed plays only 30 seconds
— that is Spotify's own limit and nothing on the page can work around it. So the
page plays full files of its own instead: drop mp3s next to `index.html` and
list them in `CONFIG.musicFiles`:

```js
musicFiles: ['music.mp3', 'music2.mp3', 'music3.mp3', 'music4.mp3'],
// or, to give a track a nicer name than its filename:
musicFiles: [{ file:'music.mp3', title:'the one from the car' }, 'music2.mp3'],
```

Every file listed is probed once; whichever ones are actually there become a
playlist that is shuffled, played through, and started over again, for as long
as the page is open. The seam between tracks is a crossfade, not a gap — two
audio elements take turns so the next track is already buffered. Files that are
not there are dropped silently, and if none of them answer (or the browser
refuses to autoplay them) the five built-in pieces play instead. Silence is the
one outcome the page will not produce.

(If she is logged into Spotify Premium in that browser, the full track plays
right in the embed too.)

## How to open it

Just double-click `index.html`.

If you want the photo from the `photos/` folder to load, you need a local server
(the browser will not serve local files to a page opened as `file://`):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## What to customise

Everything is at the top of the `<script>` in `index.html`, in the `CONFIG` block:

| What | How |
|---|---|
| **Song** | `CONFIG.spotify` — paste the id of a track or a playlist. It comes from the link: `open.spotify.com/track/`**`0tgVpDi06...`**. For a playlist set `type: 'playlist'`. |
| **Your songs** | `CONFIG.musicFiles` — a list of mp3 files next to `index.html`. The ones that exist become a shuffled playlist that loops forever. Leave it empty and the five built-in pieces play instead. |
| **Record label text** | `CONFIG.song` — the fallback title and the line printed under it on the vinyl label. |
| **Puzzle photo** | Put a picture in `photos/` under the name `1` — any extension (`.jpg`, `.jpeg`, `.png`, `.webp`), the page finds the right one. With no file there, a drawn placeholder is shown. Square photos look best. |
| **Story** | `CONFIG.story` — an array of frames. Each has: `act` — which scene (1 he is alone, 2 she followed, 3 he walks over, 4 the date, 5–7 together), `boy` and `girl` — their lines, `cap` — the narrator's caption, `title` — the frame headline, `wait` — how long to hold the frame in milliseconds. Plus: `walk` — he is walking, `freeze` — he stands there twitching, `days` — run the day counter, `show` — what to reveal (`notif`, `think`, `sweat`, `days`), `last` — the final frame. |
| **Puzzle praise** | `CONFIG.puzzleDone` and `CONFIG.puzzleNext`. |
| **Heart** | `CONFIG.heart` — the heading, the hints, and what to write above the question once the heart is broken. |
| **Question** | `CONFIG.proposal` — the question itself, the button labels, the excuses of the dodging "no", and what appears after "yes". |
| **Letter** | `CONFIG.letter` — an array, one line per paragraph. The signature is `CONFIG.signature`. |
| **Step into real life** | `CONFIG.irl` and `CONFIG.irlSub` — the last lines, after which you hand over the real flowers. |

## What happens on screen

- **Flowers.** `buildFlower()` assembles a flower out of petals: 6 shapes (peony,
  rose, anemone, zinnia, blossom, ranunculus), random palettes, random petal
  counts and rotations. A pool of 80 such flowers is baked into images once, and
  from then on the downpour is drawn as sprites on a single canvas — that is how
  there can be close to a thousand of them instead of the hundred and a half you
  would get by drawing each as its own element.
- **Trajectories.** Six flight characters: a straight fall, a zigzag, a glide
  with drift, a petal's flutter, a tumble over the edge, and a spiral. Plus three
  depth planes: the far ones are small, blurred and slow; the near ones large and
  fast.
- **The density adapts.** The engine watches the frame time and, if the device
  cannot keep up, quietly thins out the far plane; when there is room to spare it
  brings the density back.
- **The curtain.** Its flowers are not scattered at random: the count is derived
  from the screen area and the size from its shorter side (otherwise on a wide
  monitor the same flowers are too small and leave gaps). They are laid out on a
  grid above the screen and fall at almost identical speeds — spread would
  delaminate the sheet and punch a hole right through the middle of it. Measured
  coverage at the moment of the swap is 98–100%.
- **A garden instead of the downpour.** As soon as the curtain has passed,
  `stopRain()` stops returning flowers to the top and the ones still in the air
  calmly land. From then on the screen is held by grass (cut to the window width
  and swaying in the wind), a bed of growing flowers, and butterflies. A butterfly
  has three nested layers of movement — the crossing of the screen, the up-and-down
  bobbing, and the tilt — which is what makes the flight look alive instead of a
  ride along a straight line. The wings flap separately.
- **Music.** A synthesiser of its own on Web Audio, playing five pieces that take
  turns. Each piece has its own key, chord progression, tempo and voice — a bell
  that rings long and bright, rounder keys, a pluck whose filter closes as it
  decays, and glass that hangs quietly in the air. Notes come from a pentatonic
  scale and the melody walks near its previous note, which makes a line rather
  than a set of beeps; each piece resolves on a high note in its last bar so the
  handover to the next one sounds intended. Notes are scheduled a second and a
  half ahead, so the rhythm stays even; if the tab is backgrounded and the timer
  frozen, the loop simply carries on from "now". Start your own song on Spotify
  and our sound steps back but never switches off.
- **The story** is not a chat log but a little play. A frame says only who stands
  where and what is shown; the moves between positions are done by CSS, which is
  why a scene change looks like movement rather than slides being clicked through.
  The sky turns from day to evening, stars come out, and the ground sinks into
  dusk. Frames can be stepped through by hand if you would rather not wait.
- **The turntable** — wooden plinth, felt slipmat, grooves and bands between the
  tracks, a tonearm with a counterweight and a cartridge head. The lamp glare sits
  on its own motionless layer: it does not spin with the disc, which is what makes
  vinyl read as vinyl.
- **The heart.** The shards are wedges clipped to the heart's own outline; the
  clip travels with the wedge, so a piece flies off in its own shape rather than
  as a rectangle. First the cracks run, then the heart shudders and bursts, and
  behind it is the card with the question.
- **The question.** The "no" button jumps away on every attempt, cracks a joke and
  after five tries disappears entirely. "Yes" always sits above it in the stacking
  order, so the dodging button can never steal the tap at the moment that counts.
- **Touches.** A tap anywhere blooms a flower under your finger.
- **The puzzle** — a 3×3 grid, tiles swap on tap. When it all lines up, the whole
  photograph fades in over the top.
- **After "yes"** — a salute of flowers and rising hearts, the letter types itself
  out letter by letter, and last of all comes the line addressed to real life.

Works on a phone, and honours `prefers-reduced-motion`.
