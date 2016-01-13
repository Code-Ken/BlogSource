title: 【数据结构与算法】第1章一一线性表
date: 2016-01-13 09:35:38
categories: 数据结构与算法
tags: [PHP,JavaScript,数据结构,算法]
---

<center>数据结构与算法 第1章 线性表</center>
<!--more-->


<center>

</center>
![](/sa1-1.png)
## 线性表定义
>线性表(List):零个或多个数据元素的有限序列

## 线性表的抽象数据类型
```
ADT 线性表(List)
Data
    线性表的数据对象集合为{a1,a2,.....,an},每个元素的类型均为DataType。
    其中除了a1外每个元素都有一个直接前驱元素，除了an外每个元素都有一个直接后继元素。
    数据元素之间是一对一的关系。
Operation
    InitList(*L):            初始化操作，建立一个空的线性表L
    ListEmpty(L):            判断线性表是否为空
    ClearList(*L):           将线性表清空
    GetElem(L,i,*e):         将线性表L中第i个元素返回给e
    LocateElem(L,e):         在线性表中查找与给定值e相等的元素,如果查找成功,返回该元素在表中的序号,否则返回0表示失败
    ListInsert(*L,i,e):      在线性表中的第i个位置插入新元素e
    ListDelete(*L,i,*e):     删除线性表第i个位置的元素，并将其值返回给e
    ListLength(L):           返回线性表L的元素个数
endADT 
```

## 实现
### 线性表的顺序存储结构的实现
#### PHP语言的实现

```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/8
 * Time: 下午2:42
 */
class SeqStoreList
{
    private $_seq_arr;

    private $_length;


    public function __construct($arr = array())
    {
        $this->_seq_arr = $arr;
        $this->_length = count($arr);
    }


    public function InitList()
    {
        $this->_seq_arr = array();
        $this->_length = 0;
    }


    public function ListEmpty()
    {
        return $this->_length === 0;
    }


    public function ClearList()
    {
        $this->_seq_arr = array();
        $this->_length = 0;
    }


    public function GetElem($index)
    {
        if ($index > $this->_length || $index < 0 || $this->_length == 0 || !is_int($index)) return "err";
        return $this->_seq_arr[$index - 1];
    }


    public function LocateElem($elem)
    {
        for ($i = 0; $i < $this->_length; $i++) {
            if ($elem == $this->_seq_arr[$i]) {
                return $i + 1;
            }
        }
        return 0;
    }


    public function ListInsert($index, $elem)
    {
        if ($index < 1 || $index > $this->_length + 1 || !is_int($index)) return "err";
        for ($i = $this->_length - 1; $i >= $index - 1; $i--) {
            $this->_seq_arr[$i + 1] = $this->_seq_arr[$i];
        }
        $this->_seq_arr[$index - 1] = $elem;
        $this->_length++;
        return "ok";
    }


    public function ListDelete($index)
    {
        if ($index < 1 || $this->_length == 0 || !is_int($index) || $index > $this->_length) return "err";
        $elem = $this->_seq_arr[$index - 1];
        for ($i = $index; $i < $this->_length; $i++) {
            $this->_seq_arr[$i - 1] = $this->_seq_arr[$i];
        }
        unset($this->_seq_arr[$this->_length - 1]);
        $this->_length--;
        return $elem;
    }


    public function ListLength()
    {
        return $this->_length;
    }

    public function ListPrint()
    {
        return $this->_seq_arr;
    }

}

```

### 线性表的单链表存储结构的实现
#### PHP语言的实现

