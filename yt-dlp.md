# yt-dlp 

インストール
```sh
# pip
python3 -m pip install -U yt-dlp

# Homebrew
brew install yt-dlp
```

動画を最高画質のMP4形式またはMP4が利用不可の場合それ以外の形式でダウンロードする
```sh
yt-dlp -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4] / bv*+ba/b" [URL]
```

コーデックはそのままでメディアコンテナをmp4でダウンロードする
```sh
yt-dlp [URL] --remux-video mp4
```

動画の音声だけをダウンロードし、メタデータやアルバムアートも付加して保存する。
`--parse-metadata "%(title)s:%(album)s"`でアルバム名に動画タイトルを使うようにしている。
```sh
yt-dlp \
    -x --audio-quality 0 \
    --embed-thumbnail \
    --embed-metadata \
    --parse-metadata "%(title)s:%(album)s" \
    -o "%(uploader)s/%(uploader)s - %(title)s.%(ext)s" \
    [URL]
```
