# Calculate Angle to Face Target

```csharp
// Direction to Face
Vector2 dir = Vector2.up;

// May need to be positive
float offset = -90f;

float angle = Mathf.Atan2(dir.y, dir.x) * Mathf.Rad2Deg + offset;
rb.rotation = angle;
```

From this video:

{% embed url="https://www.youtube.com/watch?v=LNLVOjbrQj4" %}

