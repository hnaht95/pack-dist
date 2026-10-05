# pack-dist

Appcast và bản cài cho **[Pack](https://github.com/hnaht95/Pack)** — app nén ảnh
trên thanh menu của macOS. Repo này công khai để Sparkle đọc được `appcast.xml`;
mã nguồn nằm ở repo `pack`, private.

- `appcast.xml` — feed Sparkle, ký bằng khoá EdDSA
- Releases — file `.dmg` để cài mới, và file `.zip` mà Sparkle tải về khi cập nhật

Cài mới thì tải `Pack-<phiên bản>.dmg` ở [Releases](https://github.com/hnaht95/pack-dist/releases/latest)
của repo này. Đừng gửi link Releases của repo `Pack`: repo đó private nên ai không
đăng nhập bằng tài khoản chủ cũng chỉ thấy trang 404 - kể cả link tải trực tiếp.

## Phát hành bản mới

```bash
cd native
./build.sh --notarize
ditto -c -k --keepParent dist/Pack.app <feed>/Pack-<ver>.zip
# đặt <feed>/Pack-<ver>.html nếu muốn có mô tả trong hộp thoại cập nhật
.build/artifacts/sparkle/Sparkle/bin/generate_appcast <feed> \
  --download-url-prefix "https://github.com/hnaht95/pack-dist/releases/download/v<ver>/"
```

Rồi đẩy `appcast.xml` lên repo này và đính **cả** `.zip` lẫn `dist/Pack-<ver>.dmg` vào
Releases cùng tag - zip cho Sparkle, DMG cho người cài mới (như `shot-dist` vẫn làm).

Mỗi lần chạy chỉ để **một** file `.zip` trong thư mục feed. Để nhiều bản cùng
lúc thì `generate_appcast` viết URL của mọi mục theo cùng một
`--download-url-prefix`, nên mục của bản cũ sẽ trỏ nhầm sang thư mục release của
bản mới; nó cũng sinh thêm file delta mà nếu không đính kèm thì Sparkle tải
không ra. Một mục cho mỗi lần phát hành là đủ — không ai cần cập nhật ngược về
bản cũ.
