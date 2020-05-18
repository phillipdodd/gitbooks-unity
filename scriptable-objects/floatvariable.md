# FloatVariable

```csharp
using UnityEngine;

[CreateAssetMenu]
public class FloatVariable : ScriptableObject, ISerializationCallbackReceiver
{
	public float InitialValue;
	public float RuntimeValue;

	public void OnAfterDeserialize()
	{
		RuntimeValue = InitialValue;
	}

	public void OnBeforeSerialize() { }
}
```

