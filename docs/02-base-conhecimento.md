# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar as explicações e conceitos sobre investimentos que estão dentro do perfil do usuário.  |
| `produtos_financeiros.json` | JSON | Conhecer e explicar os produtos para o usuário |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usá-lo em suas explicações de forma didática|


---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Trabalhei com eles do jeito que estavam. 

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.
Pensei muito na possibilidade de colocar direto no prompt, mas ele vai ficar gigantesco. 
Melhor gerar via código, 

```python
perfil = json.load(open('./data/perfil_investidor.json'))
historico = pd.read_csv('./data/historico_atendimento.csv')
transacoes = pd.read_csv('./data/transacoes.csv')
produtos = json.load(open('./data/produtos_financeiros.json'))

```


### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Decidi por seguir o exemplo da aula nesse caso. 
Lembrando que podemos simplesmente "injetar" os dados em nosso prompt para que o agente tenha o melhor contexto possível ou carregar dinamicamente para que possamos ganhar mais flexibilidade.
A base de dados utilizada é uma base pequena, então tanto faz, mas se fosse algo maior seria mais interessante a segunda opção. 

```
DADOS DO CLIENTE (data/perfil_investidor.json):
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

PRODUTOS FINANCEIROS (data/produtos_financeiros.json)
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Multimercado",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "CDI + 2%",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]

HISTÓRICO (data/historico_atendimento.csv):
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

TRANSAÇÕES (data/transacoes.csv):
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida


```
---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

A ideia é usar os dados originais da base de conhecimento na hora de gerar as respostas, mas aprensentar apenas as informações mais relevantes para o usuário dentro do contexto a ser analisado. 
```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000
- Objetivo: Construir reserva de emergência
- Reserva atual: R$ 10.0000 (meta: R$15.000)

Resumo de gastos:
- Transporte: R$ 295
- Saúde: R$ 188
- Moradia: R$ 1.380
- Lazer: R$ 55,90
- Alimentação: R$ 570
- TOTAL DE SAÍDAS: R$ 2.488,90

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...

Produtos disponíveis para explicar:
- Tesouro Selic (risco baixo)
- CDB Liquidez Diária (risco baixo)
- LCI/LCA (risco baixo)
- Fundo de Ações (risco alto)
- Fundo Multimercado (risco medio)

```