```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/8
 * Time: 下午4:33
 */
class SingleLinkedList
{
    private $_headNode;

    public function __construct()
    {
        $this->_headNode = new Node();
    }

    public function InitList()
    {
        $this->__construct();
    }

    public function ClearList()
    {
        unset($this->_headNode);
        $this->__construct();
    }

    public function GetElem($i)
    {
        $j = 1;
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }

        if (!$node || $j > $i) { //$i 个元素不存在时
            return "err";
        }

        $data = $node->getData(); //取第$i个元素的数据
        return $data;
    }

    public function LocateElem($elem)
    {
        $j = 1;
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        while ($node) {
            if ($elem == $node->getData()) return $j;
            $node = $node->getNext();
            $j++;
        }

        return -1;
    }

    public function ListInsert($i, $data)
    {
        //后插
        $j = 1; //从第一个元素开始遍历
        $node = $this->_headNode; // 指向头结点

        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }

        if (!$node || $j > $i) { // $i个结点不存在时
            return "err";
        }

        $newNode = new Node();
        $newNode->setData($data);
        $newNode->setNext($node->getNext());
        $node->setNext($newNode);
        return true;
    }

    public function ListDelete($i)
    {
        $j = 1;
        $node = $this->_headNode; // 指向头结点
        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }
        if (!$node || $j > $i) {
            return "err";
        }
        $delNode = $node->getNext();        // 需要删除的结点
        $node->setNext($delNode->getNext());// 删除结点的后继结点赋值给结点
        unset($delNode);                    // 清除不需要的结点
        return true;
    }

    public function ListLength()
    {
        $j = 0;
        $headNode = $this->_headNode;
        if (!$headNode->getNext()) return 0;
        $node = $headNode->getNext();
        while (!$node) {
            $j++;
            $node = $node->getNext();
        }
        return $j;
    }

    public function ListPrint()
    {
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        $str = "";
        while ($node) {
            $str = $str.$node->getData();
            if ($node->getNext()) {
                $str = $str." => ";
            }
            $node = $node->getNext();
        }
        return $str;
    }
}

/**
 *线性表的单链表存储结构
 */
class Node
{
    private $data = ""; //存储数据
    private $next = null; //存储下一个结点的指针

    public function getData()
    {
        return $this->data;
    }

    public function getNext()
    {
        return $this->next;
    }

    public function setData($data)
    {
        $this->data = $data;
    }

    public function setNext($next)
    {
        $this->next = $next;
    }
}

```
### 线性表的双链表存储结构的实现
#### PHP语言的实现


```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/12
 * Time: 下午1:16
 */
class DoubleLinkedList
{
    private $_headNode;

    public function __construct()
    {
        $this->_headNode = new dNode();
    }

    public function InitList()
    {
        $this->__construct();
    }

    public function ClearList()
    {
        unset($this->_headNode);
        $this->__construct();
    }

    public function GetElem($i)
    {
        //与单链表相同
        $j = 1;
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }

        if (!$node || $j > $i) { //$i 个元素不存在时
            return "err";
        }

        $data = $node->getData(); //取第$i个元素的数据
        return $data;
    }

    public function LocateElem($elem)
    {
        //与单链表相同
        $j = 1;
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        while ($node) {
            if ($elem == $node->getData()) return $j;
            $node = $node->getNext();
            $j++;
        }
        return -1;
    }

    public function ListInsert($i, $data)
    {
        //后插
        $j = 1; //从第一个元素开始遍历
        $node = $this->_headNode; // 指向头结点

        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }

        if (!$node || $j > $i) { // $i个结点不存在时
            return "err";
        }

        $newNode = new dNode();
        $nextNode = $node->getNext();
        $newNode->setData($data);
        $newNode->setNext($node->getNext());
        $newNode->setPre($node);
        $node->setNext($newNode);
        if ($nextNode) $nextNode->setPre($newNode);
        return true;
    }

    public function ListDelete($i)
    {
        $j = 1;
        $node = $this->_headNode; // 指向头结点
        while ($node && $j < $i) {
            $node = $node->getNext();
            $j++;
        }
        if (!$node || $j > $i) {
            return "err";
        }
        $delNode = $node->getNext();        // 需要删除的结点
        $nextNode = $delNode->getNext();
        $node->setNext($nextNode);
        $nextNode->setPre($node);
        unset($delNode);                    // 清除不需要的结点
        return true;
    }

    public function ListLength()
    {
        $j = 0;
        $headNode = $this->_headNode;
        if (!$headNode->getNext()) return 0;
        $node = $headNode->getNext();
        while ($node) {
            $j++;
            $node = $node->getNext();
        }
        return $j;
    }

    public function ListPrint()
    {
        $headNode = $this->_headNode;
        $node = $headNode->getNext();
        $str = "";
        while ($node) {
            $str = $str . $node->getData();
            if ($node->getNext()) {
                $str = $str . " <=> ";
            }
            $node = $node->getNext();
        }
        return $str;
    }
}

class dNode
{
    private $_data = ""; //存储数据
    private $_pre = null;//储存上一个结点的指针
    private $_next = null; //存储下一个结点的指针

    public function getData()
    {
        return $this->_data;
    }

    public function getNext()
    {
        return $this->_next;
    }

    public function getPre()
    {
        return $this->_pre;
    }

    public function setData($data)
    {
        $this->_data = $data;
    }

    public function setNext($next)
    {
        $this->_next = $next;
    }

    public function setPre($pre)
    {
        $this->_pre = $pre;
    }
}


```
