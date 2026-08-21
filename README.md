# 🎬 Bộ Công Cụ Wan 2.1 Tạo Video AI Trên Google Colab Free (Tesla T4 16GB)
### ⚡ Tối ưu hóa cho YouTube Faceless, Shorts, TikTok | Hỗ trợ Wan 2.1 1.3B & 14B GGUF Q4_K_M

Bộ công cụ 1-Click giúp bạn chạy mô hình sinh video AI đỉnh cao **Wan 2.1** thông qua giao diện đồ họa **ComfyUI** trên Google Colab Free (GPU Tesla T4 16GB VRAM).

---

## 📁 Danh sách tệp tin trong bộ công cụ:
1. **[`Wan2_1_Colab_ComfyUI.ipynb`](Wan2_1_Colab_ComfyUI.ipynb)**: Notebook 1-Click tự động hóa toàn diện (Chạy ngầm Non-blocking, nạp Drive Turbo Cache 0.01s, tích hợp sẵn Cell Profiler đo VRAM/RAM và thời gian thực thi).
2. **[`Wan2_1_14B_GGUF_I2V_Workflow.json`](Wan2_1_14B_GGUF_I2V_Workflow.json)**: Workflow Image-to-Video bản **14B GGUF Q4_K_M** (Chất lượng điện ảnh, UMT5 CPU VRAM-safe, KSampler `uni_pc` / `simple` 20 steps).
3. **[`Wan2_1_I2V_Workflow.json`](Wan2_1_I2V_Workflow.json)**: Workflow Image-to-Video bản **1.3B** (Sản xuất hàng loạt siêu tốc cho Shorts / TikTok).
4. **[`Wan2_1_FLF2V_Workflow.json`](Wan2_1_FLF2V_Workflow.json)**: Workflow First Frame - Last Frame (Nội suy biến đổi mượt mà giữa khung hình đầu và khung hình cuối).
5. **[`Wan2_1_T2V_Workflow.json`](Wan2_1_T2V_Workflow.json)**: Workflow Text-to-Video bản **1.3B** (Tạo video trực tiếp từ câu lệnh prompt).

---

## 🚀 Hướng Dẫn Sử Dụng Nhanh (Chỉ 1-Click):

### Bước 1: Mở Google Colab
1. Truy cập [Google Colab](https://colab.research.google.com/).
2. Chọn menu **File** $\rightarrow$ **Upload notebook** $\rightarrow$ Chọn file `Wan2_1_Colab_ComfyUI.ipynb`.
3. Đảm bảo GPU: Vào **Runtime** $\rightarrow$ **Change runtime type** $\rightarrow$ Chọn **T4 GPU**.

### Bước 2: Bấm 1 nút Chạy Cell 1
* Bấm nút **Play** ở **Cell 1** $\rightarrow$ Hệ thống tự động:
  1. Cài đặt môi trường & vá lỗi `comfy_kitchen` (~30 giây).
  2. Nạp Model từ Google Drive qua Symlink trong **0.01 giây** (hoặc tải về Drive trong lần đầu).
  3. Khởi động ComfyUI & mở cổng Cloudflare Tunnel ngầm.
* Sau ~45–60 giây, Cell 1 sẽ hoàn tất và in ra đường link màu xanh:
  ```text
  =========================================================================
  🔗 BẤM VÀO ĐÂY ĐỂ MỞ COMFYUI: https://xxxx-xxxx.trycloudflare.com
  =========================================================================
  ```

### Bước 3: Mở ComfyUI Web & Tạo Video
* Kéo và thả file workflow JSON tương ứng vào giao diện ComfyUI.
* Tải ảnh lên ở node **Load Image** $\rightarrow$ Nhấn **Queue Prompt** (hoặc `Ctrl + Enter`).
* **Đo kiểm hiệu năng (Tùy chọn):** Bạn có thể bấm chạy **Cell 2** trên Colab bất kỳ lúc nào để xem VRAM peak, RAM peak và log server mà không bị ngắt Web UI.

---

## 📐 3 Preset Chuẩn Hóa Theo Chiến Lược "Quality Per Minute"

| Preset | Mô hình | Kích thước | Số Frames | Steps | Sampler / Scheduler | CFG | Thời gian tham chiếu trên T4 16GB | Mục đích sử dụng |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **P0 (Draft / Preview)** | Wan 2.1 1.3B | `832 x 480` | 33 | 14 – 16 | `uni_pc` / `simple` | 5.0 – 5.5 | **~1 – 1.5 phút** | Test nhanh bố cục & chuyển động. |
| **P1 (Fast Batch Production)** | Wan 2.1 1.3B | `832 x 480` hoặc `480 x 832` | 49 | 20 | `uni_pc` / `simple` | 5.0 – 5.5 | **~1.5 – 2.5+ phút** | Sản xuất hàng loạt Shorts / TikTok / Reels. |
| **P2 (Cinematic Hero Shots)** | Wan 2.1 14B Q4_K_M | `832 x 480` | 49 | 20 | `uni_pc` / `simple` | 5.0 – 5.5 | **~20 – 30+ phút** | Cảnh quan trọng (Hero shot) cho video dài YouTube. |

---

## 🛠️ Các Điểm Tối Ưu Kỹ Thuật Quan Trọng

1. **CPU Offload cho Text Encoder (VRAM-Safe):**
   * Trong các workflow 14B, `CLIPLoader` được thiết lập `device: "cpu"` nhằm giảm tải 5.1GB VRAM, giúp toàn bộ model 14B GGUF chạy trơn tru trong 16GB VRAM của Tesla T4 mà không bị tràn bộ nhớ.
2. **Thuật toán Sampler `uni_pc` + `simple`:**
   * Triệt tiêu hiện tượng vỡ hạt tuyết (*pixelated artifacts*) của `euler`, mang lại độ sắc nét cao ở 20 steps.
3. **Quy trình Hậu kỳ Chuẩn (Post-Processing Pipeline):**
   * **Wan 480p @ 16 FPS** $\rightarrow$ **AI Upscale** (Topaz Video AI / RealESRGAN / CapCut 4K) $\rightarrow$ **Frame Interpolation** (RIFE) lên **24/30/60 FPS**.
