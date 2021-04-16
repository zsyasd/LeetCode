## 题目

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

### 示例1

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```



### 示例2

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```



## 思路

* 栈：后进先出
* 队列：先进先出

所以很显然，栈无法实现队列功能。栈底元素（对应队首元素）无法直接删除，需要将上方所有元素出栈。同时两个栈可以实现元素倒叙。因为123进来就需要321出去，那另一个栈就是321进来了。

所以这样子就可以实现删除栈底元素。

```java
class CQueue {
    LinkedList<Integer> A, B;
    public CQueue() {
        A = new LinkedList<Integer>();
        B = new LinkedList<Integer>();
    }
    
    public void appendTail(int value) {
        A.addLast(value);
    }
    
    public int deleteHead() {
        if(!B.isEmpty()) {
            if(!B.isEmpty()) return B.removeLast();
        }
        if(A.isEmpty()) {
            return -1;
        }
        while(!A.isEmpty()) {
            B.addLast(A.removeLast());
        }
        return B.removeLast();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```







 
