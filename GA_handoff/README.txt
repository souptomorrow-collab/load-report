================================================================
不可轉移負載預測 — 交接給 GA 排程 (7天完整滾動 / 點預測)
================================================================

【檔案】(拆兩個檔,actual 不重複)
  load_forecast_7day_rolling.csv  滾動預測(純預測,GA 排程實際用這個)
  actual_load.csv                 真實值(每時刻一列,僅供事後畫圖比對)

【這是什麼】
最後 7 天的「日內滾動」不可轉移負載預測,供 GA 電池/排程 + 畫圖驗證。
- 不可轉移負載 = 整戶總用電 − 三個可轉移分錶(廚房S1/洗衣S2/熱水器+空調S3)
  = 必須即時供電、無法挪移的基礎用電需求(GA 排程一定要被滿足的負載)。

────────────────────────────────────────
【預測檔】load_forecast_7day_rolling.csv
  refresh_time       刷新時刻 = 「最後觀測時刻」(現在站的這一刻)。每15分一個。
  target_time        被預測的未來時刻。★一律 > refresh_time(全是未來)★
                     target_time = refresh_time + lead_step × 15分。
  lead_step          領先步數 1~96。lead=1→未來下一步(15分後),lead=96→24小時後。
  predicted_load_kW  點預測值(kW,該15分鐘區間的平均功率)。

  ★time/target_time 是 15 分鐘區間的「起始時刻」:
    target_time=21:30 的值 = [21:30,21:45) 這 15 分鐘的平均功率。

【真實檔】actual_load.csv
  time               時刻(15分區間起始)。每時刻只有一列(不重複)。
  actual_load_kW     真實負載(kW)。

【兩檔怎麼搭配(畫圖比對)】
  用 target_time(預測檔) 對 time(真實檔) 做 join,即可比對 predicted vs actual。
  例(pandas): forecast.merge(actual, left_on='target_time', right_on='time')

【★actual 的正確用法★】
  - 畫圖驗證:join 後對比 predicted vs actual,看準不準。OK。
  - 實際排程:GA「只能用 predicted_load_kW」!部署時沒有未來 actual,
    用 actual 排程 = 偷看答案(作弊)。

────────────────────────────────────────
【資料規格】
  - 解析度 15 分鐘;滾動 7 天(首 target 從 2010-11-18 00:00 起,首 refresh=11-17 23:45) = 672 個 refresh_time,各帶未來 96 步
  - 預測檔 672×96 = 64,512 列;真實檔 767 列

【日內滾動重排怎麼用】
  每個 refresh_time(現在)重排一次:
    1. 篩該 refresh_time 的 96 列(lead 1~96) = 接下來 24h 預測。
    2. 用 predicted_load_kW 重排電池充放電。
    3. 過 15 分到下一個 refresh_time,取新 96 列、再重排。
  ★同一個 target_time 會被多個 refresh 預測(越接近越準,這是滾動特性);
   使用時請「固定一個 refresh_time、取其 96 列」,勿跨 refresh 混用。

【預測準度(本7天實測)】
  下一步(lead=1) MAE ≈ 0.107 kW;整條滾動 MAE ≈ 0.156 kW。
  越遠的 lead_step 誤差越大,遠端建議多留安全裕度。

【備註】
  點預測,非真值。模型=Direct strategy（未來每個時步各一個隨機森林，共 96 個）。
  台灣情境版(法國UCI資料經季節對齊+台灣假日/氣溫框架)。
  需 P90 保守上界、原始法國版、或更長天數,請洽預測組。
================================================================
