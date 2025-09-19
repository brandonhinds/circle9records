# Circle 9 Time Tracker

The time tracker will allow users to submit their daily Circle 9 times to a database, which will keep track of their stats over time.

## Site Storage

Your browser stores all the details for your game in local storage for the site. Here is an example of what the storage looks like after one Twinominoes 3 game:

| Key | Value |
| --- | ----- |
| `site|version` | `5` |
| `twinominoes|categoryTab` | `3` |
| `twinominoes|clockTime|{"numRows":12,"numColumns":6,"cells":"...xx..xxx.......*.......xx.....x.....xx.........xx....................."}` | `16200` |
| `twinominoes|dailyPlusTab` | `Daily` |
| `twinominoes|date` | `6 2 2025` |
| `twinominoes|history|{"numRows":12,"numColumns":6,"cells":"...xx..xxx.......*.......xx.....x.....xx.........xx....................."}` | `{"commands":[["R2C5","_","o"],["R3C5","_","o"],["R3C6","_","o"],["R4C6","_","o"],["R5C6","_","o"],["R8C4","_","o"],["R8C5","_","o"],["R7C5","_","o"],["R6C5","_","o"],["R6C6","_","o"],["R5C6","o","_"],["R5C6","_","o"],["R6C6","o","_"],["R6C5","o","_"],["R7C5","o","_"],["R8C5","o","_"],["R8C4","o","_"],["R6C4","_","o"],["R6C5","_","o"],["R7C5","_","o"],["R8C5","_","o"],["R8C6","_","o"],["R9C4","_","o"],["R10C4","_","o"]],"undoCount":0}` |
| `twinominoes|solved|{"numRows":12,"numColumns":6,"cells":"...xx..xxx.......*.......xx.....x.....xx.........xx....................."}` | `true` |
| `twinominoes|state|{"numRows":12,"numColumns":6,"cells":"...xx..xxx.......*.......xx.....x.....xx.........xx....................."}` | `{"cells":{"R1C1":"_","R1C2":"_","R1C3":"_","R1C4":"x","R1C5":"x","R1C6":"_","R2C1":"_","R2C2":"x","R2C3":"x","R2C4":"x","R2C5":"o","R2C6":"_","R3C1":"_","R3C2":"_","R3C3":"_","R3C4":"_","R3C5":"o","R3C6":"o","R4C1":"_","R4C2":"_","R4C3":"_","R4C4":"_","R4C5":"_","R4C6":"o","R5C1":"_","R5C2":"x","R5C3":"x","R5C4":"_","R5C5":"_","R5C6":"o","R6C1":"_","R6C2":"_","R6C3":"x","R6C4":"o","R6C5":"o","R6C6":"_","R7C1":"_","R7C2":"_","R7C3":"x","R7C4":"x","R7C5":"o","R7C6":"_","R8C1":"_","R8C2":"_","R8C3":"_","R8C4":"_","R8C5":"o","R8C6":"o","R9C1":"_","R9C2":"x","R9C3":"x","R9C4":"o","R9C5":"_","R9C6":"_","R10C1":"_","R10C2":"_","R10C3":"_","R10C4":"o","R10C5":"_","R10C6":"_","R11C1":"_","R11C2":"_","R11C3":"_","R11C4":"_","R11C5":"_","R11C6":"_","R12C1":"_","R12C2":"_","R12C3":"_","R12C4":"_","R12C5":"_","R12C6":"_"},"hintStarCells":{}}` |
| `twinominoes|plusCount` | `0` |

If a hint has been used the following is added to storage:

| Key | Value |
| --- | ----- |
| `twinominoes|hintUsed|{"numRows":12,"numColumns":6,"cells":".......xxx...x.....x..x.....x..x..xx.x*....x...................xx....xx."}` | `true` |

The cells JSON (at least for Twinominoes) is used to track the history, time, and solved state across games. The `twinominoes|categoryTab` field changes depending on which puzzle you are on, but the fields with the cells json stay persistent.

It looks like you cannot directly edit your time, but you can remove whether you used hints or not.

Switching games adds new values to storage, with each game adding `__GAME_NAME__|xxx` keys to local storage.

## Chrome Extensions

I do not think we need to build our own Chrome extension for this. The much simpler option would be to use the [Local Storage Transfer](https://chromewebstore.google.com/detail/local-storage-transfer/mdffmjljeplghdlkmnimliaogmohnfla) extension to take local storage from circle9 to our site.

You simply have both Circle 9 and our site open, use the extension to move the local storage from that site to ours, and now the values are in our site's local storage.

## Tech Stack

I believe we need the following:

- A frontend website.
  - A vertical stack of time input fields so that user can verify/input times for as many puzzles as they want and submit together.
  - [possibly] allow multiple submits, as long as there are no double-ups on times (i.e. user can't submit different times to the same puzzle in 1 day)
  - NB: Icons for the puzzles can be retrieved e.g. https://circle9puzzle.com/matchbox/icon.svg?v=201
- A backend server that can read the site's local storage, get the times, and write to the database.
- A database to store this data.
