using UnityEngine;

public class BearAIController : MonoBehaviour
{
    private Animator animator;
    public float moveSpeed = 12f;   // Karakterin hızını ayarlamak için  
    public Transform target;          // Hedef karakter  
    public float attackRange = 1.5f; // Vuruş mesafesi  
    public float attackDamage = 50f; // Saldırı hasarı  


    void Start()
    {
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        // Hedef karaktere doğru hareket et  
        MoveTowardsTarget();

        // Hedefe yakınsa saldır  
        float distanceToTarget = Vector3.Distance(transform.position, target.position);
        if (distanceToTarget < attackRange)
        {
            Attack();
        }
    }

    void MoveTowardsTarget()
    {
        if (target != null)
        {
            float distanceToTarget = Vector3.Distance(transform.position, target.position);

            if (distanceToTarget > attackRange)
            {
                animator.SetBool("IsWalking", true);

                // Hedefe doğru yönel  
                Vector3 direction = (target.position - transform.position).normalized;
                Quaternion lookRotation = Quaternion.LookRotation(direction);
                transform.rotation = Quaternion.Lerp(transform.rotation, lookRotation, Time.deltaTime * 5f);

                // Hareket  
                transform.position += direction * moveSpeed * Time.deltaTime;
            }
            else
            {
                animator.SetBool("IsWalking", false);
            }
        }
        else
        {
            animator.SetBool("IsWalking", false);
        }
    }

    void Attack()
    {
        // Saldırı animasyonunu başlat  
        animator.SetTrigger("Attack");

        // Hedefe zarar ver  
        PlayerMovement player = target.GetComponent<PlayerMovement>();
        if (player != null)
        {
            player.TakeDamage(attackDamage); // Oyuncuya hasar ver  
        }
    }

}
