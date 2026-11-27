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
- Dưới mỗi tên file có dòng **đường dẫn**. Mặc định rút gọn còn 5 thành phần cuối
  (`…/trunk/verification/switch/tests/phase_soc`). Muốn hiện **đầy đủ** (cả tên file,
  `~` thay cho `$HOME`): `let g:winlist_full_path = 1` (mặc định `0`). Panel có
  wrap nên đường dẫn dài tự xuống dòng, không bị cắt.

| Phím | Việc |
|---|---|
| `wl` | bật/tắt WinList |
| `Ctrl-B` | bật/tắt NERDTree |
| `Enter` / double-click (trong panel) | mở file / nhảy tới entry (**click đơn chỉ dời con trỏ**) |
| `q` / `r` | đóng panel / refresh |
| `<leader>W` | đồng bộ lại bề rộng panel |
| `<leader>wa` | mở panel ở mọi tab |
| `<leader>wh` | chia lại chiều cao panel (50/50 khi có NERDTree) |

**Nhảy file: đúng SPLIT, và con trỏ không nằm lại panel (v7.141)**

- `s:WinListWindowInTab()` chọn cửa sổ có **số hiển thị trùng dòng entry**
  trong tab ghi trên header (không phải "cửa sổ đầu tiên chứa file đó"), rồi
  mới fallback tab hiện tại.
- `s:WinListRefocusTick()` — nếu bạn chỉ click vào panel mà không double-click,
  sau `g:winlist_refocus_ms` (mặc định **1500 ms**) con trỏ tự về cửa sổ nội
  dung. Không cần double-click lần thứ hai để "thoát" panel.
- `s:WinListReassertTick()` bám lại buffer đích 4 nhịp (~750 ms) sau cú nhảy,
  và panel của tab đích bị **hoãn 500 ms** (`s:EnsureWinListInTab`) để
  `topleft split` không chen ngang đúng lúc nhảy — trước đây chính nó làm con
  trỏ dừng ở file khác (b.c thay vì a.c).
- `CombinedPanelOpen()` trả con trỏ về **đúng cửa sổ cũ** (`prev_win + 1`, vì
  panel luôn chèn ở vị trí đầu), không dùng "first content window".

**Bề ngang panel được giữ cứng (v7.131)**: `s:WinListEnforceWidth()` so
`winwidth()` với `s:ColumnWidth()`, lệch là `vertical resize` lại (bọc
`noautocmd` + khoá split widths nên không giật). Nó chạy từ
`WinEnter`/`BufWinEnter`/`TabEnter`/`VimResized` (debounce 40 ms) và thêm
`CursorHold` làm lưới an toàn (chỉ so sánh, rất rẻ) → `:vsplit`, `:only`, kéo
chuột chia cửa sổ, đổi tab, bật/tắt NERDTree đều đưa panel về đúng cỡ. Tắt:
`let g:winlist_fix_width = 0`.

Lệnh: `:WinList`, `:WinListClose`, `:WinListFix`, `:WinListRefresh`,
`:WinListAllTabs`, `:WinListFixH`.

**Double-click trong panel** không còn phụ thuộc vào `<2-LeftMouse>` của Vim mà
**tự phát hiện**: `<LeftMouse>` map sang chính nó rồi gọi `s:WinListClick()`;
hai click trên cùng một dòng trong khoảng `g:winlist_dblclick_ms` (**mặc định
1200 ms**, cố ý không dùng `&mousetime` 500 ms vì VNC/NFS làm 2 click tới chậm
hơn ngưỡng của Vim) thì nhảy. Click vào **dòng path** cũng nhảy — hàm tự lùi lên
dòng entry gần nhất (≤ 3 dòng). `s:WinListJumpEntry()` chặn nhảy hai lần trong
0.25 s nên `<2-LeftMouse>` (nếu Vim vẫn bắn) không làm nhảy đúp.

**Panel không có Select/Visual bằng chuột** (v7.123): `setlocal selectmode=` +
`<LeftDrag>`/`<LeftRelease>`/`<C-LeftMouse>` và `x<LeftMouse>` đều map `<Nop>`
→ kéo chuột không tạo vùng chọn, Ctrl-click không mở file oan. Chỉ
**double-click** hoặc **`Enter`** mới mở file; click đơn chỉ dời con trỏ trong
panel.

