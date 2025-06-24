# Como usar a Groq como LLM neste projeto

## Requisitos

- Tenha uma conta ativa na Groq e uma chave de API.
- Consulte a documentação da Groq para obter o endpoint correto (exemplo: https://api.groq.com/openai/v1/).

## Configuração das Variáveis de Ambiente

No arquivo `.env` (na raiz de `python-backend`), adicione:

```env
OPENAI_API_KEY=sk-xxxxxx         # Sua chave da Groq
OPENAI_API_BASE=https://api.groq.com/openai/v1
```

Ou, exporte diretamente no terminal antes de rodar o backend:

```bash
export OPENAI_API_KEY=sk-xxxxxx
export OPENAI_API_BASE=https://api.groq.com/openai/v1
```

## Exemplo de código Python para testar

Você pode usar este script para testar se a configuração está correta:

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE")
)

response = client.chat.completions.create(
    model="mixtral-8x7b-32768",  # ou o modelo fornecido pela Groq
    messages=[
        {"role": "system", "content": "Você é um assistente útil."},
        {"role": "user", "content": "Qual a capital do Brasil?"}
    ]
)
print(response.choices[0].message.content)
```

## Observações

- Troque o valor de `model` pelo nome do modelo Groq que você tem acesso.
- Certifique-se de que a biblioteca openai está atualizada (`pip install --upgrade openai`).
- O openai-agents SDK deve usar as mesmas variáveis de ambiente, então basta configurar corretamente que funcionará para todo o backend, sem precisar alterar o código fonte.

## Referências

- [Documentação da Groq](https://console.groq.com/docs)
- [PyPI openai-agents](https://pypi.org/project/openai-agents/)