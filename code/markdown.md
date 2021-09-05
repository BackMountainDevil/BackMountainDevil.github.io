<details>
<summary>Title</summary>

必须空出一行哈content!!!
```java
System.out.println("Hello to see U!");
```
</details>

<details>
<summary>展开查看，会发现有一行空的</summary>
<pre><code>
System.out.println("Hello to see U!");
print("hello world")
</code></pre>
</details>


甘特图

```mermaid
gantt
        dateFormat  YYYY-MM-DD
        title Adding GANTT diagram functionality to mermaid
        section 现有任务
        已完成               :done,    des1, 2014-01-06,2014-01-08
        进行中               :active,  des2, 2014-01-09, 3d
        计划中               :         des3, after des2, 5d
```

UML 图

```mermaid
sequenceDiagram
张三 ->> 李四: 你好！李四, 最近怎么样?
李四-->>王五: 你最近怎么样，王五？
李四--x 张三: 我很好，谢谢!
李四-x 王五: 我很好，谢谢!
Note right of 王五: 李四想了很长时间, 文字太长了<br/>不适合放在一行.

李四-->>张三: 打量着王五...
张三->>王五: 很好... 王五, 你怎么样?
```


mermaid 流程图

```mermaid
graph TB
A[长方形] -- 链接 --> B((圆))
E((E))
A --> C(圆角长方形)
B --> D{菱形}
C --> D
D --> E
```

flowchart流程图

```mermaid
flowchat
st=>start: 开始
e=>end: 结束
op=>operation: 我的操作
cond=>condition: 确认？

st->op->cond
cond(yes)->e
cond(no)->op
```


类图

```mermaid
classDiagram
    Class01 <|-- AveryLongClass : Cool
    <<interface>> Class01
    Class09 --> C2 : Where am i?
    Class09 --* C3
    Class09 --|> Class07
    Class07 : equals()
    Class07 : Object[] elementData
    Class01 : size()
    Class01 : int chimp
    Class01 : int gorilla
    class Class10 {
        >>service>>
        int id
        size()
    }
```


注脚

一个具有注脚的文本。[^1]

一个具有注脚的文本。[^2]

(fail)这是第3个有注脚的文本。^[注脚内容 第3条]


这是第4个有注脚的文本。[^4]

[^1]: 注脚的解释
[^2]: 注脚的解释
[^4]: https://mermaid-js.github.io/mermaid/#/flowchart?id=flowcharts-basic-syntax

这是一条注释示例 。

*[(fail)注释]：注释内容

注释

Markdown将文本转换为 HTML。

*[(fail)HTML]:   超文本标记语言

; 注释2

: 注释3

表格

项目     | Value
-------- | -----
电脑  | $1600
手机  | $12
导管  | $1

| Column 1 | Column 2      |
|:--------:| -------------:|
| centered 文本居中 | right-aligned 文本居右 |

[mermaid-js.github.io](https://mermaid-js.github.io/mermaid/#/flowchart?id=flowcharts-basic-syntax)
