![Altuntun logo banner](./banner.png)

# Altuntun

**Altuntun** is an implementation of the [WireGuard<sup>®</sup>](https://www.wireguard.com/) protocol, and consists of two parts:
* The executable `altuntun-cli`, a [userspace WireGuard](https://www.wireguard.com/xplatform/) implementation for Linux and macOS.
* The library `altuntun` that implements the underlying WireGuard protocol, without the network or tunnel stacks that need to be that need to be implemented in a platform idiomatic way.

## Why Altuntun
I created cableguard using boringtun as a starting point, and I found boringtun to be a barely usable implementation of wireguard. These are my complaints:
- When boringtun can´t create the file boringtun.out when starting, it fails silently. This normally happen because it is the first time you run, but the second. This is for me unexpected behaviour, poor error handling, and to annoyed me to have to troubleshoot it.
- It does not support namespaces.
- Its performance can be improved, but the PR to fix it was pending for several months.
- I have not seen the maintainers being very responsive by any reasonable measure.
- The maintainers don´t work actively with the main wireguard C/Linux maintainers.

## Differences between Altuntun and Boringtun
- I refactored the name of many variables for readability.
- Applied outstanding PRs to add namespaces, improve performance, and fix the spurious re-keying bug
Most dependencies have been updated except:
- clap
- base64::encode

##Improvements pending
- Use tokio instead of the current approach (someone tried here: https://github.com/lz1998/wg-rs)
- Error type should support StdError and Display

## License
This project is released under the [GPLv2](COPYING).
More information may be found at [WireGuard.com](https://www.wireguard.com/).**

### Contribution
Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the 3-Clause BSD License, shall be licensed as above, without any additional terms or conditions.

If you want to contribute to this project, please contact <vpn@cableguard.org>.

## How to Install from Source
- sudo apt install pkg-config
- git clone https://github.com/cableguard/altuntun.git
- cargo build --bin altuntun-cli --release
By default the executable is placed in the `./target/release` folder. You can copy it to a desired location manually, or install it using `cargo install --bin altuntun --path .`.

You may want to add to .bashrc these lines:
- sudo setcap cap_net_admin+epi ./<path>/altuntun-cli

## How to Install from .deb package
wget https://altuntun.fra1.digitaloceanspaces.com/altuntun-cli_0.89.99_amd64.deb
sudo apt install ./altuntun-cli_0.92.58_amd64.deb

## How to Use
To start a tunnel use:
`altuntun-cli [-f/--foreground] ADDFROMBORINGTUN`

`altuntun` will drop privileges when started. When privileges are dropped it is not possible to set `fwmark`. If `fwmark` is required, such as when using `wg-quick`, run with `--disable-drop-privileges` or set the environment variable `WG_SUDO=1`.
You will need to give the executable the `CAP_NET_ADMIN` capability using: `sudo setcap cap_net_admin+epi altuntun`.

You can use [wg-quick](https://git.zx2c4.com/WireGuard/about/src/tools/man/wg-quick.8) by setting the environment variable `WG_QUICK_USERSPACE_IMPLEMENTATION` to `altuntun`. For example:
`sudo WG_QUICK_USERSPACE_IMPLEMENTATION=altuntun-cli WG_SUDO=1 wg-quick up CONFIGURATION`

## Supported platforms
- It has only been tested in AMD/Intel
- `x86-64` architecture is supported.

---
<sub><sub><sub><sub>WireGuard is a registered trademark of Jason A. Donenfeld. Altuntun or endorsed by Jason A. Donenfeld.</sub></sub></sub></sub>
