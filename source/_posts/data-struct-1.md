title: 数据结构第1章--线性表之顺序存储结构
date: 2017-11-27 19:26:53
categories: 数据结构
tags: [数据结构]
---

<center>![](liner.png)</center>

# 线性表定义
>线性表(List):零个或多个数据元素的有限序列

<!--more-->

# 线性表的抽象数据类型
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

# 实现
## C语言实现
```
/**
 * Copyright:   MIT
 * File name:   seqlist.c
 * Author:      Ken(gaosong0301@foxmail.com)
 * Description: 线性表的顺序存储结构
 * Others:      C语言版本
 * Function List:
 *      InitList        初始化操作,建立一个空的线性表
 *      ListEmpty       判断列表是否为空
 *      ClearList       将线性表清空
 *      GetElem         获取线性表中的元素
 *      LocateElem      查找线性表中的元素
 *      ListInsert      向线性表中插入元素
 *      ListDelete      删除线性表元素
 *      ListLength      返回线性表元素个数
 */

#include <stdio.h>
#include <stdlib.h>

//初始化表元素的大小
#define LIST_INIT_SIZE 100
//每次增长的大小
#define LIST_INCR 10

// 元素的类型别名  以int类型做举例
typedef int ElemType;
//线性表结构体
typedef struct {
    ElemType *elem;  //元素数组指针
    int length;   //元素长度
    int list_size;  //表长度
} SeqList;


/**
 * Function Name: InitList
 * Purpose: 初始化线性表
 * Params:
 *      @SeqList    *L     线性表实例
 * Return:  int     是否初始化成功
 * Limitation: 返回-1则代表失败
 */
int InitList(SeqList *L) {
    L->elem = (ElemType *) malloc(LIST_INIT_SIZE * sizeof(ElemType));
    if (!L->elem) {
        return -1;
    }

    L->length = 0;
    L->list_size = LIST_INIT_SIZE;
    return 0;
}

/**
 * Function Name: ListEmpty
 * Purpose: 判断线性表是否为空
 * Params:
 *      @SeqList    L     线性表实例
 * Return:  int     是否初始化成功
 * Limitation: 返回-1则代表不为空
 */
int ListEmpty(SeqList L) {
    if (L.length == 0) {
        return 0;
    }
    return -1;
}

/**
 * Function Name: ClearList
 * Purpose: 将线性表清空
 * Params:
 *      @SeqList    *L     线性表实例
 * Return:  int     是否初成功
 * Limitation: 返回-1则代表失败
 */
int ClearList(SeqList *L) {
    if (L->elem) {
        free(L->elem);
    }
    L->length = 0;
    L->list_size = 0;
    return 0;
}


/**
 * Function Name: GetElem
 * Purpose: 获取线性表第i位的元素,并将它赋值给e
 * Params:
 *      @SeqList    L     线性表实例
 *      @int        i     元素位数
 *      @ELemType  *e    元素地址
 * Return:  int     是否初成功
 * Limitation: 返回-1则代表失败
 */
int GetElem(SeqList L, int i, ElemType *e) {
    if (i < 0 || i >= L.length) return -1;
    *e = L.elem[i];
    return 0;
}

/**
 * Function Name: LocateElem
 * Purpose: 获取元素x在线性表中的位置
 * Params:
 *      @SeqList    L     线性表实例
 *      @ELemType   *e    元素地址
 * Return:  int     元素x的位置
 * Limitation: 返回-1则代表元素x不存在
 */
int LocateElem(SeqList L, ElemType x) {
    int pos = -1;
    for (int i = 0; i < L.length; i++) {
        if (L.elem[i] == x) {
            pos = i;
        }
    }
    return pos;
}

/**
 * Function Name: ListInsert
 * Purpose: 向线性表中的第i位插入元素
 * Params:
 *      @SeqList    L     线性表实例
 *      @int        i     插入位置
 *      @ELemType   e     插入元素
 * Return:  int     是否成功
 * Limitation: 返回-1则代表插入失败
 */
int ListInsert(SeqList *L, int i, ElemType e) {
    if (i < 0 || i > L->length) return -1;

    if (L->length >= L->list_size) {
        ElemType *newBase = (ElemType *) realloc(L->elem, (L->list_size + LIST_INCR) * sizeof(ElemType));
        if (!newBase) return -1;
        L->elem = newBase;
        L->list_size += LIST_INCR;
    }
    ElemType *q, *p;
    q = &(L->elem[i]);
    for (p = &(L->elem[L->length - 1]); p >= q; p--) {
        *(p + 1) = *p;
    }
    *q = e;
    ++L->length;
    return 0;
}

/**
 * Function Name: ListDelete
 * Purpose: 向线性表中的第i位删除元素
 * Params:
 *      @SeqList    L     线性表实例
 *      @int        i     删除位置
 *      @ELemType   *e    删除元素
 * Return:  int     是否成功
 * Limitation: 返回-1则代表删除失败
 */
int ListDelete(SeqList *L, int i, ElemType *e) {
    if (i < 0 || i > L->length) return -1;
    ElemType *q, *p;
    *e = L->elem[i];
    q = &(L->elem[i]);
    for (p = q; p < &(L->elem[L->length - 1]); p++) {
        *p = *(p + 1);
    }
    L->length--;
    return 0;
}

/**
 * Function Name: ListLength
 * Purpose: 获取线性表的长度
 * Params:
 *      @SeqList    L     线性表实例
 * Return:  int     返回线性表长度
 */
int ListLength(SeqList L) {
    return L.length;
}

int main() {
    SeqList L;
    InitList(&L);
    printf("List is Empty? %d\n", ListEmpty(L));
    for (int i = 0; i < 10; i++) {
        ListInsert(&L, i, i);
    }

    for (int j = 0; j < 12; j++) {
        printf("elem[%d] = %d \n", j, L.elem[j]);
    }
    printf("Length:%d\n", ListLength(L));
    ElemType a;
    ListDelete(&L, 1, &a);
    for (int j = 0; j < 12; j++) {
        printf("elem[%d] = %d %p\n", j, L.elem[j], &L.elem[j]);
    }
    printf("Length:%d\n", ListLength(L));
    printf("Delete e: %d\n", a);
    ElemType b;
    GetElem(L, 0, &b);
    printf("get Elem: %d\n", b);
    printf("Location Elem %d\n", LocateElem(L, 2));
    printf("List is Empty? %d\n", ListEmpty(L));
    printf("Addr: %p\n",&L.elem);
    printf("ClearList: %d\n",ClearList(&L));
    printf("Addr: %p\n",*(&L.elem));
    printf("elem[0] = %d %p\n", L.elem[0], &L.elem[0]);
    printf("elem[1] = %d %p\n", L.elem[1], &L.elem[1]);
    printf("elem[2] = %d %p\n", L.elem[2], &L.elem[2]);
    return 0;
}
```

