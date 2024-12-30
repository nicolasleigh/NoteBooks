Change all the file extensions in the current directory: [How do I change the extension of multiple files?](https://unix.stackexchange.com/questions/19654/how-do-i-change-the-extension-of-multiple-files)

```sh
rename "s/oldExtension$/newExtension/" *.txt
```

Rename files from `.en.srt` to `.srt`:

```sh
rename "s/.en.srt$/.srt/" *.srt
```

Rename files from `.en.srt` to `.srt` recursively: [Find multiple files and rename them in Linux](https://stackoverflow.com/questions/16541582/find-multiple-files-and-rename-them-in-linux)

```sh
find . -iname "*srt*" -exec rename "s/.en.srt$/.srt/" '{}' \;
```

```sh
find . -iname "*vtt*" -exec rename "s/[[:space:]]*English.vtt$/.vtt/" '{}' \;
```

```sh
find . -iname "*srt*" -exec rename "s/[[:space:]]*English.srt$/.srt/" '{}' \;
```
