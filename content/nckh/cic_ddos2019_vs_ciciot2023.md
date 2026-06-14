---
title: "Nghiên cứu so sánh hai bộ dữ liệu CIC-DDoS 2019 và CICIoT2023 trong bài toán phát hiện xâm nhập"
description: "Phân tích học thuật chuyên sâu so sánh bối cảnh, mô hình mạng thực nghiệm, kịch bản tấn công, quy trình trích xuất đặc trưng và đánh giá mô hình của hai bộ dữ liệu chuẩn thức CIC-DDoS 2019 và CICIoT2023."
summary: "Báo cáo khoa học phân tích so sánh toàn diện hai bộ dữ liệu an ninh mạng tiêu chuẩn của Viện An ninh mạng Canada (CIC): CIC-DDoS 2019 và CICIoT2023, định hướng phát triển hệ thống phát hiện xâm nhập (IDS) thời gian thực."
date: 2026-06-13T23:35:00+07:00
lastmod: 2026-06-13T23:35:00+07:00
draft: false
weight: 20
categories: ["Research", "Cybersecurity"]
tags: ["CIC-DDoS2019", "CICIoT2023", "IDS", "Machine Learning", "IoT Security"]
contributors: []
pinned: false
homepage: false
---

# Nghiên cứu so sánh hai bộ dữ liệu CIC-DDoS 2019 và CICIoT2023 trong bài toán phát hiện xâm nhập

## Tóm tắt
Trong nghiên cứu an ninh mạng, việc lựa chọn và hiểu rõ các bộ dữ liệu benchmark là yếu tố quyết định đến hiệu suất của các mô hình Machine Learning và Deep Learning áp dụng cho hệ thống IDS. Báo cáo này thực hiện phân tích so sánh toàn diện hai bộ dữ liệu tiêu chuẩn được công bố bởi Viện An ninh mạng Canada (CIC): **CIC-DDoS 2019** và **CICIoT2023**. Nội dung nghiên cứu được triển khai theo 5 phương diện cốt lõi bao gồm: (1) Bối cảnh và mục tiêu xây dựng, (2) Kiến trúc mạng thực nghiệm, (3) Phân loại kịch bản tấn công, (4) Phương pháp trích xuất đặc trưng và tiền xử lý dữ liệu, (5) Đánh giá hiệu suất mô hình và định hướng triển khai thực tế. Kết quả phân tích chỉ ra sự dịch chuyển trong xu hướng tấn công mạng từ hạ tầng Client-Server truyền thống sang các hệ sinh thái IoT phân tán, đồng thời đề xuất giải pháp tích hợp mô hình IDS vào quy trình DevOps và vận hành hệ thống thực tế.

---

## 1. Tổng quan và bối cảnh
Sự phát triển mạnh mẽ của AI trong lĩnh vực an ninh mạng đòi hỏi các bộ dữ liệu huấn luyện phải phản ánh chính xác các kịch bản đe dọa thực tế. Tuy nhiên, các bộ dữ liệu thế hệ cũ thường gặp phải hai hạn chế lớn: mô hình hóa quá đơn giản (thiếu tính phức tạp của lưu lượng mạng thực tế) và thiếu sự tham gia của các thiết bị phần cứng vật lý trong quá trình thu thập. 

*   **Bộ dữ liệu CIC-DDoS 2019:** Được thiết kế và xây dựng trong bối cảnh các cuộc tấn công DDoS gia tăng về cả quy mô lẫn tần suất trên hạ tầng mạng doanh nghiệp truyền thống với kiến trúc Client-Server. Mục tiêu của bộ dữ liệu này là cung cấp một thước đo benchmark chuyên sâu, tập trung vào việc nhận diện các biến thể tấn công DDoS phức tạp tại cả Network Layer và Application Layer.
*   **Bộ dữ liệu CICIoT2023:** Ra đời nhằm giải quyết thách thức bảo mật trong kỷ nguyên IoT. Với sự bùng nổ của hàng tỷ thiết bị thông minh vốn bị hạn chế về tài nguyên phần cứng và thiếu các cơ chế bảo mật tích hợp, mạng IoT trở thành mục tiêu hàng đầu và là bàn đạp lý tưởng cho các mạng botnet. CICIoT2023 được đề xuất nhằm cung cấp nguồn tài nguyên dữ liệu quy mô lớn, phản ánh lưu lượng thực tế của các thiết bị IoT để thúc đẩy các ứng dụng phân tích bảo mật trong môi trường vận hành thực tế.

