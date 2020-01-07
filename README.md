# Project4-5_Open
# 保有隱私機器學習
## 前言
在AI大環境下，最需要的就是大量的數據，而這些數據通常就來自於各位使用者使用的情形、狀況，並將此丟給AI做進一步的分析，沒有數據就沒有準確的分析結果，但在向用戶拿取資料時，往往會侵犯到用戶的隱私，不管收集的廠商有心無心，都有可能造成用戶的恐慌，防止有心人士濫用這些資料，目前希望透過混淆標籤的方式傳送一批去標籤化的資料給AI分析，儘管去標籤仍有很大的可能被駭客透過大量的資料反推回去資料提供方。
## 防止方法
### 資料通泛化
有一些特定的資料元素比較容易讓人連結到特定的個人。為了保護這些個人，我們採用通泛化處理技術，將一部分的資料移除，或是將當中某些片段換成相同的值。

通泛化處理流程有助於我們達到 K-匿名 (K-anonymity) 狀態。K-匿名這個業界標準術語所指的是一種技術，可將特定人士隱藏於相似人群中，藉此保護其身分；其中 K 代表人群的規模。針對資料集中的任何個人，如有至少 K-1 個人擁有相同屬性，該資料集即達到 K 匿名狀態。舉例來說，假設某資料集的 K 值為 50，而屬性為郵遞區號。當我們從該資料集中挑出任何一人，一定會有另外 49 人與他擁有相同的郵遞區號。因此，我們無法單單透過郵遞區號識別任何人的身分。

如果在資料集中，所有個人的某個敏感屬性都有相同的值，那麼只要知道這些人均屬於該資料集，就有可能揭露該敏感資訊。為了降低此風險，我們可利用 L-多樣性 (L-diversity) 來達到此一目的。這個業界標準術語是用於描述敏感值中的多樣性程度。舉例來說，假設有一群人全部在同一時間搜尋了同一個敏感的健康主題 (例如流感症狀)，我們檢視這個資料集時，並無法得知是誰搜尋了這個主題 (拜 K-匿名技術所賜)。不過，由於所有人都擁有相同的敏感屬性 (亦即查詢主題)，所以可能還是會有隱私權方面的疑慮。如果具備 L-多樣性，匿名資料集的查詢主題屬性不會單單包含流感查詢，而會同時納入流感查詢和其他查詢，以進一步保護使用者隱私權。
#### =>簡單來說，就是去識別化，但此種方法有很大的可能會被有心人士逆推回去。
### 在資料中加入雜訊
2016年6月，蘋果公司在全球開發者大會上首次提出了差別隱私技術（Differential Privacy），其作用能夠通過密碼學算法對用戶的數據進行「加密」上傳到蘋果伺服器。蘋果可以通過這些「加密」過的數據計算出用戶群體的行為模式，但是對每個用戶個體的數據無法解析。

差別隱私 (同樣是一個業界標準術語) 是一種在資料中加入數學雜訊的技術。資料集經過差別隱私處理後，就很難確定任何個人是否屬於該資料集，這是因為不管是否加入任何特定個人的資訊，指定演算法的輸出結果基本上看起來都一樣。舉例來說，假設我們在評估某地理區域的流感相關查詢整體搜尋趨勢，為了做到差別隱私，我們在資料集中加入雜訊，也就是將特定社區的流感相關查詢搜尋人數調高或調低，但這麼做並不會影響我們對較大地理區域的搜尋趨勢評估結果。另外有一個重要注意事項，就是在資料中加入雜訊可能會降低資料的實用性。

#### 舉例:
假設 iPhone 要收集用戶聊天打招呼是用 hello 還是 hi 的偏好。不考慮隱私的做法就直接收集就好了，差別隱私的做法會是這樣的：

首先隨機扔個硬幣，如果頭朝上，那麼就隨機上傳 hello 或者 hi，如果頭朝下，那麼就把用戶的真實 hello/hi 偏好上傳。

把所有收集到的數據進行數學統計和去噪處理，得到相對準確的偏好結果。

