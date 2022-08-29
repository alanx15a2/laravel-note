# Data Structures

## Vector

最有效率的有序資料結構  

* 記憶體使用量小，不會指數成長
* get, set, push, pop 都是 O(1)
* insert, remove, shift, unshift 都是 O(n) *還是比原生 array 快*

## Deque

念作 deck，double-end queue 的縮寫  

* get, set, push, pop, shift, unshift 都是 O(1)
* insert, remove 都是 O(n)
* 記憶體指數成長 *還是小於原生 array*

## Map

有序 & key-value 的集合  
比起陣列，key 可以是任何型態 (包含物件)  

* 如果 key 是物件則不能轉換為 array

## Set

有序並內容是獨一無二的  

* add, remove, contains are O(1)
* get 視情況可能是 O(n)

## Stack

後進先出資料結構  
只允許從最後的資料開始迭代  

## Queue

先進先出資料結構  
只允許從最前的資料開始迭代  

## PriorityQueue

帶有優先度的 Queue
優先度越高的資料會在越前方

## 方法

### get

取得指定位置資料

### set

更新指定位置資料

### remove

移除指定位置資料

### shift

取得最前&移除該筆資料

### unshift

在最前塞入資料

### push

在最後塞入資料

### pop

取得最後&移除該筆資料

