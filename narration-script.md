# 問 SvelteKit, Ask SvelteKit

以任何語言來使用 AI 搜索知識庫 
Use AI to search a knowledge knowledge using any language

大家好! 我叫亞瀾. 今天我很高興給大家一個 AI 的介紹。 我很感謝戴維廷,給我這個機會。 大概 2004，我開始做網站。 到目前我還沒有關於這方面技術的演講經驗。 有一次我給一個演講關於技術，但我的觀眾不是程式人。 所以這是我第一次真的關於技術演講。我正在學習中文，所以我想，為什麼不用中文進行我第一次技術的演講呢？ 那以後如果我用英文演講的時候，我會覺得很容易。
Hello! I'm Jacob. Today I'm happy to give everyone an introduction to AI. I'd like to thank Dài Wéitíng for giving me this opportunity. About 2004, I started building websites. Until now I haven't given a tech talk. Well, once I gave a tech related talk but my audience wasn't programmers. So this is my first real tech talk. I'm studying Chinese, so I thought, "Why not use Chinese to give my first tech talk?" Maybe in the future if I give one in English, then it'll be really easy.

在我開始之前，我需要感謝我太太麗婷，感謝她的支持還有興趣。
Before we start, I'd like to thank my wife, Lìtíng, for her support and interest.

那我們開始把。 我覺得你們已經知道怎麼用 ChatGPT，但你知道怎麼把 ChatGPT 放在自己的工具?
Let's start. I think you already know how to use ChatGPT, but do you know how to put ChatGPT into your tools?

今天我會幫助你了解我如何使用 OpenAI 和 SvelteKit 為 SvelteKit 文檔網站構建類似 ChatGPT 的多語言對話搜索。還有了解我如何使用 Firebase Auth 保護我的 API。最後我希望你獲得一些關於如何使用 Firebase 為你自己或你的公司創建多語言搜索工具的想法。
Today I will help you understand how I use OpenAI and SvelteKit to build a ChatGPT like multilingual, conversational search. Also, to understand how I use Firebase Auth to protect my API. Lastly, I hope you will gain a few ideas about how to further use Firebase in your personal or work tools.

# 搜索示範 Search demo

