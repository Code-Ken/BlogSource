title: 【数据结构与算法】第2章一一栈与队列
date: 2016-01-13 09:57:30
categories: 数据结构与算法
tags: [PHP,JavaScript,数据结构,算法]
---

<center>数据结构与算法 第2章 栈与队列</center>
<!--more-->


## 栈
### 栈的定义
>栈(stack)是限定仅在表尾进行删除和插入的线性表

把插入和删除的一端称为栈顶(top),另一端称为栈底(bottom),不含任何元素的栈称为空栈,栈又称后进先出(Last In First Out)的线性表,简称LIFO结构。

![](/sa2-1.png)
### 栈的抽象数据类型
```
ADT 栈(Stack)
Data
    同线性表。元素具有相同的类型,相邻元素具有前驱和后继的关系。
Operation
    InitStack(*S)        初始化操作,建立一个空栈
    DestoryStac(*S)      若栈存在,则销毁它
    ClearStock(*S)       将栈清空
    StackEmpty(*S)      若栈为空,返回true,否则返回false
    GetTop(S,*e)         若栈存在且非空,用e返回S的栈顶元素
    Push(S,*e)           若栈S存在,插入新元素e到栈
    Pop(*S,*e)           删除栈顶元素,并用e返回其值
    StackLength(S)       返回栈S的元素个数

endADT
```
### 实现

#### PHP语言的实现

```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/12
 * Time: 下午5:37
 */
class Stack
{
    private $_data;
    private $_end;

    public function __construct()
    {
        $this->_data = array();
        $this->_end = null;
    }

    public function InitStack()
    {
        $this->__construct();
    }

    public function ClearStock()
    {
        $this->__construct();
    }

    public function StackEmpty()
    {
        if (null === $this->_end) {
            return false;
        }
        return true;
    }

    public function Push($data)
    {
        if (null === $this->_end) {
            $this->_end = 0;
        } else {
            $this->_end++;
        }
        $this->_data[$this->_end] = $data;
    }

    public function Pop()
    {
        if (null === $this->_end) {
            return false;
        }
        $re = $this->_data[$this->_end];
        unset($this->_data[$this->_end]);
        0 === $this->_end ? $this->_end = null : $this->_end--;
        return $re;
    }

    public function GetTop()
    {
        if (null === $this->_end) {
            return false;
        }
        return end($this->_data);
    }


    public function StackLength()
    {
        return count($this->_data);
    }


    public function PrintStack()
    {
        return $this->_data;
    }
}

```

## 队列
### 队列的定义
>队列是只允许在一段进行插入操作,而在另一端进行删除操作的线性表

队列又称先进先出(First In First Out)的线性表,简称FIFO结构。

![](/sa2-2.png)

### 队列的抽象数据结构
```
ADT 队列(Queue)
Data
    同线性表。元素具有相同的类型,相邻元素具有前驱和后继的关系。
Operation
    InitQueue(*Q)        初始化操作,建立一个空队列Q
    DestoryQueue(*Q)       若队列Q存在,则销毁它
    ClearQueue(*Q)         将队列Q清空
    QueueEmpty(*Q)         若队列为空,返回true,否则返回false
    GetHand(*Q)            若队列Q存在且非空,用e返回队列Q的对头元素
    EnQueue(*Q,e)          若队列Q存在,将e插至队尾
    DeQueue(*Q,*e)         删除Q中的头元素,并用e返回其值
    QueueLength(Q)         返回队列Q的元素个数

endADT
```
### 实现
#### 普通队列的实现
##### PHP语言的实现

```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/12
 * Time: 下午6:00
 */
class Queue
{
    private $_data;

    public function __construct()
    {
        $this->_data = array();
    }

    public function InitQueue()
    {
        $this->__construct();
    }

    public function DestoryQueue()
    {
        unset($this->_data);
    }

    public function ClearQueue()
    {
        $this->__construct();
    }

    public function QueueEmpty()
    {
        return count($this->_data) == 0;
    }

    public function GetHead()
    {
        return $this->_data[0];
    }

    public function EnQueue($element)
    {
        array_push($this->_data, $element);
    }

    public function DeQueue()
    {
        return array_shift($this->_data);
    }

    public function QueueLength()
    {
        return count($this->_data);
    }

    public function PrintQueue()
    {
        return $this->_data;
    }
}
```

#### 双向队列的实现
##### PHP语言的实现
```
<?php

/**
 * Created by PhpStorm.
 * User: Ken
 * Date: 16/1/13
 * Time: 上午9:13
 */
class Deque
{

    private $_queue;

    public function __construct($dequeue = array())
    {
        if (is_array($dequeue)) {
            $this->_queue = $dequeue;
        }
    }

    public function GetFront()
    {
        return reset($this->_queue);
    }

    public function GetEnd()
    {
        return end($this->_queue);
    }

    public function DequEmpty()
    {
        return empty($this->_queue);
    }

    public function DequeLength()
    {
        return count($this->_queue);
    }

    public function PushEnd($elem)
    {
        array_push($this->_queue, $elem);
    }

    public function PushFront($elem)
    {
        array_unshift($this->_queue, $elem);
    }

    public function PopEnd()
    {
        return array_pop($this->_queue);
    }

    public function PopFront()
    {
        return array_shift($this->_queue);
    }

    public function ClearDeque()
    {
        $this->_queue = array();
    }

    public function PrintDeque()
    {
        return $this->_queue;
    }
}
```



## 附录
高级语言中实现队列和栈的常见函数
各语言实现队列的方法

![](/sa2-3.jpg)