**Nhảy file: nhanh và đúng cửa sổ (v7.125)**

- Tìm cửa sổ đích bằng cách **quét buffer theo tên file + đuôi đường dẫn**
  (`s:WinListBestBuffer()`), rồi `bufwinnr()` trong tab hiện tại → tab in trên
  header → các tab còn lại. Không dùng `win_gotoid()`/`getbufinfo()` nên chạy
  được cả trên Vim 8.0 cũ (trước đây thiếu 2 hàm đó là cửa sổ đích bị bỏ qua,
  tab nhảy mà con trỏ vẫn nằm ở panel).
- Nhảy xong **giữ yên panel 0,2 s** (`s:WinListGoQuiet()`), rồi hẹn **một lần
  dựng lại panel sau 300 ms** bằng timer (`s:WinListAfterJumpRefresh()`) để cập
  nhật dấu `>` / `◀` active. Trước đây chặn 0,8 s nên panel **không cập nhật**;
  còn dựng lại ngay lập tức thì lag trên NFS. Đo thực tế: **0,6 ms** cho cú nhảy,
  panel vẫn đúng dấu active sau 500 ms.
- Dòng entry **đang active** được đệm space cho kín nền xanh (`> 1: b.c␣␣␣…`);
  `s:WinListEntryName()` cắt khoảng trắng đuôi + ` [+]` trước khi so tên file —
  không cắt thì `fnamemodify(':t')` ra `'b.c      '` và **không khớp buffer nào**,
  nên click vào chính dòng active sẽ không nhảy.

Không nhảy được thì đứng trong panel chạy `:WinListDiag` — in ra `filetype`,
mapping `<LeftMouse>`/`<2-LeftMouse>` có tồn tại không, dòng đang đứng, **tên
file + thư mục parse được**, **buffer khớp được (`best buffer`)**, `bufwinnr`,
`b:winlist_hdr_tids`, và số click đã đếm.

**Bug đã sửa (v7.121)** — `Enter`/double-click trong panel không nhảy dù mapping
vẫn bắn: `WinListJump()` cần `b:winlist_hdr_tids` để biết dòng header thuộc tab
nào, nhưng biến đó **chỉ** được set trong `s:WinListRefresh()`; panel thật lại
được dựng bằng `WinListRefreshAllTabs()` (đường chạy ở gần như mọi event) → map
rỗng → `real_tab = -1` → hàm thoát ngay. Vá: set `b:winlist_hdr_tids` ở **cả
hai** đường render, và fallback lấy số tab in trên header (`=== Tab N`) khi map
rỗng.

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
- Màu: `` `code` `` xanh lá, `` `path/có/slash` `` xanh biển, dấu fence (3 backtick) **cam**,
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
~/.vimrc                                    [CL12:TL431][4sp][md][V7.144]
```

| Khối | Nghĩa |
|---|---|
| `[CL<dòng>:TL<tổng>]` | dòng hiện tại / tổng số dòng buffer |
| `[<cỡ><tb\|sp>]` | indent dò từ nội dung: `4sp` = 4 dấu cách, `8tb` = tab, `mix` = trộn |
| `[<filetype>]` | filetype/syntax (`[none]` nếu buffer rỗng) |
| `[Vx]` | version của `.vimrc`, đọc từ header lúc khởi động |

Cửa sổ **đang focus** (`StatusLine`): nền `#001933` — đúng nền statusline của
panel WinList (`WinListSLLabel`), chữ `#00ffff` — đúng màu `CursorLineNr` (số
hàng của dòng con trỏ đang đứng). Cửa sổ **không focus**
(`StatusLineNC`): chữ xám `#6c6c6c` trên nền `#1c1c1c` — mờ hẳn, nhìn là biết
cửa sổ nào đang làm việc.

Nền đổi theo mode: `#001933` (Normal), cam `#661900` (Insert), xanh lá
`#006619` (Replace) — **chữ giữ nguyên `#00ffff` cho cả 3 mode**
(`s:haoz_sl_insbg` / `s:haoz_sl_repbg` chỉ đổi nền). Màu được đặt trong
`s:StatusLineColors()` và gắn với autocmd `ColorScheme`, nên `colors/haoz.vim`
(đặt `StatusLine`/`StatusLineNC` màu xanh) không ghi đè được.

