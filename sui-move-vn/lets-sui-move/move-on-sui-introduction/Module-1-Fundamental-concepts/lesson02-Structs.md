# Structs Trong Move
Dữ liệu trên blockchain Sui có thể được tổ chức thành các struct. Struct có thể được xem như một nhóm các trường liên quan, mỗi trường có kiểu dữ liệu riêng như số, boolean và vector. Struct là một khái niệm cơ bản trong Sui Move.

```move
module 0x123::my_module {
   use sui::object::UID;

   // Tất cả các struct làm nền tảng của một object cần có thuộc tính `key` và một trường id kiểu UID.
   struct MyObject has key {
       id: UID,
       color: u64,
   }
}
```

Trong ví dụ trên, chúng tôi định nghĩa một struct đơn giản `MyObject` với hai trường `id` và `color`. Mỗi struct có thể được định nghĩa là có các "khả năng" - key, store, drop, copy. Chúng tôi sẽ giải thích thêm sau về ý nghĩa của các khả năng này.

#### Các kiểu dữ liệu trong Move
Move hỗ trợ nhiều loại dữ liệu khác nhau:

1. Số nguyên không dấu: u8, u16, u32, u64, u128, u256. Các loại số nguyên khác nhau có giá trị tối đa khác nhau mà chúng có thể lưu trữ. Ví dụ, u8 có thể lưu trữ các giá trị lên đến 2^8 - 1 hoặc 255, trong khi u256 có thể lưu trữ các giá trị lên đến 2^256 - 1.
2. Boolean: bool.
3. Địa chỉ: address. Địa chỉ là một cấu trúc cốt lõi trong blockchain và đại diện cho danh tính người dùng. Người dùng có thể tạo ra các địa chỉ từ các khóa chỉ có họ nắm giữ ngoài chuỗi và sử dụng chúng để ký các giao dịch. Điều này chứng minh rằng các giao dịch thực sự đến từ người dùng và không phải là giả.
4. String: Chuỗi ký tự
5. Vector. Một mảng của u64 sẽ là vector<u64>.
6. Các kiểu struct tùy chỉnh như UID, signer,... được khai báo với use sui::object::UID trong ví dụ trước.
> Hy vọng qua bài học này, bạn có thể hiểu rõ hơn về cách tổ chức và sử dụng các module trong lập trình Move trên nền tảng Sui.
## Quiz
Trong bài học trước, chúng ta đã định nghĩa cấu trúc AdminCap nhưng nó chưa phải là một kiểu object hợp lệ. Hãy:
#### Cập Nhật AdminCap
Cập nhật `AdminCap` để có khả năng key và một trường `id` kiểu `UID`.

Thêm một hàm riêng tư mới - fun init nhận một đối số ctx kiểu `&mut TxContext` và tạo object `AdminCap` với `num_frens` được thiết lập là 1000. Một hàm init được tự động gọi khi module được triển khai lên blockchain.

Chia sẻ object `AdminCap` để bất kỳ ai cũng có thể sử dụng để tạo `SuiFrens`. Chúng tôi sẽ giải thích sau cách thức chỉ các tài khoản cụ thể mới có thể có `AdminCap` và tạo `SuiFrens`.
#### Ví Dụ về Module
```move
module 0x123::sui_fren {
    use sui::object::UID;
    use sui::tx_context::TxContext;

    struct AdminCap has key {
        id: UID,
        num_frens: u64,
    }

    public fun init(ctx: &mut TxContext) {
        let admin_cap = AdminCap {
            id: UID::new(ctx),
            num_frens: 1000,
        };
        // Thêm các code ở đây
    }
}
```


## Đáp án:
```move
module 0x123::sui_fren {
    struct AdminCap {
        num_frens: u64,
    }
}
```