## PHP实现

```
<?php

/**
 * 线性表的顺序存储结构
 * @copyright   MIT.
 * @author      Ken(gaosong0301@foxmail.com)
 */
class SeqList
{
    /**
     * 数组
     */
    private $_seq_arr;
    /**
     * 数组长度
     */
    private $_length;

    /**
     * constructor.
     * @param array $arr
     */
    public function __construct($arr = array())
    {
        $this->_seq_arr = $arr;
        $this->_length = count($arr);
    }

    /**
     * 初始化线性表
     */
    public function InitList()
    {
        $this->_seq_arr = array();
        $this->_length = 0;
    }

    /**
     * 判断线性表是否为空
     * @return bool
     */
    public function ListEmpty()
    {
        return $this->_length === 0;
    }

    /**
     * 将线性表清空
     */
    public function ClearList()
    {
        $this->_seq_arr = array();
        $this->_length = 0;
    }

    /**
     * 获取线性表第i位的元素
     * @param $index
     * @return mixed|string
     */
    public function GetElem($index)
    {
        if ($index < 0 || $index >= $this->_length) return "err";
        return $this->_seq_arr[$index];
    }

    /**
     * 获取元素elem在线性表中的位置
     * @param $elem
     * @return int
     */
    public function LocateElem($elem)
    {
        for ($i = 0; $i < $this->_length; $i++) {
            if ($elem == $this->_seq_arr[$i]) {
                return $i;
            }
        }
        return -1;
    }

    /**
     * 向线性表中的第i位插入元素
     * @param $index
     * @param $elem
     * @return int
     */
    public function ListInsert($index, $elem)
    {
        if ($index < 0 || $index > $this->_length) return -1;
        if ($index == $this->_length) {
            $this->_seq_arr[$index] = $elem;
            $this->_length++;
            return 0;
        }
        for ($i = $this->_length - 1; $i >= $index; $i--) {
            $this->_seq_arr[$i + 1] = $this->_seq_arr[$i];
        }
        $this->_seq_arr[$index] = $elem;
        $this->_length++;
        return 0;
    }

    /**
     * 向线性表中的第i位删除元素
     * @param $index
     * @return mixed|string
     */
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

    /**
     * 获取线性表的长度
     * @return int
     */
    public function ListLength()
    {
        return $this->_length;
    }

    /**
     * 打印
     * @return array
     */
    public function ListPrint()
    {
        return $this->_seq_arr;
    }
}

$l = new SeqList();
$l->InitList();
for ($i = 0; $i < 10; $i++) {
    $l->ListInsert($i, $i + 2);
}
var_dump($l->ListPrint());
$l->ListInsert(0, 33);
$l->ListInsert(3, 55);
var_dump($l->ListPrint());
var_dump("elem=3 location=" . $l->LocateElem(3));
var_dump("location=3 elem=" . $l->GetElem(3));
var_dump("length = " . $l->ListLength());
$l->ListDelete(2);
var_dump($l->ListPrint());
echo "length = " . $l->ListLength() . "\n";
var_dump($l->ListEmpty());
$l->ClearList();
var_dump($l->ListEmpty());
```

