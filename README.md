# Lua Bench

Lua Bench lets you edit College Football 27's play-call scripts while the game is running. These are the Lua files that decide what the CPU calls: `Offense.lua`, `Defense.lua`, and the timeout, audible, penalty and kick scripts. Edit one in any text editor, save it, and the game runs your version from its next play call. No mod build, no restart. Delete your file and the game's own script is back.

It is made for gameplay modders who want to try 4th-down, late-game and special-teams logic quickly before putting it into a mod.

**Download:** [latest release](https://github.com/sdmart3/lua-bench-releases/releases/latest) (`Lua-Bench-<version>.zip`, with its `.sha256.txt`).

For PC College Football 27, game update of October 29 2026.

## Important: where an edit shows

On 1st, 2nd and 3rd down the CPU's normal play calls come from a separate system built into the game, not from these Lua scripts. Your script still runs and the log shows its answer, but the game usually does not use that answer on those downs. Plan on no visible change from a 1st-3rd down play-group edit.

Edits show clearly on:
- 4th down (punt, field goal or go for it),
- the two-minute drill and other late-game situations,
- special teams and the other scripts (timeouts, kicks, penalties, audibles).

## Install

1. Close the game and the Mod Manager.
2. Open the download's `Manual install` folder, copy the `Plugins` folder and paste it into your Mod Manager folder (the one with `MMCModManager.exe`). Choose to replace when Windows asks. You end up with:
   - `<Mod Manager folder>\Plugins\LuaBenchPlugin.dll`
   - `<Mod Manager folder>\Plugins\LuaBench\` (`lua_bench.dll`, `lua-bench.ini`, `plugin.cfg` and a `lua` folder)
3. Launch the game from the Mod Manager. About 30 seconds after the game window appears, `Plugins\LuaBench\lua-bench.log` gets a line `luabench: installed`.

Lua Bench changes nothing until you put a file in `Plugins\LuaBench\lua\`.

## Your first edit: go for it on every 4th down

1. Start any game (Play Now is fine). Lua Bench writes the game's own scripts, with your other mods included, to `Plugins\LuaBench\stock\`.
2. Copy `stock\Offense.lua` into the `lua` folder, so you have `lua\Offense.lua` (check the name is exactly that, not `Offense.lua.txt`).
3. Open it, jump to the last line (Ctrl+End), and paste below everything:

   ```lua
   -- My first Lua Bench edit: on 4th down, always go for it.
   function Determine4thDownDecision(allowedToPunt, allowFieldGoal)
       LUA_DEBUG_STRING("my edit: going for it on 4th down", GETYARDSTOENDZONE())
       SETPLAYGROUP(CO4THSHORT)
   end
   ```

   A second function with the same name further down the file replaces the game's own one.
4. Save. `lua-bench.log` shows `RELOAD Offense ... applied in ... ms`: your file is in the running game.
5. Keep playing until a CPU offense faces 4th down (the 1st and 3rd quarters are clearest). The log shows `LUADEBUG game=1 'my edit: going for it on 4th down 40.0'` each time your function runs, and the CPU should go for it where it used to punt.

README.txt in the download goes further: a deliberate mistake and how the error reads, the log lines, printing from your script, and the settings.

## Mistakes and undo

- A file that does not compile is never used. The game keeps the code it had and the log says, for example, `LUAERROR Offense.lua:3838: ')' expected ... near 'end'` (file and line). `errors.txt` lists the files with an error right now.
- Your file is test-run before it goes into the game, so a file that fails halfway changes nothing.
- Undo one script: delete its file from `lua`. During a game the game's own script is back from its next call.
- Turn it off: `enabled = 0` in `lua-bench.ini` (the game's own scripts from the next game), or empty the `lua` folder.

## Safety

- Lua Bench only reads the files you put in `lua`. No game file, mod file or save is changed.
- Before it connects, it checks that the game's code is exactly what it was made for. If not, it changes nothing and says so in `lua-bench.log`. After a newer game update, check for a newer Lua Bench.
- Lua Bench is for offline play only.

## Remove

Close the game and the Mod Manager, then delete `Plugins\LuaBenchPlugin.dll` and the `Plugins\LuaBench` folder (copy your own files out of `lua` first if you want to keep them).

## Checksum

Each release has `Lua-Bench-<version>.sha256.txt`. To check a download in PowerShell: `Get-FileHash .\Lua-Bench-0.1.0.zip -Algorithm SHA256`.
