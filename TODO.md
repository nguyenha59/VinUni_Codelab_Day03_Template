# Checklist Yêu Cầu — Lab #3 (theo student_guide.md)

> Cập nhật 2026-09-13: đã sửa lỗi thiếu import (`os`, `List`, `Tuple`) khiến
> `template.py` crash khi chạy trực tiếp, và thêm `sys.stdout.reconfigure(encoding="utf-8")`
> để tránh `UnicodeEncodeError` trên console Windows (cp1252). Toàn bộ 8/8 test
> trong `autograder/test_agent.py` đã PASSED.

## Milestone 1: Chatbot Baseline (`starter-code/template.py`)
- [x] Cài đặt `ChatbotBaseline.query()` để trả lời KHÔNG dùng tool (baseline, dễ bịa thông tin).
- [x] Kiểm tra: chạy với câu hỏi "Tìm chuyến bay từ HAN đi SGN dưới 2 triệu, và thời tiết SGN nên mặc gì?" và quan sát việc bịa thông tin / từ chối.

## Milestone 2: Tool Registry (`starter-code/tools.py`)
- [x] Cài đặt `get_flight_info(origin, destination, max_price)` — tra cứu `flight_data.json`.
- [x] Cài đặt `get_weather_forecast(city_code)` — tra cứu `weather_data.json`.
- [x] Khai báo đúng `TOOL_MAP`.

## Milestone 3: ReAct Loop (`ReActAgent`)
- [x] Sinh `Thought` (suy luận cần làm gì).
- [x] Sinh `Action` (tên tool + tham số JSON).
- [x] Gọi hàm tương ứng trong `TOOL_MAP`, lấy `Observation`.
- [x] Append thông tin vào `self.trace`.
- [x] Khi không còn Action (đã có final answer) → trả về `Final Answer`.

## Milestone 4: Safeguards & Trace Logging
- [x] Giới hạn số vòng lặp bằng `self.max_iterations`.
- [x] Khi vượt quá, trả về `status: max_iterations_reached` cùng trace.

## 3 Bẫy Cần Xử Lý
- [ ] **Trap 1 — KeyError khi gọi tool:** *Chưa áp dụng theo đúng tinh thần guide.* Implementation hiện tại chọn tool bằng keyword-matching cứng (không parse tên tool từ LLM), nên không có bước `.strip().lower()` trước khi tra `TOOL_MAP`. Không ảnh hưởng test hiện có, nhưng nếu đổi sang gọi LLM thật thì cần bổ sung.
- [ ] **Trap 2 — Format drift trong Action JSON:** *Chưa áp dụng.* Vì Action hiện được agent tự tạo ra (dict Python), không phải parse chuỗi JSON do LLM trả về, nên chưa có khối `json.loads()` trong `try...except`. Cần bổ sung nếu tích hợp LLM thật.
- [x] **Trap 3 — Lặp vô tận khi API lỗi:** đã có `max_iterations` an toàn.

## Kiểm Thử
- [x] Chạy: `python -m pytest autograder/test_agent.py -v`
- [x] Kết quả: **8/8 test PASSED** (autograder có 8 test, không phải 5 như guide ghi).
