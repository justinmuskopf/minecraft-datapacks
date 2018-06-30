# minecraft-datapacks
Assorted vanilla Minecraft datapacks.

The datapacks are:

 * `coords.zip` - shows player coordinates in the action bar.
 * `waypoints.zip` - manages 62 player-set waypoints plus automatically updated bed and death location waypoints.
 * `help.zip` - provides a function that implements a basic help command by calling help functions from the other datapacks.

## Installation

Simply download `coords.zip` and place it in the `world/datapacks/` folder under the server directory.

For custom waypoints and the `!help` command, download `waypoints.zip` and `help.zip` and place them in `world/datapacks/` then configure the `mark2` server wrapper per the [Mark2 Configuration](https://github.com/totemo/minecraft-datapacks/tree/master#mark2-configuration) section below.


## Custom Commands

Custom commands are supported using the [mark2 server wrapper](https://github.com/gsand/mark2)'s trigger feature. Currently, without mark2 or some equivalent server wrapper to handle triggers in chat, players cannot use the custom commands described below.

 * `help.zip` datapack:
   * `!help` - Show custom command help.
 * `coords.zip` datapack:
   * `!coords` - Turn off/on coordinate display.
 * `waypoints.zip` datapack:
   * `!bed` - Get your bed coordinates.
   * `!death` - Get your latest death coordinates.
   * `!setwpX` - Store waypoint _X_ as your current location. X can be 0..9, a..z or A..Z (62 waypoints). Examples: `!setwp9`, `!setwpz`, `!setwpZ`
   * `!delwpX` - Delete waypoint _X_. _X_ as per `!setwpX`.
   * `!wpX` - Get the coordinates of waypoint _X_. "Dim" (dimension) in the output is: -1 = nether, 0 = overworld, +1 = end. Examples: `!wp1` = get `!setwp1` and `!wpa` = get `!setwpa`
   * `!listwps` - List all of your waypoints.

## Mark2 Configuration

To use `triggers.txt` to run custom commands, add the following to `mark2.properties` in the directory containing the server JAR:
```
plugin.trigger.enabled=true
plugin.trigger.path=triggers.txt
plugin.trigger.command=execute as {user} run {message}
```

The start of `triggers.txt` should read as follows:
```
!help,function io.totemo.help:help-command

!coords,function io.totemo.coords:coords-command
!bed,function io.totemo.waypoints:bed-command
!death,function io.totemo.waypoints:death-command
!listwps,function io.totemo.waypoints:listwps-command

!delwp0,function io.totemo.waypoints:delwp0-command
!setwp0,function io.totemo.waypoints:setwp0-command
!wp0,function io.totemo.waypoints:wp0-command

# ... !delwp, !setwp and !wp entries for 61 more waypoints ...
```

The [mark2/gentriggers.sh](https://github.com/totemo/minecraft-datapacks/blob/master/mark2/gentriggers.sh) script generates a `triggers.new` file that can be appended to `triggers.txt` to supply all of the custom commands for waypoints 0..9, a..z and A..Z.
