# Tweening

{% embed url="http://dotween.demigiant.com/" %}

From Dan...

```csharp
transform
    .DOLocalMoveX(transform.localPosition.x + 1, 0.25f)
    .SetLoops(-1, LoopType.Yoyo)
    .SetEase(Ease.InOutQuad); 
```

If you put this on start it would be bouncing back and forth, not sure if the rigidbody would interfere with it though

![](https://cdn.discordapp.com/attachments/671943194832797726/683813552485826594/Bouncing.gif)

Put this in your player, on your bubble, on enemy, on everything to make them scale up and down cutely, adjust the 1 for more or less speed

```csharp
transform
    .DOScale(transform.localScale * 1.05f,1)
    .SetLoops(-1, LoopType.Yoyo)
    .SetEase(Ease.InOutQuad); 
```

Just on start The loop type yoyo makes it go back and forth and the -1 makes it keep going forever

```csharp
transform.DOScale(0, 0.25f).OnComplete((() => { Destroy(gameObject);}));
```

If your enemy takes multiple hits to kill you can do little shakes when they get hit with

```csharp
transform.DOShakeScale(0.25f, 0.2f);
```

If they get hit multiple times in a row real quick it might end up in a messed up scale since it might start a shake scale while there is already another going on, so you might want to reset in the end

```csharp
transform.DOShakeScale(0.25f, 0.2f).OnComplete(() =>
{
    //Add code to go back to original scale here.
});
```

