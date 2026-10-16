using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class QuizPortugues : MonoBehaviour
{
    public TMP_Text perguntaTexto;
    public TMP_Text resultadoTexto;
    public TMP_Text pontosTexto;

    public Button[] botoes;
    private int pontos = 0;

    private string[] perguntas =
    {
        "Qual é o plural de animal?",
        "Qual palavra é um verbo?",
        "Qual é o antônimo de feliz?"
    };

    private string[][] alternativas =
    {
        new string[] { "Animales", "Animais", "Animalis", "Animal" },
        new string[] { "Casa", "Bonito", "Correr", "Mesa" },
        new string[] { "Alegre", "Contente", "Triste", "Feliz" }
    };

    private int[] respostas = { 1, 2, 2 };
    private int indice = 0;

    void Start()
    {
        MostrarPergunta();
    }

    void MostrarPergunta()
    {
        if (indice >= perguntas.Length)
        {
            perguntaTexto.text = "Fim do quiz!";
            resultadoTexto.text = "Você fez " + pontos + " pontos!";
            pontosTexto.text = "";
            foreach (Button botao in botoes)
                botao.gameObject.SetActive(false);
            return;
        }

        perguntaTexto.text = perguntas[indice];
        pontosTexto.text = "Pontos: " + pontos;
        resultadoTexto.text = "";

        for (int i = 0; i < botoes.Length; i++)
        {
            int respostaEscolhida = i;
            botoes[i].gameObject.SetActive(true);
            botoes[i].GetComponentInChildren<TMP_Text>().text =
                alternativas[indice][i];

            botoes[i].onClick.RemoveAllListeners();
            botoes[i].onClick.AddListener(
                () => VerificarResposta(respostaEscolhida));
        }
    }

    void VerificarResposta(int escolha)
    {
        if (escolha == respostas[indice])
        {
            pontos += 10;
            resultadoTexto.text = "Resposta correta! +10 pontos";
        }
        else
        {
            resultadoTexto.text = "Resposta errada!";
        }

        indice++;
        CancelInvoke();
        Invoke(nameof(MostrarPergunta), 1.5f);
    }
}
