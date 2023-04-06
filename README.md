# Monthly Revenue Forecasting

## Dataset

<table>
  <th>
    <td><strong>Raw Data</strong></td>
    <td><strong>Final Data</strong></td>
    <td><strong>通信網路業 (Communications)</strong></td>
  </th>
  <tr>
    <td><strong>資料名稱</strong></td>
    <td colspan="3">月營收盈餘</td>
  </tr>
  <tr>
    <td><strong>資料期間</strong></td>
    <td>1988年1月至2023年1月</td>
    <td>2013年1月至2022年12月<br>
        2015年1月至2022年12月
    </td>
    <td>2002年1月至2022年12月</td>
  </tr>
  <tr>
    <td><strong>資料範圍</strong></td>
    <td colspan="3">上市、櫃公司</td>
  </tr>
  <tr>
    <td><strong>資料來源</strong></td>
    <td>TEJ Company DB</td>
    <td colspan="2">TEJ Company DB & 公開資訊觀測站 (TWSE MOPS)</td>
  </tr>
</table>


## User Guide

|Date|Code (.ipynb)|Description|References|
|---|---|---|---|
|2023/02/20|001_data_partitioning_v1|<ol><li>將原始資料依照TSE產業別區分為金融業與非金融業</li><li>找出需要補值的缺失值</li></ol>|<ul><li>Pivot Table (樞紐分析表) ([link](https://www.learncodewithmike.com/2022/02/pandas-pivot-table.html))</li><li>Counting Continuous NaN Values for Pandas Time Series ([link](https://stackoverflow.com/questions/52561874/counting-continuous-nan-values-in-panda-time-series))</li></ul>|
|2023/02/28|002_data_missing_v1|從公開資訊觀測站的每月營收資訊找出前一年的當月營收，以補缺失值 ([TWSE MOPS 月營收資訊](https://mops.twse.com.tw/mops/web/t05st10_ifrs))。<br><br>Output: 補值列表|<ul><li>Creating Time Range in Python: Date Range and Month Range ([link](https://catriscode.com/2021/02/27/creating-time-range-in-python/))</li><li>爬蟲：公開資訊觀測站-每月營收彙總表 ([link1](https://www.finlab.tw/%E8%B6%85%E7%B0%A1%E5%96%AE%E7%94%A8python%E6%8A%93%E5%8F%96%E6%AF%8F%E6%9C%88%E7%87%9F%E6%94%B6/)) ([link2](https://medium.com/renee0918/python%E7%88%AC%E8%9F%B2-%E5%80%8B%E8%82%A1%E6%AF%8F%E6%9C%88%E7%87%9F%E6%94%B6%E7%B5%B1%E8%A8%88-6651bc390b8e))</li><li>爬蟲：公開資訊觀測站-財務報表 ([link](https://www.finlab.tw/python-%E8%B2%A1%E5%A0%B1%E7%88%AC%E8%9F%B2-1-%E7%B6%9C%E5%90%88%E6%90%8D%E7%9B%8A%E8%A1%A8/))</li></ul>|
|2023/03/03|003_data_updating_v1|<ol><li>透過 002_data_missing_v1.ipynb 中爬取的月營收補值列表補值</li><li>將更新後的檔案和不需要補值的檔案合併為最終檔案</li></ol>Output: <br>1988-01 至 2022-12 補完缺失值後的完整檔案||
|~~2023/02/17~~|~~004_data_cleaning_v2~~|~~最初資料清理版本（包含刪除特定產業、刪除缺失過多的年月和公司、中間缺失值補0等）~~|<ul><li>Replace NaN Values with Zeros ([link](https://datatofish.com/replace-nan-values-with-zeros/))</li></ul>|
|2023/03/13|005_data_cleaning_v1|<ol><li>將中間有缺失值的公司，最新一個月的缺失值以前的月營收皆設為空值</li><li>將清理後的檔案和不需要補值的檔案合併為最終檔案</li></ol>Results: <br>最終所有有缺失值的公司都是從 1988-01 開始連續缺失||
|2023/03/14|006_營收公告日check|觀察各公司是否有中間月份突然缺失營收公告日者。<br><br>Conclusion: <br>所有上市櫃公司都是從 2013-01 開始才有營收公告日，且沒有中間突然缺失營收公告日者||
|2023/03/23|007_data_cleaning_final_v1|最終資料清理||
|2023/03/30|008_data_analysis_v1|EDA|<ul><li>Seasonal-Trend decomposition using LOESS (STL) ([link](https://www.statsmodels.org/dev/examples/notebooks/generated/stl_decomposition.html))</li></ul>|
|2023/04/06|**009_randomforest_xgb_v5**|Monthly revenue forecasting results from 2020/1 to 2022/12 with Random Forest Regressor & XGB Regressor, including predicting results, scores and feature importance.<br><ul><li>Model related: **MonRevForecast.ipynb**</il><li>Plot related: **MonRevPlot.ipynb**<il></ul>|<ul><li>RandomForestRegressor ([link](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html))</li><li>XGBRegressor ([link](https://xgboost.readthedocs.io/en/stable/python/python_api.html#xgboost.XGBRegressor))</li></ul>|
|2023/04/|010_rnn_lstm_v1|||

  
<ul><li>標題 ([link](網址))</li><li>標題 ([link](網址))</li></ul>


## Scores

|Scores|Formula|
|---|---|
|RMSE (Root Mean Square Error)|$$RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^n(\hat{y}_i - y_i)^2}$$|
|MAE (Mean Absolute Error)|$$MAE = \frac{1}{n}\sum_{i=1}^n|\hat{y}_i - y_i|$$|
|MAE% (MAE / mean of the sum of y_true)|$$MAE\\% = \frac{\frac{1}{n} \sum |\hat{y}_i - y_i|}{\frac{1}{n} \sum y_i} = \frac{\sum |\hat{y}_i - y_i|}{\sum y_i}$$|
|MAPE (Mean Absolute Percentage Error)|$$MAPE(\\%) = \frac{1}{n}\sum_{i=1}^n \frac{|\hat{y}_i - y_i|}{y_i}$$|






