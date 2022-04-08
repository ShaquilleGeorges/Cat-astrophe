# Cat-astrophe

Code I wrote to move the character around.

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerPlatformerController : PhysicsObject {

    AudioSource jumpSound;
    public float maxSpeed = 7;
    public float jumpTakeOffSpeed = 7;
    public Joystick joystick;

    private SpriteRenderer spriteRenderer;
    public Animator animator;

    // Use this for initialization
    void Awake () 
    {
        spriteRenderer = GetComponent<SpriteRenderer>();
        animator = GetComponent<Animator>();
        jumpSound = GetComponent<AudioSource>();
    }

    protected override void ComputeVelocity()
    {
        Vector2 move = Vector2.zero;
        move.x = joystick.Horizontal;

        animator.SetFloat("Speed", Mathf.Abs(move.x));

        if (((Input.touchCount > 1) || (Input.GetButtonUp("Jump")) && grounded))
        {
            velocity.y = jumpTakeOffSpeed;
            jumpSound.Play();
            //animator.SetBool("isJumping", true);
        }
        else if (Input.GetButtonUp("Jump"))
        {
            if (velocity.y > 0)
            {
                velocity.y = velocity.y * 0.5f;
            }
        }

        bool flipSprite = (spriteRenderer.flipX ? (move.x > 0.01f) : (move.x < 0.01f));
        if (flipSprite)
        {
            spriteRenderer.flipX = !spriteRenderer.flipX;
        }

        animator.SetBool("grounded", grounded);
        animator.SetFloat("velocityX", Mathf.Abs(velocity.x) / maxSpeed);

        targetVelocity = move * maxSpeed;
    }
}