---

## 2. Mô hình mạng thực nghiệm
Kiến trúc Testbed quyết định độ tin cậy và giá trị học thuật của dữ liệu lưu lượng mạng được ghi nhận. 

### 2.1. Kiến trúc thực nghiệm CIC-DDoS 2019
Mô hình thực nghiệm của CIC-DDoS 2019 mô phỏng cấu trúc mạng văn phòng truyền thống:
*   **Hạ tầng nạn nhân:** Bao gồm các máy tính vật lý cấu hình cao, đóng vai trò là các máy chủ dịch vụ chịu tải.
*   **Hạ tầng tấn công:** Sử dụng các máy ảo giả lập mạng botnet phân tán từ bên ngoài để tạo ra lưu lượng tấn công cường độ cao hướng vào hệ thống trung tâm.

### 2.2. Kiến trúc thực nghiệm CICIoT2023
Trái ngược với việc giả lập hoàn toàn bằng phần mềm, CICIoT2023 được xây dựng dựa trên mô hình Smart Home thực tế với **105 thiết bị phần cứng vật lý**. Kiến trúc mạng được chia thành hai nhánh chính kết nối qua thiết bị phần cứng chuyên dụng:
*   **Nhánh tấn công:** Gồm một router ASUS kết nối Internet, một máy chủ Windows 10 chia sẻ đường truyền, một Cisco Switch và một VeraPlus Access Point quản lý 7 thiết bị Raspberry Pi đóng vai trò là các nút tấn công.
*   **Nhánh nạn nhân:** Là mạng nội bộ của các thiết bị IoT đích, sử dụng một Netgear Unmanaged Switch kết nối với 5 Gateway để điều phối và quản lý các thiết bị IoT sử dụng giao thức truyền thông không dây Zigbee và Z-Wave.
*   **Thu thập dữ liệu:** Để đảm bảo tính toàn vẹn của lưu lượng mạng và không làm suy giảm hiệu năng hệ thống, nhóm nghiên cứu đã triển khai thiết bị phần cứng chuyên dụng **Gigamon Network Tap** đặt giữa nhánh tấn công và nhánh nạn nhân. Toàn bộ lưu lượng thô được chuyển hướng về hệ thống giám sát và ghi lại dưới dạng tệp PCAP thông qua công cụ Wireshark.

| Tiêu chí so sánh | Bộ dữ liệu CIC-DDoS 2019 | Bộ dữ liệu CICIoT2023 |
| :--- | :--- | :--- |
| **Môi trường mạng** | Client-Server truyền thống | IoT và Smart Home |
| **Hạ tầng thiết bị** | Máy vật lý kết hợp máy ảo giả lập | 105 thiết bị phần cứng thực tế |
| **Giao thức kết nối** | TCP/IP tiêu chuẩn | TCP/IP, Zigbee, Z-Wave |
| **Phương pháp giám sát** | Phần mềm bắt gói tin | Phần cứng Gigamon Network Tap |

---

## 3. Phân loại kịch bản tấn công
Thiết kế kịch bản tấn công của hai bộ dữ liệu phản ánh tư duy phòng thủ đặc thù trong từng môi trường mạng.