首先，我會示範我構建的聰明的搜索引擎，然後怎麼構建它。Svelte 是一個 JavaScript 框架，還有你可以利用 SvelteKit 做厲害的 web 應用程式 (好像 NextJS)。 那 SvelteKit 沒有中文的文檔。 因為內容常常變化, 翻譯文檔很難。如果我的英文不太好怎麼辦學怎麼用 SvelteKit. 比如怎創建一個 API 路由.有一點難找到. 
First I will demonstrate the smart search engine I built, then how to build it. Svelte is a JavaScript framework, and you can use SvelteKit to build 厲害的 web applications (it's like NextJS). Now SvelteKit doesn't have Chinese documentation. Because it's content often changes, it's difficult to translate. If my English isn't too great, then how do I know how to use SvelteKit. For example, how to create an API route. It's a little hard to find.

這將不會再是問題。 使用我的工具，我們可以獲取英文文檔，用中文提出問題，然後得到有用的中文答案。
This is no longer a problem. Using my tool, we can use English documentation, ask a question in Chinese, and receive a useful answer in Chinese.

- 搜索知識庫 - 搜索 SvelteKit 官方文檔 Search Knowledge Base - Search SvelteKit Official Documentation

- AI - 使用 OpenAI ChatGPT-3.5

- 任何語言 - 它將使用相同的語言回复 Any Language - It will reply using the same language of the question

讓我問幾個問題。
Let me ask a few questions.

...

最後我會讓你們試試看，所以如果你有使用 SvelteKit、NextJS、Nuxt、Astro 或任何 JavaScript 框架的經驗，請開始考慮你想問什麼問題。
At the end I'll let you have a try, so if you have experience with SvelteKit... or any JavaScript framework, start thinking about what question you want to ask.

# 用例 Use Cases

這個科技有很多領用
This technology has many uses

創建一個非常厲害的聊天助手： 
Create a highly proficient chat assistant for your:

- 公司內部知識庫 company internal knowledge base
- 產品文檔 product documentation
- 個人知識庫 personal knowledge base

你只有一個限制:你的想像力。
The only limit is your imagination.

# 怎麼構建知識庫搜索 How to build a knowledge base search

- 把文檔預先處理成小部分 Preprocess documentation into small sections
- 使用 OpenAI 創建 embeddings Create embeddings using OpenAI
- 前端：添加搜索輸入 Add a search box to the front end
- 後端：創建 SvelteKit API 端點 Create SvelteKit back-end API endpoint 
  - 把使用者的問題創建 embedding Create embedding of user's query
  - 使用 embedding 來查找最相關的文檔部分 Use embeddings to find most relevant documentation sections
  - 把相關文檔部分和使用者的問題發送給 OpenAI Send relevant documentation and user's question to OpenAI
  - 把答案流式傳輸給使用者 Stream answer back to user
- 前端：示範答案 Show answer
- 使用 Firebase Auth 保護 API Secure your API with Firebase Auth

## 把文檔預先處理成小部分 Preprocess documentation into small sections

這是預先處理過程的簡化版本. 我把每個 SvelteKit 文檔的 markdown 文件上執行(zhíxíng)這個函數:
Here is a simplified version of the process I run for every markdown file in the SvelteKit documentation.

- 讀文件內容
Read the file contents

- 使用 Remark 和 Unified 處理路徑按 markdown 標題解析為部分
Use the Remark and Unified processing path to parse the markdown headings into sections

- 根據內容為每個部分創建一個 hash, 這個 hash 是部分的 ID. 我可以在將來使用它來知道部分內容有沒有變化。
Based on each section's content, create a hash, this hash is the section ID. I can use this in the future to know if a section's content has changed.

- 然後算數一個部分有多少個 tokens。這是 OpenAI 用於計費使用的文本長度度量。
Then I count how many tokens a section is. This is a measure of text length that OpenAI uses for billing usage.

- 然後把每個部分生成一個 embedding...
Then I generate an embedding for each section...

  - 使用 OpenAi 的 embedding API

  - 請注意，我們在請求之間等待 2 秒，免得請求速度過太快而發生錯誤。
  Please note, we wait 2 seconds between every request to avoid errors for making requests too many requests at once

- 在我的項目中，我把這些處理過的部分儲存到 CSV 文件中。
In my project, I take these processed sections and save them to a CSV.

## CSV 結果

在包含許多文檔的生產應用程式中，你可能希望把它們儲存在矢量數據庫中，然後我會分享一些建議的矢量數據庫。
In a production application with many documents, you would want store them in vector database and I will share links to suggestions later.

## Embeddings 是什麼？ (矢量數據? 嵌入?)

一個 embedding 是一個數字數組有很多值，每一個值的值是 -1 (負fù一) 和 1 之間. 在很多不一樣的生活的部分(很多方面)這些數字代表單詞的意思.
An embedding is an array of numbers with many values, every value is between -1 and 1. In many different areas of life, these numbers represent a word's meaning.

說得更簡單些:Embeddings 就像是一種特殊的圖畫。就像一個畫家可以使用顏料和畫筆來創造一幅畫一樣，計算機可以使用一些數字和數學公式來創建一個 embedding。 Embedding 可以被視為一個特殊的代碼，它可以把一些東西（比如單詞或圖像）轉化成數字。這些數字可以讓計算機更好地理解這些東西，並用它們來完成各種任務，比如找到與它們相關的其他東西。
To put it more simply: Embeddings are like a special kind of picture. Just like a painter can use paint and brushes to create a painting, a computer can use some numbers and mathematical formulas to create an embedding. Embedding can be thought of as a special code that converts something (such as a word or an image) into a number. These numbers allow computers to better understand these things and use them to do various tasks, such as finding other things related to them.

我有一個例子
I have an example

我們有兩個維度/方面和四個詞。 讓我們給王和女王 0.9 的王權值，因為他們絕對是王權，並給男人和女人 0.1，因為他們可能是王權，但幾乎不是，然後給王和男人一個 -0.9 的女性值，因為他們不是女性. 給女王和 女 0.9 的女性值，因為它們是女性化的。
We have two dimensions and four words. Let's assign king and queen a royalty value of 0.9 because they are definitely royalty and give man and woman of 0.1 because they may be royalty but probably not, then give king and man a -0.9 for femininity as they are not feminine, but queen and woman 0.9 as they are feminine.

這些數字幫助計算機理解這些詞之間的意思關係。 實際上，這些漢字也代表意思的關係。 這是中文的好處，但是你可以清楚地看到英文字母不代表意思的關係.如果沒有這些數字計算機無法知道意思的關係。
These number help computers to understand the semantic relationships between these words. Actually, the Chinese characters in these words also represent the similarities. That's a benefit of Chinese, but you can clearly see that the English letters do not represent the connections so the computer has no way to know the semantic relationships without these numbers.

因為我們有了數字，我們可以把王減男人加上女人，我們就會得到女王! 0.9 - 0.1 + 0.1 = 0.9 和 -0.9 - -0.9 + 0.9 = 0.9。 所以現在一個計算機可以理解了這四個詞之間的關係。
Now that we have the number, if we subtract man from king and add woman we get queen. 0.9 - 0.1 + 0.1 = 0.9 and -0.9 - -0.9 + 0.9 = 0.9. So now the computer understands the relationships between these four words.

這個例子很簡單. 我建議你閱讀這篇文章或別的-了解更多.
This example is very simple. I recommend you read this article or a different one to understand more.

在我的工具中，我可以使用 embeddings 來查找與問題的意思最相關的文檔部分。
In my tool, I use embeddings to see which documentation sections are most related to the meaning of a user's question.

## 使用 OpenAI 創建 embeddings Create embeddings using OpenAI

- 使用你的 API 密鑰初始化 OpenAIApi
Init OpenAIApi using your API key

- 把字符串發送到 OpenAI
Send the string to OpenAI

- 接收 embedding (OpenAI 的 embeddings 有 1536 個維度/方面)
Receive back the embedding (OpenAI embeddings have 1536 dimensions)

- 請注意，這些代碼示例沒有導入(dǎorù)和錯誤捕獲(bǔhuò)，讓我比較容易給大家解釋，但然後請閱讀源代碼, 它有導入和錯誤捕獲。
- Note that I've left out all imports and error catching in my code samples to make it easier to display on screen and explain, but please read the source code to find the imports and make sure to catch errors.  

## 前端：添加搜索輸入 Add a search box to the front end

我要加新的 SvelteKit 路由: `/query` 所以我創建這個文件: `routes/query/+page.svelte` 
I want to create a new SvelteKit route __ so I create this file ___.

- 加一個輸入，bind 那個輸入的值到 query 變量. 把那個 query 發送到我們的後端 API
Add an input, bind the value to the query variable and send that to our backend API

- 等待答案, 再顯示答案
Wait for the answer, then show it

## 後端：創建 SvelteKit API 端點 Create SvelteKit back-end API endpoint 

我要加那個 SvelteKit 的服務器路由: `/api/query` 所以我創建這個文件 `routes/api/query/+server.ts` 
Create a server route that will be available at `/api/query` by creating a file in `src/routes/api/query/+server.ts`

我要把使用者的問題(query)創建一個 embedding 所以我利用一樣的創建 embeddings 的方法.
Generate an embedding for the user's query using the same method you did to create embeddings for your documentation sections

加載有 embeddings 的文檔部分，和找到與問題的意思最近的那些部分。
Load in your documentation sections which have embeddings and find those sections which are closest in meaning to the user's query. 

通過計算使用者的問題與文檔的每個部分之間的 cosine 相似度來執行(zhíxíng)此操作(cāozuò)，並留 10 個左右最相似的部分。 現在我的 SvelteKit 文檔部分 embeddings 的 CSV 文件是 5MB。 目前使用這種大小的 CSV 文件是可以的，但在某些時候我需要使用支持(zhīchí)矢量(shǐliàng)的數據庫(shùjùkù)來儲存和找最接近的 embeddings。 使用 CSV 是一種很好的開始方法，不需要用矢量數據庫。
Do this by calculating the cosine similarity between your user's query and each section of your documentation and keep the 10 or so most similar. My CSV file of SvelteKit documentation section embeddings is 5MB. Using a CSV file of this size is OK for now, but at some point I'll need to use a vector capable database to store and fetch the closest embeddings. Using a CSV is a good way to get started without having to connect to a vector searching database.

然後把相關文檔部分和使用者的問題生成提示, prompt: 
Now we take those sections and the user's question to generate a prompt.
  
首先給 ChatGPT 一個角色，然後給出指令，相關文檔部分，使用者的問題，還有更多指令。最後的指令非常重要: 用問題的語言生成答案和包括相關的代碼.
First we give the chatbot a role, then we give instruction, our document_content that we concatenated, the user's question, and then a bit more instruction. The last instruction is very important: generate the answer in the language of the question and include the related code.

創建 OpenAI 聊天請求
Create an OpenAI chat request
  
根據你的條件使用 `max_tokens` 來限制。
Use a `max_tokens` limit based on your needs.

把 OpenAI 的答案發送到前端。如果你查看我的原代碼，你會發現我是在流式傳輸答案，這不是需要的，但可以提供更好的經驗。
Take OpenAI's response and send it back to the frontend. If you view my code repo, you'll see that I'm actually streaming the answer back, which is not necessary but makes for a nicer user experience.

那,怎麼保護這個 API？
How do we secure this API?

## 使用 Firebase Auth 保護 API Secure your API with Firebase Auth

創建一個 Firebase 項目  Create a Firebase Project

... skipping Firebase setup steps translation as they are so intermixed with proper names

使用者登入後，你可以在前端獲得一個 auth token，並把它和使用者的問題一起傳遞到你的後端。
After a user is logged in you can get an auth token in the frontend and pass it to your backend along with the user's question.

為確保使用者已通過身份驗證, 在 API 端點中我們需要把 auth token 解碼. 如果他未通過身份驗證，拋出錯誤。
To make sure the user is authenticated, we need to decode the auth token in the API endpoint. If they are not authenticated, throw an error.

然後我們需要編寫 decodeToken 函數。
Now we need to create that decodeToken function.

...

初始化 firebase-admin
Init firebase-admin

驗證和解碼 auth token
Verify and decode the auth token

這就是今天的代碼介紹的全部內容:
That's all for the code walkthrough today:

- 我們處理了我們的文件
- we processed our documentation

- 添加了前端搜索輸入
- added a frontend search input

- 添加了一個後端來回答問題
- added a backend to answer questions

- 最後使用 Firebase Auth 保護 API
- and secured everything with Firebase Authentication

# 未來的任務 Future tasks

現在我們的搜索成功了，但對於像這樣的項目，我們可以採取(cǎiqǔ)一些合乎(héhū)邏輯(luójí)的後續步驟(bùzhòu)。

Now we have a working tool, but there are some logical next steps we could take with a project like this.

- 把問題和答案保儲存到 Firestore, 所以使用者可以查看他們的歷史
save answers in Firestore so user can view their history

- 計數使用, 了解使用者或收取使用費 
meter usage so we can understand users, charge usage fees

- 如果你的知識庫很大並且你想快速檢索(jiǎnsuǒ)最相關的文檔部分，你可以使用 embeddings 矢量數據庫 
If your embeddings are too many or you want a faster embeddings search, you can use an embeddings search capable database

# 了解更多 Understand more

查看源代碼 View the source code

...

那, 因為從這個工具你剛剛獲得一些知識，當你有空，我希望你能花一些時間來探索(tànsuǒ)源代碼和親自試試一下。 明天我會在 TOOCON Facebook 頁面上發布這個 Slidev PPT，你可以用這些鏈接(liànjiē)查看我的項目的源代碼或閱讀這些有用的文章。

Now that you have a little more knowledge, when you have free time, I hope you will spend a little time exploring the source code and giving it a try yourself. Tomorrow I will post the powerpoint on TOOCON Facebook's site. You can use these links to check out my project's source code and read some of these useful articles.

# Questions, Try, Contact

掃描(sǎomiáo) QR code 進入我的 Facebook 社團。你會看到一個討論區，請把你的問題放在那邊(用中文)。然後我會把你的問題放在這個輸入所以我們可以一起問 SvelteKit.

Scan the QR code to enter the Facebook community page. You will see a discussion area. Please place your questions in there (use Chinese). Then I will place your questions into the input and we can ask SvelteKit together. 