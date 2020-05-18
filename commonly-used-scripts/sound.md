# Sound

There's also Dan's SoundManager. He's mentioned it can be a bit poor on performance, but a good example nonetheless.

{% embed url="http://bittentoast.com/util/SoundManager.unitypackage" %}



## Brackey's

{% embed url="https://www.youtube.com/watch?v=6OT43pvUyfY" %}

## AudioManager Class

```csharp
using System;
using UnityEngine;

public class AudioManager : MonoBehaviour
{
    public static AudioManager instance;

    // TODO change to dictionary
    public Sound[] sounds;

    // Start is called before the first frame update
    void Awake()
    {
        // Singleton
        if (instance == null)
        {
            instance = this;
        }
        else
        {
            Destroy(gameObject);
            return;
        }
        DontDestroyOnLoad(gameObject);

        // Setup audio source components
        foreach (Sound s in sounds)
        {
            s.source = gameObject.AddComponent<AudioSource>();
            s.source.clip = s.clip;
            s.source.volume = s.volume;
            s.source.pitch = s.pitch;
            s.source.loop = s.loop;
        }
    }

    private void Start()
    {
        PlayClipNamed("Theme");
    }

    public void PlayClipNamed(string name)
    {
        Sound s = Array.Find(sounds, sound => sound.name == name);
        if (s == null)
        {
            Debug.LogWarning($"Sound: {name} not found");
            return;
        }

        s.source.Play();
    }
}

```

## Sound Class

```csharp
using UnityEngine;

[System.Serializable]
public class Sound 
{
    public string name;
    public AudioClip clip;

    [Range(0f, 1f)]
    public float volume;

    [Range(.1f, 3f)]
    public float pitch;

    public bool loop;

    // Set by AudioManager
    [HideInInspector]
    public AudioSource source;
}

```

