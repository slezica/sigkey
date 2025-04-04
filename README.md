# sigkey

Use a global hotkey to toggle (pause/resume) a target process using `SIGSTOP` and `SIGCONT`. Useful to control
cpu-intensive background processes or large downloads while working (or gaming).

## Installation

Just grab a copy of the `sigkey` python script from the `bin` directory, and put it in your `$PATH`.
It requires `python3` with the `psutil` and `pynput` packages installed.

```bash
cd sigkey
pip install -r requirements.txt
cp bin/sigkey $HOME/bin
```

## Usage

Call `sigkey <pid>` to toggle the target PID using F12 (the default). You will hear a bell
when paused, a double-bell when resumed.

See [options](#Options) below and [examples](#Examples) at the end.

### Options

Use `-k <key>` (or `--key`) to customize the global hotkey.
```bash
sigkey --key ctrl+alt+k <pid>
```

Use `-2` (or `--twice`) to require a double hotkey press.
```bash
sigkey --twice <pid>
```

Use `-q` (or `--quiet`) to silence the default bell.
```bash
sigkey -q <pid>
```

Use `--before-stop` and `--before-cont` to run commands before each signal is sent. If these commands
exit with non-zero status, the signal is skipped.
```bash
sigkey --before-cont 'false' <pid> # will never sent SIGCONT (super useful)
```

Use `--after-stop` and `--after-cont` to run commands after each signal is sent.
```bash
sigkey --after-cont 'echo yay' <pid>
```

### Hotkey Syntax

Valid key descriptions for the `-k` (or `--key`) flag follow these rules:

- Single-character keys are literal, like `a` or `]`
- Special keys are multi-character names like `ctrl` or `f1`
- Key sequences are joined by `+`, like in `ctrl+shift+a`
- All hotkeys are case-insensitive


### Special key names

- `alt`, `alt_l`, `alt_r`, `alt_gr`
- `cmd`, `cmd_l`, `cmd_r`
- `ctrl`, `ctrl_l`, `ctrl_r`
- `shift`, `shift_l`, `shift_r`

- `space`, `enter`, `tab`, `backspace`, `delete`, `esc`, `caps_lock`
- `up`, `down`, `left`, `right`, `home`, `end`, `page_up`, `page_down`
- `f1`, `f2`, `f3`, ... `f20`

- `media_play_pause`, `media_volume_mute`, `media_volume_down`
  `media_volume_up`, `media_previous`, `media_next`


## Examples

Trigger using F12 (the default) and ring a single/double bell:
```bash
sigkey <pid>
```

Trigger using F2 instead of F12:
```bash
sigkey -k f2 <pid>
```

Trigger using a multi-key combination:
```bash
sigkey -k ctrl+alt+a <pid>
```

Trigger using a double right shift press:
```bash
sigkey -2k shift_r <pid>
```

Play sounds when sending signals instead of using the default bell:
```bash
sigkey --quit --after-stop 'afplay stop.mp3' --after-cont 'afplay cont.mp3' <pid>
```

Run a hook in `/bin` before each `SIGSTOP`, skip when exit status is non-zero:
```bash
sigkey --before-stop '/bin/my-hook' <pid> # SIGCONT will never be sent
```
