# TimeSince Struct

{% page-ref page="timesince-struct.md" %}

```csharp
using UnityEngine;

// Credit: https://garry.tv/timesince
public struct TimeSince
{
    float time;

    public static implicit operator float(TimeSince ts)
    {
        return Time.time - ts.time;
    }

    public static implicit operator TimeSince(float ts)
    {
        return new TimeSince { time = Time.time - ts };
    }
}
```

