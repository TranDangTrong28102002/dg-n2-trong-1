# dg-n2-trong-1
git clone https://github.com/TranDangTrong28102002/dg-n2-trong-1

HỌC VIỆN CÔNG NGHỆ BƯU CHÍNH VIỄN THÔNG
KHOA AN TOÀN THÔNG TIN
-------🙞🙜🕮🙞🙜-------







Báo cáo bài thực hành



Tìm hiểu web application firewall cho web application và fail2ban để ngăn chặn các hành vi tấn công

					  
Sinh viên thực hiện:
      B20DCAT191  Trần Đăng Trọng

		



       Giảng viên hướng dẫn: Phạm Hoàng Duy







MỤC LỤC
MỤC LỤC	1
DANH MỤC CÁC HÌNH VẼ	2
DANH MỤC CÁC BẢNG BIỂU	3
DANH MỤC CÁC TỪ VIẾT TẮT	4
1.1 Giới thiệu chung về bài thực hành	5
1.2 Nội dung và hướng dẫn bài thực hành	5
1.2.1 Mục đích	5
1.2.2 Yêu cầu đối với sinh viên	5
1.2.3 Nội dung thực hành	5
1.3 Phân tích yêu cầu bài thực hành	20
1.4 Cài đặt và cấu hình các máy ảo	23
1.5 Tích hợp và triển khai	24
1.5.1 Docker Hub	24
1.5.2 Github	25
1.6 Thử nghiệm và đánh giá	26
TÀI LIỆU THAM KHẢO	29


DANH MỤC CÁC HÌNH VẼ

Hình 1 Gửi request độc hại lên webserver	6
Hình 2 : Kết quả khi thực hiện request độc hại	9
Hình 3 : Log của request độc hại	10
Hình 4 : Cấu hình website	11
Hình 5 : Điền các thông tin	11
Hình 6 Giao diện website	11
Hình 7 Truy cập trang quản trị của website	12
Hình 8 : Giao diện trang quản trị website	13
Hình 9 : Kiểm tra hoạt động cấu hình fail2ban với modsecurity	15
Hình 10 : IP bị ban	16
Hình 11 : Tạo bot telegram	16
Hình 12 : Tạo group với bot telegram	17
Hình 13 : Đường dẫn url của group telegram	17
Hình 14 : File fail2ban_notify_telegram.sh	18
Hình 15 : File fail2ban-notify.service	18
Hình 16 : Kết quả gửi về telegram	18
Hình 17 : File sendlog_telegram.py	19
Hình 18 : File modsecurity_log_to_telegram.service	19
Hình 19 : Log request độc hại gửi về telegram	20
Hình 20 : Giao diện labedit của lab	23
Hình 21 :  Giao diện result	23
Hình 22 : Docker file	24
Hình 23 : Add và commit bài lab	24
Hình 24 : Đẩy các vùng chứa lên dockerhub	24
Hình 25 : Tạo imodule.tar chứa bài thực hành	24
Hình 26 : Các vùng chứa được đẩy lên dockerhub	25
Hình 27 : Tạo file Imodule.tar	25
Hình 28 : File imodule.tar chứa bài thực hành	25
Hình 29 : Đẩy file imodule.tar lên github	26
Hình 30 : IP của máy apachemodsecurity	26
Hình 31 : File cấu hình modsecurity đã bât engine	27
Hình 32 :  Engine modsecurity đã hoạt động	27
Hình 33 : Thêm rule SecRule REQUEST_URI "@beginsWith /wordpress/wp-login.php" "id:1000002,phase:1,allow,ctl:ruleEngine=Off" vào file cấu hình modsecurity	27
Hình 34 : Rule chạy thành công	28
Hình 35 : Cấu hình tích hợp được fail2ban với modsecurity	28
Hình 36 : Cấu hình tích hợp được fail2ban với modsecurity đã hoạt động	28
Hình 37 : File gửi ip ban đến telegram	29
Hình 38 : File gửi log apache đến telegram	29
Hình 39 : Đánh giá kết quả bài thực hành	29



