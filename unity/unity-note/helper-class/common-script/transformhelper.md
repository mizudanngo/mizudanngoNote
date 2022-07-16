---
description: Transform 助手类
---

# TransformHelper

```csharp
  /// <summary>
  /// Transform 助手类
  /// </summary>
  public static class TransformHelper
  {
    /// <summary>
    /// 通过名字 搜寻 对应的子物体对象，只能搜寻一个
    /// </summary>
    /// <param name="currentTF"></param>
    /// <param name="childName"></param>
    /// <returns></returns>
    public static Transform FindChildByName(this Transform currentTF, string childName)
    {
      Transform childTF = currentTF.Find(childName);
      if (childTF != null)
        return childTF;

      foreach (Transform item in currentTF)
      {
        childTF = item.FindChildByName(childName);
        if (childTF != null)
        {
          return childTF;
        }
      }
      return null;
    }
  }
```
