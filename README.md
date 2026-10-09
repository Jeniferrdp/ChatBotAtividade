# Chatbot Moto Honda da Amazônia

## Descrição do projeto

Chatbot em Python que responde perguntas sobre a **Moto Honda da Amazônia**, a fábrica de motos da Honda em Manaus. Ele usa um modelo de linguagem que roda localmente pelo Ollama.

O chatbot responde **somente** com base nas informações cadastradas no código e reconhece quando **não** tem a informação pedida. O projeto também tem uma etapa de testes com perguntas simuladas, que verifica se o bot acerta as respostas e se admite quando não sabe.

## Código-fonte

Todo o código da aplicação está no arquivo **`chatbot_honda.ipynb`**, dividido em 4 células:

| Célula | O que faz |
|---|---|
| Configuração | Importa as bibliotecas e define o endereço do Ollama e o modelo |
| Informações da empresa | Lista `INFORMACOES` com os dados da Honda (assunto, palavras-chave e informação) |
| Chatbot | Funções `buscar_informacoes`, `chamar_modelo`, `sabe_responder` e `perguntar` |
| Testes | Perguntas simuladas, verificação das respostas e total de acertos |

## Tecnologias utilizadas

- **Python 3**: linguagem do projeto
- **Jupyter Notebook**: ambiente onde o código é executado (pelo VS Code ou pelo Jupyter)
- **Ollama**: executa o modelo de linguagem localmente, sem internet e sem custo
- **Llama 3.2 1B** (`llama3.2:1b`): modelo de linguagem usado para escrever as respostas
- **API compatível com OpenAI** (`/v1/chat/completions`): forma de comunicação com o Ollama
- **Bibliotecas Python**: `requests` (chamadas HTTP), `re` (expressões regulares), `unicodedata` (remoção de acentos), `json` e `os`

## Instalação das dependências

1. Instale o Python 3: https://www.python.org/downloads/
2. Instale o Ollama: https://ollama.com/download
3. Baixe o modelo:

```bash
ollama pull llama3.2:1b
```

4. Instale as bibliotecas Python:

```bash
pip install requests jupyter
```

## Como executar o chatbot

1. Deixe o Ollama aberto. Ele fica rodando em `http://localhost:11434`.
2. Abra o arquivo `chatbot_honda.ipynb` no VS Code ou no Jupyter (`jupyter notebook chatbot_honda.ipynb`).
3. Execute todas as células, em ordem.
4. Para fazer uma pergunta própria, use a função `perguntar`:

```python
print(perguntar("Qual o slogan da Honda?"))
```

## Como funciona

1. **Base de conhecimento:** a lista `INFORMACOES` guarda 16 informações sobre a empresa. Cada item tem um assunto, palavras-chave e o texto da informação.
2. **Busca:** o Python procura quais informações combinam com as palavras da pergunta. Se nenhuma combinar, o bot responde que não tem a informação, sem chamar o modelo.
3. **Resposta:** o modelo recebe só as informações encontradas (no máximo 2) e escreve uma resposta curta em português.
4. **Verificação:** se o modelo disser "não sei" mesmo tendo a informação, o bot devolve as informações encontradas.

## Exemplos de perguntas e respostas obtidas nos testes

Resultado: **14 de 14 acertos**, com o modelo `llama3.2:1b`.

| Pergunta | Resposta do chatbot | Esperado | Resultado |
|---|---|---|---|
| De que país é a Honda? | A Honda é do Japão. | Sabe | ✅ |
| Onde fica a fábrica da Honda? | A fábrica da Honda da Amazônia fica em Manaus, no Amazonas, no Polo Industrial de Manaus (Zona Franca de Manaus). | Sabe | ✅ |
| Quem fundou a Honda? | Soichiro Honda. | Sabe | ✅ |
| Quando a Honda chegou ao Brasil? | A Honda chegou ao Brasil em 1971. | Sabe | ✅ |
| Quando a fábrica de Manaus foi inaugurada? | A fábrica da Honda em Manaus foi inaugurada em 1976. | Sabe | ✅ |
| A Honda fabrica motos em Manaus? | Sim, a Honda fabrica motocicletas em Manaus. | Sabe | ✅ |
| Qual é a moto mais vendida do Brasil? | A Honda CG é a moto mais vendida do Brasil. | Sabe | ✅ |
| Os carros da Honda são fabricados em Manaus? | Não, os carros da Honda são fabricados em São Paulo. | Sabe | ✅ |
| Qual o slogan da Honda? | Não há um slogan específico da Honda, mas o slogan "The Power of Dreams" é uma expressão que pode ser associada à marca. | Sabe | ✅ |
| Qual o site oficial da Honda? | O site oficial da Honda no Brasil é www.honda.com.br. | Sabe | ✅ |
| Quantos funcionários trabalham na fábrica de Manaus? | Desculpe, não tenho essa informação sobre a Honda Manaus. | Não sabe | ✅ |
| Qual o salário de um operador de produção? | Desculpe, não tenho essa informação sobre a Honda Manaus. | Não sabe | ✅ |
| Qual o telefone do SAC da Honda? | Desculpe, não tenho essa informação sobre a Honda Manaus. | Não sabe | ✅ |
| Quem é o presidente da Honda no Brasil? | Desculpe, não tenho essa informação sobre a Honda Manaus. | Não sabe | ✅ |

## Estratégias para reduzir respostas inventadas

- **Base de conhecimento fechada:** o chatbot só usa as informações cadastradas na lista `INFORMACOES`.
- **Busca feita pelo Python, não pelo modelo:** a decisão de "sabe ou não sabe" é feita por palavras-chave. Se nenhuma informação combina com a pergunta, o modelo nem é chamado, então não tem chance de inventar.
- **Contexto mínimo:** o modelo recebe apenas as informações relacionadas à pergunta, e não a base inteira. Isso diminui a confusão em modelos pequenos.
- **Prompt restritivo:** a instrução do sistema manda responder em uma frase curta, somente com base nas informações recebidas, sem acrescentar nada.
- **Temperatura 0:** deixa as respostas mais previsíveis e menos "criativas".
- **Plano B:** se o modelo responder "não sei" tendo a informação, o chatbot devolve o texto original das informações encontradas.
- **Testes automáticos:** perguntas com e sem resposta na base verificam os dois comportamentos esperados.

## Limitações identificadas

- **Modelo pequeno:** o `llama3.2:1b` às vezes distorce a informação mesmo recebendo o texto correto. Por exemplo, respondeu *"Não há um slogan específico da Honda, mas o slogan The Power of Dreams é uma expressão que pode ser associada à marca"*, colocando em dúvida a própria informação. O teste passou porque a palavra esperada apareceu, mas a frase não está totalmente correta.
- **Busca por palavras-chave:** perguntas com sinônimos que não estão cadastrados não encontram a informação, e o bot responde que não sabe. Palavras-chave genéricas podem trazer uma informação que não tem relação com a pergunta. Por exemplo, perguntar o preço de uma "CG 160" traria a informação de modelos, mesmo sem haver preço cadastrado.
- **Verificação simples dos testes:** o teste confere apenas se uma palavra aparece na resposta. Ele não avalia se a frase inteira está correta.
- **Base estática:** as informações estão escritas no código e precisam ser atualizadas manualmente.
- **Desempenho:** como roda localmente, a velocidade depende do computador.

Uma melhoria possível é usar um modelo maior, como o `llama3.2` (3B), que segue melhor as instruções:

```bash
ollama pull llama3.2
```

Depois, altere `MODEL` na primeira célula para `"llama3.2"`.