DANH MỤC CÁC BẢNG BIỂU
Bảng1. Bảng Result	22


DANH MỤC CÁC TỪ VIẾT TẮT
Từ 
viết tắt
Thuật ngữ tiếng Anh/Giải thích
Thuật ngữ tiếng Việt/Giải thích





























































































Giới thiệu chung về bài thực hành
Bài thực hành này tập trung vào việc sử dụng công cụ ModSecurity – một Web Application Firewall (WAF) và công cụ Fail2ban để phát hiện và ngăn chặn các hành vi tấn công vào ứng dụng web. ModSecurity là một module bảo mật mạnh mẽ cho các máy chủ web như Apache, Nginx, và IIS. Nó giúp phát hiện và ngăn chặn các cuộc tấn công vào ứng dụng web (SQL Injection, XSS, ...). Fail2ban là một công cụ bảo mật tự động hóa, hoạt động bằng cách giám sát log files và thực hiện các hành động (như chặn IP) khi phát hiện hành vi đáng ngờ. Fail2ban có thể đọc log từ ModSecurity để chặn ngay các IP thực hiện tấn công ứng dụng web, đảm bảo bảo vệ cả hai lớp.
Nội dung và hướng dẫn bài thực hành
Mục đích
Giúp sinh viên tìm hiểu khái niệm, cách sử dụng công cụ ModSecurity và Fail2ban để có thể bảo vệ ứng dụng web khỏi các hoạt động độc hại, bao gồm các cuộc tấn công SQL Injection, XSS, ....
Bài thực hành này cũng có thể áp dụng vào thực tế cho các trang web bán hàng trực tuyến của công ty là trung tâm giao dịch với hàng nghìn người dùng mỗi ngày.
Để bảo vệ hệ thống với: 
ModSecurity được triển khai làm Web Application Firewall (WAF) nhằm phân tích và chặn các truy cập độc hại ngay từ đầu.
Fail2Ban được sử dụng để chặn các địa chỉ IP có hành vi tấn công lặp đi lặp lại, đồng thời gửi thông báo đến quản trị viên qua Telegram.
Yêu cầu đối với sinh viên
Có kiến thức cơ bản về hệ điều hành Linux, webserver: apache, firewall: iptables.
Nội dung thực hành
Khởi động bài lab:
Vào terminal, gõ: 
labtainer dg-n2-trong-1
 (chú ý: sinh viên sử dụng mã sinh viên của mình để nhập thông tin email người thực hiện bài lab khi có yêu cầu, để sử dụng khi chấm điểm)
Sau khi khởi động xong 1 terminal ảo sẽ xuất hiện: apache_modsecurity - máy ubuntu 20.04 -> truy cập vào root: sudo su
Trên terminal apache_modsecurity sử dụng lệnh “ifconfig”, xác định địa chỉ IP của máy 
Bài lab sử dụng web server là apache, kiểm tra status của dịch vụ apache
		systemctl status apache2
Gửi một request độc hại (XSS) đến website, kiểm tra kết quả
		http://IP/?q="><script>alert(0)</script>

