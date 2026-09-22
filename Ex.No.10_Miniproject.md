# Ex.No: 10  Implementation of 2D/3D Game Development Using Unity

### DATE: 22/09/2026                                                                           
### REGISTER NUMBER : 212224240062

### AIM: 
To develop a simple 2D Platform Shooter Game using Unity Engine where the player can move, jump, shoot enemies, collect coins, and achieve a high score.

### Algorithm – 2D Platform Shooter Game

1. Start the Unity game and initialize the player, enemies, coins, bullets, score, and lives.
2. Create the 2D game environment with platforms, background, and obstacles.
3. Read the player's left and right movement input.
4. Move the player horizontally according to the input.
5. Check whether the player is standing on the ground.
6. Allow the player to jump when the Space key is pressed.
7. Create and fire a bullet when the player clicks the mouse.
8. Detect collisions between bullets, enemies, players, and coins.
9. Increase the score when coins are collected and update the score display.
10. Continue the game until the player reaches the goal or loses all lives, then display the result.

### Program:
  
## 1.PlayerController.cs

```

using UnityEngine;
using UnityEngine.UI;

public class PlayerController : MonoBehaviour
{
    public float speed = 5f;
    public float jumpForce = 7f;

    public Transform groundCheck;
    public LayerMask groundLayer;

    public GameObject bulletPrefab;
    public Transform firePoint;
    public Text scoreText;

    private Rigidbody2D rb;
    private bool isGrounded;
    private int score = 0;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        UpdateScore();
    }

    void Update()
    {
        float horizontal = Input.GetAxisRaw("Horizontal");

        rb.velocity = new Vector2(horizontal * speed, rb.velocity.y);

        isGrounded = Physics2D.OverlapCircle(
            groundCheck.position,
            0.15f,
            groundLayer
        );

        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
        }

        if (Input.GetMouseButtonDown(0))
        {
            Shoot();
        }
    }

    void Shoot()
    {
        if (bulletPrefab == null || firePoint == null)
            return;

        GameObject bullet = Instantiate(
            bulletPrefab,
            firePoint.position,
            Quaternion.identity
        );

        Rigidbody2D bulletRb = bullet.GetComponent<Rigidbody2D>();

        if (bulletRb != null)
            bulletRb.velocity = Vector2.right * 10f;

        Destroy(bullet, 3f);
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Coin"))
        {
            score += 10;
            UpdateScore();
            Destroy(other.gameObject);
        }

        if (other.CompareTag("Enemy"))
        {
            score -= 5;
            UpdateScore();
        }
    }

    void UpdateScore()
    {
        if (scoreText != null)
            scoreText.text = "Score: " + score;
    }
}

```

### Output:

<img width="931" height="522" alt="image" src="https://github.com/user-attachments/assets/1c52d5ea-331a-4723-af13-7997d89d5ef8" />


### Result:
Thus, the 2D Platform Shooter Game was successfully developed using Unity Engine. The player can move and jump across platforms, shoot enemies, collect coins, and the score is updated based on the player's actions.
