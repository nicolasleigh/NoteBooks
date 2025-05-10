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

MP4 to HLS - Simple:

```sh
ffmpeg -i input.mp4 \
  -codec: copy \
  -start_number 0 \
  -hls_time 5 \
  -hls_list_size 0 \
  -f hls output.m3u8
```

MP4 to HLS - Complex:

```sh
ffmpeg -i input.mp4 -filter_complex \
"[0:v]split=3[v1][v2][v3]; \
 [v1]scale=w=1920:h=1080[v1080]; \
 [v2]scale=w=1280:h=720[v720]; \
 [v3]scale=w=854:h=480[v480]" \
-map "[v1080]" -c:v:0 libx264 -b:v:0 5000k -preset veryfast -g 48 -sc_threshold 0 \
-map "[v720]"  -c:v:1 libx264 -b:v:1 2800k -preset veryfast -g 48 -sc_threshold 0 \
-map "[v480]"  -c:v:2 libx264 -b:v:2 1400k -preset veryfast -g 48 -sc_threshold 0 \
-map a:0 -c:a:0 aac -b:a:0 128k \
-map a:0 -c:a:1 aac -b:a:1 128k \
-map a:0 -c:a:2 aac -b:a:2 128k \
-f hls -hls_time 6 -hls_playlist_type vod \
-hls_segment_filename "output%v_data%03d.ts" \
-master_pl_name master.m3u8 \
-var_stream_map "v:0,a:0 v:1,a:1 v:2,a:2" output%v.m3u8
```

```sh
ffmpeg -i input.mp4 -filter_complex \
"[0:v]split=2[v1][v2]; \
[v1]scale=w=1280:h=720[v1out]; [v2]scale=w=640:h=360[v2out]" \
-map "[v1out]" -c:v:0 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:0 1500k -maxrate:v:0 2000k  -bufsize:v:0 3000k -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map "[v2out]" -c:v:1 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:1 400k -maxrate:v:1 600k  -bufsize:v:1 800k -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map a:0 -c:a:0 aac -b:a:0 96k -ac 2 \
-map a:0 -c:a:1 aac -b:a:1 48k -ac 2 \
-f hls \
-hls_time 2 \
-hls_playlist_type vod \
-hls_flags independent_segments \
-hls_segment_type mpegts \
-hls_segment_filename "output%v_data%03d.ts" \
-master_pl_name master.m3u8 \
-var_stream_map "v:0,a:0 v:1,a:1" output%v.m3u8
```

