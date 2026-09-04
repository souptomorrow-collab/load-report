# 預測結果（results/）

6 折時序滾動驗證的預測結果。成績（6 折平均，日內滾動）：MAE 0.123 kW、R²(flatten) +0.70、R²(per-slot) +0.46。

## 資料檔

| 檔案 | 內容 |
|------|------|
| `predictions.csv` | 末折（最後 30 天，out-of-sample）h=1 預測 vs 實際：datetime / actual_kw / predicted_kw |
| `rolling_forecast.csv` | 最後一天的日內滾動：96 個原點各帶未來 24h（origin_time / target_time / actual_kw / predicted_kw） |
| `forecast_for_GA.csv` | 資料最後 96 點的一次性回測（datetime / actual_kw / predicted_kw）。※ 給 GA 用的正式交接檔在 `../GA_handoff/` |
| `preds.npz` | 6 折測試集的完整預測矩陣：P (17280×96)、Y 真值、t 原點時刻。供事後重畫，免重跑 |

## 圖（figures/）

| 檔案 | 內容 |
|------|------|
| `folds_curve.png` | 6 折各取一個代表日：實際 vs 日初單次(t0) vs 日內滾動 |
| `feature_importance_bar.png` | Per-step (h=1/24/96) feature importance, block sums |
| `feature_importance_detail.png` | Per-step top-18 individual feature importance |
| `compare.png` | Direct + 7-day lookback vs MIMO RF baseline：t0 / rolling MAE（6 折） |
| `curves_t0.png` | 6 折代表日整天預測：Actual vs MIMO vs Direct |
| `rolling_snapshots.png` | 日內滾動 6 個時點快照 |
| `rolling.gif` | 日內滾動動畫（每 15 分刷新未來 24h） |
| `rolling6.gif` | 6 折代表日同步滾動動畫（2×3） |

## 終端機輸出（完整跑一次的紀錄）

| 檔案 | 內容 |
|------|------|
| `train_run_log.txt` | `train.py` 完整輸出：資料筆數與缺值補法、6 折各折的日初單次／日內滾動成績（MAE / RMSE / R²）、6 折平均彙整、直接多步 區塊特徵重要度、各檔輸出訊息 |
| `predict_run_log.txt` | `predict.py` 輸出：載入模型、交接包列數與起訖時刻、雲端上傳筆數 |
