gtk4-playground
===============
```
pacman -S mingw-w64-ucrt-x86_64-gtk4
pacman -S mingw-w64-ucrt-x86_64-toolchain base-devel
gcc $(pkg-config --cflags gtk4) -o hello-world-gtk main.c $(pkg-config --libs gtk4)
```

  windows:
    name: Windows
    runs-on: windows-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup MSYS2
        uses: msys2/setup-msys2@v2
        with:
          msystem: UCRT64
          update: true
          install: mingw-w64-ucrt-x86_64-gtk4 mingw-w64-ucrt-x86_64-libadwaita mingw-w64-ucrt-x86_64-python-gobject mingw-w64-ucrt-x86_64-python-yaml mingw-w64-ucrt-x86_64-python-requests mingw-w64-ucrt-x86_64-python-pillow mingw-w64-ucrt-x86_64-desktop-file-utils mingw-w64-ucrt-x86_64-ca-certificates mingw-w64-ucrt-x86_64-meson git

      - name: Compile
        shell: msys2 {0}
        run: |
          meson setup _build
          ninja -C _build install
          pacman --noconfirm -Rs mingw-w64-ucrt-x86_64-desktop-file-utils mingw-w64-ucrt-x86_64-meson git
