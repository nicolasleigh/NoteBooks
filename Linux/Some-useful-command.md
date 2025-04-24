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

```sh
psql -U <dataBaseUserName> <dataBaseName>
```

```sh
# Show all the running Caddy processes
ps aux | grep caddy

# Example outputs
# nicolas   824072  0.0  1.9 1266748 37840 ?       Sl   Apr04   0:01 caddy run --pingback 127.0.0.1:35775
# nicolas   829157  0.0  0.1   6560  2368 pts/0    S+   10:12   0:00 grep --color=auto caddy

# Kill the Caddy process
kill -9 824072

# Then start Caddy
systemctl start caddy
```

```sh
# Build react-native android apk file
# Step 1
npx react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res

# Step 2 - Generated apk file is located at "android/app/build/outputs/apk"
# option 1: create debug build
cd android && ./gradlew assembleDebug && cd ..
# option 2: create release build
cd android && ./gradlew assembleRelease && cd ..
```

```sh
# import data into mongodb database
mongoimport --uri <MONGODB_CONNECTION_STRING> --collection <collection_name> --type json --jsonArray --file <file_name>.json
```

