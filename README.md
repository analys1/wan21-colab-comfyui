# 🎬 Bộ Công Cụ Wan 2.1 Tạo Video AI Miễn Phí Trên Google Colab (Tesla T4)
### Tối ưu hóa cho YouTube Faceless, Shorts, TikTok | Hỗ trợ Wan 2.1 1.3B & 14B GGUF

Bộ công cụ 1-Click giúp bạn chạy mô hình sinh video AI đỉnh cao **Wan 2.1** thông qua giao diện đồ họa **ComfyUI** trên Google Colab Free hoàn toàn miễn phí ($0).

---

## 📁 Danh sách tệp tin trong bộ công cụ:
1. **[`Wan2_1_Colab_ComfyUI.ipynb`](Wan2_1_Colab_ComfyUI.ipynb)**: File Notebook 1-Click tải lên Google Colab (tự động cài đặt, tải model và mở giao diện Web ComfyUI qua Cloudflare).
2. **[`Wan2_1_I2V_Workflow.json`](Wan2_1_I2V_Workflow.json)**: Workflow Image-to-Video bản **1.3B** (Chuyển ảnh tĩnh thành video điện ảnh - **Khuyên dùng nhất cho YouTube Faceless / TikTok**).
3. **[`Wan2_1_14B_GGUF_I2V_Workflow.json`](Wan2_1_14B_GGUF_I2V_Workflow.json)**: Workflow Image-to-Video bản **14B GGUF** (Chất lượng điện ảnh đỉnh cao, chi tiết khuôn mặt & chuyển động cực sắc nét).
4. **[`Wan2_1_T2V_Workflow.json`](Wan2_1_T2V_Workflow.json)**: Workflow Text-to-Video bản **1.3B** (Tạo video trực tiếp từ câu lệnh prompt).
5. **[`Wan2_1_FLF2V_Workflow.json`](Wan2_1_FLF2V_Workflow.json)**: Workflow First Frame - Last Frame (Nội suy biến đổi mượt mà giữa khung hình đầu và khung hình cuối).

---

## 🚀 Hướng Dẫn Sử Dụng Nhanh (Chỉ mất 3 phút):

### Bước 1: Mở Google Colab và tải Notebook lên
1. Truy cập [Google Colab](https://colab.research.google.com/).
2. Chọn menu **File (Tệp)** -> **Upload notebook (Tải sổ tay lên)** -> Chọn file `Wan2_1_Colab_ComfyUI.ipynb`.
3. Kiểm tra GPU: Vào **Runtime (Thời gian chạy)** -> **Change runtime type (Thay đổi loại thời gian chạy)** -> Đảm bảo chọn **T4 GPU**.

### Bước 2: Chạy các ô lệnh (Run Cells)
* Nhấn **Runtime** -> **Run all (Chạy tất cả)** (hoặc chạy lần lượt từ Cell 1 đến Cell 4).
* **Chọn Model (Cell 2):**
  * `download_wan_1_3B`: (Mặc định `True`) - Bản 1.3B siêu nhẹ, render nhanh (~1-2 phút/video), tối ưu sản xuất số lượng lớn.
  * `download_wan_14B_GGUF`: (Tùy chọn `True`) - Bản 14B lượng tử hóa Q4_K_M cho chất lượng hình ảnh và độ chi tiết chuyển động điện ảnh đỉnh cao.
* **Lưu trữ Drive (Cell 3):** Cấp quyền Google Drive. Hệ thống sẽ tự động tạo **Symlink** kết nối thư mục Output sang `MyDrive/Wan21_Videos` (video render xong tự động lưu vĩnh viễn, không lo sập Colab).

### Bước 3: Mở giao diện ComfyUI & Kéo thả Workflow
* Tại Cell 4, sau khi ComfyUI khởi động xong, bấm vào đường link Cloudflare:
  ```text
  =========================================================================
  🔗 BẤM VÀO ĐÂY ĐỂ MỞ COMFYUI: https://xxxx-xxxx.trycloudflare.com
  =========================================================================
  ```
* Kéo và thả một trong các file workflow JSON vào màn hình ComfyUI.
* Tải ảnh / nhập Prompt và nhấn **Queue Prompt** (hoặc `Ctrl + Enter`) để bắt đầu render!

---

## 📐 Bảng Thông Số Khuyến Nghị Theo Kênh Sản Xuất

| Mục tiêu | Chiều Rộng (Width) | Chiều Cao (Height) | Số Frames | Steps | Sampler / Scheduler | CFG |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **YouTube Ngang (16:9)** | **832** | **480** | 33 – 49 frames (~2 - 3s) | 20 – 30 | `euler` / `simple` | 5.0 – 6.0 |
| **Shorts / TikTok (9:16)** | **480** | **832** | 33 – 49 frames (~2 - 3s) | 20 – 30 | `euler` / `simple` | 5.0 – 6.0 |
| **Bản 14B GGUF (Chất lượng cao)** | 832 x 480 hoặc 480 x 832 | 49 – 65 frames | 25 – 35 | `euler` / `simple` | 5.0 – 6.0 |

> 💡 **Mẹo Tạo Prompt Tối Ưu cho Wan 2.1:**
> * **Image-to-Video:** Mô tả chuyển động camera hoặc hành động của chủ thể (VD: *"slow cinematic camera zoom in, dynamic wind blowing hair, photorealistic 4k, hyper-detailed"*).
> * **Chống giật/mượt mà:** Đặt `frame_rate` trong node `VHS_VideoCombine` là **16 fps** (chuẩn của Wan 2.1).
