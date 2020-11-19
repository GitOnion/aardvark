---
disqus: hackmd
---
{%hackmd @docs/base-theme %}

# Hotkeys in HackMD

## Switching modes

![](https://i.imgur.com/ZyONHC0.png)

Using hotkeys to switch between edit and preview mode allows you to quickly see how the note would look like. 

| Mode         | Windows              | Mac                     |
| ------------ | -------------------- | ----------------------- |
| Edit         | `Ctrl` + `Alt` + `e` | `Ctrl` + `Option` + `e` |
| Both (Split) | `Ctrl` + `Alt` + `b` | `Ctrl` + `Option` + `b`    |
| View         | `Ctrl` + `Alt` + `v` | `Ctrl` + `Option` + `v`    |

## Text emphasis

Use hotkeys to give selected text typographic emphasis.

| Style | Windows | Mac |
|--|--|--|
| Bold | `Ctrl`+`b` | `Cmd` + `b` |
| Italic | `Ctrl`+`i` | `Cmd` + `i` |
| Strikethrough | `Ctrl` + `Shift` + <code>&#x60;</code> | `Ctrl` + `Cmd` + `k` |

## IDE-like hotkeys

HackMD supports three different flavors of IDE-like hotkeys: Sublime, Emacs, and Vim. You can change them at the bottom of the editor.

![](https://i.imgur.com/GBez2xQ.png)

### Sublime style hotkeys
| Description                     | Windows/Linux                     | Mac                           |
| ------------------------------- | --------------------------------- | ----------------------------- |
| Go Line Start Smart             | N/A                               | `Cmd` + `Left`                |
| Indent                          | `Tab`                             | `Tab`                         |
| Indent Less                     | `Shift` + `Tab`                   | `Shift` + `Tab`               |
| Delete Line                     | `Shift` + `Ctrl` + `K`            | `Shift` + `Ctrl` + `k`        |
| Wrap Lines                      | `Alt` + `Q`                       | `Option` + `Q`                |
| Transpose Chars                 | `Ctrl` + `T`                      | N/A                           |
| Go Subword Left                 | `Alt` + `Left`                    | `Option` + `Left`             |
| Go Subword Right                | `Alt` + `Right`                   | `Option` + `Right`            |
| Scroll Line Up                  | `Ctrl` + `Up`                     | `Ctrl` + `Option` + `Up`      |
| Scroll Line Down                | `Ctrl` + `Down`                   | `Ctrl` + `Option` + `Down`    |
| Select Line                     | `Ctrl` + `L`                      | `Cmd` + `L`                   |
| Split Selection By Line         | `Shift` + `Ctrl` + `L`            | N/A                           |
| Single Selection Top            | `Esc`                             | `Esc`                         |
| Insert Line After               | `Ctrl` + `Enter`                  | `Cmd` + `Enter`               |
| Insert Line Before              | `Shift` + `Ctrl` + `Enter`        | `Shift` + `Cmd` + `Enter`     |
| Select Next Occurrence          | `Ctrl` + `D`                      | `Cmd` + `D`                   |
| Select Scope                    | `Shift` + `Ctrl` + `Space`        | `Shift` + `Cmd` + `Space`     |
| Select Between Brackets         | `Shift` + `Ctrl` + `M`            | `Shift` + `Cmd` + `M`         |
| Go To Bracket                   | `Ctrl` + `M`                      | `Cmd` + `M`                   |
| Swap Line Up                    | `Shift` + `Ctrl` + `Up`           | `Cmd` + `Ctrl` + `Up`         |
| Swap Line Down                  | `Shift` + `Ctrl` + `Down`         | `Cmd` + `Ctrl` + `Down`       |
| Toggle Comment Indented         | `Ctrl` + `/`                      | `Cmd` + `/`                   |
| Join Lines                      | `Ctrl` + `J`                      | `Cmd` + `J`                   |
| Duplicate Line                  | `Shift` + `Ctrl` + `D`            | `Shift` + `Cmd` + `D`         |
| Sort Lines                      | `F9`                              | `Cmd` + `F5`                  |
| Sort Lines Insensitive          | `Ctrl` + `F9`                     | `Cmd` + `F5`                  |
| Next Bookmark                   | `F2`                              | `F2`                          |
| Prev Bookmark                   | `Shift` + `F2`                    | `Shift` + `F2`                |
| Toggle Bookmark                 | `Ctrl` + `F2`                     | `Cmd` + `F2`                  |
| Clear Bookmarks                 | `Shift` + `Ctrl` + `F2`           | `Shift` + `Cmd` + `F2`        |
| Select Bookmarks                | `Alt` + `F2`                      | `Alt` + `F2`                  |
| Smart Backspace                 | `Backspace`                       | `Backspace`                   |
| Skip And Select Next Occurrence | `Ctrl` + `K` `Ctrl` + `D`         | `Cmd` + `K Cmd` + `D`         |
| Del Line Right                  | `Ctrl` + `K` `Ctrl` + `K`         | `Cmd` + `K Cmd` + `K`         |
| Upcase At Cursor                | `Ctrl` + `K` `Ctrl` + `U`         | `Cmd` + `K Cmd` + `U`         |
| Downcase At Cursor              | `Ctrl` + `K` `Ctrl` + `L`         | `Cmd` + `K Cmd` + `L`         |
| Set Sublime Mark                | `Ctrl` + `K` `Ctrl` + `Space`     | `Cmd` + `K Cmd` + `Space`     |
| Select To Sublime Mark          | `Ctrl` + `K` `Ctrl` + `A`         | `Cmd` + `K Cmd` + `A`         |
| Delete To Sublime Mark          | `Ctrl` + `K` `Ctrl` + `W`         | `Cmd` + `K Cmd` + `W`         |
| Swap With Sublime Mark          | `Ctrl` + `K` `Ctrl` + `X`         | `Cmd` + `K Cmd` + `X`         |
| Sublime Yank                    | `Ctrl` + `K` `Ctrl` + `Y`         | `Cmd` + `K Cmd` + `Y`         |
| Show In Center                  | `Ctrl` + `K` `Ctrl` + `C`         | `Cmd` + `K Cmd` + `C`         |
| Clear Bookmarks                 | `Ctrl` + `K` `Ctrl` + `G`         | `Cmd` + `K Cmd` + `G`         |
| Del Line Left                   | `Ctrl` + `K` `Ctrl` + `Backspace` | `Cmd` + `K Cmd` + `Backspace` |
| Fold All                        | `Ctrl` + `K` `Ctrl` + `1`         | `Cmd` + `K Cmd` + `1`         |
| Unfold All                      | `Ctrl` + `K` `Ctrl` + `0`         | `Cmd` + `K Cmd` + `0`         |
| Unfold All                      | `Ctrl` + `K` `Ctrl` + `J`         | `Cmd` + `K Cmd` + `J`         |
| Add Cursor To Prev Line         | `Ctrl` + `Alt` + `Up`             | `Ctrl` + `Shift` + `Up`       |
| Add Cursor To Next Line         | `Ctrl` + `Alt` + `Down`           | `Ctrl` + `Shift` + `Down`     |
| Find Under                      | `Ctrl` + `F3`                     | `Cmd` + `F3`                  |
| Find Under Previous             | `Shift` + `Ctrl` + `F3`           | `Shift` + `Cmd` + `F3`        |
| Find All Under                  | `Alt` + `F3`                      | `Alt` + `F3`                  |
| Fold                            | `Shift` + `Ctrl` + `[`            | `Shift` + `Cmd` + `[`         |
| Unfold                          | `Shift` + `Ctrl` + `]`            | `Shift` + `Cmd` + `]`         |
| Find Incremental                | `Ctrl` + `I`                      | `Cmd` + `I`                   |
| Find Incremental Reverse        | `Shift` + `Ctrl` + `I`            | `Shift` + `Cmd` + `I`         |
| Replace                         | `Ctrl` + `H`                      | `Cmd` + `H`                   |
| Find Next                       | `F3`                              | `F3`                          |
| Find Prev                       | `Shift` + `F3`                    | N/A                           |



:::info
:bulb: **Hint:**
:::

:::warning
:warning: **Note**: 
:::


###### tags: `tutorials`