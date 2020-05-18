# Player Movement Script

## WASD

```csharp
using UnityEngine;
public class PlayerMovement : MonoBehaviour
{
    public float movementSpeed = 2f;

    private Vector2 movementDir;
    private Rigidbody2D rb;
    private Animator anim;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        anim = GetComponent<Animator>();
    }
    // Update is called once per frame
    void Update()
    {
        movementDir.x = Input.GetAxisRaw("Horizontal");
        movementDir.y = Input.GetAxisRaw("Vertical");

        if (anim)
        {
            //anim.SetFloat("Horizontal", rb.velocity.x);
            //anim.SetFloat("Vertical", rb.velocity.y);
            anim.SetFloat("Speed", rb.velocity.sqrMagnitude);
        }
    }
    private void FixedUpdate()
    {
        rb.velocity = movementDir * movementSpeed;
    }
}
```

## Click to Move

```csharp
using Sirenix.OdinInspector;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public BoolVariable playerIsAlive;

    [FoldoutGroup("Constants")]
    public FloatVariable playerSpeed;

    [FoldoutGroup("Constants")]
    public FloatVariable playerMaxVelocityMagnitude;

    [FoldoutGroup("Constants")]
    public FloatVariable playerLookDirOffset;

    [FoldoutGroup("Shared Variables")]
    public Vector2Variable playerLookDir;
    [FoldoutGroup("Shared Variables")]
    public Vector2Variable playerPosition;

    private Camera cam;
    private Rigidbody2D rb;
    private Animator anim;

    private Vector2 movementDir;
    private Vector2 mousePos;

    private void Start()
    {
        cam = Camera.main;
        anim = GetComponent<Animator>();
        rb = GetComponent<Rigidbody2D>();
    }

    private void Update()
    {
        movementDir.x = Input.GetAxisRaw("Horizontal");
        movementDir.y = Input.GetAxisRaw("Vertical");

        mousePos = cam.ScreenToWorldPoint(Input.mousePosition);
        playerPosition.RuntimeValue = transform.position;
    }

    private void FixedUpdate()
    {
        if (playerIsAlive.RuntimeValue)
        {
            float currentVelocityMagnitude = rb.velocity.magnitude;
            //print(currentVelocityMagnitude);
            anim.SetFloat("Speed", currentVelocityMagnitude);
        
            if (currentVelocityMagnitude < playerMaxVelocityMagnitude.RuntimeValue)
            {
                rb.AddForce(movementDir * playerSpeed.RuntimeValue * Time.deltaTime);
            }

            playerLookDir.RuntimeValue = mousePos - rb.position;

            float angle = Mathf.Atan2(playerLookDir.RuntimeValue.y, playerLookDir.RuntimeValue.x) * Mathf.Rad2Deg + playerLookDirOffset.RuntimeValue;

            rb.rotation = angle;
        }
    }
}

```

