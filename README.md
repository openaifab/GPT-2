# GPT-2

這是使用開源套件transformers的GPT-2產生文本的範例

步驟如下:

1.用transformers的TFGPT2LMHeadModel讀取預訓練模型GPT-2

2.用transformers的GPT2Tokenizer將指定的prefix轉成GPT-2指定的格式input_ids

3.將input_ids放入GPT-2產生output

4.用GPT2Tokenizer將output轉成生成的文本

![image](https://github.com/openaifab/GPT-2/blob/master/gpt-2.jpg)
