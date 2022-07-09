---
description: 编辑器相关的 助手类
---

# Editor related

```csharp
  public static class EditorRelated
  {
    /// <summary>
    /// 编辑器加载 - 初始化
    /// </summary>
    [InitializeOnLoadMethod]
    private static void InitializeOnLoadMethod()
    {
      #region 编辑器想要关闭 事件绑定
      EditorApplication.wantsToQuit -= EditorApplication_wantsToQuit;
      EditorApplication.wantsToQuit += EditorApplication_wantsToQuit;
      #endregion
    }


    /// <summary>
    /// 返回值代表 是否关闭 unity 编辑器
    /// </summary>
    private static bool EditorApplication_wantsToQuit()
    {
      SaveAssets.ReserizlizeAssets();
      return true;
    }
  }
```
