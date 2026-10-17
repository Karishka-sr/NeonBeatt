# NeonBeat

**NeonBeat** is a modern rhythm game for Android inspired by osu!mania and classic vertical-scrolling rhythm games.  
Notes fall down multiple lanes — tap them in perfect sync with the music. The game includes fully procedural built-in tracks, a powerful map editor, support for custom audio, and a complete scoring system.

- **Minimum Android version:** 7.0 (API 24)
- **Target SDK:** 34
- **Orientation:** Portrait only
- **Language:** Kotlin
- **Dependencies:** None (pure Android SDK)

---

## Features

### Gameplay
- Classic vertical-scrolling rhythm gameplay with 4–7 keys
- Support for both tap notes and hold (long) notes
- Multi-touch input
- Five difficulty levels: **Easy**, **Normal**, **Hard**, **Expert**, **Insane**
- Health system with drain on misses and regeneration on Perfect hits
- Combo counter and maximum combo tracking
- Judgment system: **Perfect**, **Great**, **Good**, **Miss**
- Rank system based on accuracy
- Audio latency calibration
- Auto-play mode (for testing)
- No-fail option

### Built-in Content
- 5 original tracks with fully procedural music generation
- Each track has unique mood, BPM, and instrumentation
- Charts are generated procedurally based on the music structure

### Map Editor
- Create new maps from scratch
- Support for 4–7 keys
- Manual note placement
- Auto-generation of charts
- Snap-to-grid
- Support for custom audio files
- Built-in tracks can also be used as a base
- Save and load maps in `.nbm` format

### Technical
- Completely written in pure Kotlin
- No external libraries
- No Jetpack Compose — UI is drawn with classic `View` + `Canvas`
- Custom audio engine with precise timing
- Onset detection for chart generation
- Lightweight and runs well on older devices

---

## Built-in Tracks

| Track             | BPM  | Style                     | Description                          |
|-------------------|------|---------------------------|--------------------------------------|
| Neon Horizon      | 128  | Neon Synthwave            | Atmospheric and melodic              |
| Pixel Rush        | 150  | Chiptune / Pixel          | Fast and energetic                   |
| Midnight Drive    | 100  | Atmospheric / Chill       | Dark and driving                     |
| Gravity Wave      | 140  | Heavy Bass / Breaks       | Powerful and rhythmic                |
| Hyperdrive        | 174  | Drum & Bass               | High-speed intense track             |

---

## How to Play

1. From the main menu, select **Play**.
2. Choose a track and difficulty.
3. Notes will fall from the top of the screen.
4. Tap the corresponding lane when a note reaches the hit line.
5. For hold notes — press and hold until the note ends.
6. Try to maintain high accuracy and combo for better ranks.

### Tips
- Use multiple fingers for dense patterns.
- If the audio feels early or late, go to **Settings → Latency Calibration** and adjust the offset.
- Higher difficulties have tighter timing windows and faster health drain.

---

## Scoring & Judgments

| Judgment | Timing Window (base) | Score Weight |
|----------|----------------------|--------------|
| Perfect  | ±50 ms               | 1.0          |
| Great    | ±100 ms              | 0.8          |
| Good     | ±150 ms              | 0.5          |
| Miss     | outside windows      | 0.0          |

*Timing windows are scaled depending on difficulty (stricter on higher levels).*

---

## Map Editor

You can create your own maps:

1. Go to **Map Editor** → **+ New Map**
2. Set:
   - Title and artist
   - BPM
   - Number of keys (4–7)
   - Audio source (built-in track or custom file)
3. Place notes manually or use auto-generation tools
4. Use snap-to-grid for precise placement
5. Save the map

Custom maps are stored locally and appear in the song selection list.

---

## Map File Format (`.nbm`)

NeonBeat uses a simple human-readable text format:
NEONBEAT-MAP 1
title=My Awesome Map
artist=My Name
bpm=128.0
offset=0
keys=4
audio=builtin:neon_horizon
length=60000
notes:
1234,0
1500,2,400
2340,1
2500,3,600
### Field description

| Field     | Description                                      |
|-----------|--------------------------------------------------|
| `title`   | Map title                                        |
| `artist`  | Artist name                                      |
| `bpm`     | Beats per minute                                 |
| `offset`  | Audio offset in milliseconds                     |
| `keys`    | Number of lanes (4–7)                            |
| `audio`   | `builtin:<id>`, `file:<filename>` or empty       |
| `length`  | Approximate length in milliseconds               |
| `notes:`  | List of notes                                    |

### Note format
- Regular note: `time_ms,lane`
- Hold note: `time_ms,lane,duration_ms`

Lanes are numbered starting from **0**.

---

## Building the Project

### Requirements
- JDK 17
- Android SDK 34
- Android Studio (recommended) or command line tools

### Build commands

```bash
# Debug APK
./gradlew assembleDebug

# Release APK
./gradlew assembleRelease
APK files will be located in:
app/build/outputs/apk/debug/
app/build/outputs/apk/release/
GitHub Actions
The repository includes a ready-to-use GitHub Actions workflow (.github/workflows/build.yml) that builds both debug and release APKs on every push.
Project Structure
NeonBeat/
├── app/
│   └── src/main/
│       ├── java/com/ryxens/neonbeat/
│       │   ├── MainActivity.kt          # Entry point & navigation
│       │   ├── Env.kt                   # Settings, records, map storage
│       │   ├── audio/
│       │   │   └── Audio.kt             # Audio decoding & precise clock
│       │   ├── core/
│       │   │   ├── Engine.kt            # Game logic, judgments, scoring
│       │   │   ├── Models.kt            # Data classes & enums
│       │   │   ├── Composer.kt          # Procedural music composition
│       │   │   ├── Synth.kt             # Sound synthesis
│       │   │   ├── ChartGen.kt          # Chart generation
│       │   │   ├── Songs.kt             # Built-in song definitions
│       │   │   ├── MapCodec.kt          # .nbm format read/write
│       │   │   └── OnsetDetector.kt     # Audio onset detection
│       │   └── ui/
│       │       ├── MenuScreens.kt
│       │       ├── GameScreen.kt / GameView.kt
│       │       ├── EditorScreen.kt / EditorView.kt
│       │       ├── MapScreens.kt
│       │       ├── SettingsScreens.kt
│       │       └── Ui.kt                 # Common UI helpers
│       ├── res/                         # Icons and theme
│       └── AndroidManifest.xml
├── build.gradle
├── settings.gradle
└── .github/workflows/build.yml
Technical Details
UI: Completely custom, drawn with Canvas. No XML layouts, no Compose.
Audio: Custom timing system synchronized with audio playback. Supports both synthesized music and external files.
Music Generation: Tracks are composed and synthesized at runtime using a custom engine (drums, bass, lead, pads, etc.).
Chart Generation: Can generate playable charts based on the musical structure and onset detection.
Storage: Settings, high scores and custom maps are stored locally.
License
This project currently has no specified license.
You are free to use, modify and distribute it for personal or educational purposes.
