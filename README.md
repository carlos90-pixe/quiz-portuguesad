using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class QuizPortugues2 : MonoBehaviour
{
    public TMP_Text perguntaTexto;
    public TMP_Text pontosTexto;
    public TMP_Text mensagemTexto;
    public Button[] botoes;

    private int pontos = 0;
    private int perguntaAtual = 0;

    private string[] perguntas =
    {
        "Qual é o aumentativo de casa?",
        "Qual palavra está escrita corretamente?",
        "Qual é o feminino de ator?",
        "Qual é o coletivo de peixes?",
        "Qual palavra é um adjetivo?"
    };

    private string[,] opcoes =
    {
        { "Casinha", "Casarão", "Casebre", "Casota" },
        { "Caxorro", "Cachoro", "Cachorro", "Cachoroa" },
        { "Atriz", "Autora", "Atora", "Atorina" },
        { "Alcateia", "Cardume", "Enxame", "Manada" },
        { "Correr", "Rapidamente", "Bonito", "Escola" }
    };

    private int[] corretas = { 1, 2, 0, 1, 2 };

    void Start()
    {
        MostrarPergunta();
    }

    void MostrarPergunta()
    {
        if (perguntaAtual >= perguntas.Length)
        {
            perguntaTexto.text = "Quiz finalizado!";
            mensagemTexto.text = "Pontuação final: " + pontos;
            pontosTexto.text = "";

            foreach (Button botao in botoes)
                botao.gameObject.SetActive(false);

            return;
        }

        perguntaTexto.text = perguntas[perguntaAtual];
        pontosTexto.text = "Pontos: " + pontos;
        mensagemTexto.text = "";

        for (int i = 0; i < botoes.Length; i++)
        {
            int escolha = i;

            botoes[i].gameObject.SetActive(true);
            botoes[i].GetComponentInChildren<TMP_Text>().text =
                opcoes[perguntaAtual, i];

            botoes[i].onClick.RemoveAllListeners();
            botoes[i].onClick.AddListener(
                () => Responder(escolha)
            );
        }
    }

    void Responder(int escolha)
    {
        foreach (Button botao in botoes)
            botao.interactable = false;

        if (escolha == corretas[perguntaAtual])
        {
            pontos += 10;
            mensagemTexto.text = "Muito bem! +10 pontos!";
        }
        else
        {
            mensagemTexto.text = "Ops! Tente acertar a próxima!";
        }

        perguntaAtual++;

        Invoke(nameof(Avancar), 1.5f);
    }

    void Avancar()
    {
        foreach (Button botao in botoes)
            botao.interactable = true;

        MostrarPergunta();
    }
}
