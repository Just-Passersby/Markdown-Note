Markdown語法
===

- [Markdown語法](#markdown語法)
- [概述](#概述)
  - [理念](#理念)
  - [內崁HTML](#內崁html)
- [區塊元素](#區塊元素)
  - [段落和換行符號](#段落和換行符號)
  - [標頭](#標頭)
  - [區塊引用](#區塊引用)
  - [清單](#清單)
  - [程式碼區塊](#程式碼區塊)
  - [橫線](#橫線)
- [橫跨元素](#橫跨元素)
  - [連結](#連結)
  - [強調](#強調)
  - [代碼](#代碼)
  - [圖片](#圖片)
- [其餘各種](#其餘各種)
  - [反斜線轉譯](#反斜線轉譯)
  - [自動連結](#自動連結)
- [Reference:](#reference)

---
# 概述
## 理念
Markdown目標是盡可能的易於可讀與可寫

然而，可讀性才是最重要的。Markdown格式的文檔應該可以按照原樣以純文本的形式發布，而不會看起來像是標記或格式化指令。雖然Markdown的語法受到現有的多種HTML過濾器的引響，包括Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，但Markdown語法最大的靈感來源是純文本電子郵件的格式。

為此，Markdown語法完全是由標點符號組成，這些標點符號都是精心選擇，以便看起來像他們的意思一樣。例如: 用星號圍繞一個詞看起來像**強調**，Markdown列表看起來像...額...，列表。即使是區塊引用也像是引用的文字段落，假使你曾經用過電子郵件。

## 內崁HTML
Markdown的語法為了一個目的: 用於在網路上**寫作**的格式。

Markdown不是HTML的替代品，或者類似它。這個語法非常小，只有對應非常小的HTML標籤子集。這個想法**不是**創建一個語法，以便更容易的插入HTML標籤。在作者看來，HTML標籤已經很容易插入了。Markdown的想法是讓閱讀、寫作和編輯散文變得更容易。HTML是一種*發布*格式; Makrdown是種寫作格式。因此，Markdown的格式語法只能解決可以用純文字解決的問題。

對於Markdown語法未涵蓋的任何標記，你只需使用HTML本身即可。沒有必要加入前墜或界線表明正在從Markdown切換成HTML; 你只需要使用標記。

唯一的限制只有區塊分級(block-level)HTML元素——例如`<div>`、`<table>`、`<pre>`、`<p>`等。——必須用空行與周圍符號分開，並且區塊的開始與結束標記不應使用Tab或空白鍵。Markdown足夠聰明，不需要加額外的(不需要的)`<p>`HTML區塊分級標籤。

例如，要將HTML表格加入到Markdown文章:

    This is a regular paragraph.

    <table>
        <tr>
            <td>Foo</td>
        </tr>
    </table>
    This is another regular paragraph.

請注意，Markdown格式語法不會在區塊分級HTML標間中處理。例如你不能在HTML區塊中使用Markdwon樣式`*強調*`

跨級(Span-level)HTML標籤——例如: `<span>`、`<cite>`或`<del>`等可以在Markdown的任何段落、清單項目或標題中使用。如果你想，你甚至可以使用HTML標籤而不是Markdown格式 ; 比如你更偏好使用HTML的`<a>`或`<img>`標籤而不是Markdown的link或image語法，去做吧!!

有別於區塊分級HTML標籤，Markdown語法**是**在跨級標籤中處理。

##特殊符號自動轉譯
在HTML中，有2個特殊符號要處理: `<`和`&`。小於符號/左角括號(Left angle brackets)用於起始標籤 ; and符號(ampersands)用於表示HTML實體。若你打算將他們作為文字字符，就必須轉譯為實體，例如`&lt;`和`&amp;`。

&符號對於網路作者來說特別頭痛，比如說你想寫"AT&T"，你需要寫成`AT&amp;T`，甚至在URL中，你需要轉譯才能使用&符號。因此，假如你想連接以下URL:
```
http://images.google.com/images?num=30&q=larry+bird
```
你需要將URL編碼為:
```
http://images.google.com/images?num=30&amp;q=larry+bird
```
在你的錨點(anchor)標記`href`中。不用說，這很容易被忽視，並且可能是在其他方面標記良好的網站中出現HTML驗證錯誤的最常見原因。

Markdown允許你很自然的使用這些字符，並替你處理任何必要的轉譯。如果你使用&符號作為HTML實體的一部分，將維持不變; 不然都會將其轉譯成`&amp;`。

所以，如果你想在你的文章中引入版權符號，你可以這麼寫:
```
&copy
```
然後Markdown會無視它。但若你這樣寫:
```
AT&T
```
Markdown將會轉譯成:
```
AT&amp;T
```
同樣，因為Markodwn支援內崁HTML，如果你使用小於符號作為HTML分隔符號，Markdown將同樣視為HTML標籤。但如果你這樣寫:
```
4 < 5
```
Markdown將會轉譯成:
```
4 &lt; 5
```
但是，在Markodwn代碼區塊，小於符號和&符號是**隨時**自動編碼的。這使得在Markdown中更容易的編寫HTML語法。(與原始的HTML不同，原始的HTML對於撰寫HTML語法是一種糟糕的格式，因為你的代碼中的每個`<`和`&`都需要進行轉義。)

* * *

# 區塊元素

## 段落和換行符號
一個段落只是簡單一個或是連續多行的文字，由一個或多個空行分開。(一個空行是任何看起來很空的行——一行甚麼都沒有至多只包含空白鍵(space)或TAB的行稱為空白行。)正常段落不應該使用空白鍵或是TAB進行縮排。

"一個或連續多行的文字"規則的意思是Markdown支持"[硬包裝(Hard-wrapped)](https://stackoverflow.com/questions/319925/difference-between-hard-wrap-and-soft-wrap)"文本段落。這與大多數其他的text轉HTML的轉譯器(包括可動類型的"轉換跳行"選項)有著顯著的不同，後者會將每個跳行符號轉為`<br />`標籤。

當你使用Markdown時想插入`<br />`跳行標籤，你可以用2個以上的空白鍵來結束一行，然後鍵入return。



## 標頭

## 區塊引用

## 清單

## 程式碼區塊

## 橫線

---

# 橫跨元素

## 連結

## 強調

## 代碼

## 圖片

---

# 其餘各種

## 反斜線轉譯

## 自動連結

---

# Reference:
 - [(Official) Darong Ball - Markdown:Syntax](https://daringfireball.net/projects/markdown/syntax)
 - [Markdown Guide: Basic Syntax](https://www.markdownguide.org/basic-syntax/)
 - [Markdown How to delete line](https://webapps.stackexchange.com/questions/14986/strikethrough-with-github-markdown)