: Gửi request độc hại lên webserver
-> kiểm tra và thấy được request đã được gửi đến và mã trả về 200, trường hợp website có tồn tại lỗ hổng thì hacker dễ dàng rce
Mục tiêu 1: Cấu hình waf modsecurity:
Hoạt động của modsecurity:
Modsecurity có cơ chế hoạt động như một cỗ máy IDPS. Công cụ này thực hiện quá trình phát hiện các xâm nhập lạ vào website.
ModSecurity bắt đầu từ việc giám sát luồng dữ liệu HTTP với các yêu cầu được gửi tới Server. Sau đó, Modsecurity sẽ phân tích các yêu cầu để tìm ra sự cố bất thường.
Những bất thường này thường vi phạm chính sách an ninh của các web server. Khi đó, Modsecurity sẽ giúp ngăn chặn hoặc loại bỏ các yêu cầu để bảo vệ hệ thống web.
Có hai phương pháp để Modsecurity thực hiện ngăn chặn các tấn công. Đó là dựa vào mẫu được lập trình sẵn và dựa vào các dấu hiệu bất thường. Việc phát hiện lỗi từ luồng dữ liệu của Modsecurity trở nên đơn giản hơn, nhanh chóng hơn.
Đối với các lỗi được xác định bất thường, Modsecurity sẽ tập hợp và đánh giá theo mức độ. Lỗi càng cao đồng nghĩa với mức độ đe dọa an ninh càng lớn. Khi đó, công cụ sẽ chặn hoặc loại bỏ các yêu cầu được gửi đến.
Tính năng nổi bật và vai trò của Modsecurity:
Phòng chống tấn công: Modsecurity cung cấp các bộ quy tắc mạnh mẽ để phát hiện và ngăn chặn các loại tấn công web phổ biến như SQL Injection, Cross-Site Scripting (XSS), Remote File Inclusion (RFI), và nhiều hơn nữa. 
Bảo vệ dữ liệu và hệ thống: Bằng cách ngăn chặn các cuộc tấn công, Modsecurity có thể giúp bảo vệ dữ liệu quan trọng và hệ thống khỏi sự phá hủy hoặc truy cập trái phép. 
Kiểm soát truy cập: Modsecurity cho phép thiết lập các quy tắc để kiểm soát việc truy cập vào ứng dụng web, bao gồm việc chặn hoặc cho phép truy cập từ các địa chỉ IP cụ thể, ngăn chặn các yêu cầu từ các user-agent không mong muốn, và nhiều hơn nữa. 
Phân tích hành vi tấn công: Modsecurity cung cấp các công cụ phân tích và báo cáo để theo dõi và hiểu rõ các loại tấn công và mẫu tấn công phổ biến, từ đó cung cấp thông tin quý giá cho việc tăng cường bảo mật hệ thống. 
Tùy biến linh hoạt: Với các cấu hình linh hoạt và hỗ trợ cho ngôn ngữ lập trình Lua, Modsecurity cho phép người quản trị tùy chỉnh và mở rộng chức năng của nó để phù hợp với nhu cầu cụ thể của môi trường và ứng dụng web của họ.
Cấu trúc của luật ModSecurity: 
Một luật ModSecurity thường được viết trong file cấu hình (.conf) theo định dạng:
SecRule [target] [operator] [action]
Thành phần chi tiết:
Target: Xác định thành phần HTTP nào sẽ được kiểm tra (vd: URL, headers, body, v.v.).
Operator: Xác định điều kiện kiểm tra (vd: chuỗi, biểu thức chính quy, so sánh số).
Action: Định nghĩa hành động khi luật được kích hoạt.
- Các nguồn hình thành luật ModSecurity
a. Bộ luật OWASP Core Rule Set (CRS)
Đây là bộ luật phổ biến và mạnh mẽ nhất, được duy trì bởi cộng đồng bảo mật OWASP.
CRS bao gồm các luật để phát hiện và ngăn chặn:
SQL Injection
Cross-Site Scripting (XSS)
Local File Inclusion (LFI)
Remote File Inclusion (RFI)
Brute force login
Và nhiều lỗ hổng OWASP Top 10 khác.
b. Luật tùy chỉnh
Quản trị viên có thể tự viết luật để phù hợp với nhu cầu bảo mật riêng.
Ví dụ: 
Chặn yêu cầu với các từ khóa đặc biệt:
SecRule REQUEST_URI "@rx (?i)select.*from" "id:1001,phase:2,deny,status:403,msg:'SQL Injection detected'"
Chặn yêu cầu đến một endpoint cụ thể:
SecRule REQUEST_URI "/admin" "id:1002,phase:2,deny,status:403,msg:'Unauthorized access to /admin'"
Bỏ chặn yêu cầu đến một endpoint trang wp-login.php cụ thể:
SecRule REQUEST_URI "@beginsWith /wp-login.php" 
"id:1000002,phase:1,allow,ctl:ruleEngine=Off"
Với lab này đã được cấu hình modsecurity với apache và sử dụng OWASP Core Rule Set (CRS)
Sửa file cấu hình để bật waf modsecurity:
nano /etc/modsecurity/modsecurity.conf
Chuyển SecRuleEngine Off -> SecRuleEngine On, đóng file và restart lại apache:
systemctl restart apache2
Kiểm tra xem modsecurity đã hoạt động hạy chưa, thực hiện gửi 1 request độc hại đến website:
http://IP/?q="><script>alert(0)</script>

