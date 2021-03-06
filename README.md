# BMTranslator

Converts your BMS levels to zip archives that you can import into other rhythm games.

## How to Use

Output to Quaver: `bmt.exe -i /path/to/input -o /path/to/output`

Output to osu: `bmt.exe -i /path/to/input -o /path/to/output -type osu`

**NOTE: Don't set the output (`-o`) to your game's Songs folder!** Create a separate output folder first, then import by dragging them into the game.

## Options

| Option | Arguments? | Description  | Default |
| ------------ | ---- | ---------- | ---- |
|  `-i` | Yes | Path of input folder containing FOLDERS of bms charts (NOT zip files!!!). If the folder is nested in another folder, just leave it, BMT will automatically handle that.  | N/A |
|  `-o` | Yes | Path to output the converted files to. | N/A |
|  `-vol` | Yes | Volume of hit sounds. (0-100) | 100 |
|  `-type` | Yes | Which type of file to convert to. You can choose `quaver` or `osu`. | quaver |
|  `-hp` | Yes | **osu! only.** Specify the HP drain rate. (0.0-10.0) | 8.5 |
|  `-od` | Yes | **osu! only.** Specify the overall difficulty. (0.0-10.0) | 8.0 |
|  `-v` | No | If this is specified, all logs (including some debug information) will be shown. Useful if you want to know why some maps didn't convert. | N/A |
|  `-auto-scratch` | No | If this is specified, all notes in the scratch lane will be replaced with sound effects instead, and the scratch lane will not be shown in all clients.
|  `-keep-subtitles` | No | If this is specified, [implicit subtitles](https://hitkey.nekokan.dyndns.info/cmds.htm#TITLE-IMPLICIT-SUBTITLE) will **not** be removed from song titles. | N/A |
|  `-no-storyboard` | No | **osu! only.** If this is specified, background animation frames won't be parsed or inserted into the output files. | N/A |
|  `-no-measure-lines` | No | If this is specified, timing points will **not** be added at the end of each track to create visible measure lines. (It's a cosmetic thing and doesn't affect gameplay, but it might make slowjam unreadable. Some BMS files' notes will appear unsnapped if this is enabled.) | N/A |
|  `-no-timing-points` | No | If this is specified, **no** timing points will be added to the output file. This means no SV changes and is useful for SV maps which don't convert correctly. | N/A |
|  `-dump-file-data` | No | If this is specified, raw file data will be dumped to a `.txt` file, which is put into the output folder. Each file will contain everything that BMTranslator knew about a BMS file. (Don't enable this unless you know what you're doing) | N/A |

## Credits

This tool wouldn't be possible without help from others. Thank you to:

- [Swan](https://github.com/Swan) for helping me with .qua files
- [yahweh](https://osu.ppy.sh/users/10465260) for moral support and for being a cool friend
- [mat](https://osu.ppy.sh/users/6283029) for helping me with the math side of things
- [Jakads](https://osu.ppy.sh/users/259972) for helping with 0 BPM change in osu
- [hitkey](https://hitkey.nekokan.dyndns.info) for their very helpful [BMS notes](https://hitkey.nekokan.dyndns.info/cmds.htm)
- Various people on the osu! forums who explained how timing points in osu! work.

## Limitations

- `#IF 1` will always be used. All other `#IF` blocks will be ignored. If an `#IF N` block where `n` is not 1 is never terminated with `#ENDIF`, the rest of the track will not be read.
- `#SWITCH` blocks can't be parsed (yet). If people want this i'll add it in.
- Almost no BMS maps use long notes in channels `51-59`, they use `#LNOBJ`. As a result, LNs placed in channels `51-59` are **untested**, but they are implemented. If you find a problem with them, please open an issue.
- BMS maps that use images as frames for the Background Animation can't be reliably parsed if the frames are <1ms apart, since osu! requires truncation of the decimal.
- If a BPM change occurs at any point within a STOP command, BMTranslator will still be able to parse the map, but the timing of the rest of the song will most likely be fucked. *However*, this has not appeared in a single map that I've tested, and by this reasoning, I think the only way to do this is by editing a BMS file by hand.