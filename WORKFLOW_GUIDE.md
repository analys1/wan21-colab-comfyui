# 🎬 Hướng Dẫn Chi Tiết Các ComfyUI Workflows (Wan 2.1)
### Cẩm nang sản xuất Video AI cho YouTube Faceless, Shorts & TikTok

Tài liệu này hướng dẫn chi tiết cách sử dụng 4 bộ Workflow ComfyUI chuyên dụng được tối ưu hóa cho mô hình **Wan 2.1** trên Google Colab Free (Tesla T4).

---

## 📌 Danh Sách 4 Bộ Workflow

| Tên File | Loại Hình | Mục Đích Sử Dụng | Thời Gian Render (T4) |
| :--- | :--- | :--- | :--- |
| **Wan2_1_14B_GGUF_I2V_Workflow.json** ⭐ | **Image-to-Video (14B)** | **[KHUYÊN DÙNG NHẤT]** Biến ảnh tĩnh thành video điện ảnh 8K siêu nét, chi tiết khuôn mặt & chuyển động chân thực. | ~4 – 5 phút / video |
| **Wan2_1_I2V_Workflow.json** | **Image-to-Video (1.3B)** | Biến ảnh tĩnh thành video với tốc độ nhanh nhẹ, phù hợp làm hàng loạt. | ~1 – 2 phút / video |
| **Wan2_1_T2V_Workflow.json** | **Text-to-Video (1.3B)** | Tạo video trực tiếp từ câu lệnh mô tả văn bản (Prompt), không cần ảnh gốc. | ~1 – 2 phút / video |
| **Wan2_1_FLF2V_Workflow.json** | **First / Last Frame** | Tạo chuyển cảnh biến hình (Morphing Transition) giữa ảnh bắt đầu và ảnh kết thúc. | ~4 – 5 phút / video |

---

## 🏗️ Sơ Đồ Kiến Trúc Luồng Dữ Liệu (Workflow Pipeline)

`mermaid
graph TD
    subgraph 1. Loaders [1. Tải Mô Hình]
        M[Model / UnetLoader]
        C[CLIP Text Encoder]
        V[VAE Loader]
        CV[CLIP Vision Loader]
    end

    subgraph 2. Inputs [2. Nhập Liệu & Prompt]
        IMG[Load Image]
        POS[Positive Prompt]
        NEG[Negative Prompt]
    end

    subgraph 3. Processing [3. Xử Lý Chuyển Động]
        WAN[WanImageToVideo / Latent]
        KSAMP[KSampler - 20-25 Steps]
    end

    subgraph 4. Output [4. Giải Mã & Xuất Video]
        DEC[VAEDecodeTiled - Tile 512]
        VID[VHS_VideoCombine - MP4 16fps]
    end

    M --> KSAMP
    C --> POS & NEG
    V --> WAN & DEC
    CV --> WAN
    IMG --> WAN
    POS --> WAN
    NEG --> WAN
    WAN -->|Positive, Negative, Latent| KSAMP
    KSAMP --> DEC
    DEC --> VID
`

---

## 📐 Bảng Kích Thước & Thông Số Chuẩn Theo Định Dạng Kênh

### 1. Kích thước (Resolution):
* **YouTube Ngang (16:9):** Width: 832 | Height: 480
* **Shorts / TikTok / Reels Dọc (9:16):** Width: 480 | Height: 832
* **Video Vuông (1:1):** Width: 640 | Height: 640

### 2. Số khung hình (Frames) & Thời lượng:
* 33 frames: ~2 giây *(phù hợp test nhanh)*.
* 49 frames: ~3 giây *(chuẩn khuyến nghị cho video điện ảnh)*.
* 65 frames: ~4 giây *(chuyển động dài)*.

### 3. Cài đặt KSampler:
* **Steps:** 20 – 25 steps.
* **CFG Scale:** 5.0 – 6.0 *(quá cao sẽ bị gai hình, quá thấp sẽ thiếu chi tiết)*.
* **Sampler Name:** euler hoặc uni_pc.
* **Scheduler:** simple.
* **Frame Rate (FPS):** Đặt 16 trong node Video Combine (chuẩn FPS gốc của Wan 2.1).

---

## ✍️ Công Thức Viết Prompt Tối Ưu Cho Wan 2.1

### 🟢 Positive Prompt (Câu lệnh mô tả chuyển động):
> **Cấu trúc:** [Hành động chính của chủ thể] + [Chuyển động Camera] + [Ánh sáng & Chi tiết môi trường] + [Chất lượng điện ảnh]

* **Mẫu chân dung nhân vật:**
  `	ext
  slow cinematic camera zoom in, subtle smile, dynamic wind blowing hair, natural eye blinking, volumetric lighting, photorealistic 8k, masterpiece
  `
* **Mẫu phong cảnh / Xe cộ / Khoa học viễn tưởng:**
  `	ext
  dramatic cinematic drone shot, smooth panning, hyperrealistic lighting, fluid motion, atmospheric dust particles, 4k resolution
  `

### 🔴 Negative Prompt (Khử lỗi hình ảnh & méo tay chân):
> Đặt sẵn trong ô **Negative Prompt** để video luôn mượt mà, không bị răng cưa:

`	ext
blurry, static, distortion, deformed hands, bad anatomy, low quality, jitter, watermark, oversaturated, glitch, sudden cuts
`

---

## 💡 Mẹo Sản Xuất Video Faceless 5 Bước Tối Ưu ( Chi Phí)

1. **Ý tưởng & Kịch bản:** Dùng **ChatGPT / Claude** viết kịch bản Shorts 45s (cấu trúc Hook 3s đầu).
2. **Giọng đọc (Voice):** Dùng **Kokoro TTS** hoặc **Edge-TTS** tạo file đọc MP3 tự nhiên.
3. **Ảnh gốc:** Dùng **FLUX.1** hoặc **Midjourney** tạo 4–6 tấm ảnh tĩnh sắc nét theo từng câu trong kịch bản.
4. **Tạo chuyển động:** Kéo thả từng ảnh vào **Wan 2.1 ComfyUI** tạo chuỗi video 3 giây.
5. **Dựng & Phụ đề:** Kéo toàn bộ vào **CapCut**, bật **Auto Captions** nhảy chữ sinh động, chèn nhạc nền Lo-fi và xuất video 1080p!
