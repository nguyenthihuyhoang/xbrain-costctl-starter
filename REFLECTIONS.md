# Reflections — costctl (W6 side challenge)

## 1) Multi-account
Để chạy `costctl` cho ~100 AWS accounts, mình sẽ không dùng 1 bộ static keys mà dùng cross-account role + automation.

- Dùng AWS Organizations / danh sách account IDs, và ở mỗi account có một IAM Role tiêu chuẩn (ví dụ `CostctlReadRole` / `CostctlWriteRole`).
- Ở account “tooling”, dùng `sts:AssumeRole` để lấy credential tạm thời cho từng account, chạy các lệnh `list/cost/...`, rồi tổng hợp ra CSV/JSON theo account.
- Thiết kế output theo kiểu: `account_id, region, resource_id, tags..., cost...` để dễ merge và upload lên S3 hoặc gửi Slack.

## 2) idle vs Trusted Advisor
`idle` (tự viết) nhìn CPU average trong 24h nên phản ứng nhanh với thay đổi gần đây; Trusted Advisor dùng cửa sổ dài hơn (thường 14 ngày) nên “ổn định” hơn.

- Mình tin `idle` hơn khi cần xử lý nhanh các resource vừa tạo để demo/lab và muốn dọn trong ngày.
- Mình tin Trusted Advisor hơn khi muốn quyết định cleanup dài hạn, tránh false positive do workload theo chu kỳ.
- Nếu production: nên combine cả CPU + network + EBS IO, và có allowlist/tag “keep=true” để tránh terminate nhầm.
