<img width="1080" height="814" alt="21aa18861a4d8eb121b3fd8be771292e39ad188bffcf3bc3dbf0baa51d039ec9" src="https://github.com/user-attachments/assets/5de8e43e-07ad-4046-8fd5-050551ee8279" />

# Vim / GVim config — Hướng dẫn sử dụng

Bộ cấu hình Vim & GVim cá nhân, chạy tốt trên **Linux/X11** (đã dùng hằng ngày trên máy
VNC). Mục tiêu: đọc/ghi chú nhanh, ít bấm nhầm, và **không lag** khi mở nhiều tab.

> Trong GVim bấm **`F1`** để xem manual đầy đủ (bản trong file này là bản rút gọn cho
> người mới).

---

## 1. Cài đặt

### Cấu trúc file

```
.vimrc                 -> ~/.vimrc            (cấu hình chính)
.gvimrc                -> ~/.gvimrc           (phần riêng của GVim)
.vim/                  -> ~/.vim/             (plugin + màu + syntax + font)
.cshrc                 -> ~/.cshrc            (không liên quan Vim)
```

### Các bước

```bash
cp .vimrc  ~/.vimrc
cp .gvimrc ~/.gvimrc
mkdir -p ~/.vim && cp -r .vim/* ~/.vim/
```

Sau đó mở **GVim**. Nếu mở bằng `vim` trong terminal thì `.gvimrc` không được nạp —
một số phím tắt riêng của GVim sẽ không có.

### Yêu cầu

| Thành phần | Ghi chú |
|---|---|
| Vim/GVim **8.2+** | có `popup_menu` (menu chèn câu); thiếu thì tự dùng `inputlist` |
| `wmctrl` | phím `F11` (full màn hình ⇄ 1600×900) và tự maximize lúc khởi động |
| `xclip` hoặc `xsel` | chỉ cần khi Vim **không** có `+clipboard` (GVim thường có sẵn) |
| `xrandr` / `xprop` | để `F11` biết độ phân giải & trạng thái full màn hình |
| Font mono có dấu Việt | xem mục 9 |

---

## 2. Cheat sheet — 20 phím dùng nhiều nhất

