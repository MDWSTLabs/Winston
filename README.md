<p align="center">
  <img src="docs/winston.png" alt="Winston the corgi" width="160">
</p>

<h1 align="center">Winston</h1>

<p align="center"><b>Bootable USB drives, made simple.</b><br>
Pick an image (or pick a distro), pick a drive, press START.</p>

---

## Why "Winston"?

Winston is my very special corgi boy. He also likes to have a job to do. So, hes now the face of the brand.


<div align="center">
  <img src="(docs/winston-photo.jpg" alt="Winston" width="500">
</div>

## What is it?

Winston is a friendlier front end for creating bootable USB drives. It is a fork of
[**Rufus**](https://github.com/pbatard/rufus) by Pete Batard ([rufus.ie](https://rufus.ie)), and it keeps
everything that makes Rufus reliable: the partitioning, formatting, bootloader handling, ISO and
raw-image writing, UEFI/BIOS support, Windows installer tweaks, and so on. **Winston changes how you
drive it, not how it writes to the disk.**

> Winston is an independent project. It is not affiliated with, endorsed by, or supported by Rufus,
> Akeo Consulting or Pete Batard. Please do not report Winston problems to the Rufus project.

## What's different from Rufus

### 1. Simple mode by default, expert mode one click away

Rufus shows every option at once. Winston opens with three steps:

1. **Select or drop an image**
2. *(a small graphic shows the data flowing to the drive)*
3. **Choose the destination drive**, then **START**

Everything else is chosen for you:

| Setting | Simple mode |
|---|---|
| Partition scheme / target system | Auto-detected from the image and drive |
| File system / cluster size / label | Chosen automatically |
| Quick format | On |
| Create extended label and icon files | On |
| Check device for bad blocks | Off (1 pass) |

Tick **Expert mode** (bottom right) and you get the full, traditional Rufus dialog with every option:
partition scheme, target system, volume label, file system, cluster size, persistence, advanced drive and
format options. Your last choice is remembered. Switching back and forth is non-destructive: the traditional
layout is restored exactly as you left it.

### 2. One-click Linux downloads

Open the dropdown on the **SELECT** button and pick a distribution. Winston finds the current release,
downloads it, verifies it, and loads it as if you had selected it yourself:

| Distribution | Image |
|---|---|
| CachyOS | Desktop edition |
| Linux Mint | Cinnamon edition |
| Debian | Network installer (amd64) |
| Fedora | Workstation Live |
| Bazzite | Stable |
| Arch Linux | Latest monthly ISO |
| elementary OS | Latest stable |

- **Never goes stale.** Release numbers are not hard-coded; each entry asks the distribution's own
  infrastructure for the current release at the moment you click.
- **Verified.** Where the distribution publishes a SHA-256 (all of the above except elementary OS), the
  download is checked and deleted if it doesn't match.
- **Reusable.** Images are kept in `Downloads\Winston`. If the copy there still matches its checksum, it
  is reused, so making a batch of identical drives only downloads once.

### 3. Quieter and tidier

- No update-policy prompt, and Winston never checks rufus.ie for a "newer version" that would replace it.
- Language, settings and info are three clear icons at the bottom left, using the supplied corgi artwork.
- Own name, icon, window titles, version and settings location, so it can sit alongside Rufus.

## Using it

1. Run `winston.exe` (it asks for administrator rights, like Rufus, because it writes to disks).
2. Drop an ISO on the window, click **SELECT**, or choose a distro from the SELECT dropdown.
3. Choose the destination drive.
4. Press **START**. **Everything on the destination drive is erased.**

## Building from source

Winston builds with MSYS2 / MinGW-w64, exactly like Rufus:

```bash
# from an MSYS2 MINGW64 shell
pacman -S --needed mingw-w64-x86_64-toolchain base-devel autotools git upx
git clone https://github.com/MDWSTLabs/Winston.git
cd Winston
./configure --disable-debug
make -j"$(nproc)"
# the result is src/rufus.exe - copy or rename it to winston.exe
```

## Status

Winston is an early, personal project.

- The simple / expert interface and the distro downloader build cleanly, and the distro resolvers were
  tested against the live servers.
- **The new interface has not yet had a full interactive test pass** (all the dialogs, languages, dark mode
  and high-DPI scaling). Expect rough edges and please open an issue.
- Always double check the destination drive. Winston will happily erase the wrong one if you pick it.

## Credits and license

- **Rufus** is Copyright © 2011-2026 Pete Batard and contributors, released under the GPL v3.
  The original README is kept in [`docs/README.rufus.md`](docs/README.rufus.md).
- Josh Ryan (josh@mdwst.cloud)'s changes are released under the same license. See [`LICENSE.txt`](LICENSE.txt) (GNU GPL v3).
- Rufus's own data (Secure Boot revocation lists, the Windows ISO download script and some bootloader
  files) is still fetched from the Rufus project's servers, as in upstream.
- Winston Ryan: CBO (Chief Barking Officer)
