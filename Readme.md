# Phép Nhân Đa Thức Trong Trường Hữu Hạn GF(2^4)

Dự án này triển khai thuật toán nhân hai đa thức trong trường Galois GF(2^4).

---

## 🧠 Cơ sở lý thuyết

### 1. Trường hữu hạn GF(2^4) là gì?
GF(2^4) là một trường hữu hạn chứa $2^4 = 16$ phần tử. Mỗi phần tử trong trường này được biểu diễn dưới dạng một đa thức có hệ số là 0 hoặc 1 (thuộc GF(2)) và bậc lớn nhất cho phép là 3.

**Ví dụ về các dạng biểu diễn:**
* **Đa thức:** $A(x) = x^3 + x$
* **Nhị phân 4-bit:** `1010`
* **Thập phân:** `10`
* **Hexadecimal (Hệ 16):** `0xA`

### 2. Các phép toán cơ bản
* **Phép cộng / trừ:** Trong GF(2^n), phép cộng và phép trừ hoàn toàn giống nhau và tương đương với phép toán **XOR** từng bit (không có nhớ).
  * *Ví dụ:* $x^3 + x^3 = 0$ (vì $1 \oplus 1 = 0$).
* **Đa thức tối giản (Irreducible Polynomial):** Đóng vai trò giống như một "số nguyên tố". Mọi phép nhân nếu sinh ra đa thức có bậc $\ge 4$ đều phải được hạ bậc bằng cách chia lấy dư (modulo) cho đa thức tối giản này.
  * Trong chương trình, ta sử dụng đa thức chuẩn: $m(x) = x^4 + x + 1$
  * **Mã nhị phân:** `10011` (Thập phân: `19`, Hex: `0x13`).

---

## ⚙️ Thuật toán nhân (Shift & XOR)

Để nhân hai phần tử $a$ và $b$, máy tính không nhân chéo như toán học thông thường mà sử dụng thuật toán **Dịch bit (Shift)** và **Cộng (XOR)** qua 4 vòng lặp (tương ứng 4 bit của GF(2^4)):

1. **Kiểm tra bit cuối của $b$:** Nếu bit ngoài cùng bên phải của $b$ bằng 1 (`b & 1`), cộng (`XOR`) giá trị hiện tại của $a$ vào kết quả (`result`).
2. **Nhân $a$ với $x$ (Dịch trái):** Đẩy tất cả các bit của $a$ sang trái 1 vị trí (`a << 1`).
3. **Hạ bậc (Modulo):** Nếu $a$ vừa bị dịch trái chạm mốc bậc 4 (tức là bit thứ 4 bị bật thành 1, giá trị $\ge 16$), ta khử bit này bằng cách cho $a$ **XOR** với đa thức tối giản $m(x)$ (`0x13`).
4. **Dịch phải $b$:** Đẩy $b$ sang phải 1 bit (`b >> 1`) để loại bỏ bit vừa xét và chuyển sang bit tiếp theo.