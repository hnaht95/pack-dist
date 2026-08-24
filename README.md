# pack-dist

Appcast và bản cài cho **[Pack](https://github.com/hnaht95/Pack)** — app nén ảnh
trên thanh menu của macOS. Repo này công khai để Sparkle đọc được `appcast.xml`;
mã nguồn nằm ở repo `pack`, private.

- `appcast.xml` — feed Sparkle, ký bằng khoá EdDSA
- Releases — file `.zip` mà Sparkle tải về khi cập nhật

Cài mới thì tải `Pack.dmg` ở [Releases của repo pack](https://github.com/hnaht95/Pack/releases).

## Phát hành bản mới

```bash
cd native
./build.sh --notarize
ditto -c -k --keepParent dist/Pack.app <feed>/Pack-<ver>.zip
# đặt <feed>/Pack-<ver>.html nếu muốn có mô tả trong hộp thoại cập nhật
.build/artifacts/sparkle/Sparkle/bin/generate_appcast <feed> \
  --download-url-prefix "https://github.com/hnaht95/pack-dist/releases/download/v<ver>/"
```

Rồi đẩy `appcast.xml` lên repo này và đính file `.zip` vào Releases cùng tag.
