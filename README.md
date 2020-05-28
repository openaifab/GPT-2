# GPT-2

GPT-2：基於 Transformer 的巨大語言模型
GPT-2 的前身是 GPT，其全名為 Generative Pre-Training。在 GPT-2 的論文裡頭，作者們首先從網路上爬了將近 40 GB，名為 WebText（開源版） 的文本數據，並用此龐大文本訓練了數個以 Transformer 架構為基底的語言模型（language model），讓這些模型在讀進一段文本之後，能夠預測下一個字（word）。

![image](https://leemeng.tw/images/bert/lm-equation.jpg)

基本上只要了解 Transformer 架構，你馬上就懂 GPT-2 了。因為該語言模型的本體事實上就是 Transformer 裡的 Decoder：

![image](https://leemeng.tw/images/gpt2/elmo-bert-gpt2.jpg)

更精準地說，GPT-2 使用的 Transformer Decoder 是原始 Transformer 論文的小變形（比方說沒有了關注 Encoder 的 Decoder-Encoder Attention Layer），但序列生成（Sequence Generation）的概念是完全相同的。

架構本身沒什麼特別。但 GPT-2 之所以出名，是因為它訓練模型時所使用的數據以及參數量都是前所未有地龐大：

訓練數據：使用從 800 萬個網頁爬來的 40 GB 高品質文本。把金庸 14 部著作全部串起來也不過 50 MB。WebText 的數據量是金庸著作的 800 倍。想像一下光是要看完這 14 部著作一遍所需花費的時間就好。
模型參數：15 億參數，是已經相當巨大、擁有 3.4 億參數的 BERT-Large 語言代表模型的 4.5 倍之多。BERT-Large 使用了 24 層 Transformer blocks，GPT-2 則使用了 48 層。

這可是有史以來最多參數的語言模型。而 GPT-2 獨角獸（unicorn）的形象則是因為當初作者在論文裡生成文本時，給了 GPT-2 一段關於「住在安地斯山脈，且會說英文的一群獨角獸」作為前文脈絡，而 GPT-2 接著生成的結果有模有樣、頭頭是道，讓許多人都驚呆了：

![image](https://leemeng.tw/images/gpt2/gpt2-unicorns.jpg)

此模型用了Transformer 架構的一部份。而這主要是因為 Transformer 裡頭的自注意力機制（Self-Attention Mechanism）十分有效且相當適合平行運算。GPT(-2) 的 Transformer Decoder 裡頭疊了很多層 Decoder blocks，以下則是某一層 Decoder block 透過自注意力機制處理一段文字的示意圖：

![image](https://leemeng.tw/images/gpt2/decoder-block-attention.jpg)

給定一段文本：<s> a robot must obey the orders given it …
你可以很輕易地看出 it 指代前面出現過的 robot。而這是因為你懂得去關注（pay attention to）前文並修正當前詞彙 it 的語意。在給定相同句子時，傳統詞嵌入（Word Embeddings）方法是很難做到這件事情的。所幸，透過強大的自注意力機制，我們可以讓模型學會關注上文以決定每個詞彙所代表的語意。

以上例而言，訓練好的 GPT 可以在看到 it 時知道該去關注前面的 a 及 robot，並進而調整 it 在當下代表的意思（即修正該詞彙的 vector representation）。而被融入前文語意的新 representation 就是所謂的 Contextual Word Representation。
關於自注意力機制（Self-Attention），有個值得記住的重要概念：在 GPT 之後問世的 BERT 是同時關注整個序列來修正一個特定詞彙的 representation，讓該詞彙的 repr. 同時隱含上下文資訊；而 GPT 是一個由左到右的常見語言模型，會額外透過遮罩技巧（Masking）來確保模型只會關注到某詞彙以左的上文資訊。

![image](https://leemeng.tw/images/gpt2/self-attention-vs-masked-version.jpg)

在原始的 Transformer 架構裡頭就包含了 Encoder 與 Decoder，分別使用左側與右側的自注意力機制。BERT 跟 GPT 其實只是各選一邊來用 

這是使用開源套件transformers的GPT-2產生文本的範例

步驟如下:

1.用transformers的TFGPT2LMHeadModel讀取預訓練模型GPT-2

2.用transformers的GPT2Tokenizer將指定的prefix轉成GPT-2指定的格式input_ids

3.將input_ids放入GPT-2產生output

4.用GPT2Tokenizer將output轉成生成的文本

![image](https://github.com/openaifab/GPT-2/blob/master/gpt-2.jpg)