: Kết quả khi thực hiện request độc hại
->  Kết quả trả về mã trạng thái 403 Forbidden, như vậy ngăn chặn được request độc hại 
	- Kiểm tra trong log xem thông tin độc hại
		nano  /var/log/apache2/modsec_audit.log  
Kết quả thấy được đây là requests được đánh giá là dạng tấn công XSS áp dụng rule của OWASP_CRS/3.3.0

: Log của request độc hại
Mục tiêu 2: Tạo luật cho modsecurity 
Trong bài lab có tạo 1 trang sử dụng wordpress – chưa được cấu hình, truy cập  http://IP/wordpress để cấu hình wordpress:

: Cấu hình website
	-> Chọn tiếng việt

: Điền các thông tin 
	-> Điền các nội dung cơ bản 
      -	Sau khi hoàn tất quay trở lại  http://IP/wordpress


: Giao diện website
Trang web có trang wp-admin dành cho quản trị viên truy cập để code, bảo trì. Sau khi cài modsecurity lên thì bị waf chặn vì bị đánh giá là request độc hại. Tiến hành tạo rule cho phép truy cập vào trang wp-admin.
Truy cập http://IP/wordpress/wp-admin -> trả về kết quả 403 Forbidden


: Truy cập trang quản trị của website
Dựa vào luật tùy chỉnh của modsecurity phần mục tiêu 1 để thực hiện tạo rule
Mở file cấu hình modsecurity để tạo rule:
nano  /etc/modsecurity/modsecurity.conf 
	Thêm các thông tin sau: 
SecRule REQUEST_URI "@beginsWith /wordpress/wp-login.php" "id:1000002,phase:1,allow,ctl:ruleEngine=Off"
Viết rule đóng file cấu hình, restart lại apache sau đó kiểm tra hoạt động của rule vừa tạo:
systemctl restart apache2
Truy cập lại http://IP/wordpress/wp-admin -> trả về 200 xuất hiện màn hình login như vậy rule đã hoạt động

: Giao diện trang quản trị website
Mục tiêu 3: Tích hợp với fail2ban để ban ip 
Fail2ban làm một ứng dụng được viết bằng ngôn ngữ Python hỗ trợ phân tích, theo dõi log của hệ thống nhằm phát hiện và ngăn chặn các cuộc tấn công
Ứng dụng Fail2ban tập trung phát triển để bảo vệ SSH và ngăn chặn các cuộc tấn công vào SSH như brute force attack là chính. Tuy nhiên, có thể thiết lập các rule, các tham số để sử dụng trên bất cứ dịch vụ nào có hỗ trợ log file.
Cấu hình Fail2ban:
Nếu không biết nhiều về cách cấu hình Fail2ban, có thể sử dụng chế độ mặc định của Fail2ban được thiết lập sẵn và rất ổn. Chạy lệnh sau đây để xem cấu hình mặc định của Fail2ban bằng Nano text:
nano /etc/fail2ban/jail.conf
Đây là cấu hình mặc định của Fail2ban:
[DEFAULT]
ignoreip = 127.0.0.1
bantime = 600
findtime = 600
maxretry = 3
Giải thích về cấu hình mặc định của Fail2ban:
ignoreip: danh sách những địa chỉ IP không bị block, có thể thấy, địa chỉ 127.0.0.1 sẽ mặc định không bị chặn.
bantime: nếu bị block, địa chỉ IP đó sẽ bị block trong x giây.
findtime: khoản thời gian (tính bằng giây) địa chỉ IP đó phải đăng nhập thành công. Nếu không, Fail2ban sẽ block IP dựa trên số lần đăng nhập thất bại.
maxretry: số lần đăng nhập thất bại được cho phép
Ngoài ra còn có:
action:  là tập hợp các bước hoặc lệnh để xử lý khi phát hiện vi phạm. Có 
nhiều loại hành động phổ biến. Thường sử dụng iptables hoặc ufw để 
chặn IP vi phạm trên một hoặc nhiều cổng.
	port: Trường port xác định cổng hoặc các cổng mà Fail2Ban sẽ chặn khi một
