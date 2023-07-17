# Dự án bảo vệ mã nguồn Python dành cho người Việt - Made by MHP0920 with 💖

## Copyright
Copyright © MHP0920 2023. _This work is licensed under a [CC BY-ND 4.0 license](http://creativecommons.org/licenses/by-nd/4.0/)._

![image](https://i.creativecommons.org/l/by-nd/4.0/88x31.png)

## Mục lục
- [Phần 1](#phần-1): Chuyển đổi Python thành tệp tin nén (.exe)
  1. [Giới thiệu](#giới-thiệu)
  2. [Cài đặt](#cài-đặt)
- Phần 2: Bảo vệ string (Comming soon)
- Phần 3: MHP Key Trial Service (MHPKTS) (Comming soon)


## Phần 1
### Giới thiệu
Thông thường, chúng ta thường dùng [Pyinstaller](https://pyinstaller.org/) hoặc những thư viện khác để nén sản phẩm thành tệp tin ".exe", nhưng như vậy có thực sự an toàn?

Thực tế, những tệp tin chúng ta nén lại chỉ là những [bytecode](https://en.wikipedia.org/wiki/Bytecode), và chúng rất dễ để được giải nén trở lại thành mã nguộc gốc, dù cho có bảo mật bằng **encryption key** của [Pyinstaller](https://pyinstaller.org/) thì cũng chưa chắc an toàn (Key được lấy trong [runtime](https://en.wikipedia.org/wiki/Runtime_system))

Vậy nên trong Phần 1 mình sẽ hướng dẫn các bạn cách chuyển mã nguồn python sang [mã máy](https://en.wikipedia.org/wiki/Machine_code), dẫu vậy thì cũng không nên chủ quan, vì **mã máy** chỉ là mã thấp nhất trong lập trình nên sẽ khó bị [reverse engineering](https://en.wikipedia.org/wiki/Reverse_engineering) hơn **bytecode**. Bạn có thể nghiên cứu thêm về **High-level và Low-level** trong ngôn ngữ lập trình.

### Cài đặt
Để chuyển python về **mã máy**, chúng ta phải chuyển về **low-level programming language** (mã bậc thấp), và python có 1 thư viện hỗ trợ chính là [Cython](https://cython.org/)
Yêu cầu trước khi làm:
- Phải có [Visual Studio & Visual Studio Build tool](https://visualstudio.microsoft.com/downloads/) đã cài đặt và cài bộ công cụ C++ ở 1 trong 2 bản này (~10GB)

Đầu tiên, ta cài đặt **Cython**, trong Command Prompt, nhập
> pip install Cython

Nếu như màn hình hiển thị kết quả đã báo thành công, chúng ta sẽ chuyển qua bước tiếp theo

Vào thanh tìm kiếm của Windows, tìm "Developer Command Prompt for VS <phiên bản VS mà bạn đang sử dụng>"

Sử dụng _Developer Command Prompt for VS_ vừa tìm được và truy cập vào **thư mục** có chứa đoạn mã của bạn, sau đó sử dụng lệnh
> cython <tên-tệp-tin-mã-nguồn>.py --embed

Giải thích: Lệnh trên dùng để tạo một tệp C từ tệp tin python của các bạn, "**--embed**" để đóng gói thành 1 tệp tin C hoàn chỉnh.

Sau khi có mã nguồn dạng C, chúng ta sẽ dùng **complier** của Visual Studio để tiến hành tạo tệp tin nén .exe. Trong Command Prompt (cmd) hiện tại, bạn chạy lệnh
> cl.exe  /nologo /Ox /MD /W3 /GS- /DNDEBUG -I <thêm-đường-dẫn-tới-thư-mục-"include"> /Tc<tên-mã-nguồn-C> /link /OUT:<tên-tệp-tin-muốn-xuất> /SUBSYSTEM:CONSOLE /MACHINE:X86 /LIBPATH:<thêm-đường-dẫn-tới-thư-mục-"libs">

Giải thích: Câu lệnh trên sẽ tạo cho bạn 1 tệp tin nén .exe được làm bằng C, khi chạy chỉ cần có môi trường python là đủ.

Sau khi đã có tệp tin nén .exe, bạn có thể bỏ cùng với **Pyinstaller** để chạy độc lập

E.g example.py:
```py
import os

os.system("<tên-tệp-exe>")
```
Sau đó đóng gói đoạn mã với tệp tin nén .exe và Pyinstaller là được. Chúc các bạn thành công :)

## Phần 2 (Comming Soon)
## Phần 3 (Comming soon)
