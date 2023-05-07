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
|2023/03/23|007_data_cleaning_final_v2|最終資料清理||
|2023/03/30|008_data_analysis_v1|EDA|<ul><li>Seasonal-Trend decomposition using LOESS (STL) ([link](https://www.statsmodels.org/dev/examples/notebooks/generated/stl_decomposition.html))</li></ul>|
|2023/04/06|**009_randomforest_xgb_v5**|Monthly revenue forecasting results from 2020/1 to 2022/12 with Random Forest Regressor & XGB Regressor, including predicting results, scores and feature importance.<br><ul><li>Model related: **MonRevForecast.ipynb**</il><li>Plot related: **MonRevPlot.ipynb**<il></ul>|<ul><li>RandomForestRegressor ([link](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html))</li><li>XGBRegressor ([link](https://xgboost.readthedocs.io/en/stable/python/python_api.html#xgboost.XGBRegressor))</li></ul>|
|2023/05/07|**009_randomforest_xgb_v6**|Monthly revenue forecasting results from 2018/1 to 2022/12 with Random Forest Regressor & XGB Regressor, including predicting results, scores and feature importance.<br><ul><li>Model related: **MonRevForecast.ipynb**</il><li>Plot related: **MonRevPlot.ipynb**<il></ul>|<ul><li>RandomForestRegressor ([link](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html))</li><li>XGBRegressor ([link](https://xgboost.readthedocs.io/en/stable/python/python_api.html#xgboost.XGBRegressor))</li></ul>|
|2023/04/08|010_rf_xgb_comms_v1|Communications industry monthly revenue forecasting results from 2007/1 to 2022/12 with Random Forest Regressor & XGB Regressor.||
|~~2023/04/16~~|~~011_rnn_lstm_v1~~|~~RNN & LSTM version 1<ul><li>loss = 'mse'</li><li>epoch = 100</li><li>no validation set</li></ul>~~|<ul><li>SimpleRNN layer ([link](https://keras.io/api/layers/recurrent_layers/simple_rnn/))</li><li>LSTM layer ([link](https://keras.io/api/layers/recurrent_layers/lstm/))</li></ul>|
|2023/04/19|**011_rnn_lstm_v7**|Monthly revenue forecasting results from 2020/1 to 2022/12 with RNN & LSTM (loss = 'mse'), including predicting results and scores.<br><ul><li>Model related: **MonRevForecast.ipynb**</il><li>Plot related: **MonRevPlot.ipynb**<il></ul>|<ul><li>SimpleRNN layer ([link](https://keras.io/api/layers/recurrent_layers/simple_rnn/))</li><li>LSTM layer ([link](https://keras.io/api/layers/recurrent_layers/lstm/))</li></ul>|
|2023/05/|**011_rnn_lstm_v8**|Monthly revenue forecasting results from 2018/1 to 2022/12 with RNN & LSTM (loss = 'mse'), including predicting results and scores.<br><ul><li>Model related: **MonRevForecast.ipynb**</il><li>Plot related: **MonRevPlot.ipynb**<il></ul>|<ul><li>SimpleRNN layer ([link](https://keras.io/api/layers/recurrent_layers/simple_rnn/))</li><li>LSTM layer ([link](https://keras.io/api/layers/recurrent_layers/lstm/))</li></ul>|


<ul><li>標題 ([link](網址))</li><li>標題 ([link](網址))</li></ul>


## Function

<table>
  <tr>
    <td><strong>Date</strong></td>
    <td><strong>Code (.ipynb)</strong></td>
    <td><strong>Records</strong></td>
    <td><strong>References</strong></td>
  </tr>
  
  <tr>
    <td>2023/04/14</td>
    <td><strong>MonRevForecast</strong></td>
    <td>
      2023/04/04 - Save multiple df results as dict.<br>
      2023/04/05 - Add <I>9. Best and Worst Model</I>.<br>
      2023/04/06 - Add <I>trainMonthlyRevenue()</I> for training and saving models in advance.<br>
      2023/04/08 - Fix fragmented problem (for Comms).<br>
      <br>
      2023/04/13 - Add <I>5.2 RNN & LSTM</I>.<br>
      2023/04/14 - Add <I>6.3 RNN & LSTM</I>.<br>
      2023/04/14 - Add <I>9.2 RNN & LSTM</I>.<br>
      2023/04/16 - Add loss='mae' option in <I>5.2 RNN & LSTM</I>.<br>
      2023/04/18 - Modify epoch numbers for deflating data in <I>5.2 RNN & LSTM</I>.<br>
      2023/04/18 - Add training data as validation data in <I>5.2 RNN & LSTM</I>.<br>
      2023/05/04 - Add industry feature in <I>5. Model Training</I> & <I>6. Predicting and Evaluation</I>.<br>
      2023/05/05 - Modify Random Forest Regressor & XGB Regressor.<br>
      2023/05/06 - <I>Add 4.5 Feature Encoding for Industry Data</I>.<br>
    </td>
    <td>
      <ul>
        <li>RandomForestRegressor (<a href="https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html" target="_blank">link</a>)</li>
        <li>XGBRegressor (<a href="https://xgboost.readthedocs.io/en/stable/python/python_api.html#xgboost.XGBRegressor" target="_blank">link</a>)</li>
        <li>SimpleRNN layer (<a href="https://keras.io/api/layers/recurrent_layers/simple_rnn/" target="_blank">link</a>)</li>
        <li>LSTM layer (<a href="https://keras.io/api/layers/recurrent_layers/lstm/" target="_blank">link</a>)</li>
        <li>Forecast Sales with Machine Learning Algorithms (<a href="https://www.embedded-robotics.com/forecast-sales-using-machine-learning/#comparing-forecast-sales-using-machine-learning-algorithms" target="_blank">link</a>)</li>
        <li>Time Series Forecasting with Keras 
          (<a href="https://towardsdatascience.com/a-quick-deep-learning-recipe-time-series-forecasting-with-keras-in-python-f759923ba64" target="_blank">link 1</a>) 
          (<a href="https://towardsdatascience.com/a-practical-guide-to-rnn-and-lstm-in-keras-980f176271bc" target="_blank">link 2</a>) 
          (<a href="https://www.datacamp.com/tutorial/tutorial-for-recurrent-neural-network" target="_blank">link 3</a>)
        </li>
        <li>Time Series Prediction with SimpleRNN (<a href="https://machinelearningmastery.com/understanding-simple-recurrent-neural-networks-in-keras/" target="_blank">link</a>)</li>
        <li>Time Series Prediction with LSTM 
          (<a href="https://machinelearningmastery.com/time-series-prediction-lstm-recurrent-neural-networks-python-keras/" target="_blank">link 1</a>) 
          (<a href="https://machinelearningmastery.com/time-series-forecasting-long-short-term-memory-network-python/" target="_blank">link 2</a>) 
          (<a href="https://github.com/yifan-guo-cwru/Time-Series-Prediction-with-LSTM-Recurrent-Neural-Networks-in-Python-with-Keras/blob/master/run.py" target="_blank">link 3</a>)
        </li>
        <li>Keras: EarlyStopping 
          (<a href="https://keras.io/api/callbacks/early_stopping/" target="_blank">link 1</a>)
          (<a href="https://medium.com/zero-equals-false/early-stopping-to-avoid-overfitting-in-neural-network-keras-b68c96ed05d9" target="_blank">link 2</a>)
        </li>
        <li>Keras: Save training history (<a href="https://stackoverflow.com/questions/41061457/keras-how-to-save-the-training-history-attribute-of-the-history-object" target="_blank">link</a>)</li>
      </ul>
    </td>  
  </tr>
  
  <tr>
    <td>2023/04/05</td>
    <td><strong>MonRevPlot</strong></td>
    <td>
      2023/04/05 - Add <I>9. Best and Worst Model - Feature Importance</I>.<br>
      2023/04/08 - Date and figure size updated for Comms.<br>
      2023/04/15 - Update 8.1 & 8.2 for RNN and LSTM.
    </td>
    <td>-</td>
  </tr>

</table>  
  

<li>標題 (<a href="" target="_blank">link</a>)</li>
<li>標題 (<a href="" target="_blank">link</a>)</li>


  
  

  
## Evaluation

|Scores|Formula|
|---|---|
|**Root Mean Square Error**|$$RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^n(\hat{y}_i - y_i)^2}$$|
|**Mean Absolute Error**|$$MAE = \frac{1}{n}\sum_{i=1}^n \|\hat{y}_i - y_i\|$$|
|**Mean Absolute Error %**|$$MAE\\% = \frac{\frac{1}{n} \sum \|\hat{y}_i - y_i\|}{\frac{1}{n} \sum y_i} = \frac{\sum \|\hat{y}_i - y_i\|}{\sum y_i}$$|
|**Mean Absolute Percentage Error**|$$MAPE(\\%) = \frac{1}{n}\sum_{i=1}^n \frac{\|\hat{y}_i - y_i\|}{y_i}$$|






