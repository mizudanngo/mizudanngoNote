---
description: 算是一些定义吧~
---

# Some definition

## Unity生命周期函数

{% hint style="info" %}
<mark style="color:green;">Awake</mark>-<mark style="color:yellow;">OnEnable</mark>-<mark style="color:blue;">Start</mark>-<mark style="color:purple;">FixedUpdate</mark>-<mark style="color:blue;">Update</mark>-<mark style="color:purple;">LateUpdate</mark>-OnGUI-<mark style="color:yellow;">OnDisable</mark>-<mark style="color:red;">OnDestroy</mark>
{% endhint %}

Unity 对于脚本的 <mark style="color:red;">禁用/启用</mark> 只是对于变量 enable 的状态改变，受影响的则为生命周期中的函数**（如上）**

<details>

<summary><mark style="color:blue;"><strong>Awake与Start的区别：</strong></mark></summary>

* Awake 在挂载的游戏对象首次激活时候执行，和脚本状态无关
* Start 则在挂载的游戏对象为激活状态且脚本首次激活后在OnEnable函数执行后执行

</details>

<details>

<summary><mark style="color:blue;"><strong>Update与FixedUpdate的区别：</strong></mark></summary>

* Update是每次渲染帧调用一次
* FixedUpdate



</details>