---

## 10. Tự động hoá

| Tính năng | Mặc định | Tắt bằng |
|---|---|---|
| **Auto-save** (ghi sau **1 s** ngừng sửa ở Normal; **nhấn `Esc` thoát Insert là ghi ngay** (`InsertLeavePre`), rời buffer / rời cửa sổ / rời tab / mất focus / thoát Vim cũng ghi **ngay** — kể cả khi đang `diffsplit`/`gdiff`; trong lúc đang gõ ở Insert thì **chưa** ghi) | bật | `let g:autosave_enable = 0`* |
| **Auto-detect indent** kiểu VSCode (quét ≤ 400 dòng, đặt `shiftwidth`/`expandtab` theo file) | bật | `let g:haoz_autoindent = 0` |
| **Tự nạp lại file** khi file bị ghi từ ngoài (chỉ khi buffer không có sửa chưa ghi) | bật | `let g:haoz_checktime_safe = 0` |
| **Auto-format markdown** | **tắt** | bật: `let g:md_autoformat = 1` |
| `autoread` của Vim | **tắt** | (để tránh reload liên tục trên NFS) |

**Đo bề rộng thụt (v7.144)** — `s:HaozAutoIndent()` (đặt `shiftwidth` thật) và
`s:DetectIndent()` (số hiện trên statusline `[Nsp]`) dùng **cùng một quy tắc**:

1. Với mỗi cặp dòng liên tiếp có thụt **tăng**, ghi nhận delta (2–8).
2. Bề rộng = delta **xuất hiện nhiều nhất**; hoà thì lấy số nhỏ.
3. Dòng trống **không** cắt mạch so sánh — note `.md` của bạn có dòng trống sau
   mỗi dòng nội dung, trước đây chính chỗ này làm mất hết delta nên rơi về 2.

Trước đây statusline lấy **GCD** của mọi độ thụt: file thụt 4 mà có một dòng 6
(hoặc 2) thì GCD = 2 → báo `[2sp]` trong khi indent thật là 4.

**File `.md` và autoread (v7.125)**: mặc định của md (`nowrap`, `shiftwidth=4`)
chỉ được đặt **lần đầu** buffer được nạp (`s:MdBufferDefaults()` + cờ
`b:haoz_md_defaults`). Trước đây autocmd chạy ở **mọi** `BufReadPost`/`WinEnter`
nên sau mỗi lần file bị ghi lại từ ngoài (Claude sửa file chẳng hạn), `wrap` bạn
tự bật bị đè về 0. Giờ bật wrap là giữ nguyên qua reload.

\* `g:autosave_enable = 0` tắt hẳn auto-save; `g:autosave_delay` là số ms chờ
sau thay đổi cuối (mặc định `1000`; đặt `0` cũng tắt). Tắt rồi thì chỉ ghi khi
bạn tự `:w`.

**Chế độ diff (`:diffsplit`, `gdiff`, `vim -d`)**: bạn sửa bên trái rồi nhảy
sang bên phải/cửa sổ khác — file vẫn được ghi ngay, **không cần focus lại tab
đã sửa**. Lý do: mọi event rời (`BufLeave`, `WinLeave`, `TabLeave`,
`BufWinLeave`) đều truyền buffer vừa rời vào `s:AutoSaveNow(<abuf>)`, và timer
có fallback `s:AutoSaveWrite()`: nếu buffer đó không còn là buffer hiện tại thì
nó tự `noautocmd` nhảy sang cửa sổ/tab đang hiển thị buffer (kể cả tab khác),
ghi, rồi trả lại đúng tab/cửa sổ cũ.

**Tương thích Vim**: danh sách event đó được đăng ký bằng vòng lặp +
`exists('##Event')`, nên Vim cũ thiếu `InsertLeavePre` (chỉ có từ Vim 8.2)
**không** báo `E216` và **không** bị cắt phần còn lại của `.vimrc`. Trên Vim 9.1
mọi event đều có → ghi sớm nhất có thể.

