# 預測結果（results/）

6 折時序滾動驗證的預測結果。成績（6 折平均，日內滾動）：MAE 0.123 kW、R²(flatten) +0.70、R²(per-slot) +0.46。

## 資料檔

| 檔案 | 內容 |
|------|------|
| `predictions.csv` | 末折（最後 30 天，out-of-sample）h=1 預測 vs 實際：datetime / actual_kw / predicted_kw |

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

## 實驗紀錄

| 檔案 | 內容 |
|------|------|
| `train_run_log.txt` | 訓練完整終端機輸出：資料筆數與缺值補法、6 折各折的日初單次／日內滾動成績（MAE / RMSE / R²）、6 折平均彙整、各時步區塊特徵重要度 |
