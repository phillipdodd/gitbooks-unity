# GameEvent & GameEventListener

```csharp
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu]
public class GameEvent : ScriptableObject
{
	private List<GameEventListener> listeners =
		new List<GameEventListener>();

	public void Raise()
	{
		for (int i = listeners.Count - 1; i >= 0; i--)
			listeners[i].OnEventRaised();
	}

	public void RegisterListener(GameEventListener listener)
	{ listeners.Add(listener); }

	public void UnregisterListener(GameEventListener listener)
	{ listeners.Remove(listener); }
}
```

And the accompanying **GameEventListener** script...

```csharp
using UnityEngine;
using UnityEngine.Events;

public class GameEventListener : MonoBehaviour
{
    public GameEvent Event;
    public UnityEvent Response;

    private void OnEnable()
    { Event.RegisterListener(this); }

    private void OnDisable()
    { Event.UnregisterListener(this); }

    public void OnEventRaised()
    { Response.Invoke(); }
}

```

