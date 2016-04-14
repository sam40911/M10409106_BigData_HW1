# 一. 作業說明

> [巨量資料分析＿第一次作業_M10409106_許勝翔] Data Collection and Persistence, 利用 TwitterAPI 抓取 1GB JSON 格式的資料，並使用 Markdown 撰寫 Readme.md

專案中共有兩個資料夾：dataset 以及 crawler :
#### dataset資料夾中有三個檔案: 
"JSON_Output.json.zip.001"與 "JSON_Output.json.zip.002" 請解開壓縮檔\
可得到原先超過 1G 的 JSON_Output.json檔案（因為Github限制單個檔案大小不能超過 100MB ）\
"m10409106.log" 紀錄資料抓取時間的 log檔
>
#### crawler資料夾中有三個檔案: 
包含 TwitterAPI_New.py 以及 PersistingIO.py 、PersistingIO.pyc 
# 二. 於終端機中執行.py指令
```
$python TwitterAPI_NEW.py
```
# 三. 抓取資料的敘述
#### 1. 資料來源
    Twitter
#### 2. 資料主題
    "MLB and 13 teams" (美國職棒大聯盟以及其中的13隊隊名)
#### 3. 查詢關鍵字
```
    "MLB", "Yankees", "RedSox", "BlueJays", "Orioles", "Royals", 
    "WhiteSox", "Mariners", "Dodgers", "Phillies", "Rockies", 
    "Mets", "Cubs", "Reds"
```

#### 4. JSON格式架構分析
```
此次作業利用TwitterAPI所輸出的JSON格式包含以下欄位：
* "contributors": 
    象徵對Twitter有重要貢獻的人，通常是代表官方tweet作者。
* "truncated":
    因為目前單一則 Tweets的文字數目上限為140字，所以目前大部分推特文章此欄皆為
    “false”，若以前在推特發布超過140字，超過的文字會以 “...”省略，則此欄值為
    “true”
* "text"
    存放該則推特所發佈的內容
* "is_quote_status"
    該則推特是否被引用
* "in_reply_to_status_id"
    可為空值，如果回覆某則推特，則此欄位會儲存被回覆推特文章的ID
* "id"
    存放此篇Tweet的ID
* "favorite_count"
    記錄多少人喜歡該篇Tweets
* "entities"
        * symbols : 類似hashtag的功能，但是以”$“符號表示
        * user_mentions : 是否提及其他Twitter使用者
        * hashtags : Tweet文章中加註的標籤，以”＃“符號表示
        * urls : 存放Tweet內容中的超連結相關內容 
        * media : 存放Tweet內容中的多媒體相關內容（圖片或影片等）
        
* "retweeted"
    該篇Tweet是否有被其他Twitter使用者轉推
* "coordinates"
    座標可為空值，用來儲存該則推特文章於哪個地點發布
* "source"
    被使用在發布Tweet時，屬於HTML格式字串
* "in_reply_to_screen_name"
    可為空值，若該篇Tweet為回覆別人的Tweet，會顯示原Tweet名稱
* "in_reply_to_user_id"
    可為空值，若該篇Tweet為回覆別人的Tweet，會顯示原Tweet的使用者ID
* "retweet_count"
    該篇Tweet被轉推的次數
* "id_str"
    該篇Tweet的ID編號
* "favorited"
    記錄該篇Tweet是否被其他使用者喜歡
* "retweeted_status"
    該篇Tweet是否有被其他Twitter使用者轉推，此欄位可保留完整原始被轉推的Tweet內容
    所以此欄位底下包含眾多細項，比如：contributors, truncated,
    text, is_quote_status ...... 這些欄位在上方皆已解釋過，用途相同
    *  "user": 
        * "follow_request_sent":
        可為空值，是否有已驗證的使用者要求追蹤這位受到保護的使用者帳戶,
        * "has_extended_profile": 有沒有擴展的profile,
        * "profile_use_background_image":
        使用者是否同意可以使用他們上傳的背景圖片
        * "default_profile_image":
        使用者是否有上傳他們的照片，沒有上傳則以預設的圖案代替
        * "id": 使用者的ID（以整數表示）
        * "profile_background_image_url_https":
        使用者背景圖片的網址（HTTPS）
        * "verified": 使用者是否有通過驗證
        * "profile_text_color": 使用者使用的的字體顏色（十六進位）
        * "profile_image_url_https": 使用者照片的網址（HTTPS）,
        * "profile_sidebar_fill_color":
        使用者使用的側邊欄位的背景顏色（十六進位）,
        * "entities": Entity會提供metadata以及額外有關於這篇Tweet的資訊
           * "description": 
              * "urls": 這篇Tweet涵蓋的網址
        * "followers_count": 追蹤這個使用者帳號的人數,
        * "profile_sidebar_border_color":
        使用者使用的側邊欄位的邊框顏色（十六進位）
        * "id_str": 使用者（以String字串表示）
        * "profile_background_color": 使用者使用的的背景顏色（十六進位）
        * "listed_count": 使用者所擁有的list數量
        * "is_translation_enabled": 使用者是不是允許翻譯
        * "utc_offset": 與GMT標準時間的時差（以秒顯示）
        * "statuses_count": 使用者發出的Tweet數量
        * "description": 使用者自介
        * "friends_count": 使用者追蹤其他使用者的數量
        * "location": 使用者可自行定義的地區位置
        * "profile_link_color": 使用者使用的的連結顏色（十六進位）
        * "profile_image_url":使用者個人照片的網址
        * "following": 使用者是否有被其他已驗證的使用者追蹤
        * "geo_enabled": 使用者是否同意將地理位置顯示於照片上
        * "profile_background_image_url":使用者背景圖片的網址（基於HTTP）
        * "screen_name": 使用者自行定義的名稱，可以做更改
        * "lang": 使用者自行選擇顯示介面的語言，以BCP47編碼顯示
        * "profile_background_tile": 是否顯示使用者背景圖片的網址
        * "favourites_count": 使用者喜歡的Tweet數量
        * "name":使用者顯示的名稱，可以做更改
        * "notifications": 當使用者發布新的Tweet時，是否同意收到SMS的更新通知
        * "url": 使用者所提供的網址
        * "created_at": 使用者創建帳號的時間
        * "contributors_enabled": 
            使用者是否有連結貢獻者模式(contributor mode)
        * "time_zone": 使用者自行定義的時區位置
        * "protected": 使用者是否有選擇保護他們的Tweet
        * "default_profile": 使用者是否使用預設的照片
        * "is_translator": 使用者是否有參與幫助翻譯Twitter的工作
     *  "geo": 不建議使用，現在已被coordinates欄位取代
     *  "in_reply_to_user_id_str":
        若這篇Tweet為回覆別人的Tweet，會顯示被回覆的Tweet的作者ID
     *  "lang": 根據機器偵測自動判斷出該Tweet內容所使用的語言
     *  "created_at": 使用者發表Tweet的時間
     *  "in_reply_to_status_id_str":
        若這篇Tweet為回覆別人的Tweet，會顯示被回覆的Tweet的ID
     *  "place": 該Tweet是否有與某地連結
     *  "metadata": 
        * "iso_language_code": 該網站所使用的編碼語系
        * "result_type": Tweet的型態為何，是熱門Tweet還是近期所發出的Tweet
```