### 3.1. Các kịch bản tấn công trong CIC-DDoS 2019
Bộ dữ liệu này tập trung chuyên sâu vào các biến thể tấn công DDoS và được chia thành hai nhóm chính:
1.  **Reflection-based DDoS:** Kẻ tấn công lợi dụng các máy chủ trung gian chạy các dịch vụ công khai để khuếch đại lưu lượng. Các giao thức bị khai thác bao gồm NTP, DNS, LDAP, NetBIOS, SSDP, SNMP, v.v.
2.  **Exploitation-based DDoS:** Tấn công trực diện vào nạn nhân bằng cách gửi lượng lớn gói tin để làm cạn kiệt tài nguyên hệ thống. Các dạng phổ biến gồm SYN Flood, UDP Flood, UDP-Lag và WebDDoS.

### 3.2. Các kịch bản tấn công trong CICIoT2023
CICIoT2023 sở hữu danh mục tấn công đa dạng hơn với **33 kịch bản tấn công** được phân thành **7 nhóm chính**:
1.  **DDoS:** TCP-SYN Flood, UDP Flood, ICMP Flood, HTTP Flood, v.v.
2.  **DoS:** Khai thác các lỗ hổng từ chối dịch vụ trên thiết bị IoT.
3.  **Reconnaissance:** OS Scan, Port Scan, Vulnerability Scan, Host Discovery.
4.  **Web-based attacks:** SQL Injection, Cross-Site Scripting (XSS), Command Injection.
5.  **Brute Force:** Tấn công dò quét mật khẩu hệ thống.
6.  **Spoofing:** ARP Spoofing.
7.  **Malware:** Phát tán và thực thi mã độc mã nguồn mở Mirai.

*Đặc tính độc bản của CICIoT2023:* Toàn bộ các cuộc tấn công đều được thực hiện từ chính các thiết bị IoT bị compromised nằm trong mạng nội bộ để tấn công các thiết bị IoT khác. Điều này mô phỏng chính xác kịch bản lateral movement và sự lây lan trong mạng IoT thực tế.

---

## 4. Trích xuất đặc trưng và đặc tả dữ liệu
Quá trình chuyển đổi dữ liệu lưu lượng mạng thô từ PCAP sang CSV để huấn luyện mô hình Machine Learning được thực hiện bằng các phương pháp khác nhau.

### 4.1. Đặc tả đặc trưng của CIC-DDoS 2019
*   **Công cụ trích xuất:** Sử dụng công cụ **CICFlowMeter** phát triển bởi chính nhóm nghiên cứu CIC.
*   **Đặc tả:** Trích xuất hơn **80 đặc trưng** flow-based. Các đặc trưng này bao gồm Flow Inter-Arrival Time (IAT), tổng số gói tin theo hướng forward/backward, kích thước gói tin, và các cờ trạng thái TCP. Phương pháp này tối ưu cho việc mô hình hóa các đặc điểm tĩnh và động của luồng dữ liệu DDoS.

### 4.2. Đặc tả đặc trưng của CICIoT2023
*   **Quy mô dữ liệu thô:** Lên tới **548 GB** dữ liệu PCAP.
*   **Tiền xử lý:** Tệp PCAP khổng lồ được phân mảnh thành các tệp nhỏ hơn (10 MB) bằng công cụ `tcpdump` để hỗ trợ xử lý song song.
*   **Công cụ trích xuất:** Sử dụng thư viện Python **DPKT** thay vì CICFlowMeter.
*   **Đặc tả:** Trích xuất **47 đặc trưng** mạng. Danh sách đặc trưng bao gồm các thông số ở tầng vật lý và tầng mạng (như Protocol Type, Rate, Packet Size) cùng với các cờ trạng thái TCP (FIN, SYN, RST, PSH, ACK, URG, CWR, ECE).
*   **Xử lý mất cân bằng dữ liệu:** Để giải quyết sự chênh lệch lớn về số lượng gói tin giữa các đợt tấn công volumetric (DDoS/DoS) và các cuộc tấn công Web-based hay Brute Force, nhóm nghiên cứu áp dụng kỹ thuật gom cụm dữ liệu theo sliding window sizes là **10 và 100 gói tin**, sau đó tính toán các giá trị thống kê trung bình bằng thư viện `pandas` và `numpy`. Phương pháp này giúp mô hình AI tránh hiện tượng overfitting hoặc lệch phân bố khi huấn luyện.

