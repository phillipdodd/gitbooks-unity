# PersistentStorage

{% embed url="https://catlikecoding.com/unity/tutorials/object-management/persisting-objects/" %}

## PersistentStorage

## Persient

```csharp
using System.IO;
using UnityEngine;

public class PersistentStorage : MonoBehaviour
{
    private string savePath;

    private void Awake()
    {
        savePath = Path.Combine(Application.persistentDataPath, "saveFile");
    }

    public void Save(PersistableObject o, int version)
    {
        using(
            var writer = new BinaryWriter(File.Open(savePath, FileMode.Create))
        ) {
            writer.Write(-version);
            o.Save(new GameDataWriter(writer));
        }
    }

    public void Load(PersistableObject o)
    {
        using (
            var reader = new BinaryReader(File.Open(savePath, FileMode.Open))    
        ) {
            o.Load(new GameDataReader(reader, -reader.ReadInt32()));
        }
    }
}

```

```csharp
using UnityEngine;

[DisallowMultipleComponent]
public class PersistableObject : MonoBehaviour
{
    public virtual void Save(GameDataWriter writer)
    {
        writer.Write(transform.localPosition);
        writer.Write(transform.localRotation);
        writer.Write(transform.localScale);
    }

    public virtual void Load(GameDataReader reader)
    {
        transform.localPosition = reader.ReadVector3();
        transform.localRotation = reader.ReadQuaternion();
        transform.localScale = reader.ReadVector3();
    }
}

```



```csharp
using System.IO; using UnityEngine;
public class GameDataReader {public int Version { get; }    
private BinaryReader reader;

public GameDataReader(BinaryReader reader, int version)
{
    this.reader = reader;
    Version = version;
}

public float ReadFloat()
{
    return reader.ReadSingle();
}

public int ReadInt()
{
    return reader.ReadInt32();
}

public Quaternion ReadQuaternion()
{
    Quaternion value;
    value.x = reader.ReadSingle();
    value.y = reader.ReadSingle();
    value.z = reader.ReadSingle();
    value.w = reader.ReadSingle();
    return value;
}

public Vector3 ReadVector3()
{
    Vector3 value;
    value.x = reader.ReadSingle();
    value.y = reader.ReadSingle();
    value.z = reader.ReadSingle();
    return value;
}

public Color ReadColor()
{
    Color value;
    value.r = reader.ReadSingle();
    value.g = reader.ReadSingle();
    value.b = reader.ReadSingle();
    value.a = reader.ReadSingle();
    return value;
}
```

}

using System.IO; using UnityEngine;

public class GameDataWriter { BinaryWriter writer;

```csharp
public GameDataWriter(BinaryWriter writer)
{
    this.writer = writer;
}

public void Write(float value)
{
    writer.Write(value);
}

public void Write(int value)
{
    writer.Write(value);
}

public void Write(Quaternion value)
{
    writer.Write(value.x);
    writer.Write(value.y);
    writer.Write(value.z);
    writer.Write(value.w);
}

public void Write(Vector3 value)
{
    writer.Write(value.x);
    writer.Write(value.y);
    writer.Write(value.z);
}

public void Write(Color value)
{
    writer.Write(value.r);
    writer.Write(value.g);
    writer.Write(value.b);
    writer.Write(value.a);
}
```

}



using System.Collections.Generic; using UnityEngine;

public class Game : PersistableObject {

```text
public override void Save(GameDataWriter writer)
{
    writer.Write(shapes.Count);
    for (int i = 0; i < shapes.Count; i++)
    {
        writer.Write(shapes[i].ShapeId);
        writer.Write(shapes[i].MaterialId);
        shapes[i].Save(writer);
    }
}

public override void Load(GameDataReader reader)
{
    int version = reader.Version;
    if (version > saveVersion)
    {
        Debug.LogError("Unsupported future save version " + version);
        return;
    }

    int count = version <= 0 ? -version : reader.ReadInt();
    for (int i = 0; i < count; i++)
    {
        int shapeId = version > 0 ? reader.ReadInt() : 0;
        int materialId = version > 0 ? reader.ReadInt() : 0;
        Shape instance = shapeFactory.Get(shapeId, materialId);
        instance.Load(reader);
        shapes.Add(instance);
    }
}
```