| Phím | Việc |
|---|---|
| `wl` | bật/tắt panel **WinList** (danh sách tab/file bên trái) |
| `Ctrl-B` | bật/tắt **NERDTree** |
| `Ctrl-Shift-T` | khôi phục tab/cửa sổ vừa đóng (như browser) |
| `Ctrl-W` | đóng cửa sổ hiện tại (đẩy vào stack để khôi phục) |
| `Ctrl-\` | chia dọc + nhảy sang cửa sổ mới |
| `1..9`, `0` | nhảy thẳng tới tab N (`0` = tab 10) |
| `Ctrl-Up/Down` | tab trước / tab sau |
| `Alt-Left/Right` | chuyển giữa các cửa sổ code (bỏ qua panel) |
| `Enter Enter` | phóng to tạm cửa sổ hiện tại (Enter lần nữa để trả lại) |
| `za` | đóng/mở fold đang đứng |
| `zo` | mở hết ⇄ gập về mức mặc định |
| `cc` | bật/tắt comment (Normal: 1 dòng, Visual: vùng chọn) |
| `hh` / `ch` | thêm tag `[haozxje-YYYYMMDD-HHMM]` ở cuối / đầu dòng |
| `mh` / `ml` | đánh dấu (highlight) từ / vùng chọn · xoá hết mark |
| `dd` / `dm` | chèn câu mẫu (GVim) / mở menu chọn câu |
| `Ctrl-C` / `Ctrl-V` / `Ctrl-X` | copy / paste / cut qua clipboard hệ thống |
| `F1` | manual đầy đủ |
| `F11` | full màn hình ⇄ cửa sổ 1600×900 ở giữa |
| `F12` | copy đường dẫn đầy đủ của file đang mở |
| `:R a --- b` / `:D x` | thay thế / xoá **toàn bộ** chuỗi trong file |

---

## 3. Điều hướng

| Phím | Việc |
|---|---|
| `Ctrl-Left/Right` | nhảy theo từ (Normal/Visual/Insert) |
| `Ctrl-Shift-Left/Right` | mở rộng vùng chọn theo từ |
| `Shift-Arrow/Home/End` | chọn text kiểu GUI |
| `Home` | về ký tự đầu tiên (không phải cột 1) |
| `n` / `N` | kết quả tìm kế/trước + tự căn giữa màn hình |
| `Ctrl-O` | mở netrw ngay trong cửa sổ hiện tại; bấm lần 2 để quay lại file cũ |

> Các phím `a h j k l s c v d x p o` bị **disable ở Normal mode trong GVim**
> (trong `.gvimrc`) để một lần gõ lệch không xoá mất chữ. Điều hướng bằng phím
> mũi tên/chuột hoặc các phím ở trên. Chạy `vim` trong terminal thì các phím đó
> vẫn bình thường.

---

## 4. Tab & cửa sổ

| Phím | Việc |
|---|---|
| `Ctrl-\` | chia dọc, focus cửa sổ mới |
| `Ctrl-W` | đóng cửa sổ → đẩy vào stack (nhớ file + tab + vị trí + bề rộng) |
| `Ctrl-Shift-T` | mỗi lần bấm khôi phục **đúng một** thứ: ưu tiên tab vừa đóng, sau đó tới cửa sổ vừa đóng |
| `Enter Enter` | phóng to tạm thời cửa sổ hiện tại |
| `Ctrl-Enter` | phóng to ngay lập tức |
| `1..9,0` / `Ctrl-Up/Down` | chuyển tab |

`:ClosedInfo` in ra 2 ngăn xếp (tab / file) + danh sách Tab-ID để chẩn đoán.

---

## 5. Panel trái — WinList + NERDTree

- **WinList** luôn hiện ở cột trái, liệt kê **mọi tab** và các file trong tab đó
  (dấu `>` = cửa sổ đang active), có kích thước mặc định **36 cột**.
- **NERDTree** mở ở nửa dưới cột trái; khi NERDTree mở, cả cột rộng **78 cột**
  (chừa ≥ 20 cột cho cửa sổ code).

| Phím | Việc |
|---|---|
| `wl` | bật/tắt WinList |
| `Ctrl-B` | bật/tắt NERDTree |
| `Enter` / double-click (trong panel) | mở file / nhảy tới entry |
| `q` / `r` | đóng panel / refresh |
| `<leader>W` | đồng bộ lại bề rộng panel |
| `<leader>wa` | mở panel ở mọi tab |
| `<leader>wh` | chia lại chiều cao panel (50/50 khi có NERDTree) |

Lệnh: `:WinList`, `:WinListClose`, `:WinListFix`, `:WinListRefresh`,
`:WinListAllTabs`, `:WinListFixH`.

---

## 6. Comment / Mark / Câu mẫu

| Phím / lệnh | Việc |
|---|---|
| `cc` | toggle comment (leader tự nhận theo filetype: `//` `#` `%` `"` `;` `>`) |
| `hh` | thêm tag haozxje ở **cuối** dòng |
| `ch` | thêm tag haozxje ở **đầu** code |
| `mh` | mark từ dưới con trỏ (Normal) / vùng chọn (Visual) |
| `ml` | xoá toàn bộ mark trong buffer |
| `dd` / `dm` (GVim) | chèn câu mẫu / menu chọn câu — chèn **dòng mới dưới** con trỏ, giữ đúng lề |
| `:SentenceAdd`, `:SentenceList` | thêm / xem danh sách câu mẫu |

---

## 7. Fold

| | `.md` | `.sv` `.v` `.svh` `.svt` `.svti` |
|---|---|---|
| Cách tính cấp | `foldmethod=indent`, **4 space = 1 cấp** | `foldmethod=indent` |
| Mở file | mở **hết** (`foldlevel=99`) | `foldlevel=3` |
| `za` | đóng/mở fold đang đứng | giống |
| `zo` | mở hết ⇄ gập về mức 2 | mở hết ⇄ gập về mức 3 |
| `zA` | — (dùng `za`) | toggle fold hiện tại + **toàn bộ node con** |
| Dòng fold hiện | `▸ <tiêu đề>  ⋯ N dòng` | `▸ <dòng đầu>  ⋯ N dòng` |

