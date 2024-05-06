Index:
[toc]

# 什麼是Linked List
Linked list中文譯為鏈結陣列，一種**基礎的**資料結構
線性(linear)的資料儲存位置並不連續，而是透過指標來指定下筆資料的位置，示意圖如下：
![](Pictures/Single-Linked-List-Data-Structure.png)

## Linked list結構
整個Linked list的結構分成如下:
- Node:
  每個Node(節點)大致分為以下2點:
  - Data:
    每個節點對應一筆資料
  - Next Point:
    下個節點的記憶體位址
- Head and Tail:
  一個Linked list的存取起始點由Head所定義，指向第一個節點。一個Linked list的尾部會指向NULL或者空的指標，這便是Tail。

## 優點
- **動態大小**
  在執行時可靈活修改串列大小
- **可從中插入與刪除**
  可以根據需要再任意地方插入或者刪除資料
- **靈活性**
  除了可在串列中任意增加資料外，記憶體位置不需連續

也因為上述優點，另Linked list有著以下特性:
- 動態資料結構
- 共容易的資料插入與刪除
- 更有效的記憶體利用
- 可以實現更高階的資料結構(e.g. stack, queue etc.)

## 缺點
也因為特性，Linked list有以下缺點：
- **Random Access**
  Linked list內的元素不能直接指定並訪問，需要遍歷(traversal)各個節點直到目的
- **消耗更多的記憶體**
  單筆資料需要使用相比陣列(array)多的記憶體用於指向下一筆（甚至是前一筆）的位址

## Linked list類型
1. 單向鏈結陣列 Single-linked list
   s
  ![Single-linked list](Pictures/Single-Linked-List-Data-Structure.png)

2. 雙向鏈結陣列 Double-linked list
   
3. 環形鏈結陣列 Circular linked list


# Reference
[geeks for geeks](https://www.geeksforgeeks.org/data-structures/linked-list/?ref=shm&fbclid=IwAR10MeYhNPXig4CK-ySB0NwpdYPQ4s1I53re-Zxx8WXLMEmFhIJdqp-A0Ow)
