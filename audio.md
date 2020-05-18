# Streaming Assets

{% embed url="https://www.raywenderlich.com/479-using-streaming-assets-in-unity" %}

{% embed url="https://audio.online-convert.com/convert-to-ogg" %}

Example from Streaming Assets tutorial:

```csharp
    private IEnumerator LoadBackgroundMusic(FileInfo musicFile)
    {
        if (musicFile.Name.Contains("meta"))
        {
            yield break;
        }
        else
        {
            string musicFilePath = musicFile.FullName.ToString();
            string url = string.Format("file://{0}", musicFilePath);
            WWW www = new WWW(url);
            yield return www;
            musicPlayer.clip = www.GetAudioClip(false, false);
            musicPlayer.Play();
        }
    }
```

> This code should look pretty familiar. Loading an audio file is very similar to loading a texture. You use the URL to load the file and then apply audio to `musicPlayer's` `clip` property.
>
> Finally, you call `Play()` on `musicPlayer` to get the soundtrack thumping.