`zc` `zC` `zO` `zM` `zR` vẫn là lệnh gốc của Vim. `:MdRefold` áp lại cấu hình fold
cho buffer markdown đang mở.

---

## 8. Markdown (`.md`)

- Chỉ dùng **syntax tô màu** + fold + nowrap. Toàn bộ formatter của plugin
  **đã tắt** (`g:md_ftplugin_enable = 0`); muốn bật lại thì đặt biến đó = 1.
- Màu: `` `code` `` xanh lá, `` `path/có/slash` `` xanh biển, dấu ` ``` ` **cam**,
  dòng `| --- |` (và dòng `---  ---  ----` của bảng viết tay) xanh biển.
- Trong fence có ghi ngôn ngữ (```` ```systemverilog ````, ```` ```verilog ````,
  ```` ```c ````…) thì nội dung được tô theo ngôn ngữ đó
  (`g:md_fenced_languages`).
- **Theo link nội bộ**: đặt con trỏ lên `[chữ](#anchor)` rồi `<C-]>` hoặc
  **double-click** → nhảy tới tiêu đề khớp slug; nhảy xong tự về Normal mode.
- Các lệnh format **chỉ có khi bật ftplugin** (`let g:md_ftplugin_enable = 1`):
  `:MdFormat` (format cả file: căn bảng, sửa tiêu đề, đệm space trong fence…),
  `:MdFormatTable`, `:MdAutoFormat`. Mặc định chúng **không tồn tại**.
- `.md` mở ra là **nowrap** (mọi file khác cũng nowrap; `<M-z>` để bật lại).

---

## 9. Statusline

```
~/.vimrc                                    [CL12:TL431][4sp][md][V7.105]
```

| Khối | Nghĩa |
|---|---|
| `[CL<dòng>:TL<tổng>]` | dòng hiện tại / tổng số dòng buffer |
| `[<cỡ><tb\|sp>]` | indent dò từ nội dung: `4sp` = 4 dấu cách, `8tb` = tab, `mix` = trộn |
| `[<filetype>]` | filetype/syntax (`[none]` nếu buffer rỗng) |
| `[Vx]` | version của `.vimrc`, đọc từ header lúc khởi động |

Nền statusline đổi màu theo mode: xanh đậm (Normal), cam (Insert), xanh lá (Replace).

---

## 10. Tự động hoá

| Tính năng | Mặc định | Tắt bằng |
|---|---|---|
| **Auto-save** (ghi sau 2 s ngừng gõ; `InsertLeave`/thoát ghi ngay) | bật | `let g:autosave_delay = 0`* |
| **Auto-detect indent** kiểu VSCode (quét ≤ 400 dòng, đặt `shiftwidth`/`expandtab` theo file) | bật | `let g:haoz_autoindent = 0` |
| **Tự nạp lại file** khi file bị ghi từ ngoài (chỉ khi buffer không có sửa chưa ghi) | bật | `let g:haoz_checktime_safe = 0` |
| **Auto-format markdown** | **tắt** | bật: `let g:md_autoformat = 1` |
| `autoread` của Vim | **tắt** | (để tránh reload liên tục trên NFS) |

\* đặt `0` chỉ giãn nhịp; vẫn ghi khi rời Insert/thoát Vim.

---

## 11. Font & tiếng Việt (GVim)

Thứ tự font (tự lùi nếu thiếu): **JetBrains Mono → Noto Sans Mono → DejaVu Sans Mono**,
cỡ **5pt**. Sửa trong `.vimrc` (dòng `guifont`) hoặc `.gvimrc`.

### Cài font đóng gói sẵn
Repo có `.vim/fonts.b64` — **một file text** chứa 4 nét JetBrains Mono
(Regular/Bold/Italic/BoldItalic), dùng khi chỉ copy-paste được qua clipboard.

**Cách 1 (khuyên — chỉ mở GVim 1 lần):** chạy trong terminal, **trước khi** mở GVim:
```bash
mkdir -p ~/.local/share/fonts && base64 -d ~/.vim/fonts.b64 | \
  tar -xz -C ~/.local/share/fonts && fc-cache -f
fc-list | grep -i jetbrains       # thấy 4 dòng là xong
```

