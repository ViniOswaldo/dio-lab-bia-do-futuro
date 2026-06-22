## Estratégia de Integração

### Como os dados são carregados?

O FinEduca carrega os arquivos da pasta `data/` durante a inicialização da aplicação.

Os arquivos CSV são processados utilizando a biblioteca Pandas, enquanto os arquivos JSON são carregados utilizando a biblioteca nativa `json` do Python.

```python
import pandas as pd
import json

# Histórico de aprendizado e interações
historico = pd.read_csv("data/historico_atendimento.csv")

# Exemplos financeiros e exercícios práticos
transacoes = pd.read_csv("data/transacoes.csv")

# Perfil do usuário
with open("data/perfil_investidor.json", "r", encoding="utf-8") as f:
    perfil = json.load(f)

# Produtos financeiros utilizados para fins educacionais
with open("data/produtos_financeiros.json", "r", encoding="utf-8") as f:
    produtos = json.load(f)
```

Após o carregamento, os dados permanecem disponíveis durante toda a sessão para consulta e personalização das respostas.

---

### Estrutura dos Dados Utilizados

A Base de Conhecimento do FinEduca é composta por quatro fontes principais:

#### historico_atendimento.csv

```csv
data,tema,pergunta,status
2026-06-01,Inflação,O que é inflação?,Concluído
2026-06-03,Juros Compostos,Como funcionam os juros compostos?,Concluído
2026-06-05,Sistema Financeiro Nacional,Qual o papel do Banco Central?,Concluído
2026-06-07,Renda Fixa,O que é Tesouro Selic?,Em andamento
2026-06-09,Fundos de Investimento,Como funcionam os fundos?,Em andamento
2026-06-11,Tributação,Como funciona o imposto de renda nos investimentos?,Pendente
```

#### transacoes.csv

```csv
data,categoria,descricao,valor
2026-05-01,Alimentação,Supermercado,-450.00
2026-05-03,Assinaturas,Streaming,-55.00
2026-05-05,Transporte,Combustível,-250.00
2026-05-10,Salário,Recebimento,5000.00
2026-05-12,Investimentos,Aplicação Tesouro Selic,-500.00
2026-05-15,Lazer,Cinema,-80.00
2026-05-18,Educação,Curso CPA-10,-199.00
2026-05-20,Investimentos,Aporte CDB,-300.00
```

#### perfil_investidor.json

```json
{
  "nome": "João Silva",
  "idade": 28,
  "nivel_conhecimento": "Iniciante",
  "certificacao_objetivo": "CPA-10",
  "interesses": [
    "Educação Financeira",
    "Renda Fixa",
    "Fundos de Investimento",
    "Economia"
  ],
  "topicos_concluidos": [
    "Sistema Financeiro Nacional",
    "Inflação",
    "Juros Compostos"
  ],
  "topicos_em_andamento": [
    "Renda Fixa",
    "Fundos de Investimento"
  ],
  "topicos_com_dificuldade": [
    "Tributação",
    "Marcação a Mercado"
  ],
  "progresso": 35
}
```

#### produtos_financeiros.json

```json
[
  {
    "nome": "Tesouro Selic",
    "categoria": "Renda Fixa",
    "risco": "Baixo",
    "liquidez": "Alta"
  },
  {
    "nome": "CDB",
    "categoria": "Renda Fixa",
    "risco": "Baixo a Médio",
    "liquidez": "Variável"
  }
]
```

---

### Como os dados são usados no prompt?

O FinEduca utiliza uma abordagem baseada em contexto dinâmico.

Antes de responder, o agente consulta os arquivos da Base de Conhecimento e seleciona apenas as informações relevantes para a pergunta realizada pelo usuário.

Esses dados são transformados em contexto e enviados juntamente com o System Prompt do agente.

Exemplo:

```python
contexto = f"""
Perfil do Usuário:
- Nome: {perfil['nome']}
- Certificação: {perfil['certificacao_objetivo']}
- Nível: {perfil['nivel_conhecimento']}
- Progresso: {perfil['progresso']}%

Tópicos com dificuldade:
{perfil['topicos_com_dificuldade']}

Últimos conteúdos estudados:
{historico.tail(3).to_string(index=False)}

Pergunta:
{pergunta_usuario}
"""
```

---

## Exemplo de Contexto Montado

```text
Perfil do Usuário:
- Nome: João Silva
- Certificação Objetivo: CPA-10
- Nível de Conhecimento: Iniciante
- Progresso Geral: 35%

Conteúdos Concluídos:
- Sistema Financeiro Nacional
- Inflação
- Juros Compostos

Conteúdos em Andamento:
- Renda Fixa
- Fundos de Investimento

Principais Dificuldades:
- Tributação
- Marcação a Mercado

Últimas Interações:
- O que é Tesouro Selic?
- Como funcionam os fundos?
- Como funciona o imposto de renda nos investimentos?

Pergunta Atual:
Explique o conceito de marcação a mercado.
```
