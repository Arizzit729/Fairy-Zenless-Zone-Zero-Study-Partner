# Project Fairy

Everything lives in this one folder. Double-click `fairy-timer.html` (Firefox) and it works.

```
Project Fairy/
  fairy-timer.html
  fairy_video.mp4          <- loaded automatically, loops forever
  fairy voice lines/       <- welcome-1..30, procras-1..7, break-1..10,
                              work-1..8, hwork-1..6, health-1..6,
                              random-1..7, rare-1..7,
                              10pm-1..3, 12am-1..3, 1am-1..3, 2am-1..3, 3am-1..3,
                              opening-shortcuts, opening-achievements, opening-analytics
                              (all .mp3)
                              work-sound.mp3  (work session start sound)
  README.md
```

Names must match exactly (`break-3.mp3`, not `break_3.mp3`). A missing line is skipped quietly (a window still opens, it just opens without her line). A missing video shows a notice.

## When fairy speaks
- **Welcome**: once every time the page is opened.
- **Opening a window**: pressing H (Shortcuts), C (Achievements) or A (Analytics), or clicking their buttons, makes her say `opening-shortcuts`, `opening-achievements` or `opening-analytics` first. The interface fades out while she talks and the window opens when she is done. Closing a window has no line, and extra presses while she is talking are ignored.
- **Time reminders**: when the clock crosses 10 PM, 12 AM, 1 AM, 2 AM or 3 AM she says a random `10pm-`, `12am-`, `1am-`, `2am-` or `3am-` line. They only fire when the page is open and the clock really crosses the hour (opening the page at 10:20 PM does not trigger 10 PM, and waking the computer much later does not announce an old hour). Each hour is said once per day, even with two copies of the page open.
- **Procrastination**: when the "Come back to work" alarm fires (timer idle for a random 1-10 minutes). She says one line, once, then the blue screen appears silently. Hold Space for 3 seconds to dismiss it. If you start working while she is talking, the screen is skipped.
- **Break**: once at the start of every break.
- **During work**: 2 or 3 times per work hour, at least 10 minutes apart, never in the first or last 12 minutes. The kind of line depends on how much you have worked today:
  under 3 h `work`, 3-4 h `hwork`, 4-5 h `health`, 5-6 h `random`, 6-7 h `rare`. From 7 h on, every spoken moment is one line from the `health`, `random` and `rare` lines together, drawn from a single shuffled deck of all 20 (so one hour might give health + rare, the next random + health, any combination, and no line comes back until all the others have been heard). It is still only 2 or 3 moments per work hour.
- **Lines go round like a shuffled deck.** For every kind (welcome, break, work, ...) each line plays once, in a random order, before any line is heard again; then the deck is reshuffled and the first line of the new round is never the one that just played. From 7 h on, the health, random and rare lines share one combined deck. The decks are remembered between page loads (saved as `chillTimer.voiceBag`) and are read fresh on every draw, so two open copies of the page deal from the same deck, so even the once-per-open welcome lines go round evenly. Missing files simply use up their turn.
- While she talks, everything except the video fades away and comes back when she finishes. If you are busy in the UI (typing, moving around the task or deadline lists, or a window such as Analytics, Shortcuts or Achievements is open) the UI stays and she only talks.

## One voice at a time
Two lines never play together, and a line that is playing is never cut off. If something else is due while she is talking, it waits and goes next, in this order of priority:
1. time reminders (10 PM to 3 AM). Even if a break, a work line or the away alarm is due in the very same second, the reminder is checked first and goes first
2. the welcome greeting
3. opening a window (H, C, A), which waits behind the welcome greeting if that is about to play (for example when your very first action is a click on a window button)
Procrastination, break and work lines are never allowed to interrupt. If she is busy: a break line waits its turn and plays right after (unless the break has already ended); a work line is postponed and plays as soon as she is quiet, unless that work session ends first; a procrastination line is skipped, and the alarm screen simply appears silently once she finishes. The work-start sound is also skipped while she is talking. Popups (achievements, level-ups, Z and backup notices) also wait until she has finished, so you never miss one behind the faded-out screen and no chime plays over her voice.

## Good to know
- **Selecting a task with the mouse**: click anywhere on a task row (its name, tag or time) to highlight it, the same as moving to it with the arrow keys. Then G (track), 1-4 (subject), Enter (tick) and Delete work on it. Click the same task again to let go of it (like Esc). Clicking the tick box or the x still ticks or deletes as before.
- **Tab icon**: the round blue icon in the browser tab is built into `fairy-timer.html` itself (a small embedded PNG), so no extra file is needed. Firefox caches tab icons, so after an update you may need to close the tab and open the page again to see a change.
- **Note to self**: a small read-only panel of its own, styled exactly like the other panels (sharp corners, glow, corner brackets), centred in the gap between the task list and the clock (always at least 12 px clear of both, it never overlaps). It shows the newest note you wrote on an earlier day (yesterday's, or the last one if you skipped days) with how long ago it was. Today's own note never appears there; it moves in from tomorrow. It fades and hides together with everything else (while she talks, and with V). It hides itself on narrow windows (1100 px or less) and when the gap between the task list and the clock is too small for it.
- **Video sleeps in the background**: when you switch to another tab or minimize the window, only the video pauses (to save resources). The timer, her voice lines, reminders and the away alarm keep running, and the video carries on from the same frame when you come back.
- **Sound at start-up**: Firefox only allows sound after a first key press or click. If it blocks the welcome line, she greets you right after your first key press instead. A time reminder blocked the same way is said after your next key press if it is less than 15 minutes old. To make it fully automatic, in Firefox open `about:config` and set `media.autoplay.default` to `0` (this allows sound without a click on every page).
- **Your saved data belongs to this exact folder location.** Firefox keeps it per file path, so a new folder starts empty. To move your history: in the old page press X (backup, saved as `fairy-backup-<date>.json`), open this page and press I (restore). Do the same if you ever move or rename this folder. Backups made before the rename (`chill-timer-backup-...`) still restore fine.
- **Reset today (R in Analytics)** sets today's work time and task time to zero and puts the work countdown back to 1h 0m 0s, not started. If today had counted as a day you reached your daily goal, that is taken back too (it counts again if you reach the goal again).
- **Work-start sound**: `work-sound.mp3` (in the `fairy voice lines` folder) plays whenever a work session begins, whether you start it (W or Space) or the break ends. The sound switch (M inside Achievements) does not affect it. It is skipped when the key press that starts work is also the first key press of the page (that is the moment fairy greets you), and while she is talking. Breaks have no sound; fairy announces them. If the file is missing nothing plays.
- **V** hides everything except the video and keeps it hidden until you press V again. The alarm screen and any open window (Analytics, Shortcuts, Achievements) still show. It resets when the page is reloaded.
- **Z** (away alarm on/off) and backup messages show a small plain "System" notice, not an achievement card.
- **Weekly-total achievements** (30 / 50 / 70 / 90 / 110-Hour Week) look at the best stretch of any 7 days in a row in your whole history, plus the last 7 days up to now, as their description says.
- **Heatmap arrow keys** (Analytics, tab 2): with a square selected, left / right move one week sideways (same weekday) and up / down move one day inside the week. They stop at the edges instead of wrapping.
- **Sound switch (M inside Achievements)** turns only the achievement sounds on or off: the achievement chime and the level-up sound. Her voice, the work-start sound, and the task-tick sound always play.
