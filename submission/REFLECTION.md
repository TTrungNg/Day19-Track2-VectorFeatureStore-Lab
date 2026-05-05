# Reflection — Lab 19

**Tên:** Nguyễn Việt Trung
**Cohort:** _<A20-K1>_
**Path đã chạy:** Docker

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Trên golden set 50 queries, kết quả Precision@10 cho thấy mỗi mode có thế mạnh riêng theo từng loại query:

- **Exact** (15 queries): BM25 = Hybrid (96.7%) > Semantic (88.7%). BM25 thắng vì các thuật ngữ kỹ thuật xuất hiện nguyên văn trong corpus — TF-IDF signal mạnh, không cần embedding.
- **Paraphrase** (15 queries): cả ba mode đều yếu (BM25 33.3%, Hybrid 32.0%, Semantic 24.0%). Nguyên nhân: model `bge-small-en` được train trên tiếng Anh, không capture tốt paraphrase tiếng Việt — embedding model choice sai cho ngôn ngữ đích.
- **Mixed** (20 queries): Hybrid 100% > Semantic 98.5% > BM25 97.0%. RRF kết hợp cả hai signal, loại bỏ điểm mù của từng retriever.

**Khi nào không dùng hybrid:** (1) Latency budget rất chặt — keyword P99 chỉ 20ms so với hybrid 33ms; trong mobile real-time search, 13ms là quan trọng. (2) Corpus chứa toàn thuật ngữ cố định, không có paraphrase — ví dụ tìm kiếm mã sản phẩm, SKU, mã hóa đơn: BM25 đủ và nhanh hơn. (3) Không có GPU/ONNX runtime — chi phí embedding không bù được gain.

---

## Điều ngạc nhiên nhất khi làm lab này

Ngạc nhiên nhất là paraphrase tiếng Việt hoàn toàn không được hưởng lợi từ semantic search khi dùng model English-trained — Hybrid thậm chí thua BM25 trên slice đó. Đây là minh chứng rõ nhất rằng embedding model choice quan trọng hơn thuật toán retrieval.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