IP bị đưa vào danh sách cấm (ban)
	logpath: Trường logpath chỉ định đường dẫn tới file log mà Fail2Ban sẽ giám  
sát để phát hiện các hành vi đáng ngờ.
- Ở bài lab này đã được cấu hình sẵn công cụ fail2ban.
- Cấu hình fail2ban với modsecurity: sử dụng iptables, port http https, logpath:  %(apache_error_log)s, thời gian ban 600 giây, số lần maxretry 10: 
	Thêm đoạn dưới đây vào file jail.local
[apache-modsecurity]
action   = iptables[name=ModSecurity, port=http, protocol=tcp]
port     = http,https
logpath  = %(apache_error_log)s
maxretry = 10
bantime  = 600
enabled = true
Mở file jail.local và thêm cấu hình trên vào dưới [apache-modsecurity]
nano  /etc/fail2ban/jail.local 
Đóng file kiểm tra status của fail2ban xem đã nhận thêm cấu hình chưa:
fail2ban-client status
Kiểm tra hoạt động của cấu hình với modsecurity 
		fail2ban-client status apache-modsecurity

: Kiểm tra hoạt động cấu hình fail2ban với modsecurity
Thực hiện gửi nhiều request độc hại đến website kiểm tra xem ip có bị ban hay không

: IP bị ban 
Như vậy sau khi thực hiện các requests độc hại từ ip 172.20.0.1 thì địa chỉ này đã bị ban, không thể truy cập đến website.
Để unban chạy lệnh: fail2ban-client unban 172.20.0.1
Mục tiêu 4: Cấu hình gửi thông báo đến quản trị viên qua Telegram
Bước 1: Tạo 1 bot trên Telegram
Mở ứng dụng Telegram và tìm kiếm "BotFather".
Chọn BotFather trong kết quả tìm kiếm.
Gửi tin nhắn "/newbot" để tạo một bot mới.
Bạn sẽ được yêu cầu đặt tên cho bot. Hãy chọn một tên phù hợp và tuân thủ theo quy tắc của Telegram.
Lấy TOKEN

: Tạo bot telegram 
Sau khi hoàn thành, BotFather sẽ cung cấp cho bạn mã truy cập (token) cho bot. Lưu mã này lại vì chúng ta sẽ sử dụng nó trong bước tiếp theo.
Bước 2: Tạo group, lấy id nhóm 
Tạo group trên telegram
Thêm “@myidbot” vào nhóm
Chat “/getgroupid@myidbot” trong nhóm -> lấy ID (lấy cả dấu trừ)
Thêm Bot Telegram vừa tạo vào nhóm.

: Tạo group với bot telegram
Chú ý trong trường hợp không trả về ID group thì có thể kiểm tra trên đường dẫn url của group telegram

: Đường dẫn url của group telegram
Bước 3: Nhập token và id 
Mở file  /usr/local/bin/fail2ban_notify_telegram.sh thay token và id đã có được:  nano  /usr/local/bin/fail2ban_notify_telegram.sh 

