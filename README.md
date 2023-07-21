# Dự án bảo vệ mã nguồn Python dành cho người Việt - Made by MHP0920 with 💖

## Notice from author
Please star ⭐ and fork this project if it helps you and the community, besides it's an effort for me to keep doing my work 🥰

Hãy thả sao ⭐ và fork dự án này nếu nó giúp ích cho bạn và cả cộng đồng, hơn hết nó cũng là động lực để mình tiếp tục phát triển trong tương lai 🥰

## Copyright
Copyright © MHP0920 2023. _This work is licensed under a [CC BY-ND 4.0 license](http://creativecommons.org/licenses/by-nd/4.0/)._

![image](https://i.creativecommons.org/l/by-nd/4.0/88x31.png)

## Mục lục
- [Phần 1](#phần-1-py-to-exe): Chuyển đổi Python thành tệp tin nén (.exe)
  1. Giới thiệu
  2. Cài đặt
- [Phần 2](#phần-2-bảo-vệ-string): Bảo vệ string
  1. Giới thiệu
  2. Các phương thức
- Phần 3: MHP Key Trial Service (MHPKTS) (Comming soon)


## Phần 1: PY-TO-EXE
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

## Phần 2: Bảo vệ String
### Giới thiệu
Ở số trước, chúng ta đã được tìm hiểu về cách bảo vệ mã nguồn bằng cách đổi qua mã máy, tuy vậy, vẫn còn một vấn đề không thể giải quyết bằng cách trên - **mã hóa string** trong mã nguồn.

Tại sao phải mã hóa string? 

Tưởng tượng bạn cần kiểm tra mật khẩu để cho phép sử dụng, nhưng reverse enginneer có thể **chỉnh sửa password** hoặc **đọc password** (Nếu bạn không hash) của bạn và sử dụng như bình thường. 
Hay bạn đặt bản quyền bên trong app (e.g: Copyright (c) MHP0920 2023), reverse enginneer có thể chỉnh sửa thành (e.g: Copyright (c) MHPHacker 2023) và có thể bán lại mà không có vấn đề gì.

String là một phần rất quan trọng trong một mã nguồn, thế nên bảo vệ là điều tất yếu.

Reference: [Thay đổi string trong extension](https://www.facebook.com/100036713590904/videos/809772267409549/)

### Các phương thức
Để mã hóa string, có rất nhiều cách thức, trong số này chúng ta sẽ nghiên cứu 3 cách đơn giản nhất.

#### Phân rải string
Tưởng tượng, bạn có một string "code", khi nén thành file .exe, string "code" sẽ hiện trực tiếp trong binary và bạn có thể chỉnh sửa dễ dàng.

Thay vì vậy, bạn hãy tạo một list có chứa tất cả kí tự alphabet-ALPHABET và lắp ghép tạo thành chữ "Hello World".

e.g:
```py
lst = ['e', 'b', 'c', 'd', 'o']
print(lst[2]+lst[4]+lst[3]+lst[0]) # --> code
```

Lúc này, khi nén thành file .exe, đoạn string "code" đã không còn, thay vào đó reverse enginneer sẽ thấy nhiều dòng kí tự là 'e' 'b' 'c' 'd' 'o' nhưng họ sẽ không biết dòng chữ này có ý nghĩa gì, và khiến việc reverse khó hơn. (Không phải là không thể nhé)

#### Text shifting
Ngoài cách load string từ list, chúng ta có thể dùng cách khác chính là Text shifting. Text shifting là một phương thức thêm hoặc bớt giá trị của [mã Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters), nhằm thay đổi giá trị gốc của chuỗi.

e.g: Mình sẽ shift đoạn string "abc" với giá trị là 1, nghĩa là mỗi mã Unicode của từng kí tự trong chuỗi sẽ tăng lên 1 -> "bcd"
Các bạn có thể shift trong đoạn alphabet-ALPHABET hoặc shift đến các special Unicode, nếu như shift đến các special Unicode thì có thể đánh lừa reverse enginneer đoạn string của bạn là một binary. (Bạn có thể tìm hiểu thêm về minifier để hiểu rõ tại sao lại bị đánh lừa là binary)

#### Load from cloud
Cả 2 cách trên đều tăng khả năng bảo vệ string trong mã nguồn của bạn, thế nhưng không gì là an toàn tuyệt đối. Thay vì đặt string trong app, bạn có thể load chúng bằng cách liên kết API. Tuy nhiên cách này chỉ hiệu quả nếu như string là một loại rất quan trọng (dùng để active, để chạy lệnh, ...), nếu không thì reverse enginneer có thể redirect và load từ server của họ.

## Phần 3 (Comming soon)
