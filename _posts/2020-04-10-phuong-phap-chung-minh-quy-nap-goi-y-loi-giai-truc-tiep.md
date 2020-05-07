---
layout: post
title:  "Phương pháp chứng minh quy nạp gợi ý lời giải trực tiếp"
date:   2020-04-10 18:36:48 +0900
tags: math
---

Chứng minh biểu thức sau:

$$\frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{(n-1).n} = 1 - \frac{1}{n}, \quad\quad (n\geq2) \quad\quad (1)$$

Dùng quy nạp chứng minh thử:

Kiểm tra (1) với $n = 2$:

$$\frac{1}{1.2} = 1 - \frac{1}{2} \quad \text{(Đúng)}$$

Giả sử $(1)$ đúng với $n = k, k\geq2$, nghĩa là $(2)$ đúng:

$$\frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{(k-1).k} = 1 - \frac{1}{k} \quad\quad (2) $$

Chứng minh $(1)$ đúng với $n = k+1$. Thay $n = k + 1$ vào $(1)$:

$$
\begin{aligned}

& \frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{((k+1)-1).(k+1)} = 1 - \frac{1}{k+1} \\

\iff& \frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{k.(k+1)} = 1 - \frac{1}{k +1} \\

\iff& \frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{(k-1).k} + \frac{1}{k.(k+1)} = 1 - \frac{1}{k +1} \quad \text{(Viết thêm số hạng } \frac{1}{(k-1).k} \text{)}\\

\iff& 1 - \frac{1}{k} + \frac{1}{k.(k+1)}= 1 - \frac{1}{k +1} \quad \text{(Thay } (k-1) \text{ số hạng đầu bằng vế phải của (2))} \\

\iff& \frac{1}{k} - \frac{1}{k.(k+1)}=\frac{1}{k +1} \quad \text{(Cộng (-1), và nhân hai vế cho (-1))} \quad (3) \\

\iff& \frac{k + 1 - 1}{k.(k+1)} = \frac{1}{k +1}  \\

\iff& \frac{1}{k+1} = \frac{1}{k +1} \quad \text{(Mệnh đề có chân trị đúng)} \\

\end{aligned}
$$

Như vậy $(1)$ đúng với  $n = k+1$. Do đó $(1)$ đã được chứng minh theo quy nạp.

Trong lúc chứng minh quy nạp, $(3)$ gợi ý cách chứng minh trực tiếp:

Viết lại $(3)$ :

$$
\begin{aligned}
& \frac{1}{k} - \frac{1}{k.(k+1)}=\frac{1}{k +1} \\

\iff& \frac{1}{k.(k+1)}= \frac{1}{k} - \frac{1}{k +1} \quad (4)

\end{aligned}
$$

Lần lượt thay $k = 1, 2, ..., n-1$ vào $(4)$ :

$$
\begin{aligned}

& \frac{1}{1.2} = \frac{1}{1} - \frac{1}{2} \\
& \frac{1}{2.3} = \frac{1}{2} - \frac{1}{3} \\
& ... \\
& \frac{1}{(n-1).n} = \frac{1}{n-1} - \frac{1}{n}

\end{aligned}
$$

Cộng hai vế từ trên xuống:

$$
\begin{aligned}

& \frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{(n-1).n} = \frac{1}{1} - \frac{1}{2} + \frac{1}{2} - \frac{1}{3} + ... + \frac{1}{n-1} - \frac{1}{n} \\
\iff & \frac{1}{1.2} + \frac{1}{2.3} + ... + \frac{1}{(n-1).n} = 1 - \frac{1}{n} \quad \text{(điều cần chứng minh)}

\end{aligned}
$$