: File fail2ban_notify_telegram.sh
File fail2ban_notify_telegram.sh sẽ lấy thông tin ban ip của fail2ban để gửi về telegram thông qua token và id 
fail2ban_notify_telegram.sh sẽ được chạy với một service được tạo ra fail2ban-notify.service, khi có ip bị ban thì service này hoạt động chạy fail2ban_notify_telegram.sh để gửi thông tin đó về telegram.

: File fail2ban-notify.service
Sau khi sửa token và id thực hiện restart fail2ban-notify.service;
systemctl restart fail2ban-notify.service
Thực hiện gửi requests độc hại đến website -> IP bị ban đã được gửi về telegram

: Kết quả gửi về telegram
Tương tự sửa token và id của file sendlog_telegram.py ở đường dẫn /root

: File sendlog_telegram.py
File này gửi các request độc hại cảnh báo về telegram thông qua service modsecurity_log_to_telegram.service

: File modsecurity_log_to_telegram.service

: Log request độc hại gửi về telegram 
Kết thúc bài lab:
Trên terminal đầu tiên sử dụng câu lệnh sau để kết thúc bài lab:
stoplab apache_modsecurity
Khi bài lab kết thúc, một tệp zip lưu kết quả được tạo và lưu vào một vị trí được hiển thị bên dưới stoplab.
Khởi động lại bài lab:
Trong quá trình làm bài sinh viên cần thực hiện lại bài lab, dùng câu lệnh:
                        labtainer –r dg-n2-trong-1
Phân tích yêu cầu bài thực hành
Bài thực hành yêu cầu cấu hình ModSecurity trên Apache, đồng thời tích hợp với Fail2ban để xử lý và giám sát các vi phạm quy tắc bảo mật. Khi một địa chỉ IP bị cấm bởi Fail2ban, thông tin về IP, lý do bị cấm và thời gian bị cấm cần được gửi về Telegram để thông báo. Để hoàn thành bài thực hành, sinh viên cần hiểu rõ các khái niệm và công cụ được sử dụng: modsecurity, fai2ban, api telegram. 
Để đáp ứng yêu cầu bài thực hành, hệ thống cần cung cấp một máy ảo chứa Docker file chạy hệ điều hành linux đã được cài sẵn các dịch vụ apache, modsecurity, fail2ban, iptables. Hệ thống cần ghi lại được các thao tác sử dụng trên hệ thống của sinh viên thông qua các câu lệnh để tạo ra được kết quả đánh giá. Hệ thống yêu cầu sinh viên nhập email gắn liền với danh tính của sinh viên, và ghi lại thao tác mở tệp để thực hiện việc cá nhân hóa cho từng sinh viên.
Để bắt đầu bài thực hành, sinh viên cần phải sử dụng các câu lệnh khởi tạo (startlab <tên bài lab>) và câu lệnh kết thúc (stoplab <tên bài lab>) để hệ thống chạy bài lab cũng như lưu lại kết quả. 
Thiết kế bài thực hành 
Trên môi trường máy ảo Ubuntu được cung cấp, sử dụng docker tạo ra 1 container: mang tên “apache_modsecurity”.
Dockerfile: sử dụng images được tạo sẵn (đã cài đặt đầy đủ apache. modsecurity, fail2ban, iptables). Địa chỉ trong mạng LAN: 172.20.0.2 Gateway: 172.20.0.1
 docs: lưu phần mô tả hướng dẫn làm bài thực hành cho sinh viên:
