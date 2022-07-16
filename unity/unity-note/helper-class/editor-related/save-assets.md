# 强制序列化资产

```csharp
  public static class SaveAssets
  {
    /// <summary>
    /// 需要重新序列化的 文件夹
    /// </summary>
    private static readonly string[] m_searchInFolders = new string[] { "Assets/ScriptableObjects" };

    /// <summary>
    /// 强制性 - 重新序列化 ScriptableObjects 文件夹下所有文件
    /// </summary>
    [MenuItem("MizuTools/Reserizlize Assets &s")]
    public static void ReserizlizeAssets()
    {
      string[] filterFiles = AssetDatabase.FindAssets($"t:{typeof(ScriptableObject)}", m_searchInFolders);
      filterFiles = filterFiles.Select(guid => AssetDatabase.GUIDToAssetPath(guid));
      AssetDatabase.ForceReserializeAssets(filterFiles);
      Debug.Log("Assets Reserizlize Succeeded ^_^");
    }
  }
```