---

## 11. Font & tiếng Việt (GVim)

Thứ tự font (tự lùi nếu thiếu): **JetBrains Mono NL → JetBrains Mono → Noto Sans Mono → DejaVu Sans Mono**,
cỡ **5pt**. Sửa trong `.vimrc` (dòng `guifont`) hoặc `.gvimrc`.

**Thanh cuộn ngang (v7.125)**: `set guioptions+=b` trong khối
`has('gui_running')` → GVim có thêm scrollbar ngang ở đáy cửa sổ (thanh dọc vẫn
giữ nguyên). Thanh ngang chỉ chạy khi buffer ở `nowrap` **và** dòng dài hơn bề
ngang cửa sổ; file đang `wrap` thì thanh này không có gì để cuộn.

### Cài font đóng gói sẵn
Repo có `.vim/fonts.b64` (554 KB) — **một file text** chứa 4 nét **JetBrains Mono NL**
(bản **KHÔNG ligature**: `->`, `!=`, `>=` hiện đúng từng ký tự), dùng khi chỉ
copy-paste được qua clipboard.

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
let g:autosave_delay     = 1000   " ms
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
.vim/fonts.b64         JetBrains Mono **NL** (base64, 4 nét, không ligature)
.vim/fonts/*.ttf       JetBrains Mono NL (bản nhị phân, nếu có)
```

---

## 15. Ghi chú khi sửa cấu hình

- `.vimrc` **không dùng comment**; giải thích được đưa vào **manual F1**
  (hàm cuối file, các dòng `call add(l:L, …)`).
- Mỗi lần sửa phải **bump `Version`** trong header box (statusline đọc từ đó) và
  cập nhật mục tương ứng trong F1.
- Khung header phải rộng đúng **78 ký tự**.
- File trong `.vim/` dùng **LF**; `.vimrc`/`.gvimrc` có thể CRLF.

---

## 16. Cách viết file `.md` — toàn bộ quy tắc (file demo)

Đây là **toàn bộ** quy ước viết note. Mọi file `.md` đều theo đúng khuôn này: mỗi
cấp thụt thêm **4 space**, bảng viết trong **text fence**, code thật viết trong fence
có ghi tên ngôn ngữ.

### 16.1 Tên file và frontmatter

**Quy tắc đặt tên** — pattern 3 phần, nối bằng dấu `-`:

```
<project_ID>-<IP_name>-<name of note>.md
```

| STT | Phần | Ý nghĩa | Ví dụ |
|---|---|---|---|
| 1. | `project_ID` | mã dự án, giữ chữ HOA | RLE1396, RL7129 |
| 2. | `IP_name` | tên IP, giữ chữ HOA | IPSEC, SPI_SLV |
| 3. | `name of note` | nội dung, tiếng Anh, nối bằng `_` | bug1_description |

**Dấu cách bị cấm** ở mọi vị trí trong tên file:

| SAI | ĐÚNG |
|---|---|
| RLE1396-IPSEC-bug1 description.md | RLE1396-IPSEC-bug1_description.md |
| RLE1396-IPSEC-giải thích testcase.md | RLE1396-IPSEC-testcase_explain.md |

**Frontmatter là tuỳ chọn.** Dùng khi cần khai báo `tags`, `aliases`, ngày tạo.
Note cũ không có frontmatter thì **để nguyên, không thêm**.

````markdown
---
tags: [IPSEC, testcase]
created: 2026-11-20
---
````

### 16.2 Contents và anchor

Contents là **danh sách link anchor**, đặt **ngay sau frontmatter** và **trước dòng
kẻ ngang**. Mục lớn bỏ số thứ tự ở chữ hiển thị, mục con giữ số.

| STT | Heading | Anchor |
|---|---|---|
| 1. | `## 1. Tôi là ai` | `#1-tôi-là-ai` |
| 2. | `### 1.1 Tóm tắt nhanh` | `#11-tóm-tắt-nhanh` |
| 3. | `#### 1.1.1 Chi tiết` | `#111-chi-tiết` |

**Quy tắc sinh anchor**: bỏ dấu chấm cuối số thứ tự, bỏ dấu câu, chuyển chữ thường,
đổi dấu cách thành `-`, **giữ nguyên chữ có dấu tiếng Việt**. Dùng Markdown link
`[text](#slug)` — **không** dùng wikilink kiểu hai ngoặc vuông.

````markdown
# Contents

1. [Tên file và frontmatter](#1-tên-file-và-frontmatter)
    - [1.1 Quy tắc đặt tên](#11-quy-tắc-đặt-tên)
    - [1.2 Frontmatter](#12-frontmatter)
````

Mục con thụt **4 space**; nhảy bằng `<C-]>` hoặc double-click lên link.

### 16.3 Thụt đầu dòng

**Công thức**: `heading = (số dấu # − 1) × 4`, `nội dung = heading + 4`,
`fence = heading + 8`.

| STT | Cấp | Dấu | Thụt heading | Thụt nội dung |
|---|---|---|---|---|
| 1. | 1 | `#` | 0 space | không dùng |
| 2. | 2 | `##` | 4 space | 8 space |
| 3. | 3 | `###` | 8 space | 12 space |
| 4. | 4 | `####` | 12 space | 16 space |

**Ví dụ lồng bốn cấp** (nội dung cấp 3 thụt 12; heading cấp 4 thụt 16; fence dưới
heading cấp 4 thụt 20):

````text
    ### 3.2.1 …            <- heading cấp 4, thụt 16
        (nội dung 16)      <- nội dung = heading + 4
            ```text        <- fence = heading + 8 = 20
````

### 16.4 Code block

Fence và nội dung code thụt **thêm một cấp** so với nội dung thường:
**fence = indent heading + 8**. Nội dung bên trong giữ nguyên **thụt gốc tương đối**
(copy code từ editor vào là xong, không phải sửa lề).

````text
    ### 4.1 Fence và nội dung        <- heading thụt 8

        Đoạn văn mô tả               <- nội dung thụt 12

            ```systemverilog         <- fence thụt 16 (= 8 + 8)
            constraint EIGHTBYTE_CMD {
                (type3 == 0) -> src_dat_addr_base%8 == 0;
            }
            ```
````

| STT | Dòng | Indent | Giải thích |
|---|---|---|---|
| 1. | `### 4.1 …` | 8 | heading cấp 3 |
| 2. | Đoạn văn mô tả | 12 | heading + 4 |
| 3. | dòng fence `systemverilog` | 16 | fence = 8 + 8 |
| 4. | `constraint … {` | 16 | code gốc 0 → 16 + 0 |
| 5. | `(type3 == 0) …` | 20 | code gốc 4 → 16 + 4 |

### 16.5 Bảng

**a) Bảng trong code block** (kiểu dùng thường xuyên): mỗi hàng nội dung nằm trên
**đúng một dòng** (không wrap), cột đầu là **STT** (`1.`, `2.`, `3.`), cột tự nới
theo nội dung dài nhất, dòng phân cách chỉ gồm `-`:

````text
STT  Nội dung              Hệ quả
---  --------------------  ------------------------------
1.   Một hàng một dòng     đọc tốt trong gvim
2.   Có cột STT            dễ đối chiếu khi trao đổi
3.   Cột tự nới            không bao giờ wrap trong ô
````

**b) Bảng Markdown (`| … |`)** — quy tắc khác: mọi hàng **cùng độ dài**, dấu `|`
**thẳng cột**, toàn bộ bảng **đi liền mạch** (không chèn dòng trống giữa các hàng).

````text
| STT | Tiêu chí | Kết quả |
| --- | -------- | ------- |
| 1.  | Độ dài hàng bằng nhau | OK |
| 2.  | Dấu sổ thẳng cột | OK |
````

**c) Đo độ rộng tiếng Việt và chữ Hán** — tiếng Việt ở dạng **NFC** nên `len()` cho
kết quả đúng, nhưng **chữ Hán rộng 2 cột** trong gvim nên dùng `len()` để pad sẽ làm
lệch cột. Phải đo **độ rộng hiển thị**:

````python
import unicodedata

def w(s):
    return sum(2 if unicodedata.east_asian_width(c) in ('W', 'F') else 1 for c in s)
````

### 16.6 Dòng trống

**Quy tắc**: sau mỗi dòng nội dung thêm **đúng một** dòng trống — không phân biệt
khối, không phân biệt heading hay bảng. **Dòng cuối file không có dòng trống.**

| STT | Vị trí | Dòng trống sau? |
|---|---|---|
| 1. | Dòng cuối file | KHÔNG |
| 2. | Ngay sau frontmatter | Có 1 |
| 3. | Trong code block | KHÔNG, giữ nguyên |
| 4. | Trong bảng Markdown | KHÔNG, giữ nguyên |
| 5. | Trong khối Contents | chỉ sau mục lớn `1.`, `2.` |

### 16.7 Checklist trước khi báo xong

| STT | Bước | Lệnh hoặc cách kiểm |
|---|---|---|
| 1. | Chạy script indent | phải đạt `loi: 0` |
| 2. | Kiểm Contents khớp body | số mục con hai bên bằng nhau |
| 3. | Kiểm cột STT | mọi bảng có separator phải có STT |
| 4. | Kiểm cột thẳng hàng | đo bằng display width |
| 5. | Kiểm wikilink | không trỏ tới file không tồn tại |
| 6. | So nội dung với bản trước | không được mất từ nào |

**Lỗi hay mắc**:

| STT | Lỗi | Cách tránh |
|---|---|---|
| 1. | Fence ghi đè mất | khi sửa bảng, kiểm lại dòng fence đóng |
| 2. | Gộp hàng gây thành hàng mới | không tự động hoá bảng có ô gãy |
| 3. | Bảng dùng `\|` với separator ngắn | dùng dấu cách trong fence thay thế |
| 4. | Báo xong khi chưa kiểm STT | chạy đủ sáu bước ở mục 7.1 |

## 17. Toàn bộ phím tắt

Sinh từ `maplist()` của chính `.vimrc` (bỏ các `<Plug>` nội bộ của plugin).
In lại từ mapping đang sống: `:HaozKeys`.

### NORMAL (63 phím)

| Phím | Việc |
|---|---|
| `<CR>` | `:call SplitExpandHandleEnter()<CR>` |
| `0` | `:call GoToTab(0)<CR>` |
| `1` | `:call GoToTab(1)<CR>` |
| `2` | `:call GoToTab(2)<CR>` |
| `3` | `:call GoToTab(3)<CR>` |
| `4` | `:call GoToTab(4)<CR>` |
| `5` | `:call GoToTab(5)<CR>` |
| `6` | `:call GoToTab(6)<CR>` |
| `7` | `:call GoToTab(7)<CR>` |
| `8` | `:call GoToTab(8)<CR>` |
| `9` | `:call GoToTab(9)<CR>` |
| `L` | `:nohlsearch<CR>` |
| `N` | `Nzz` |
| `R` | `R<C-r>=<SID>feedPopup()<CR>` |
| `V` | `<C-v>` |
| `\wh` | `:call WinListFixPanelHeights()<CR>` |
| `\wa` | `:call WinListOpenInAllTabs()<CR>` |
| `\W` | `:call WinListFixWidth()<CR>` |
| `\w` | `:call WinListToggle()<CR>` |
| `a` | `a<C-r>=<SID>feedPopup()<CR>` |
| `ch` | `:call ToggleHaozCommentAtBeginning()<CR>` |
| `cc` | `:call ToggleCommentLine()<CR>` |
| `hh` | `:call ToggleCommentSyntaxHaoz()<CR>` |
| `i` | `i<C-r>=<SID>feedPopup()<CR>` |
| `ml` | `:call ClearAllMarks()<CR>` |
| `mh` | `:call ToggleMarkWord()<CR>` |
| `n` | `nzz` |
| `o` | `<Nop>` |
| `wl` | `:call WinListToggle()<CR>` |
| `x` | `<Nop>` |
| `<F1>` | `:call ToggleVimrcManual()<CR>` |
| `<C-S-T>` | `:call ReopenLastClosed()<CR>` |
| `<C-CR>` | `:call SplitExpandCtrlEnter()<CR>` |
| `<F10>` | `:call ToggleGvimMenuToolbar()<CR>` |
| `<Del>` | `<Nop>` |
| `<M-z>` | `:set wrap!<CR>` |
| `<C-U>` | `:<C-u>call ToggleCase()<CR>` |
| `<S-Del>` | `dd` |
| `<C-Z>` | `u` |
| `<Home>` | `^` |
| `<M-Right>` | `:call WinListNavRight()<CR>` |
| `<M-Left>` | `:call WinListNavLeft()<CR>` |
| `<C-Down>` | `:tabnext<CR>` |
| `<C-Up>` | `:tabprevious<CR>` |
| `<C-S-Left>` | `vb` |
| `<C-S-Right>` | `vew` |
| `<C-Left>` | `b` |
| `<C-Right>` | `w` |
| `<S-End>` | `v<End>` |
| `<S-Home>` | `v<Home>` |
| `<S-Down>` | `v<Down>` |
| `<S-Up>` | `v<Up>` |
| `<S-Right>` | `v<Right>` |
| `<S-Left>` | `v<Left>` |
| `<C-F>` | `:let @/ = '\<'.expand('<cword>').'\>'<CR>:set hlsearch<CR>` |
| `<C-D>` | `:let @/='\<'.expand('<cword>').'\>'<CR>viw` |
| `<C-X>` | `"+dd` |
| `<C-V>` | `"+p` |
| `<C-C>` | `"+yy` |
| `<C-L>` | `V` |
| `<C-A>` | `ggVG` |
| `<F12>` | `:call CopyFullPath()<CR>` |
| `<F5>` | `:edit!<CR>` |

### VISUAL (22 phím)

| Phím | Việc |
|---|---|
| `<Tab>` | `:<C-u>call VisualModeTab()<CR>gv` |
| `ch` | `:<C-u>call ToggleHaozCommentRangeAtBeginning(line("'<"), line("'>"))<CR>` |
| `cc` | `:<C-u>call ToggleCommentRange(line("'<"), line("'>"))<CR>` |
| `hh` | `:call ToggleCommentSyntaxHaoz()<CR>` |
| `mh` | `:<C-u>call ToggleMarkSelection()<CR>` |
| `<C-U>` | `:<C-u>call ToggleCase()<CR>` |
| `<C-Z>` | `<Esc>u` |
| `<S-Tab>` | `:<C-u>call VisualModeShiftTab()<CR>gv` |
| `<C-S-Left>` | `b` |
| `<C-S-Right>` | `e` |
| `<C-Left>` | `b` |
| `<C-Right>` | `e` |
| `<S-End>` | `<End>` |
| `<S-Home>` | `<Home>` |
| `<S-Down>` | `<Down>` |
| `<S-Up>` | `<Up>` |
| `<S-Right>` | `<Right>` |
| `<S-Left>` | `<Left>` |
| `<C-F>` | `"zy:let @/ = escape(@z, '/\')<CR>:set hlsearch<CR>` |
| `<C-X>` | `"+d` |
| `<C-V>` | `"+p` |
| `<C-C>` | `"+y` |

### INSERT (17 phím)

| Phím | Việc |
|---|---|
| `<C-S-T>` | `<Esc>:call ReopenLastClosed()<CR>` |
| `<C-Z>` | `<C-o>u` |
| `<Home>` | `<C-o>^` |
| `<C-Left>` | `<C-o>b` |
| `<C-Right>` | `<C-o>w` |
| `<C-S-Right>` | `<S-Right><C-Right>` |
| `<C-S-Left>` | `<S-Left><C-Left>` |
| `<S-End>` | `<C-o>v<End>` |
| `<S-Home>` | `<C-o>v<Home>` |
| `<S-Down>` | `<C-o>v<Down><Left>` |
| `<S-Up>` | `<Left><C-o>v<Up><Right>` |
| `<S-Right>` | `<C-o>v` |
| `<S-Left>` | `<Left><C-o>v` |
| `<C-D>` | `<Esc>viw` |
| `<C-V>` | `<C-r>+` |
| `<C-L>` | `<Esc>V` |
| `<F12>` | `<C-o>:call CopyFullPath()<CR>` |

### CMD-LINE (1 phím)

| Phím | Việc |
|---|---|
| `<C-V>` | `<C-r>+` |

