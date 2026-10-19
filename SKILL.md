# Interactive MV

Build a web page that plays a song as an interactive MV. The proven shape is a full screen canvas stage with code drawn characters, a lyric rail with karaoke fill, an interaction bar, and a branching ending. The method was proven on a 3 minute 59 second song with 14 shots and 38 timestamped lyric lines.

## Collect the inputs

- Ask the user for the audio file and the lyric timeline before building anything.

- Read lyric timestamps from the MP3 LYRICS tag with `ffprobe` when the file carries one. Fall back to an LRC file or a JSON list of start time plus text pairs.

- Define the cast as draw functions with fixed faces, such as a boy drawn as wind and a girl drawn as a leaf. Keep every shot calling the same functions so the characters stay consistent.

## Shrink the audio and harden playback

- Re-encode the song with `ffmpeg -i in.mp3 -codec:a libmp3lame -b:a 128k out.mp3`. A 4 minute song drops from about 10 MB to about 4 MB, which is the difference between playing and stalling on mobile networks.

- Verify with `ffprobe` that the new file keeps the exact duration and still carries its title and LYRICS tags. The page timeline is driven by `audio.currentTime`, so even a one second drift breaks lyric sync.

- Never let a failed `play()` fail silently. Show a loading state on the play button until the audio is ready, auto start when it arrives, and re-fetch with a hint if it stalls. Silent catch on `play()` was the exact bug that made taps feel dead.

## Build the interactive page

- Drive everything from one `requestAnimationFrame` loop that reads `audio.currentTime`. Shots, lyric highlight, karaoke fill and particle systems all derive from that clock, so seeking stays in sync for free.

- Build the interaction set from these proven pieces. Tap the stage to burst particles. Press and drag to make wind that bends particles, scarves and a wind meter. Tap a character to pop a dialogue bubble. Scatter collectible items such as golden leaves with a counter. Add a wind slider, weather switcher, full screen particle rain, floating hearts, and a danmaku input whose messages fly across the stage.

- Render karaoke lyrics as large text with per word fill timed to the lyric timestamps. Keep a tappable lyric list that seeks the audio and auto scrolls to the current line.

- Trigger the ending choice at a fixed timestamp near the song end. Each branch swaps the final shot set, such as a reunion with petals versus a lone leaf drifting away, and offer a replay button to try the other ending.

- Show live interaction stats, such as wind count and hearts sent, at the top and bottom. They cost little and make the page feel alive.

## Verify with a probe before delivery

- Drive the built page with a probe and confirm each of these. Tapping play flips the button to pause and the clock advances. Tapping a lyric seeks and the list scrolls to the right line. Triggering the ending jump lands on the right shot with lyrics in sync.

- Pointer gestures like drag wind and danmaku cannot be probed directly. Confirm they share code paths with the verified particle and counter systems instead of claiming they were tested.

- Run the platform audit and fix every console error before handing the page over.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- When the user wants to share the MV somewhere that blocks the public link, such as WeChat chat, export the page as one self contained HTML file with `artifact.export` and send the file itself. Remind them to open it with an explicit browser app, since tapping it directly can fail with no registered handler.

- When the user wants the MV as a plain video instead, build the shots with the cartoon MV pipeline and render to MP4. That path gives no interactivity but plays everywhere.
