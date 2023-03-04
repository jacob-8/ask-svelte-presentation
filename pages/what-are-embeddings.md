## Embeddings 是什麼？ (矢量數據? 嵌入?)

`單詞: [0.22, 0.34, -0.97, 0.01, ...]`

<div flex>

<div mr-2 border v-click>

||王權 royalty|女性 feminine|
| --- | --- | --- |
|王 king|0.9|-0.9|
|女王 queen|0.9|0.9|
|男人 man|0.1|-0.9|
|女人 woman|0.1|0.9|

</div>

<div border v-click>

|||[王權,|女性]|
| --- | --- | --- | --- |
||王|[0.9,|-0.9]|
|-|男人|[0.1,|-0.9]|
|+|女人|[0.1,|0.9]|
|=|女王|[0.9,|0.9]|

</div>
</div>

<div v-click>

來源: [The amazing power of word vectors](https://blog.acolyer.org/2016/04/21/the-amazing-power-of-word-vectors/)

</div>

<!-- qiànrù

一個數字數組有很多值，每一個值的值是 -1 (消極一) 和 1 之間. 在很多不一樣的生活的部分(很多方面)這些數字代表單詞的意思.

說得更簡單:嵌入就像是一種特殊的圖畫。就像一個畫家可以使用顏料和畫筆來創造一幅畫一樣，計算機可以使用一些數字和數學公式來創建一個嵌入。 嵌入可以被看作是一個特殊的代碼，它可以把一些東西（比如單詞或圖像）轉化成數字。這些數字可以讓計算機更好地理解這些東西，並用它們來完成各種任務，比如找到與它們相關的其他東西。

我有一個例子...

在我的工具中，我可以使用嵌入來查找與問題的意思最相關的文檔部分。

[chinese-word-vectors](https://primer.ai/developer/chinese-word-vectors/)

An array of values between -1 and 1 for that paint the semantic meaning of a word using multiple dimensions

 -->