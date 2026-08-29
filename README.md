# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Kho lưu trữ tài liệu nghiên cứu cho pipeline UAV closed-loop **PX4 + AURA + FAST/T1/C1 + bounded incremental candidate + WISE / World Model + AEGIS**.

## Mục tiêu của repository

Repository này lưu các tài liệu có giá trị lâu dài để có thể phục hồi nhanh trạng thái nghiên cứu, audit causal/scientific validity, và tiếp tục triển khai mà không phụ thuộc vào các artifact runtime dung lượng lớn.

Các raw runtime roots, telemetry, capture, replay lớn và dataset không được đưa vào repository này; chúng tiếp tục nằm dưới storage root `/media/nahhao74/KINGSTON` theo provenance được ghi trong từng report.

## Cấu trúc

- `docs/00_current_state/` — trạng thái pipeline và blocker hiện tại.
- `docs/01_architecture/` — kiến trúc Moving Mode và invariant chính.
- `docs/02_source_registry/` — source registry phục vụ research/audit.
- `docs/03_reports/` — các report/closure gốc, giữ nguyên nội dung.
- `docs/04_research/` — research notes liên quan trực tiếp đến causal closed-loop identification.

## Trạng thái hiện tại

Pipeline đã đóng các blocker lớn về uXRCE head-of-line logging, timestamp dual-domain, atomic SensorCombined provenance wire, và StateBank/AURA bootstrap readiness. Randomized pilot gần nhất dừng **trước scientific `T_D`** vì specialized pilot runner bỏ sót `bootstrap_only=True` ở transaction `block_index=-1`.

Do đó trạng thái hiện tại là **implementation-infrastructure pre-science blocker**, không phải scientific failure, efficacy result, source failure hay AURA/StateBank regression.

Xem `docs/00_current_state/PIPELINE_CURRENT_STATE_20260829.md` và `docs/00_current_state/REPORT_INDEX.md`.

## Scientific target

`G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)`

với `B = active PX4 + AURA + FAST/T1/C1 baseline`.

Randomized candidate là incremental treatment trên baseline đang active. Future FAST/PX4 response sau candidate là một phần của closed-loop treatment response, không bị loại khỏi estimand.

## Governance hiện tại

- Moving Mode only.
- PX4 inner loops authoritative.
- FAST/T1/C1 baseline luôn active.
- StateBank luôn warm.
- World Model không được block first response.
- SEALED locked trước approved final evaluation.
- `production_authority=false`.
- Failed scientific roots immutable; không patch-and-continue hoặc pool partial invalid roots.
- Large runtime/data artifacts phải ở `/media/nahhao74/KINGSTON`.
