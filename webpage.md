## google.com

```css
/* 検索結果の下に出てくる「他の人はこちらも検索」を非表示 */
.g [id^=eob_] {
  display: none;
}
```

## youtube.googleapis.com

```css
/* Google Driveとかの動画の色を反転してダークテーマにする */
video {
  filter: invert(1) hue-rotate(180deg);
}
```