**Cách 2 (lưới an toàn):** cứ để `.gvimrc` tự giải nén khi mở GVim lần đầu; nếu lần đó
chưa nhận font thì **mở lại GVim đúng 1 lần** nữa (fontconfig chỉ đọc danh sách font
lúc tiến trình khởi động; sau đó không bao giờ lặp lại).

**Cách 3:** cài bằng gói: `sudo apt install fonts-jetbrains-mono`
(hoặc `sudo dnf install jetbrains-mono-fonts`).

Kiểm tra bằng mắt:
```vim
:echo 'Tiếng Việt: ắ ằ ẳ ẵ ặ ế ề ể ễ ệ ố ồ ổ ỗ ộ ớ ờ ở ỡ ợ ứ ừ ử ữ ự ỳ ỵ ỷ ỹ đ Đ'
```

---

## 12. Xử lý sự cố

| Triệu chứng | Nguyên nhân / cách xử |
|---|---|
| `.md` không có màu, không fold | thiếu `~/.vim/syntax/md.vim`; kiểm tra `:set ft?` phải ra `md` (nếu ra `markdown` là thiếu ftplugin/dòng `au … *.md setlocal ft=md`) |
| GVim báo `E596: Invalid font(s)` | font chưa cài → làm mục 11, hoặc đổi `guifont` sang font có sẵn |
| Dấu tiếng Việt bị rơi/lệch | file ở dạng **NFD** → chuẩn hoá NFC (`python3` + `unicodedata.normalize`), không phải lỗi font |
| File tự reload, mất dòng đang sửa | đã tắt `autoread`; file có sửa chưa ghi sẽ **không** bị nạp đè (dùng `:e!` nếu muốn nạp đè) |
| `F11` không làm gì | thiếu `wmctrl` (và `xrandr`/`xprop` để căn giữa) |
| Panel trái hẹp quá / rộng quá | `let g:winlist_width = 36` (15–50) · NERDTree: `let g:NERDTreeWinSize = 78` |
| Muốn xem Vim đang nạp những file nào | `:scriptnames` |

---

## 13. Tùy chọn hay dùng

```vim
let g:winlist_width      = 36     " bề ngang panel WinList
let g:NERDTreeWinSize    = 78     " bề ngang cột trái khi NERDTree mở
let g:haoz_win_w         = 1600   " kích thước khi bấm F11 (thu nhỏ)
let g:haoz_win_h         = 900
let g:autosave_delay     = 2000   " ms
let g:haoz_autoindent    = 0      " tắt auto-detect indent
let g:haoz_checktime_safe= 0      " tắt tự nạp lại file
let g:md_autoformat      = 1      " bật lại auto-format markdown
let g:md_ftplugin_enable = 1      " bật lại toàn bộ ftplugin md
let g:md_fence_bg        = '#363636'  " nền khối ``` (mặc định 'NONE' = tắt)
let g:md_fenced_languages = [...] " ngôn ngữ được tô trong fence
```

---

## 14. Cấu trúc repo

```
.vimrc                 cấu hình chính (không comment — manual nằm ở F1)
.gvimrc                phần GVim-only (disable phím nguy hiểm, dd/dm)
.vim/plugin/           NERDTree, indentLine, sleuth, bookmark, verilog_systemverilog…
.vim/colors/haoz.vim   colorscheme
.vim/syntax/md.vim     syntax markdown (bản riêng, sửa từ hughbien/md-vim)
.vim/ftplugin/md.vim   ftplugin markdown (mặc định TẮT — xem mục 8)
.vim/fonts.b64         JetBrains Mono (base64, 4 nét)
.vim/fonts/*.ttf       JetBrains Mono (bản nhị phân, nếu có)
```

---

## 15. Ghi chú khi sửa cấu hình

- `.vimrc` **không dùng comment**; giải thích được đưa vào **manual F1**
  (hàm cuối file, các dòng `call add(l:L, …)`).
- Mỗi lần sửa phải **bump `Version`** trong header box (statusline đọc từ đó) và
  cập nhật mục tương ứng trong F1.
- Khung header phải rộng đúng **78 ký tự**.
- File trong `.vim/` dùng **LF**; `.vimrc`/`.gvimrc` có thể CRLF.
