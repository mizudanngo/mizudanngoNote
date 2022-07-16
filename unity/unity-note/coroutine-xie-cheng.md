# Coroutine 协程

协程是在 <mark style="color:blue;">Update</mark> 之后 <mark style="color:blue;">LateUpdate</mark> 之前执行的

协程是在主线程<mark style="color:blue;">每次渲染帧</mark>（即每次需要Update更新）中，但是在<mark style="color:blue;">**Update之后**</mark>**，**

判断 (<mark style="color:blue;">`执行MoveText()`</mark>) **** 游戏对象下 <mark style="color:blue;">Coroutine</mark> 的 <mark style="color:blue;">keepWaiting</mark> 的结果来决定是否唤醒（执行）协程。开启后的协程不受脚本影响，但受挂载脚本的游戏对象状态影响，且如果协程正在执行，只要游戏对象关闭，那么这个协程就等于直接StopCoroutine了，再次开启游戏对象协程是不会再进行了。

``[<mark style="color:blue;">`自定义中断指令`</mark>](https://docs.unity3d.com/ScriptReference/CustomYieldInstruction.html)<mark style="color:blue;">``</mark>

```csharp
/// <summary>
/// 用户自定义 协程内 中断指令
/// 这个写法 其实和 unity提供的 yield return new WaitUntil()是一样的
/// </summary>
public class CustomCoroutine : CustomYieldInstruction
{
  private Func<bool> m_action;
  
  public CustomCoroutine(Func<bool> action)
  {
    m_action = action;
  }
  
  public override bool keepWaiting => !m_action();
}
```
