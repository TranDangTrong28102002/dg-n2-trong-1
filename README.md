**HỌC VIỆN CÔNG NGHỆ BƯU CHÍNH VIỄN THÔNG**

**KHOA AN TOÀN THÔNG TIN**

\-\-\-\-\-\--🙞🙜🕮🙞🙜\-\-\-\-\-\--

![](media/image19.png){width="1.0010662729658792in"
height="0.9652001312335958in"}

**Báo cáo bài thực hành**

**Tìm hiểu web application firewall cho web application và fail2ban để
ngăn chặn các hành vi tấn công**

> Sinh viên thực hiện:
>
> B20DCAT191 Trần Đăng Trọng
>
> Giảng viên hướng dẫn: Phạm Hoàng Duy

**\
\
\
\
**

[]{#_heading=h.gjdgxs .anchor}**[MỤC LỤC]{.smallcaps}**

[MỤC LỤC 1](#_heading=h.gjdgxs)

[DANH MỤC CÁC HÌNH VẼ 2](#_heading=h.1fob9te)

[DANH MỤC CÁC BẢNG BIỂU 3](#_heading=h.3znysh7)

[DANH MỤC CÁC TỪ VIẾT TẮT 4](#_heading=h.2et92p0)

> [1.1 Giới thiệu chung về bài thực hành 5](#_heading=h.tyjcwt)
>
> [1.2 Nội dung và hướng dẫn bài thực hành 5](#_heading=h.3dy6vkm)
>
> [**1.2.1** Mục đích 5](#_heading=h.1t3h5sf)
>
> [**1.2.2** Yêu cầu đối với sinh viên 5](#_heading=h.4d34og8)
>
> [**1.2.3** Nội dung thực hành 5](#_heading=h.2s8eyo1)
>
> [1.3 Phân tích yêu cầu bài thực hành 20](#_heading=h.23ckvvd)
>
> [1.4 Cài đặt và cấu hình các máy ảo 23](#_heading=h.2u6wntf)
>
> [1.5 Tích hợp và triển khai 24](#_heading=h.nmf14n)
>
> [***1.5.1*** **Docker Hub** 24](#_heading=h.37m2jsg)
>
> [***1.5.2*** **Github** 25](#_heading=h.3l18frh)
>
> [1.6 Thử nghiệm và đánh giá 26](#_heading=h.1egqt2p)

[TÀI LIỆU THAM KHẢO 29](#_heading=h.34g0dwd)

**[DANH MỤC CÁC HÌNH VẼ]{.smallcaps}**

[]{#_heading=h.1fob9te .anchor}

[Hình 1 Gửi request độc hại lên webserver 6](#_heading=h.17dp8vu)

[Hình 2 : Kết quả khi thực hiện request độc hại 9](#_heading=h.35nkun2)

[Hình 3 : Log của request độc hại 10](#_heading=h.1ksv4uv)

[Hình 4 : Cấu hình website 11](#_heading=h.44sinio)

[Hình 5 : Điền các thông tin 11](#_heading=h.2jxsxqh)

[Hình 6 Giao diện website 11](#_heading=h.z337ya)

[Hình 7 Truy cập trang quản trị của website 12](#_heading=h.3j2qqm3)

[Hình 8 : Giao diện trang quản trị website 13](#_heading=h.1y810tw)

[Hình 9 : Kiểm tra hoạt động cấu hình fail2ban với modsecurity
15](#_heading=h.2xcytpi)

[Hình 10 : IP bị ban 16](#_heading=h.1ci93xb)

[Hình 11 : Tạo bot telegram 16](#_heading=h.3whwml4)

[Hình 12 : Tạo group với bot telegram 17](#_heading=h.2bn6wsx)

[Hình 13 : Đường dẫn url của group telegram 17](#_heading=h.qsh70q)

[Hình 14 : File fail2ban_notify_telegram.sh 18](#_heading=h.3as4poj)

[Hình 15 : File fail2ban-notify.service 18](#_heading=h.1pxezwc)

[Hình 16 : Kết quả gửi về telegram 18](#_heading=h.49x2ik5)

[Hình 17 : File sendlog_telegram.py 19](#_heading=h.2p2csry)

[Hình 18 : File modsecurity_log_to_telegram.service
19](#_heading=h.147n2zr)

[Hình 19 : Log request độc hại gửi về telegram 20](#_heading=h.3o7alnk)

[Hình 20 : Giao diện labedit của lab 23](#_heading=h.19c6y18)

[Hình 21 : Giao diện result 23](#_heading=h.3tbugp1)

[Hình 22 : Docker file 24](#_heading=h.28h4qwu)

[Hình 23 : Add và commit bài lab 24](#_heading=h.1mrcu09)

[Hình 24 : Đẩy các vùng chứa lên dockerhub 24](#_heading=h.46r0co2)

[Hình 25 : Tạo imodule.tar chứa bài thực hành 24](#_heading=h.2lwamvv)

[Hình 26 : Các vùng chứa được đẩy lên dockerhub 25](#_heading=h.111kx3o)

[Hình 27 : Tạo file Imodule.tar 25](#_heading=h.206ipza)

[Hình 28 : File imodule.tar chứa bài thực hành 25](#_heading=h.4k668n3)

[Hình 29 : Đẩy file imodule.tar lên github 26](#_heading=h.2zbgiuw)

[Hình 30 : IP của máy apachemodsecurity 26](#_heading=h.3ygebqi)

[Hình 31 : File cấu hình modsecurity đã bât engine
27](#_heading=h.2dlolyb)

[Hình 32 : Engine modsecurity đã hoạt động 27](#_heading=h.sqyw64)

[Hình 33 : Thêm rule SecRule REQUEST_URI \"@beginsWith
/wordpress/wp-login.php\"
\"id:1000002,phase:1,allow,ctl:ruleEngine=Off\" vào file cấu hình
modsecurity 27](#_heading=h.3cqmetx)

[Hình 34 : Rule chạy thành công 28](#_heading=h.1rvwp1q)

[Hình 35 : Cấu hình [tích hợp được fail2ban với modsecurity]{.mark}
28](#_heading=h.4bvk7pj)

[Hình 36 : Cấu hình tích hợp được fail2ban với modsecurity đã hoạt động
28](#_heading=h.2r0uhxc)

[Hình 37 : File gửi ip ban đến telegram 29](#_heading=h.1664s55)

[Hình 38 : File gửi log apache đến telegram 29](#_heading=h.3q5sasy)

[Hình 39 : Đánh giá kết quả bài thực hành 29](#_heading=h.25b2l0r)

[]{#_heading=h.3znysh7 .anchor}**[DANH MỤC CÁC BẢNG BIỂU]{.smallcaps}**

[Bảng1. Bảng Result 22](#_heading=h.32hioqz)

[]{#_heading=h.2et92p0 .anchor}**[DANH MỤC CÁC TỪ VIẾT
TẮT]{.smallcaps}**

  -----------------------------------------------------------------------
  **Từ\    **Thuật ngữ tiếng Anh/Giải      **Thuật ngữ tiếng Việt/Giải
  viết     thích**                         thích**
  tắt**                                    
  -------- ------------------------------- ------------------------------
                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           

                                           
  -----------------------------------------------------------------------

1.  []{#_heading=h.tyjcwt .anchor}**Giới thiệu chung về bài thực hành**

Bài thực hành này tập trung vào việc sử dụng công cụ ModSecurity -- một
Web Application Firewall (WAF) và công cụ Fail2ban để phát hiện và ngăn
chặn các hành vi tấn công vào ứng dụng web. ModSecurity là một module
bảo mật mạnh mẽ cho các máy chủ web như Apache, Nginx, và IIS. Nó giúp
phát hiện và ngăn chặn các cuộc tấn công vào ứng dụng web (SQL
Injection, XSS, \...). Fail2ban là một công cụ bảo mật tự động hóa, hoạt
động bằng cách giám sát log files và thực hiện các hành động (như chặn
IP) khi phát hiện hành vi đáng ngờ. Fail2ban có thể đọc log từ
ModSecurity để chặn ngay các IP thực hiện tấn công ứng dụng web, đảm bảo
bảo vệ cả hai lớp.

2.  []{#_heading=h.3dy6vkm .anchor}**Nội dung và hướng dẫn bài thực
    hành**

    1.  []{#_heading=h.1t3h5sf .anchor}***Mục đích***

Giúp sinh viên tìm hiểu khái niệm, cách sử dụng công cụ ModSecurity và
Fail2ban để có thể bảo vệ ứng dụng web khỏi các hoạt động độc hại, bao
gồm các cuộc tấn công SQL Injection, XSS, \....

Bài thực hành này cũng có thể áp dụng vào thực tế cho các trang web bán
hàng trực tuyến của công ty là trung tâm giao dịch với hàng nghìn người
dùng mỗi ngày.

Để bảo vệ hệ thống với:

-   **ModSecurity** được triển khai làm Web Application Firewall (WAF)
    > nhằm phân tích và chặn các truy cập độc hại ngay từ đầu.

-   **Fail2Ban** được sử dụng để chặn các địa chỉ IP có hành vi tấn công
    > lặp đi lặp lại, đồng thời gửi thông báo đến quản trị viên qua
    > Telegram.

    1.  []{#_heading=h.4d34og8 .anchor}***Yêu cầu đối với sinh viên***

Có kiến thức cơ bản về hệ điều hành Linux, webserver: apache, firewall:
iptables.

2.  []{#_heading=h.2s8eyo1 .anchor}***Nội dung thực hành***

Khởi động bài lab:

Vào terminal, gõ: 

***labtainer dg-n2-trong-1***

* (chú ý: sinh viên sử dụng mã sinh viên của mình để nhập thông tin
email người thực hiện bài lab khi có yêu cầu, để sử dụng khi chấm điểm)*

-   [Sau khi khởi động xong 1 terminal ảo sẽ xuất hiện:
    > apache_modsecurity - máy ubuntu 20.04 -\> truy cập vào root:
    > **sudo su**]{.mark}

-   [Trên terminal apache_modsecurity sử dụng lệnh "ifconfig", xác định
    > địa chỉ IP của máy]{.mark}

-   Bài lab sử dụng web server là apache, kiểm tra status của dịch vụ
    > apache

**systemctl status apache2**

-   Gửi một request độc hại (XSS) đến website, kiểm tra kết quả

[[http://IP/?q=\"\>\<script\>alert(0)\</script]{.underline}](http://ip/?q=%22%3e%3cscript%3ealert(0)%3c/script)\>

![A screenshot of a computer Description automatically
generated](media/image27.png){width="6.575in"
height="1.9770833333333333in"}

1.  []{#_heading=h.17dp8vu .anchor}*: Gửi request độc hại lên webserver*

-\> kiểm tra và thấy được request đã được gửi đến và mã trả về 200,
trường hợp website có tồn tại lỗ hổng thì hacker dễ dàng rce

**Mục tiêu 1: Cấu hình waf modsecurity:**

-   **Hoạt động của modsecurity:**

    -   Modsecurity có cơ chế hoạt động như một cỗ máy IDPS. Công cụ này
        > thực hiện quá trình phát hiện các xâm nhập lạ vào website.

    -   ModSecurity bắt đầu từ việc giám sát luồng dữ liệu HTTP với các
        > yêu cầu được gửi tới Server. Sau đó, Modsecurity sẽ phân tích
        > các yêu cầu để tìm ra sự cố bất thường.

    -   Những bất thường này thường vi phạm chính sách an ninh của các
        > [web server](https://lanit.com.vn/web-server.html). Khi đó,
        > Modsecurity sẽ giúp ngăn chặn hoặc loại bỏ các yêu cầu để bảo
        > vệ hệ thống web.

    -   Có hai phương pháp để Modsecurity thực hiện ngăn chặn các tấn
        > công. Đó là dựa vào mẫu được lập trình sẵn và dựa vào các dấu
        > hiệu bất thường. Việc phát hiện lỗi từ luồng dữ liệu của
        > Modsecurity trở nên đơn giản hơn, nhanh chóng hơn.

    -   Đối với các lỗi được xác định bất thường, Modsecurity sẽ tập hợp
        > và đánh giá theo mức độ. Lỗi càng cao đồng nghĩa với mức độ đe
        > dọa an ninh càng lớn. Khi đó, công cụ sẽ chặn hoặc loại bỏ các
        > yêu cầu được gửi đến.

```{=html}
<!-- -->
```
-   **Tính năng nổi bật và vai trò của Modsecurity:**

    -   Phòng chống tấn công: Modsecurity cung cấp các bộ quy tắc mạnh
        > mẽ để phát hiện và ngăn chặn các loại tấn công web phổ biến
        > như SQL Injection, Cross-Site Scripting (XSS), Remote File
        > Inclusion (RFI), và nhiều hơn nữa.

    -   Bảo vệ dữ liệu và hệ thống: Bằng cách ngăn chặn các cuộc tấn
        > công, Modsecurity có thể giúp bảo vệ dữ liệu quan trọng và hệ
        > thống khỏi sự phá hủy hoặc truy cập trái phép.

    -   Kiểm soát truy cập: Modsecurity cho phép thiết lập các quy tắc
        > để kiểm soát việc truy cập vào ứng dụng web, bao gồm việc chặn
        > hoặc cho phép truy cập từ các địa chỉ IP cụ thể, ngăn chặn các
        > yêu cầu từ các user-agent không mong muốn, và nhiều hơn nữa.

    -   Phân tích hành vi tấn công: Modsecurity cung cấp các công cụ
        > phân tích và báo cáo để theo dõi và hiểu rõ các loại tấn công
        > và mẫu tấn công phổ biến, từ đó cung cấp thông tin quý giá cho
        > việc tăng cường bảo mật hệ thống.

    -   Tùy biến linh hoạt: Với các cấu hình linh hoạt và hỗ trợ cho
        > ngôn ngữ lập trình Lua, Modsecurity cho phép người quản trị
        > tùy chỉnh và mở rộng chức năng của nó để phù hợp với nhu cầu
        > cụ thể của môi trường và ứng dụng web của họ.

```{=html}
<!-- -->
```
-   **[Cấu trúc của luật ModSecurity:]{.mark}**

    -   [Một luật ModSecurity thường được viết trong file cấu hình
        > (.conf) theo định dạng:]{.mark}

> **[SecRule \[target\] \[operator\] \[action\]]{.mark}**

-   [Thành phần chi tiết:]{.mark}

    -   [Target: Xác định thành phần HTTP nào sẽ được kiểm tra (vd: URL,
        > headers, body, v.v.).]{.mark}

    -   [Operator: Xác định điều kiện kiểm tra (vd: chuỗi, biểu thức
        > chính quy, so sánh số).]{.mark}

    -   [Action: Định nghĩa hành động khi luật được kích hoạt.]{.mark}

### **[- Các nguồn hình thành luật ModSecurity]{.mark}** {#các-nguồn-hình-thành-luật-modsecurity .unnumbered}

#### [a. Bộ luật OWASP Core Rule Set (CRS)]{.mark} {#a.-bộ-luật-owasp-core-rule-set-crs .unnumbered}

-   [Đây là bộ luật phổ biến và mạnh mẽ nhất, được duy trì bởi cộng đồng
    > bảo mật OWASP.]{.mark}

-   [CRS bao gồm các luật để phát hiện và ngăn chặn:]{.mark}

    -   **[SQL Injection]{.mark}**

    -   **[Cross-Site Scripting (XSS)]{.mark}**

    -   **[Local File Inclusion (LFI)]{.mark}**

    -   **[Remote File Inclusion (RFI)]{.mark}**

    -   **[Brute force login]{.mark}**

    -   [Và nhiều lỗ hổng OWASP Top 10 khác.]{.mark}

#### [b. Luật tùy chỉnh]{.mark} {#b.-luật-tùy-chỉnh .unnumbered}

-   [Quản trị viên có thể tự viết luật để phù hợp với nhu cầu bảo mật
    > riêng.\
    > Ví dụ:]{.mark}

-   [Chặn yêu cầu với các từ khóa đặc biệt:\
    > **SecRule REQUEST_URI \"@rx (?i)select.\*from\"
    > \"id:1001,phase:2,deny,status:403,msg:\'SQL Injection
    > detected\'\"**]{.mark}

-   [Chặn yêu cầu đến một endpoint cụ thể:\
    > **SecRule REQUEST_URI \"/admin\"
    > \"id:1002,phase:2,deny,status:403,msg:\'Unauthorized access to
    > /admin\'\"**]{.mark}

-   [Bỏ chặn yêu cầu đến một endpoint trang wp-login.php cụ thể:]{.mark}

> **[SecRule REQUEST_URI \"@beginsWith /wp-login.php\"]{.mark}**
>
> **[\"id:1000002,phase:1,allow,ctl:ruleEngine=Off\"]{.mark}**

-   [Với lab này đã được cấu hình modsecurity với apache và sử dụng
    > **OWASP Core Rule Set (CRS)**]{.mark}

-   [Sửa file cấu hình để bật waf modsecurity:]{.mark}

> **[nano /etc/modsecurity/modsecurity.conf]{.mark}**

-   [Chuyển **SecRuleEngine Off -\> SecRuleEngine On,** đóng file và
    > restart lại apache:]{.mark}

> **[systemctl restart apache2]{.mark}**

-   [Kiểm tra xem modsecurity đã hoạt động hạy chưa, thực hiện gửi 1
    > request độc hại đến website:]{.mark}

> [[http://IP/?q=\"\>\<script\>alert(0)\</script]{.underline}](http://ip/?q=%22%3e%3cscript%3ealert(0)%3c/script)\>

![A screenshot of a computer Description automatically
generated](media/image6.png){width="6.575in"
height="2.423611111111111in"}

2.  []{#_heading=h.35nkun2 .anchor}*: Kết quả khi thực hiện request độc
    > hại*

> -\> Kết quả trả về mã trạng thái 403 Forbidden, như vậy ngăn chặn được
> request độc hại

\- Kiểm tra trong log xem thông tin độc hại

**nano /var/log/apache2/modsec_audit.log**

-   Kết quả thấy được đây là requests được đánh giá là dạng tấn công XSS
    > áp dụng rule của OWASP_CRS/3.3.0

![A computer screen shot of a computer screen Description automatically
generated](media/image33.png){width="6.575in"
height="3.3243055555555556in"}

3.  []{#_heading=h.1ksv4uv .anchor}*: Log của request độc hại*

**Mục tiêu 2: Tạo luật cho modsecurity**

-   Trong bài lab có tạo 1 trang sử dụng wordpress -- chưa được cấu
    > hình, truy cập
    > [**[http://IP/wordpress]{.underline}**](http://ip/wp-admin) để cấu
    > hình wordpress:

![A screenshot of a computer Description automatically
generated](media/image21.png){width="6.267716535433071in"
height="3.3194444444444446in"}

4.  []{#_heading=h.44sinio .anchor}*: Cấu hình website*

-\> Chọn tiếng việt

![A screenshot of a computer Description automatically
generated](media/image35.png){width="6.267716535433071in"
height="3.5555555555555554in"}

5.  []{#_heading=h.2jxsxqh .anchor}*: Điền các thông tin*

-\> Điền các nội dung cơ bản

\- Sau khi hoàn tất quay trở lại
[**[http://IP/wordpress]{.underline}**](http://ip/wp-admin)

![A screenshot of a computer Description automatically
generated](media/image23.png){width="6.267716535433071in"
height="3.3472222222222223in"}

6.  []{#_heading=h.z337ya .anchor}*: Giao diện website*

-   Trang web có trang wp-admin dành cho quản trị viên truy cập để code,
    > bảo trì. Sau khi cài modsecurity lên thì bị waf chặn vì bị đánh
    > giá là request độc hại. Tiến hành tạo rule cho phép truy cập vào
    > trang wp-admin.

-   Truy cập
    > **[[http://IP/wordpress/wp-admin]{.underline}](http://ip/wp-admin)
    > -\> trả về kết quả 403** Forbidden

![A screenshot of a computer Description automatically
generated](media/image26.png){width="6.267716535433071in"
height="2.9722222222222223in"}

7.  []{#_heading=h.3j2qqm3 .anchor}*: Truy cập trang quản trị của
    > website*

-   [Dựa vào luật tùy chỉnh của modsecurity phần mục tiêu 1 để thực hiện
    > tạo rule]{.mark}

-   [Mở file cấu hình modsecurity để tạo rule:]{.mark}

> **[nano /etc/modsecurity/modsecurity.conf]{.mark}**

**[Thêm các thông tin sau:]{.mark}**

> **[SecRule REQUEST_URI \"@beginsWith /wordpress/wp-login.php\"
> \"id:1000002,phase:1,allow,ctl:ruleEngine=Off\"]{.mark}**

-   [Viết rule đóng file cấu hình, restart lại apache sau đó kiểm tra
    > hoạt động của rule vừa tạo:]{.mark}

> **[systemctl restart apache2]{.mark}**

-   [Truy cập lại]{.mark}
    > **[[http://IP/wordpress/wp-admin]{.underline}](http://ip/wp-admin)
    > -\> trả về 200 xuất hiện màn hình login như vậy rule đã hoạt
    > động**

![A screenshot of a computer Description automatically
generated](media/image25.png){width="6.267716535433071in"
height="2.9444444444444446in"}

8.  []{#_heading=h.1y810tw .anchor}*: Giao diện trang quản trị website*

**Mục tiêu 3: Tích hợp với fail2ban để ban ip**

-   Fail2ban làm một ứng dụng được viết bằng ngôn ngữ
    > [Python](https://wiki.tino.org/python-la-gi/) hỗ trợ phân tích,
    > theo dõi log của hệ thống nhằm phát hiện và ngăn chặn các cuộc tấn
    > công

-   Ứng dụng [Fail2ban](https://www.fail2ban.org/) tập trung phát triển
    > để bảo vệ SSH và ngăn chặn các cuộc tấn công vào SSH như [brute
    > force
    > attack](https://wiki.tino.org/brute-force-attack-va-cach-phong-chong/)
    > là chính. Tuy nhiên, có thể thiết lập các rule, các tham số để sử
    > dụng trên bất cứ dịch vụ nào có hỗ trợ log file.

#### Cấu hình Fail2ban:

Nếu không biết nhiều về cách cấu hình Fail2ban, có thể sử dụng chế độ
mặc định của Fail2ban được thiết lập sẵn và rất ổn. Chạy lệnh sau đây để
xem cấu hình mặc định của Fail2ban bằng Nano text:

**nano /etc/fail2ban/jail.conf**

-   Đây là cấu hình mặc định của Fail2ban:

> \[DEFAULT\]
>
> ignoreip = 127.0.0.1
>
> bantime = 600
>
> findtime = 600
>
> maxretry = 3

Giải thích về cấu hình mặc định của Fail2ban:

-   **ignoreip**: danh sách những địa chỉ IP không bị block, có thể
    > thấy, địa chỉ
    > [127.0.0.1](https://wiki.tino.org/dia-chi-127-0-0-1-la-gi/) sẽ mặc
    > định không bị chặn.

    > **bantime:** nếu bị block, địa chỉ IP đó sẽ bị block trong x giây.

    > **findtime:** khoản thời gian (tính bằng giây) địa chỉ IP đó phải
    > đăng nhập thành công. Nếu không, Fail2ban sẽ block IP dựa trên số
    > lần đăng nhập thất bại.

    > **maxretry:** số lần đăng nhập thất bại được cho phép

Ngoài ra còn có:

**action:** là tập hợp các bước hoặc lệnh để xử lý khi phát hiện vi
phạm. Có

nhiều loại hành động phổ biến. Thường sử dụng **iptables** hoặc **ufw**
để

chặn IP vi phạm trên một hoặc nhiều cổng.

**port**: Trường port xác định cổng hoặc các cổng mà Fail2Ban sẽ chặn
khi một

IP bị đưa vào danh sách cấm (ban)

**logpath:** Trường logpath chỉ định đường dẫn tới file log mà Fail2Ban
sẽ giám

sát để phát hiện các hành vi đáng ngờ.

\- Ở bài lab này đã được cấu hình sẵn công cụ fail2ban.

\- Cấu hình fail2ban với modsecurity: sử dụng iptables, port http https,
logpath: %(apache_error_log)s, thời gian ban 600 giây, số lần maxretry
10:

Thêm đoạn dưới đây vào file **jail.local**

> **\[apache-modsecurity\]**
>
> **action = iptables\[name=ModSecurity, port=http, protocol=tcp\]**
>
> **port = http,https**
>
> **logpath = %(apache_error_log)s**
>
> **maxretry = 10**
>
> **bantime = 600**
>
> **enabled = true**

-   Mở file jail.local và thêm cấu hình trên vào dưới
    > **\[apache-modsecurity\]**

> **nano /etc/fail2ban/jail.local**

-   Đóng file kiểm tra status của fail2ban xem đã nhận thêm cấu hình
    > chưa:

> **fail2ban-client status**

-   Kiểm tra hoạt động của cấu hình với modsecurity

**fail2ban-client status apache-modsecurity**

![A computer screen shot of a computer error Description automatically
generated](media/image37.png){width="6.575in"
height="1.6916666666666667in"}

9.  []{#_heading=h.2xcytpi .anchor}*: Kiểm tra hoạt động cấu hình
    > fail2ban với modsecurity*

-   Thực hiện gửi nhiều request độc hại đến website kiểm tra xem ip có
    > bị ban hay không

![A computer screen shot of a computer error Description automatically
generated](media/image24.png){width="6.575in"
height="1.9458333333333333in"}

10. []{#_heading=h.1ci93xb .anchor}*: IP bị ban*

-   Như vậy sau khi thực hiện các requests độc hại từ ip 172.20.0.1 thì
    > địa chỉ này đã bị ban, không thể truy cập đến website.

-   Để unban chạy lệnh: **fail2ban-client unban 172.20.0.1**

**Mục tiêu 4: [Cấu hình gửi thông báo đến quản trị viên qua
Telegram]{.mark}**

**Bước 1: Tạo 1 bot trên Telegram**

-   Mở ứng dụng Telegram và tìm kiếm \"**BotFather**\".

-   Chọn BotFather trong kết quả tìm kiếm.

-   Gửi tin nhắn \"**/newbot**\" để tạo một bot mới.

-   Bạn sẽ được yêu cầu đặt tên cho bot. Hãy chọn một tên phù hợp và
    > tuân thủ theo quy tắc của Telegram.

-   Lấy **TOKEN**

![A screenshot of a chat Description automatically
generated](media/image22.png){width="6.267716535433071in"
height="3.6666666666666665in"}

11. []{#_heading=h.3whwml4 .anchor}*: Tạo bot telegram*

Sau khi hoàn thành, BotFather sẽ cung cấp cho bạn mã truy cập (token)
cho bot. Lưu mã này lại vì chúng ta sẽ sử dụng nó trong bước tiếp theo.

**Bước 2: Tạo group, lấy id nhóm**

-   **Tạo group trên telegram**

```{=html}
<!-- -->
```
-   Thêm "**\@myidbot**" vào nhóm

-   Chat "**/getgroupid@myidbot**" trong nhóm -\> **lấy ID (lấy cả dấu
    > trừ)**

-   Thêm Bot Telegram vừa tạo vào nhóm.

![Lấy ID nhóm muốn gửi tin
nhắn](media/image30.png){width="6.267716535433071in"
height="3.9166666666666665in"}

12. []{#_heading=h.2bn6wsx .anchor}*: Tạo group với bot telegram*

-   Chú ý trong trường hợp không trả về ID group thì có thể kiểm tra
    > trên đường dẫn url của group telegram

![A screenshot of a computer Description automatically
generated](media/image28.png){width="5.9070745844269466in"
height="0.625087489063867in"}

13. []{#_heading=h.qsh70q .anchor}*: Đường dẫn url của group telegram*

**Bước 3: Nhập token và id**

-   Mở file **/usr/local/bin/fail2ban_notify_telegram.sh** thay token và
    > id đã có được: **nano /usr/local/bin/fail2ban_notify_telegram.sh**

![A screenshot of a computer Description automatically
generated](media/image9.png){width="6.575in"
height="1.4270833333333333in"}

14. []{#_heading=h.3as4poj .anchor}*: File fail2ban_notify_telegram.sh*

-   File **fail2ban_notify_telegram.sh** sẽ lấy thông tin ban ip của
    > fail2ban để gửi về telegram thông qua **token** và **id**

-   **fail2ban_notify_telegram.sh** sẽ được chạy với một service được
    > tạo ra **fail2ban-notify.service,** khi có ip bị ban thì service
    > này hoạt động chạy **fail2ban_notify_telegram.sh** để gửi thông
    > tin đó về telegram.

![A computer screen with a purple background Description automatically
generated](media/image29.png){width="6.575in"
height="1.3680555555555556in"}

15. []{#_heading=h.1pxezwc .anchor}*: File fail2ban-notify.service*

-   Sau khi sửa token và id thực hiện restart
    > **fail2ban-notify.service;**

> **systemctl restart fail2ban-notify.service**

-   **Thực hiện gửi requests độc hại đến website -\> IP bị ban đã được
    > gửi về telegram**

![A screenshot of a chat Description automatically
generated](media/image34.png){width="6.267716535433071in"
height="2.0555555555555554in"}

16. []{#_heading=h.49x2ik5 .anchor}*: Kết quả gửi về telegram*

-   Tương tự sửa token và id của file **sendlog_telegram.py** ở đường
    > dẫn **/root**

![A screenshot of a computer Description automatically
generated](media/image11.png){width="6.575in"
height="2.5965277777777778in"}

17. []{#_heading=h.2p2csry .anchor}*: File sendlog_telegram.py*

-   File này gửi các request độc hại cảnh báo về telegram thông qua
    > service **modsecurity_log_to_telegram.service**

![A screenshot of a computer Description automatically
generated](media/image31.png){width="6.575in" height="1.46875in"}

18. []{#_heading=h.147n2zr .anchor}*: File
    > modsecurity_log_to_telegram.service*

![A screenshot of a computer Description automatically
generated](media/image36.png){width="6.267716535433071in"
height="4.013888888888889in"}

19. []{#_heading=h.3o7alnk .anchor}*: Log request độc hại gửi về
    > telegram*

**Kết thúc bài lab:**

-   Trên terminal đầu tiên sử dụng câu lệnh sau để kết thúc bài lab:

> **stoplab apache_modsecurity**

-   Khi bài lab kết thúc, một tệp zip lưu kết quả được tạo và lưu vào
    > một vị trí được hiển thị bên dưới stoplab.

**Khởi động lại bài lab:**

-   Trong quá trình làm bài sinh viên cần thực hiện lại bài lab, dùng
    > câu lệnh:

> **labtainer --r dg-n2-trong-1**

1.  []{#_heading=h.23ckvvd .anchor}Phân tích yêu cầu bài thực hành

Bài thực hành yêu cầu cấu hình ModSecurity trên Apache, đồng thời tích
hợp với Fail2ban để xử lý và giám sát các vi phạm quy tắc bảo mật. Khi
một địa chỉ IP bị cấm bởi Fail2ban, thông tin về IP, lý do bị cấm và
thời gian bị cấm cần được gửi về Telegram để thông báo. Để hoàn thành
bài thực hành, sinh viên cần hiểu rõ các khái niệm và công cụ được sử
dụng: modsecurity, fai2ban, api telegram.

Để đáp ứng yêu cầu bài thực hành, hệ thống cần cung cấp một máy ảo chứa
Docker file chạy hệ điều hành linux đã được cài sẵn các dịch vụ apache,
modsecurity, fail2ban, iptables. Hệ thống cần ghi lại được các thao tác
sử dụng trên hệ thống của sinh viên thông qua các câu lệnh để tạo ra
được kết quả đánh giá. Hệ thống yêu cầu sinh viên nhập email gắn liền
với danh tính của sinh viên, và ghi lại thao tác mở tệp để thực hiện
việc cá nhân hóa cho từng sinh viên.

Để bắt đầu bài thực hành, sinh viên cần phải sử dụng các câu lệnh khởi
tạo (startlab \<tên bài lab\>) và câu lệnh kết thúc (stoplab \<tên bài
lab\>) để hệ thống chạy bài lab cũng như lưu lại kết quả.

**Thiết kế bài thực hành**

Trên môi trường máy ảo Ubuntu được cung cấp, sử dụng docker tạo ra 1
container: mang tên "**apache_modsecurity**".

-   **Dockerfile**: sử dụng images được tạo sẵn (đã cài đặt đầy đủ
    > apache. modsecurity, fail2ban, iptables). Địa chỉ trong mạng LAN:
    > 172.20.0.2 Gateway: 172.20.0.1

-   **docs**: lưu phần mô tả hướng dẫn làm bài thực hành cho sinh viên:

    -   Các nhiệm vụ cần thực hiện:

        -   [Cấu hình được waf modsecurity]{.mark}

        -   [Tạo được rule cho waf modsecurity]{.mark}

        -   [Cấu hình được fail2ban tích hợp với log của
            > modsecurity]{.mark}

        -   [Cấu hình gửi thông báo đến quản trị viên qua
            > Telegram]{.mark}

-   [**instr_config**: lưu cấu hình cho phần nhận kết quả và chấm
    > điểm.]{.mark}

-   [Sau khi hoàn thành bài thực hành, hệ thống cần tự động lưu lại kết
    > quả vào 1 file.]{.mark}

-   [Để đánh giá được sinh viên đã hoàn thành bài thực hành hay chưa,
    > cần chia bài thực hành thành các nhiệm vụ nhỏ, mỗi nhiệm vụ cần
    > phải chỉ rõ kết quả để có thể dựa vào đó đánh giá, chấm
    > điểm.]{.mark}

-   **config**: lưu cấu hình hoạt động của hệ thống

**Các nhiệm vụ cần phải thực hiện để thực hành thành công:**

-   Bật engine waf modsecurity: chuyển "[SecRuleEngine On]{.mark}" trong
    > file /etc/modsecurity/modsecurity.conf

-   Tạo được rule cho waf modsecurity: tạo rule trong file
    > /etc/modsecurity/modsecurity.conf , kết quả xem rule hoạt động
    > không: kiểm tra file [/var/log/apache2/access.log]{.mark}
    > [có]{.mark} \"GET
    > /wordpress/wp-login.php?redirect_to=http%3A%2F%2F172.20.0.2%2Fwordpress%2Fwp-admin%2F&reauth=1
    > HTTP/1.1\" 200

-   Cấu hình được fail2ban tích hợp với log của modsecurity: tạo cấu
    > hình trong file jail.local, kiểm tra status xem đã hoạt động chưa:
    > kiểm tra file [/var/log/fail2ban.log]{.mark} [có]{.mark} Jail
    > \'apache-modsecurity\' started

-   Cấu hình gửi thông báo đến quản trị viên qua Telegram: kiểm tra
    > trong file /var/log/modsecurity_telegram_log.log có Log sent
    > successfully

**Kết thúc bài lab và đóng gói kết quả.**

Để đánh giá được sinh viên đã hoàn thành bài thực hành hay chưa, cần
chia bài thực hành thành các nhiệm vụ nhỏ, mỗi nhiệm vụ cần phải chỉ rõ
kết quả để có thể dựa vào đó đánh giá, chấm điểm. Do vậy, trong bài thực
hành này hệ thống cần ghi nhận các thao tác, sự kiện được mô tả và cấu
hình như bảng 1:

[]{#_heading=h.32hioqz .anchor}Bảng1. Bảng Result

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [Result Tag]{.mark}        [Container]{.mark}            [File]{.mark}                                    [Field Type]{.mark} [Field ID]{.mark}                                                                                [Timestamp
                                                                                                                                                                                                                                 Type]{.mark}
  -------------------------- ----------------------------- ------------------------------------------------ ------------------- ------------------------------------------------------------------------------------------------ ---------------
  [modsecurity]{.mark}       [apache_modsecurity]{.mark}   [/etc/modsecurity/modsecurity.conf]{.mark}       [CONTAINS]{.mark}   [SecRuleEngine On]{.mark}                                                                        [file]{.mark}

  [fail2ban-client]{.mark}   [apache_modsecurity]{.mark}   [/var/log/fail2ban.log]{.mark}                   [CONTAINS]{.mark}   [Jail \'apache-modsecurity\' started]{.mark}                                                     [file]{.mark}

  [access-rule]{.mark}       [apache_modsecurity]{.mark}   [/var/log/apache2/access.log]{.mark}             [CONTAINS]{.mark}   [\"GET                                                                                           [file]{.mark}
                                                                                                                                /wordpress/wp-login.php?redirect_to=http%3A%2F%2F172.20.0.2%2Fwordpress%2Fwp-admin%2F&reauth=1   
                                                                                                                                HTTP/1.1\" 200]{.mark}                                                                           

  [telegram]{.mark}          [apache_modsecurity]{.mark}   [/var/log/modsecurity_telegram_log.log]{.mark}   [CONTAINS]{.mark}   [Log sent successfully]{.mark}                                                                   [file]{.mark}
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  []{#_heading=h.2u6wntf .anchor}**Cài đặt và cấu hình các máy ảo**

![A computer screen shot of a network Description automatically
generated](media/image3.png){width="6.575in"
height="2.2881944444444446in"}

20. []{#_heading=h.19c6y18 .anchor}*: Giao diện labedit của lab*

![A screenshot of a computer Description automatically
generated](media/image4.png){width="6.575in"
height="2.372916666666667in"}

21. []{#_heading=h.3tbugp1 .anchor}*: Giao diện result*

![A white background with black text Description automatically
generated](media/image16.png){width="6.575in" height="3.775in"}

22. []{#_heading=h.28h4qwu .anchor}*: Docker file*

    1.  []{#_heading=h.nmf14n .anchor}**Tích hợp và triển khai**

Bài thực hành đã được triển khai như sau:

1.  []{#_heading=h.37m2jsg .anchor}**Docker Hub**

https://hub.docker.com/u/ phamhduy

![](media/image12.png){width="6.2652777777777775in"
height="0.8305555555555556in"}

23. []{#_heading=h.1mrcu09 .anchor}*: Add và commit bài lab*

![](media/image14.png){width="5.607638888888889in"
height="1.1076388888888888in"}

24. []{#_heading=h.46r0co2 .anchor}*: Đẩy các vùng chứa lên dockerhub*

![](media/image17.png){width="6.2652777777777775in"
height="0.9041666666666667in"}

25. []{#_heading=h.2lwamvv .anchor}*: Tạo imodule.tar chứa bài thực
    > hành*

![A close up of a screen Description automatically
generated](media/image5.png){width="6.575in"
height="1.2569444444444444in"}

26. []{#_heading=h.111kx3o .anchor}*: Các vùng chứa được đẩy lên
    > dockerhub*

    1.  []{#_heading=h.3l18frh .anchor}**Github**

** **https://github.com/TranDangTrong28102002/dg-n2-trong-1

Nhập lệnh create-imodules.sh

![A screenshot of a computer program Description automatically
generated](media/image13.png){width="6.267716535433071in"
height="1.0694444444444444in"}

27. []{#_heading=h.206ipza .anchor}*: Tạo file Imodule.tar*

![A screenshot of a computer Description automatically
generated](media/image18.png){width="6.575in"
height="2.3465277777777778in"}

28. []{#_heading=h.4k668n3 .anchor}*: File imodule.tar chứa bài thực
    > hành*

Tạo repo mới để đẩy imodule.tar lên và tạo phần release mới

![A screenshot of a computer Description automatically
generated](media/image20.png){width="6.575in"
height="2.245138888888889in"}

29. []{#_heading=h.2zbgiuw .anchor}*: Đẩy file imodule.tar lên github*

    1.  []{#_heading=h.1egqt2p .anchor}**Thử nghiệm và đánh giá**

Bài thực hành đã được xây dựng thành công, dưới đây là hình ảnh minh họa
về bài thực hành:

![A screenshot of a computer Description automatically
generated](media/image32.png){width="6.575in"
height="2.3534722222222224in"}

30. []{#_heading=h.3ygebqi .anchor}*: IP của máy apachemodsecurity*

![A screenshot of a computer Description automatically
generated](media/image2.png){width="6.575in"
height="2.686111111111111in"}

31. []{#_heading=h.2dlolyb .anchor}*: File cấu hình modsecurity đã bât
    > engine*

![A screenshot of a computer Description automatically
generated](media/image6.png){width="6.573966535433071in"
height="2.4166666666666665in"}

32. []{#_heading=h.sqyw64 .anchor}*: Engine modsecurity đã hoạt động*

![A screenshot of a computer program Description automatically
generated](media/image7.png){width="6.575in"
height="3.0409722222222224in"}

33. []{#_heading=h.3cqmetx .anchor}*: Thêm rule SecRule REQUEST_URI
    > \"@beginsWith /wordpress/wp-login.php\"
    > \"id:1000002,phase:1,allow,ctl:ruleEngine=Off\" vào file cấu hình
    > modsecurity*

![A screenshot of a computer Description automatically
generated](media/image8.png){width="6.575in"
height="2.7111111111111112in"}

34. []{#_heading=h.1rvwp1q .anchor}*: Rule chạy thành công*

![A screenshot of a computer Description automatically
generated](media/image15.png){width="6.575in"
height="1.2895833333333333in"}

35. []{#_heading=h.4bvk7pj .anchor}*: Cấu hình [tích hợp được fail2ban
    > với modsecurity]{.mark}*

![A computer screen with white text Description automatically
generated](media/image10.png){width="6.575in"
height="1.7527777777777778in"}

36. []{#_heading=h.2r0uhxc .anchor}*: Cấu hình tích hợp được fail2ban
    > với modsecurity đã hoạt động*

![A screenshot of a computer Description automatically
generated](media/image9.png){width="6.575in"
height="1.4270833333333333in"}

37. []{#_heading=h.1664s55 .anchor}*: File gửi ip ban đến telegram*

![A screenshot of a computer Description automatically
generated](media/image11.png){width="6.575in"
height="2.5965277777777778in"}

38. []{#_heading=h.3q5sasy .anchor}*: File gửi log apache đến telegram*

![A computer screen with text Description automatically
generated](media/image1.png){width="6.575in"
height="1.3291666666666666in"}

39. []{#_heading=h.25b2l0r .anchor}*: Đánh giá kết quả bài thực hành*

[]{#_heading=h.34g0dwd .anchor}**[TÀI LIỆU THAM KHẢO]{.smallcaps}**
