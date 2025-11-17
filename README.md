<div align="center">
    <h1 align="center" style="color: #007bff;">
        <i class="fas fa-robot"></i> Hệ Thống Phân Loại Vật Thể Tự Động (Color & Shape Sorter)
    </h1>
    <p>Dự án mô phỏng cánh tay robot công nghiệp tự động phát hiện, phân loại và sắp xếp các vật thể dựa trên **Màu sắc** và **Hình dạng** trên băng tải.</p>
    <hr style="border-top: 3px solid #007bff;">
    <h2 align="center" style="color: #28a745;">
        <i class="fas fa-search"></i> Quá Trình Phát Hiện và Phân Loại
    </h2>
    <h3 style="color: #6c757d;">
        <i class="fas fa-podcast"></i> 1. Phát Hiện Vật Thể (Sử dụng Cảm biến Siêu âm)
    </h3>
    <table align="center" border="1" style="width: 80%; border-collapse: collapse; text-align: center;">
        <thead>
            <tr style="background-color: #e9ecef;">
                <th>Thiết Bị</th>
                <th>Thông Số Kỹ Thuật</th>
                <th>Điều Kiện Kích Hoạt</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Cảm biến HC-SR04</td>
                <td>TRIG: PIN 8, ECHO: PIN 10</td>
                <td>Khoảng cách đo được $\leq 5.5 \text{ cm}$ ($\text{DREF} - \text{THRESHOLD}$)</td>
            </tr>
            <tr>
                <td colspan="3" style="font-weight: bold; background-color: #fff3cd;">
                    Hành động: Băng tải DỪNG (RELAY_PIN 2 set LOW) và Quá trình Quét bắt đầu.
                </td>
            </tr>
        </tbody>
    </table>
    <h3 style="color: #6c757d;">
        <i class="fas fa-camera"></i> 2. Quét Hình Dạng và Màu Sắc
    </h3>
    <h4 style="color: #fd7e14;">Phân Loại Hình Dạng (Dựa trên Chiều cao trung bình)</h4>
    <table align="center" border="1" style="width: 50%; border-collapse: collapse; text-align: center;">
        <thead>
            <tr style="background-color: #f8f9fa;">
                <th>Loại Hình Dạng</th>
                <th>Điều Kiện (Chiều cao trung bình)</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Hình Tròn** 🟢</td>
                <td>$ < 5.3 \text{ (giả định)}$</td>
            </tr>
            <tr>
                <td>**Hình Vuông** 🟨</td>
                <td>$ \geq 5.3 \text{ (giả định)}$</td>
            </tr>
        </tbody>
    </table>
    <h4 style="color: #fd7e14;">Nhận Diện Màu Sắc (Cảm biến TCS3200)</h4>
    <p>Sử dụng Cảm biến TCS3200 (PIN S0-S3, sensorOut 7) để đo tần số R, G, B, sau đó chuyển đổi thành giá trị RGB $(0-255)$ thông qua **Hàm mapColor** và các giá trị hiệu chỉnh.</p>
    <p><strong>Kết quả:</strong> Xác định màu: **Đỏ**, **Vàng**, **Xanh Dương**, hoặc **Khác**.</p>
    <hr style="border-top: 3px solid #007bff;">
    <h2 align="center" style="color: #dc3545;">
        <i class="fas fa-cogs"></i> Cơ Chế Vận Hành Cánh Tay Robot
    </h2>
    <h3 style="color: #6c757d;">
        <i class="fas fa-hand-paper"></i> 1. Trình Tự Gắp và Đặt Vật
    </h3>
    <table align="center" border="1" style="width: 90%; border-collapse: collapse; text-align: center;">
        <thead>
            <tr style="background-color: #e9ecef;">
                <th>#</th>
                <th>Hành Động</th>
                <th>Góc Servo (Ví dụ)</th>
                <th>Mục Đích</th>
            </tr>
        </thead>
        <tbody>
            <tr><td>1</td><td>Chuẩn bị gắp</td><td>Đế $20^\circ$, Kẹp $120^\circ$ (Mở)</td><td>Vị trí chuẩn bị</td></tr>
            <tr><td>2</td><td>Vươn và Kẹp</td><td>Tay đòn $10^\circ$, Càng $75^\circ$</td><td>Định vị chính xác vật thể</td></tr>
            <tr><td>3</td><td>Gắp Vật</td><td>Kẹp $90^\circ$ (Đóng)</td><td>Giữ chặt vật thể</td></tr>
            <tr><td>4</td><td>Thu về & Nâng</td><td>Tay đòn $105^\circ$</td><td>Nâng vật thể lên cao</td></tr>
            <tr><td>5</td><td>Xoay Đế đến đích</td><td>$\rightarrow$ **Góc Phân Loại**</td><td>Di chuyển vật thể đến khu vực đích</td></tr>
            <tr><td>6</td><td>Thả Vật</td><td>$\rightarrow$ Góc Hạ & Vươn</td><td>Đặt vật thể vào khu vực phân loại</td></tr>
            <tr><td>7</td><td>Trở về Home</td><td>Đế $90^\circ$, Tay đòn $100^\circ$</td><td>Sẵn sàng cho chu trình tiếp theo</td></tr>
        </tbody>
    </table>
    <h3 style="color: #6c757d;">
        <i class="fas fa-th-large"></i> 2. Vị Trí Phân Loại Chi Tiết (6 Khu Vực)
    </h3>
    <table align="center" border="1" style="width: 100%; border-collapse: collapse; text-align: center;">
        <thead>
            <tr style="background-color: #20c997; color: white;">
                <th>Mã Vật Thể</th>
                <th>Hình Dạng</th>
                <th>Màu Sắc</th>
                <th>Khu Vực Đặt</th>
                <th>Góc Đế (**gocXoayDe**)</th>
                <th>Góc Càng (**gocVuonCang**)</th>
                <th>Góc Hạ Tay Đòn (**gocHaTayDon**)</th>
            </tr>
        </thead>
        <tbody>
            <tr><td>**T\_D**</td><td>Tròn</td><td>Đỏ</td><td>**Khu vực 1**</td><td>$130^\circ$</td><td>$80^\circ$</td><td>$40^\circ$</td></tr>
            <tr><td>**V\_D**</td><td>Vuông</td><td>Đỏ</td><td>**Khu vực 2**</td><td>$120^\circ$</td><td>$80^\circ$</td><td>$10^\circ$</td></tr>
            <tr><td>**T\_V**</td><td>Tròn</td><td>Vàng</td><td>**Khu vực 3**</td><td>$105^\circ$</td><td>$70^\circ$</td><td>$70^\circ$</td></tr>
            <tr><td>**V\_V**</td><td>Vuông</td><td>Vàng</td><td>**Khu vực 4**</td><td>$105^\circ$</td><td>$70^\circ$</td><td>$10^\circ$</td></tr>
            <tr><td>**T\_X**</td><td>Tròn</td><td>Xanh</td><td>**Khu vực 5**</td><td>$80^\circ$</td><td>$90^\circ$</td><td>$60^\circ$</td></tr>
            <tr><td>**V\_X**</td><td>Vuông</td><td>Xanh</td><td>**Khu vực 6**</td><td>$80^\circ$</td><td>$90^\circ$</td><td>$10^\circ$</td></tr>
        </tbody>
    </table>
    <hr style="border-top: 3px solid #007bff;">
    <h2 align="center" style="color: #ffc107;">
        <i class="fas fa-desktop"></i> Giao Diện & An Toàn
    </h2>
    <h3 style="color: #6c757d;">
        <i class="fas fa-chart-bar"></i> 1. Giao Diện Người Dùng (LCD 16x2 - Địa chỉ 0x27)
    </h3>
    <div style="border: 2px dashed #ffc107; padding: 10px; margin: 10px; display: inline-block;">
        <pre>
            Hàng 1: T_D:X V_D:Y T_V:Z
            Hàng 2: V_V:A T_X:B V_X:C
        </pre>
    </div>
    <p>Hiển thị số lượng vật thể đã được phân loại thành công (biến đếm: *count\_TD, count\_VD, v.v.*).</p>
    <h3 style="color: #6c757d;">
        <i class="fas fa-shield-alt"></i> 2. Cơ Chế An Toàn Dự Phòng (Cảm biến Hồng ngoại)
    </h3>
    <table align="center" border="1" style="width: 70%; border-collapse: collapse; text-align: center;">
        <thead>
            <tr style="background-color: #e9ecef;">
                <th>Thiết Bị</th>
                <th>Chân Kết Nối</th>
                <th>Điều Kiện Kích Hoạt</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Cảm biến Hồng ngoại (IR)</td>
                <td>IR_PIN 11</td>
                <td>Phát hiện vật thể (digitalRead(IR_PIN) == LOW)</td>
            </tr>
            <tr>
                <td colspan="3" style="font-weight: bold; background-color: #fcebeb;">
                    Hành động: Dừng băng tải và Kích hoạt ngay lập tức **Chu trình Gắp và Đặt** theo các góc đã định.
                </td>
            </tr>
        </tbody>
    </table>
    <hr style="border-top: 3px solid #007bff;">
    <hr style="border-top: 3px solid #007bff;">
    <h2 align="center" style="color: #6f42c1;">
        <i class="fas fa-magic"></i> Tài Liệu Trực Quan & Kết Quả Thực Nghiệm
    </h2>
    <h3 style="color: #6c757d;">
        <i class="fas fa-network-wired"></i> 1. Sơ Đồ Khối Hệ Thống (System Block Diagram)
    </h3>
    <p>Sơ đồ khối thể hiện mối liên kết giữa các thành phần Cảm biến, Bộ điều khiển (Vi điều khiển), và Cơ cấu Chấp hành.</p>
    <div align="center" style="margin-bottom: 20px;">
    </div>
    <h3 style="color: #6c757d;">
        <i class="fas fa-drafting-compass"></i> 2. Tổng Quan Mô Hình Thực Tế (Model Overview)
    </h3>
    <p>Hình ảnh tổng quan về mô hình cánh tay robot, băng tải và các khu vực phân loại khi đã hoàn thiện.</p>
    <div align="center" style="margin-bottom: 20px;">
    </div>
    <h3 style="color: #6c757d;">
        <i class="fas fa-video"></i> 3. Video Kết Quả Thực Nghiệm
    </h3>
    <p>Video trình diễn hoạt động thực tế của hệ thống phân loại, từ khâu phát hiện vật thể đến quá trình gắp và đặt chính xác vào các khu vực chỉ định.</p>
    <div align="center" style="margin-bottom: 20px; border: 1px solid #ddd; padding: 10px;">
        <p><strong>[CHÈN VIDEO KẾT QUẢ THỰC NGHIỆM TẠI ĐÂY]</strong></p>
    </div>
    <hr style="border-top: 3px solid #007bff;">
    <div style="padding: 15px; border: 3px solid #17a2b8; border-radius: 10px; background-color: #e6f7ff; margin-top: 20px;">
        <h4 style="color: #17a2b8; margin-top: 0;">
            <i class="fas fa-lightbulb"></i> TỔNG KẾT HỆ THỐNG
        </h4>
        <p style="text-align: left;">
            Hệ thống này thể hiện sự tích hợp của các chức năng cốt lõi trong lĩnh vực Robot và IoT: 
            <strong>Cảm nhận</strong> (Siêu âm, Màu, IR), 
            <strong>Xử lý</strong> (Phân loại Hình dạng/Màu) và 
            <strong>Chấp hành</strong> (Servo motor, Băng tải).
        </p>
    </div>
</div>
