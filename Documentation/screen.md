# Screen

## Installation

`sudo apt-get install screen`

## Command

* Simply run `screen` to start it
* Everything starts with `Ctrl + A`.
    * `Ctrl + A` + `c` -> **create** a new virtual console
    * `Ctrl + A` + `n` -> go to **next** virtual console
    * `Ctrl + A` + `p` -> go to **previous** virtual console
    * `Ctrl + A` + `k` -> kill current virtual console
    * `Ctrl + A` + `?` -> show command list
    * `Ctrl + A` + `"` -> lists all available virtual consoles and their assigned numbers
    * `Ctrl + A` + `"number"` -> brings the corresponding virtual console to the foreground
* Detach screen by running `Ctrl + A` + `d`
* Resume screen by running `screen -r`
* Terminate screen by running `Ctrl + A` + `\`
