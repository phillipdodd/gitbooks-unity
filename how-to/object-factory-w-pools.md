# Object Factory w/ Pools

## Reference

{% embed url="https://catlikecoding.com/unity/tutorials/object-management/" %}

{% hint style="danger" %}
Is it \(too\) bad for data locality to have dynamically sized pools?
{% endhint %}

```csharp
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu]
public class ShapeFactory : ScriptableObject 
{
    public bool recyle;

    [SerializeField]
    private Shape[] prefabs;

    [SerializeField]
    private Material[] materials;

    private List<Shape>[] pools;

    public Shape Get(int shapeId = 0, int materialId = 0)
    {
        Shape instance;
        if (recyle)
        {
            if (pools == null)
            {
                CreatePools();
            }

            List<Shape> pool = pools[shapeId];
            int lastIndex = pool.Count - 1;
            if (lastIndex >= 0)
            {
                instance = pool[lastIndex];
                instance.gameObject.SetActive(true);
                pool.RemoveAt(lastIndex);
            }
            else
            {
                instance = Instantiate(prefabs[shapeId]);
                instance.ShapeId = shapeId;
            }
        }
        else
        {
            instance = Instantiate(prefabs[shapeId]);
            instance.ShapeId = shapeId;
        }
        
        instance.SetMaterial(materials[materialId], materialId);
        return instance;
    }

    public Shape GetRandom()
    {
        return Get(
            Random.Range(0, prefabs.Length),
            Random.Range(0, materials.Length)
            );
    }

    private void CreatePools()
    {
        pools = new List<Shape>[prefabs.Length];
        for (int i = 0; i < prefabs.Length; i++)
        {
            pools[i] = new List<Shape>();
        }
    }

    public void Reclaim(Shape shapeToRecycle)
    {
        if (recyle)
        {
            if (pools == null)
            {
                CreatePools();
            }
            pools[shapeToRecycle.ShapeId].Add(shapeToRecycle);
            shapeToRecycle.gameObject.SetActive(false);
        }
        else
        {
            Destroy(shapeToRecycle.gameObject);
        }
    }
}

```

