# Como usar o Gemini como LLM neste projeto

## Requisitos

- Tenha uma conta ativa no Google AI Studio e uma chave de API Gemini.
- Instale a biblioteca oficial:  
  ```bash
  pip install google-generativeai
  ```

## Exemplo de código Python para testar

Você pode usar este script para testar a Gemini:

```python
import google.generativeai as genai
import os

genai.configure(api_key=os.getenv("GEMINI_API_KEY"))

model = genai.GenerativeModel("gemini-pro")
response = model.generate_content("Qual a capital do Brasil?")
print(response.text)
```

## Integração com o projeto

O openai-agents SDK NÃO é compatível diretamente com Gemini (a API Gemini não segue a interface OpenAI).  
Portanto, para usar o Gemini neste backend, é necessário:

- Adaptar o código Python para incluir uma função de roteamento, que escolha entre OpenAI, Groq ou Gemini baseado em uma variável de ambiente (ex: LLM_PROVIDER=openai/groq/gemini).
- Implementar, no backend, os métodos de chamada e formatação de resposta conforme a documentação da Gemini.

### Exemplo de lógica de roteamento no backend (simplificado):

```python
import os

llm_provider = os.getenv("LLM_PROVIDER", "openai")

if llm_provider == "gemini":
    import google.generativeai as genai
    genai.configure(api_key=os.getenv("GEMINI_API_KEY"))
    model = genai.GenerativeModel("gemini-pro")
    def generate_response(messages):
        # Concatene as mensagens para enviar ao Gemini
        prompt = "\n".join([m["content"] for m in messages])
        resp = model.generate_content(prompt)
        return resp.text
elif llm_provider in ("openai", "groq"):
    from openai import OpenAI
    client = OpenAI(
        api_key=os.getenv("OPENAI_API_KEY"),
        base_url=os.getenv("OPENAI_API_BASE")
    )
    def generate_response(messages):
        resp = client.chat.completions.create(
            model=os.getenv("MODEL_NAME", "gpt-4"),
            messages=messages
        )
        return resp.choices[0].message.content
```

Adapte o restante do backend para usar essa função de roteamento para todas as requisições LLM.

## Referências

- [Documentação Gemini](https://ai.google.dev/)
- [PyPI google-generativeai](https://pypi.org/project/google-generativeai/)