---

## 5. Đánh giá mô hình và triển khai thực tiễn

### 5.1. Hiệu suất của các thuật toán Machine Learning trên CICIoT2023
Nghiên cứu gốc thực hiện đánh giá hiệu suất của 5 thuật toán Machine Learning và Deep Learning phổ biến: **Logistic Regression (LR)**, **Perceptron**, **AdaBoost**, **Random Forest (RF)**, và **Deep Neural Networks (DNN)** qua 3 cấp độ phân loại:
1.  **Binary Classification:** Phân biệt lưu lượng Benign và Attack. Tất cả 5 mô hình đều đạt Accuracy vượt mức **98%**.
2.  **8-class Classification:** Nhận diện các nhóm tấn công chính.
3.  **34-class Classification:** Phân loại cụ thể từng loại trong số 33 cuộc tấn công. Ở cấp độ này, các mô hình tuyến tính và ensemble đơn giản như Logistic Regression và AdaBoost bị suy giảm hiệu suất rõ rệt với Accuracy giảm xuống dưới **80%**. Ngược lại, Random Forest và DNN duy trì hiệu suất vượt trội với Accuracy lần lượt đạt **99.1%** và **98.6%**.
*Hạn chế ghi nhận:* Các mô hình Machine Learning vẫn gặp khó khăn trong việc phân biệt các cuộc tấn công Web-based và Brute Force với lưu lượng Benign hoặc Reconnaissance do sự tương đồng về hành vi gửi nhận gói tin ở mức độ vi mô.

### 5.2. Định hướng triển khai thực tế trong hệ thống
Việc nghiên cứu hai bộ dữ liệu này đặt nền móng cho việc xây dựng các hệ thống IDS/IPS thời gian thực trong môi trường Production:
1.  **Containerization:** Các mô hình đã huấn luyện (như Random Forest hoặc kiến trúc DNN rút gọn) được tối ưu hóa và đóng gói vào các Docker container để dễ dàng triển khai và mở rộng.
2.  **Giám sát lưu lượng thời gian thực:** Sử dụng các kỹ thuật như eBPF hoặc AF_PACKET ở mức Linux Kernel để bắt và phân tích luồng dữ liệu đi qua các network interfaces mà không làm ảnh hưởng đến hiệu năng định tuyến.
3.  **Tự động hóa phản ứng:** Kết hợp hệ thống IDS với các công cụ điều phối hạ tầng như Kubernetes hoặc Ansible. Khi mô hình AI phát hiện hành vi bất thường từ một địa chỉ IP hoặc một thiết bị IoT cụ thể, hệ thống tự động kích hoạt các Network Policy hoặc cấu hình lại Firewall để cô lập (quarantine) thiết bị bị nhiễm độc ngay lập tức, ngăn chặn sự lây lan mã độc Mirai trong mạng nội bộ.

---

## Kết luận
Tóm lại, trong khi **CIC-DDoS 2019** cung cấp dữ liệu chuyên sâu để đối phó với các cuộc tấn công từ chối dịch vụ quy mô lớn ở hạ tầng truyền thống, thì **CICIoT2023** mang lại cái nhìn thực tế và toàn diện về các mối đe dọa đa dạng trong hệ sinh thái IoT hiện đại. Việc thấu hiểu các đặc tính kỹ thuật của hai bộ dữ liệu này là bước đệm quan trọng để xây dựng các giải pháp an ninh mạng thông minh, tự động hóa và có khả năng thích ứng cao.
