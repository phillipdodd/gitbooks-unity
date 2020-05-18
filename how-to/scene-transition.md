# Scene Transition

Scenes must be added to the project's Build Settings to be accessible via [SceneManager.LoadScene\(\)](https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.LoadScene.html)

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class LevelLoader : MonoBehaviour
{
    public Animator transition;
    public float transitionTime = 1f;

    public void LoadNextScene()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        if (currentSceneIndex == 1)
        {
            StartCoroutine(LoadSceneWithTransition(0));
        }
        else
        {
            StartCoroutine(LoadSceneWithTransition(1));
        }
    }

    public IEnumerator LoadSceneWithTransition(int sceneIndex)
    {
        transition.SetTrigger("Start");
        yield return new WaitForSeconds(transitionTime);
        SceneManager.LoadScene(sceneIndex);
    }
}
```

## Reference

{% embed url="https://www.youtube.com/watch?v=CE9VOZivb3I" %}