簡單一點理解如下圖，假設 hello 和 hi 偏好相近時，可以很容易看出，在上面例子裡加了隨機噪音過後的數據是不會影響最終的機率分布的。
![](https://i.imgur.com/jDEPugV.png)

今天所DEMO提到Google Chrome　已有在使用的技術
https://github.com/google/rappor



### 同態加密

同態加密(Homomorphic Encryption)為AI系統匯整不同來源的資料，而非實際分享資料，讓各方可在保有資料隱私的情況下進行訓練與學習。

同態加密是一種特別的加密方法，它允許人們對密文進行特定形式的代數運算得到仍然是加密的結果，將其解密所得到的結果與對明文進行同樣的運算結果一樣。
這項技術令人們可以在加密的資料中進行諸如檢索、比較等操作，得出正確的結果，而在整個處理過程中無需對資料進行解密。其意義在於，真正從根本上解決將資料及其操作委託給第三方時的保密問題，可以保證實現處理者無法訪問到數據自身的信息。

### PATE框架
PATE(Private Aggregation of Teacher Ensemble)框架使任何知道如何訓練有監督機器學習模型的人都可以為機器學習的差別隱私研究做出貢獻。PATE 框架通過仔細協調幾種不同機器學習模型的行為來實現隱私學習。只要遵循 PATE框架指定的程序，最終得到的總模型將具有可衡量的隱私保證。

在PATE中，首先將隱私數據集劃分為數據子集。這些子集是不同的分區，因此任何分區所包含的數據之間不會有重疊。我們在每個分區上訓練一個稱為「教師」的機器學習模型(如何訓練這個模型沒有任何限制)。這實際上是 PATE 的優點之一：構建「教師」模型的學習演算法是不可知的。所有的「教師」模型都解決了相同的機器學習任務，但他們的訓練過程都是獨立進行的。

現在有一套獨立訓練的「教師」模型集合，但沒有任何隱私保證。在 PATE 中增加噪音，同時將每個「教師」單獨進行的預測聚合起來，以形成一個統一的預測。計算產生每個預測類的「教師」模型數量（即每個類的投票數），然後通過添加從拉普拉斯或高斯分布採樣的隨機雜訊來擾亂計數。當兩個輸出類別的投票數相同時，這種噪音將確保擁有最多投票數的類將是隨機選擇的這兩個類中的一個。另一方面，如果大多數「教師」模型產生了同一個分類結果，增加噪音並不會改變這個類得到最多投票數的事實。只要「教師」之間的共識度足夠高，這種微妙的協調便為雜訊聚合機制所做的預測提供了正確性和隱私保證。

PATE框架面臨兩個限制。首先，由聚合機製得到的每個預測都會增加總隱私預算。這意味著，當要預測許多標籤時，最終總的隱私預算會變得很大，所提供的隱私保證變得毫無意義。因此，API 必須對所有用戶限制查詢的最大數量，並在達到上限時獲取一組新的數據來訓練新的「教師」模型集合。其次，我們不能公開發布「教師」模型的集合。否則，攻擊者可以檢查已發布「教師」的內部參數，以了解訓練模型的隱私數據。由於這兩個原因，PATE 中有一個額外的步驟，就是創建「學生」模型。

「學生」模型是通過一種保護隱私的方式將「教師」模型集合獲得的知識轉化進行訓練的。當然，雜訊聚合機制是其重要的工具。「學生」從一組未標記的公共數據中選擇輸入，並將這些輸入提交給「教師」集合來標記它們。雜訊聚合機制會給出隱私標籤，「學生」會用這些標籤來訓練模型。「學生」模型是 PATE 的最終產品，由它來響應最終用戶的任何查詢預測。在這一點上，隱私數據和「教師」模型可以安全地被丟棄，「學生」是用於推斷的唯一模型。

#### 補充
以我目前的了解，這個框架的運作原理是先把隱私數據分成不同的子集合。每個子集合的數據不會出現在其他的子集合當中，亦即每個數據只會在一個子集合中出現。然後每個子集合分別訓練一個教師模型，這個教學模型使用的機器學習方法及規則不受限，可是也不能對外公開，保護隱私。

假設今天模型是用來預測癌症，把一筆數據丟到所有教師模型，每個教師模型都會有自己的結果(癌症/健康)，然後把結果進行統計，就是一個結果。為了要保護統計結果，會加上雜訊，讓統計數值長得不一樣，可是結果不變。最後，就是利用這些結果去訓練一個學生模型。學生的input是由眾老師的統計結果所得，學生並不知道老師是怎樣判斷這個結果，當學生模型訓練完成後，前面的教師模型和統計的框架可以整個捨棄，只剩學生模型。


### 對機器學習的攻擊方式

1.  Membership attack：

攻擊方式：給定一筆資料，測試它是否在訓練資料中。

已有例子：以上作法如果運用在以病患資料訓練而成的模型，即有可能洩漏訓練資料中個別病患的資訊。在 [Shokri et al（2016）](https://arxiv.org/abs/1610.05820)中，已提出可以做 membership inference attack 的黑箱攻擊手法，並且已在 Google Perdiction API 與 Amazon ML 服務測試成功，對於用這些服務訓練出來的模型各有 94% 與 74% 機會猜到它們的訓練資料中是否存在某一筆資料。且這個作法需要模型回傳機率分布而不是只有預測的標籤。

2. Training data extraction：

攻擊方式：取得整個訓練資料的大致內容，足以得知其統計分佈。在文獻中的模型反制攻擊即屬於此類。

已有例子：[Fredrikson et al（2014）](https://www.biostat.wisc.edu/~page/WarfarinUsenix2014.pdf)較早提出的模型反制攻擊是針對用藥建議系統，用模型與某位病患的人口屬性，可以回推出該病患的遺傳資料（即該模型中的input）。後續的 [Fredrikson et al（2015）](https://dl.acm.org/doi/10.1145/2810103.2813677)則有對臉部辨識系統的例子，用一個姓名（即模型的output）可以回推出這個人的臉部影像。Model inversion這種攻擊手法與 algorithmic fairness 研究所考慮的 redundant encoding 問題類似，但其實用性仍有待研究。

3.  Model extraction：

攻擊方式：在黑箱攻擊情境下，取得 model 內部的參數值。

已有例子：研究發現，模型參數值至少能部份地記下訓練資料，所以能夠取得模型參數除了安全問題，也可能洩漏隱私資訊。目前 [Tramèr et al（2016）](https://www.usenix.org/conference/usenixsecurity16/technical-sessions/presentation/tramer)提出的作法可以用 API 取得模型參數。這個作法需要 API 回傳機率分布而不是只有標籤。

#### 個人見解：

1.  Membership attack：我認為主要還是只能用在資料庫較小的地方，像是一家醫院，一間學校等專一性比較重的地方，在較為有限的資料庫中比較能用已有的資料得知訓練資料中是否有目標資料，之於那種大型的資料庫，像是一整個國家的醫院病患資料，或更甚至整個洲等級的資料統計，個人的資料獨立性將被稀釋的差不多，這類型的資料攻擊也將變得像是海底撈針一樣不可行。

2.  Training data extraction：對於資料的攻擊性，不能像Membership attack一樣針對同筆資料去做比對，而是對局部部分的資料去做逆推，這對於個資問題也只能確認局部的資料，想要獲取完整資料庫還是極其困難。

3.  Model extraction：直接獲取模型的參數，我認為算是最能知道模型資料的方式，畢竟其他方式都只是逆推回去，而不是正面直接地獲取資料，對於想取得完整性資料來說，是最直接也是最完整的。但目前這方法也只能提取出模型的標籤，還不能完整還原訓練資料本身的參數。

---

### 參考來源:
https://policies.google.com/technologies/anonymization?hl=zh-TW
https://kknews.cc/zh-tw/tech/k8x9lb.html
https://kknews.cc/tech/2vp2l8g.html
https://www.eettaiwan.com/news/article/20190826NT01-holy-grail-of-encryption-could-be-a-game-changer-for-ai
https://zh.wikipedia.org/wiki/%E5%90%8C%E6%80%81%E5%8A%A0%E5%AF%86
https://www.xuehua.us/2018/06/19/%E6%84%8F%E6%83%B3%E4%B8%8D%E5%88%B0%E7%9A%84%E7%9B%9F%E5%8F%8B%EF%BC%9A%E6%94%B9%E5%96%84%E9%9A%90%E7%A7%81%E9%97%AE%E9%A2%98%E5%8F%AF%E4%BB%A5%E5%B8%A6%E6%9D%A5%E8%A1%A8%E7%8E%B0%E6%9B%B4%E5%A5%BD/zh-tw/
https://medium.com/trustableai/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92%E6%BD%9B%E5%9C%A8%E7%9A%84%E9%9A%B1%E7%A7%81%E5%95%8F%E9%A1%8C-9410eb951411
