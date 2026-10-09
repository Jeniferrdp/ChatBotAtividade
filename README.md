# Chatbot Moto Honda da Amazônia

Chatbot em Python que responde perguntas sobre a **Moto Honda da Amazônia**, fábrica de motos da Honda em Manaus, usando apenas as informações cadastradas no código. Ele também reconhece quando **não** tem a informação.

## Requisitos

- Python 3
- [Ollama](https://ollama.com/download) instalado
- Biblioteca `requests`

```bash
pip install requests
ollama pull llama3.2:1b
```

## Como executar

1. Deixe o Ollama aberto.
2. Abra o arquivo `chatbot_honda.ipynb` no VS Code ou no Jupyter.
3. Rode todas as células, em ordem.

## Como funciona

1. **Base de conhecimento:** a lista `INFORMACOES` guarda as informações da empresa. Cada item tem um assunto, palavras-chave e o texto da informação.
2. **Busca:** o Python procura quais informações combinam com as palavras da pergunta. Se nenhuma combinar, o bot responde que não tem essa informação.
3. **Resposta:** o modelo (`llama3.2:1b`) recebe só as informações encontradas e escreve a resposta. Se mesmo assim ele disser "não sei", o bot devolve a informação encontrada.
4. **Testes:** a última célula faz perguntas simuladas e confere se o bot respondeu certo ou reconheceu que não sabia. No final mostra o total de acertos.

## Estrutura do notebook

| Célula | O que faz |
|---|---|
| Configuração | Define o endereço do Ollama e o modelo |
| Informações da empresa | Lista `INFORMACOES` com os dados da Honda |
| Chatbot | Funções de busca, chamada ao modelo e verificação |
| Testes | Perguntas simuladas e resultado final |

## Como adicionar uma informação

Inclua um novo item na lista `INFORMACOES`:

```python
{
    "assunto": "Horário",
    "palavras_chave": ["horario", "funciona", "abre", "fecha"],
    "informacao": "A fábrica funciona de segunda a sexta.",
}
```

As palavras-chave devem estar em minúsculas e sem acento.

## Trocar o modelo

Na primeira célula, altere a variável `MODEL`. Por exemplo, use `llama3.2` (versão 3b) para respostas melhores:

```python
MODEL = os.getenv("LLM_MODEL", "llama3.2")
```

Antes, baixe o modelo com `ollama pull llama3.2`.
