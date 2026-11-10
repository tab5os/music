# Music

A music player for [Tab5 OS](https://github.com/tab5os/tab5os) on the M5Stack Tab5. It plays MP3, FLAC (up to 192 kHz) and WAV from the SD card's `music` folder.

- Browse by song, album and artist, from the files' tags (ID3v1 and v2, Vorbis comments, WAV INFO).
- Album art from the files, or from a `cover.jpg` in the album folder.
- M3U playlists, read from anywhere and made and edited in `music/playlists`.
- A queue with play next, shuffle and repeat, and a seekable progress bar.
- No gap between tracks of the same format, and playback resumes where it left off.
- The standby screen's music card controls it.

It needs Tab5 OS with app API level 1 and asks for the `shared_read` and `shared_write` permissions. The OS has no MP3, FLAC or JPEG decoder, so the app brings its own: see [third_party](third_party/README.md).

## Build

You need the [Tab5 OS SDK](https://github.com/tab5os/tab5os/blob/main/docs/sdk.md): a tab5os checkout with a firmware build, or the SDK release. The compiler is ESP-IDF v6.1's RISC-V toolchain.

```sh
t5 app-build .            # build/com.t5.example.music.t5app
t5 run .                  # build, push to a paired Tab5, launch, stream the log
```

Or with CMake directly: `cmake -S . -B build -G Ninja -DT5_SDK=<sdk dir> && cmake --build build`.

To install by hand, copy the `.t5app` to the SD card and tap it in Files.

## Tests

Tag, playlist and library parsing are plain C and run on the host, with no SDK:

```sh
cmake -S test -B test/build -G Ninja && cmake --build test/build && ctest --test-dir test/build
```

CI runs them on every push.

## Icon fonts

`music_icons_48.c` and `music_icons_80.c` hold the player's icons that the OS icon font leaves out. `python3 gen_fonts.py` regenerates them, and needs Node and a tab5os checkout next to this repo (or `TAB5OS=<path>`).

## History

This app was an example in the tab5os repo (`apps/examples/music`) until Nov 10, 2026. Its history came with it.

## License

Apache 2.0, see [LICENSE](LICENSE). The bundled decoders have their own licences, listed in [third_party](third_party/README.md).