## JS实现

```
/**
 * 线性表的顺序存储结构
 * @copyright   MIT.
 * @author      Ken(gaosong0301@foxmail.com)
 */
class SeqList {
    /**
     * 构造函数
     */
    constructor() {
        this.seqArr = [];
        this.length = null;
    }

    /**
     * 初始化线性表
     * @param {object} arr
     */
    InitList(arr = []) {
        this.seqArr = arr;
        this.length = 0;
    }

    /**
     * 判断线性表是否为空
     * @return {boolean}
     */
    ListEmpty() {
        return this.length === 0;
    }

    /**
     * 将线性表清空
     */
    ClearList() {
        this.seqArr = [];
        this.length = 0;
    }

    /**
     * 获取线性表第i位的元素
     * @param {int} index
     * @return {string}
     */
    GetElem(index) {
        if (index < 0 || index >= this.length) return -1;
        return this.seqArr[index];
    }

    /**
     * 获取元素elem在线性表中的位置
     * @param {int} elem
     * @return {int}
     */
    LocateElem(elem) {
        for (let i = 0; i < this.length; i++) {
            if (elem === this.seqArr[i]) {
                return i;
            }
        }
        return -1;
    }

    /**
     * 向线性表中的第i位插入元素
     * @param {int} index
     * @param {string} elem
     * @return {int}
     */
    ListInsert(index, elem) {
        if (index < 0 || index > this.length) return -1;
        if (index === this.length) {
            this.seqArr[index] = elem;
            this.length++;
            return 0;
        }
        for (let i = this.length - 1; i >= index; i--) {
            this.seqArr[i + 1] = this.seqArr[i];
        }
        this.seqArr[index] = elem;
        this.length++;
        return 0;
    }

    /**
     * 向线性表中的第i位删除元素
     * @param {int} index
     * @return {string|int}
     */
    ListDelete(index) {
        if (index < 0 || index >= this.length) return -1;
        let elem = this.seqArr[index];
        for (let i = index; i < this.length; i++) {
            this.seqArr[i - 1] = this.seqArr[i];
        }
        this.seqArr.pop();
        return elem;
    }

    /**
     * 获取线性表的长度
     * @return int
     */
    ListLength() {
        return this.length;
    }

    /**
     * 打印
     * @return {Object}
     */
    ListPrint() {
        return this.seqArr;
    }
}

let arr = new SeqList();
arr.InitList();
for (let i = 0; i < 10; i++) {
    arr.ListInsert(i, i + 2);
}
console.log(arr.ListPrint());
arr.ListInsert(0, 33);
arr.ListInsert(3, 55);
console.log(arr.ListPrint());
console.log(`elem=3 location=${arr.LocateElem(3)}`);
console.log(`location=3 elem=${arr.GetElem(3)}`);
arr.ListDelete(2);
console.log(arr.ListPrint());
console.log(`length = ${arr.ListLength()}`);
console.log(arr.ListEmpty());
console.log(arr.ClearList());
console.log(arr.ListEmpty());

```
