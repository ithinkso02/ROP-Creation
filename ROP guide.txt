BASIC ROP
BÀI HỌC ĐẦU TIÊN: Phương pháp CPY
StringCPY: C8 03 32 30
Syntax:
34 7B 31 30 [Addr muốn copy sang][Addr gốc] C8 03 32 30
Copy từ Addr gốc sang addr muốn copy cho đến khi gặp null byte (tức là byte 00)

Memcpy: 50 94 30 30,32 89 31 30
Syntax:
34 7B 31 30 [Addr muốn copy sang][Addr gốc] 50 94 30 30 [Độ dài muốn copy <2 byte> ]
DA 7B 31 30 [Độ dài muốn copy <2 byte> ][Addr gốc][Addr muốn copy sang] 30 30 32 89 31 30 30 30
Copy từ Addr gốc sang addr muốn copy với độ dài mong muốn

Ví dụ: Program dài 100 byte thì quy ra hex sẽ là 64,nhập độ dài sẽ là 64 00
* Có nhiều Memcpy nhưng 2 cái này tiêu biểu nhất nên lấy vào

BÀI TIẾP THEO: Phương pháp loop program
Loop với StringCPY và 2 vùng địa chỉ (Inject program vào vùng addr chính)

34 7B 31 30 [Vùng addr backup <Copy lên vùng addr chính khi vùng addr chính bị phá do set sp (Giải thích sau)>][Vùng addr chính (TỪ PROGRAM TRỞ ĐI) ] C8 03 32 30 A8 9F 30 30 (Setlr để loop ổn định)

[Program <Không được có null byte> ]

34 7B 31 30 [Vùng addr chính][Vùng addr backup] C8 03 32 30 78 5C 31 30 [Vùng addr chính -2] 60 0D 32 30 (set sp nè) 00

* Set sp: Hiểu nôm na là launcher gián tiếp đi

Launcher cho program:
78 5C 31 30 [Vùng addr chính -2] 60 0D 32 30
• Ưu điểm : Đơn giản
• Nhược điểm : Program không được chứa null byte, không thể skip

Loop với StringCPY và 3 vùng địa chỉ (Inject program vào vùng addr Backup 2)

34 7B 31 30 [Vùng addr backup] [Vùng addr chính] C8 03 32 30 A8 9F 30 30
[Program <Không được có null byte> ]
34 7B 31 30 [Vùng addr chính] [Vùng addr backup] C8 03 32 30 78 5C 31 30 [Vùng addr chính -2] 60 0D 32 30 00

Launcher cho program:
34 7B 31 30 [Vùng addr chính] [Vùng addr backup 2] C8 03 32 30 78 5C 31 30 [Vùng addr chính -2] 60 0D 32 30

• Ưu điểm : Đơn giản, có thể skip
• Nhược điểm : Program vẫn không được chứa null byte
