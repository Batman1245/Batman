# Batman
Movimentação Unyti

using UnityEngine;

public class MovimentoJogador : MonoBehaviour
{
    private CharacterController controller; // Componente para controlar o movimento do personagem
  [SerializeField]  private Transform myCamera; // Refer�ncia � transforma��o da c�mera principal
    private Animator animator; // Componente para controlar as anima��es do personagem

    // Inicializa��o de componentes
    private void Start()
    {
        controller = GetComponent<CharacterController>(); // Obt�m o componente CharacterController do GameObject
        myCamera = Camera.main.transform; // Atribui a transforma��o da c�mera principal
        animator = GetComponent<Animator>(); // Obt�m o componente Animator do GameObject
    }

    // Atualizado a cada quadro
    private void Update()
    {
        float horizontal = Input.GetAxis("Horizontal"); // Obt�m o input horizontal (A, D, esquerda, direita)
        float vertical = Input.GetAxis("Vertical"); // Obt�m o input vertical (W, S, cima, baixo)
        Vector3 movimento = new Vector3(horizontal, 0, vertical); // Cria um vetor de movimento baseado nos inputs

        movimento = myCamera.TransformDirection(movimento); // Ajusta o vetor de movimento para corresponder � orienta��o da c�mera
        movimento.y = 0; // Garante que n�o h� movimento no eixo Y (vertical)

        controller.Move(movimento * Time.deltaTime * 5f); // Aplica o movimento ao personagem
        controller.Move(new Vector3(0, -9.81f, 0) * Time.deltaTime); // Aplica gravidade ao personagem

        // Rota��o do personagem para enfrentar a dire��o do movimento
        if (movimento != Vector3.zero)
        {
            transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(movimento), Time.deltaTime * 10);
        }

        animator.SetBool("Mover", movimento != Vector3.zero); // Ativa a anima��o de movimento se houver deslocamento
    }
}
