---
description: 算是一些定义吧~
layout: landing
---

# Some definition

{% hint style="info" %}
#### <mark style="color:red;">**在分析性能和内存分配的时候，永远不要在Editor（编辑器）里面分析，始终分析构建出来的游戏**</mark>

例如：" 在编辑器你调用 <mark style="color:blue;">`GetComponent<>`</mark>` ``来查询一个不存在的组件时，你会看到`<mark style="color:blue;">`C#`</mark>`内存分配正在发生，因为我们正在新分配的伪空对象中生成这个自定义的警告字符串。但是，这种内存分配不会发生在内置游戏中。"`
{% endhint %}

#### Unity生命周期函数

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

<details>

<summary><mark style="color:blue;">GameObject 相关</mark> </summary>

1. [我们是否保留 Unity自定义 operator == ？](https://blog.unity.com/technology/custom-operator-should-we-keep-it)
2. [不要使用 Unity Objects == Null](https://jacx.net/2015/11/20/dont-use-equals-null-on-unity-objects.html)

Unity对==进行了特殊处理，也就是<mark style="color:blue;">重写了==运算符</mark>（还有<mark style="color:blue;">!=</mark>），更改了其行为，但这样更改有个缺点：就是unity要进行两次对象比较，对象之间相互传递或传递到 <mark style="color:blue;">null</mark> 的速度比预期的慢。所以 用 <mark style="color:blue;">==</mark> 来进行的判 <mark style="color:blue;">null</mark> 的 代价会比预期的昂贵

Unity对象是 由 <mark style="color:blue;">C/C++</mark> 对象包装的，称为<mark style="color:blue;">C#</mark>包装对象。所以当对象被销毁时，底层<mark style="color:blue;">C++</mark>对象也会被销毁，但是<mark style="color:blue;">C#</mark>对象必须等待垃圾回收。这也意味着可能存在<mark style="color:blue;">C#</mark>包装对象包装了一个已经被销毁了的 <mark style="color:blue;">C/C++</mark> 对象，如果将此对象与 <mark style="color:blue;">null</mark> 进行比较的话，在这种情况下， Unity 重载的 <mark style="color:blue;">==</mark> 运算符会返回 <mark style="color:blue;">true</mark>，表示对象已经被销毁，但是如果是把对象转换为 <mark style="color:blue;">C#</mark> 的基类 <mark style="color:blue;">`System.Object`</mark> 的话，则情况相反，如果GC垃圾回收还没有进行。

<mark style="color:blue;">`?? 和 ? 运算符`</mark>的行为与 unity 重载的== 不一致，它们也是做了一个空检查，但那个是做了一个 <mark style="color:blue;">纯 c#</mark> 空检查，所以没法绕过它们去调用Unity重载自定义的空检查，包括类似情况的还有 <mark style="color:blue;">`is 和 as 运算符`</mark>，还有下面的例子

因此，建议在判断为 <mark style="color:blue;">null</mark> 的时候使用&#x20;

```csharp
void isVaild(UnityObject obj)
{
    if (obj) 
    {
        // 理由是 UnityObject 对象实现了 对 Bool 的隐式（implicit）转换
    }
}
```

```csharp
public class GameObjectIsNull : MonoBehaviour
{
  private void Awake()
  {
    object o1 = 1, o2 = 1;
    if (o1 == o2) print($"o == 1");                  // × 等于运算 == 对于值类型是计算的值，对于引用类型则是计算的地址
    else print("o1 != o2");                          // √ 判断会进这里(this way)，因为两个对象地址不同

    GameObject go = new GameObject();
    DestroyImmediate(go);

    if (go == null) print($"go是：{go}");             // √ 原因是 GameObject 重载了 == 运算
    if (ReferenceEquals(go, null)) print("RE go");   // × 地址不一样

    object oo = (object)go;
    print($"oo是：{oo}");                             // 一样是 null
    
    if (oo.Equals(null)) print("oo.Equals(null)");   // √ Equals是比较的值
    if (oo is null) print("oo is null");             // × is 是会忽略对象的所有重载，真正意义的去检查这个对象类型的
    if (oo == null) print("oo == null");             // × 由于这里是转换为 System.Object, 所以也屏蔽了子类go写的重载，和 is 有异曲同工之妙
    if (ReferenceEquals(oo, null)) print("RE oo");   // × 该方法直接调用的 == 进行判断返回，引用类型判断的引用地址，不一样

    object oc = null; print(oc == null);             // √
  }
}
```

![](../../.gitbook/assets/Snipaste\_2022-07-10\_13-02-31.png) -- [<mark style="color:blue;">详阅 object.cs</mark>](https://referencesource.microsoft.com/#mscorlib/system/object.cs)<mark style="color:blue;"></mark>



</details>

```csharp
private 
```



