# Passo a Passo do Código de Aplicação

Essa pasta contem o código do meu agente financeiro (FIN) e alguns comentários das alterações que fiz ao longo do projeto.

---

## Setup do Ollama:

- [x] Instalar Ollama
- [x] Baixar o modelo gpt-oss:20b
- [x] Testar se funciona

```
OLLAMA_URL = 'http://localhost:11434/api/generate'
MODELO = "gpt-oss:20b"
```


---

## Código completo:

Para consultar o código fonte na integra acesse a pasta `app.py`

---

## Como rodar:

- [x] Instalar dependências
- [x] Garantir que o Ollama está rodando (ollama serve)
- [x] Rodar o App (ollama run gpt-oss:20b)


Aqui precisei modificar, na aula o código seria:
```
pip instal streamlit pandas request
streamlit run appy.py
```
A versão do streamlit estava desatualizada. Precisei alterar uma linha
```
pip instal streamlit pandas request
`python -m streamlit run ./src/app.py`
```







