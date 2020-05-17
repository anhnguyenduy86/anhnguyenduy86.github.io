---
layout: post
title:  "Thêm các phần tử vào danh sách liên kết được sắp xếp"
date:   2020-04-10 18:36:48 +0900
tags: C++ lamda_function binary_search upper_bound
---

Đoạn mã nguồn dưới đây là một ví dụ sử dụng hàm upper_bound của thư viện std (standard template libary) có tham số là hàm so sánh hai phần tử ở dạng lamda function.

Hàm upper_bound có dạng như sau:

```cpp
  template<class ForwardIterator, class T>
  ForwardIterator
    upper_bound(ForwardIterator first,
                ForwardIterator last,
                const T& value);       // (1) C++03

  template<class ForwardIterator, class T, class Compare>
  ForwardIterator
    upper_bound(ForwardIterator first,
                ForwardIterator last,
                const T& value,
                Compare comp);         // (2) C++03
```

Hàm upper_bound dùng thuật toán tìm kiếm nhị phân (binary search) để tìm vị trí trong giới hạn từ phần tử đầu (first) đến phần tử cuối (last), phần tử đầu tiên có giá trị lớn hơn giá trị tìm kiếm (value). Tất nhiên, dãy phần tử đầu đến cuối phải thỏa mãn điều kiện là được sắp xếp theo thứ tự tăng dần vì thuật toán được dùng ở đây là thuật toán tìm kiếm nhị phân.

Ở dạng (1), hàm upper_bound dùng toán tử so sánh nhỏ hơn (<) để so sánh hai phần tử trong dãy. Dạng (2) cho phép ta customize toán tử so sánh nhỏ hơn theo nhu cầu riêng bằng cách truyền hàm so sánh nhỏ hơn vào tham số comp. Hàm comp có dạng
```cpp
bool function_name (const T&, const T&)
```

Giải thích đoạn mã nguồn bên dưới:
1. Tạo một danh sách liên kết chưa được sắp xếp (random_list) của các cặp (key, value), bằng cách tạo số ngẫu nhiên

2. Lần lượt thêm các phần tử trong random_list vào danh sách liên kết có thứ tự sắp xếp theo key (key_sorted_list). Khi thêm một phần tử vào key_sorted_list ta tìm vị trí thích hợp để key_sorted_list luôn luôn có thứ tự. 

```cpp
#include <iostream>
#include <list>
#include <algorithm>
#include <memory>
#include <ctime>

using namespace std;

// dùng typedef để định nghĩa tên kiểu dữ  liệu mới ngằn gọn hơn
typedef pair<int,int> keyvalpair_t;
typedef list<shared_ptr<keyvalpair_t>> list_t;

int main(){
    list_t key_sorted_list;
    list_t random_list;

    // dùng thời gian hiện tại làm seed cho hàm phát sinh số ngẫu nhiên
    srand(time(nullptr));

    for (int i = 0; i < 10; i++){
        // tạo số ngẫu nhiên trong khoảng [0, 19]
        int key = rand() % 20;
        int val = rand() % 20;
        
        // tạo một cặp (key,value)
        auto rand_pair = shared_ptr<keyvalpair_t>(new keyvalpair_t());
        rand_pair->first = key;
        rand_pair->second = val;

        // thêm vào danh sách liên kết chưa được sắp xếp
        random_list.push_back(rand_pair);
    }

    // vòng lặp duyệt các phần tử trong danh sách liên kết chưa sắp xếp,
    // thêm vào danh sách liên kết có thứ tự
    for(auto it = random_list.begin(); it != random_list.end(); ++it){
        // dùng hàm upper_bound (#include <algorithm>)
        // để tìm vị trí thêm vào theo thuật toán binary search
        auto insert_pos = upper_bound(
            key_sorted_list.begin(),
            key_sorted_list.end(),
            *it,
            // hàm so sánh nhỏ hơn (<) dưới dạng lamda function
            [](const shared_ptr<keyvalpair_t>& left, const shared_ptr<keyvalpair_t>& right) -> bool{
                return left->first < right->first;
            }
        );

        if (insert_pos != random_list.end()){
            // chèn vào vị trí đã tìm thấy
            key_sorted_list.insert(insert_pos, *it);
        }
        else{
            // thêm vào cuối nếu không tìm thấy vị trí chèn vào
            key_sorted_list.push_back(*it);
        }
    }

    // In danh sách liên kết không có thứ tự 
    cout << "random list:" << endl;
    for(auto it = random_list.begin(); it != random_list.end(); ++it){
        cout << "(" << (*it)->first << "," << (*it)->second << "), ";
    }
    cout << endl;

    // In danh sách liên kết có thứ tự
    cout << "key sorted list:" << endl;
    for(auto it = key_sorted_list.begin(); it != key_sorted_list.end(); ++it){
        cout << "(" << (*it)->first << "," << (*it)->second << "), ";
    }
}
```

__Kết quả thực hiện chương trình__

```
random list:
(17,19), (2,6), (11,16), (17,2), (1,11), (7,13), (7,12), (0,5), (7,0), (15,0),
key sorted list:
(0,5), (1,11), (2,6), (7,13), (7,12), (7,0), (11,16), (15,0), (17,19), (17,2),
```

__Tham khảo:__

<a href="https://cpprefjp.github.io/reference/algorithm/upper_bound.html">https://cpprefjp.github.io/reference/algorithm/upper_bound.html</a>