Các nhiệm vụ cần thực hiện:
Cấu hình được waf modsecurity
Tạo được rule cho waf modsecurity
Cấu hình được fail2ban tích hợp với log của modsecurity
Cấu hình gửi thông báo đến quản trị viên qua Telegram
instr_config: lưu cấu hình cho phần nhận kết quả và chấm điểm.
Sau khi hoàn thành bài thực hành, hệ thống cần tự động lưu lại kết quả vào 1 file. 
Để đánh giá được sinh viên đã hoàn thành bài thực hành hay chưa, cần chia bài thực hành thành các nhiệm vụ nhỏ, mỗi nhiệm vụ cần phải chỉ rõ kết quả để có thể dựa vào đó đánh giá, chấm điểm. 
config: lưu cấu hình hoạt động của hệ thống
Các nhiệm vụ cần phải thực hiện để thực hành thành công:
Bật engine waf modsecurity: chuyển “SecRuleEngine On” trong file /etc/modsecurity/modsecurity.conf 
Tạo được rule cho waf modsecurity: tạo rule trong file /etc/modsecurity/modsecurity.conf , kết quả xem rule hoạt động không: kiểm tra file /var/log/apache2/access.log có "GET /wordpress/wp-login.php?redirect_to=http%3A%2F%2F172.20.0.2%2Fwordpress%2Fwp-admin%2F&reauth=1 HTTP/1.1" 200 
Cấu hình được fail2ban tích hợp với log của modsecurity: tạo cấu hình trong file jail.local, kiểm tra status xem đã hoạt động chưa: kiểm tra file /var/log/fail2ban.log có Jail 'apache-modsecurity' started
Cấu hình gửi thông báo đến quản trị viên qua Telegram: kiểm tra trong file /var/log/modsecurity_telegram_log.log có Log sent successfully
Kết thúc bài lab và đóng gói kết quả.
Để đánh giá được sinh viên đã hoàn thành bài thực hành hay chưa, cần chia bài thực hành thành các nhiệm vụ nhỏ, mỗi nhiệm vụ cần phải chỉ rõ kết quả để có thể dựa vào đó đánh giá, chấm điểm. Do vậy, trong bài thực hành này hệ thống cần ghi nhận các thao tác, sự kiện được mô tả và cấu hình như bảng 1:
Bảng1. Bảng Result
Result Tag
Container
File
Field Type
Field ID


Timestamp Type


modsecurity
apache_modsecurity
/etc/modsecurity/modsecurity.conf
CONTAINS
SecRuleEngine On
file
fail2ban-client
apache_modsecurity
/var/log/fail2ban.log
CONTAINS
Jail 'apache-modsecurity' started
file
access-rule
apache_modsecurity
/var/log/apache2/access.log
CONTAINS
"GET /wordpress/wp-login.php?redirect_to=http%3A%2F%2F172.20.0.2%2Fwordpress%2Fwp-admin%2F&reauth=1 HTTP/1.1" 200
file
telegram
apache_modsecurity
/var/log/modsecurity_telegram_log.log
CONTAINS
Log sent successfully
file


Cài đặt và cấu hình các máy ảo

: Giao diện labedit của lab

:  Giao diện result


: Docker file 
Tích hợp và triển khai
Bài thực hành đã được triển khai như sau:
Docker Hub
https://hub.docker.com/u/ phamhduy

: Add và commit bài lab

: Đẩy các vùng chứa lên dockerhub

: Tạo imodule.tar chứa bài thực hành


: Các vùng chứa được đẩy lên dockerhub
Github
 https://github.com/TranDangTrong28102002/dg-n2-trong-1
Nhập lệnh create-imodules.sh

: Tạo file Imodule.tar

: File imodule.tar chứa bài thực hành
Tạo repo mới để đẩy imodule.tar lên và tạo phần release mới

: Đẩy file imodule.tar lên github
Thử nghiệm và đánh giá
Bài thực hành đã được xây dựng thành công, dưới đây là hình ảnh minh họa về bài thực hành:

: IP của máy apachemodsecurity



: File cấu hình modsecurity đã bât engine

:  Engine modsecurity đã hoạt động


: Thêm rule SecRule REQUEST_URI "@beginsWith /wordpress/wp-login.php" "id:1000002,phase:1,allow,ctl:ruleEngine=Off" vào file cấu hình modsecurity


: Rule chạy thành công



: Cấu hình tích hợp được fail2ban với modsecurity


: Cấu hình tích hợp được fail2ban với modsecurity đã hoạt động

: File gửi ip ban đến telegram

: File gửi log apache đến telegram

: Đánh giá kết quả bài thực hành


TÀI LIỆU THAM KHẢO



