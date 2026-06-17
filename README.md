# 🤖 FinEduca - Agente Financeiro Inteligente para Educação Financeira e Certificações ANBIMA

## Contexto

O FinEduca é um agente financeiro inteligente baseado em Inteligência Artificial Generativa, desenvolvido para promover educação financeira personalizada e apoiar a preparação para certificações ANBIMA, como CPA-10 e CPA-20.

O agente combina personalização, aprendizado contínuo e mecanismos de segurança para transformar conteúdos complexos de economia, finanças e investimentos em experiências de aprendizagem acessíveis e contextualizadas.

### Principais Capacidades

* **Antecipar necessidades** de aprendizagem com base no histórico e progresso do usuário.
* **Personalizar conteúdos** conforme nível de conhecimento, objetivos e dificuldades identificadas.
* **Cocriar trilhas de aprendizagem** e planos de estudo personalizados.
* **Garantir segurança e confiabilidade** por meio de validação de respostas e prevenção de alucinações.
* **Promover educação financeira** sem realizar recomendações financeiras ou indicar ativos específicos.

---

## Documentação

### 1. Documentação do Agente

Define o propósito, comportamento, arquitetura e mecanismos de segurança do FinEduca.

📄 **Documento:** [`docs/01-documentacao-agente.md`](./docs/01-documentacao-agente.md)

---

### 2. Base de Conhecimento

Descreve os dados utilizados pelo agente, adaptações realizadas e estratégia de integração com a aplicação.

📄 **Documento:** [`docs/02-base-conhecimento.md`](./docs/02-base-conhecimento.md)

---

### 3. Prompts do Agente

Contém o System Prompt, exemplos de interação, tratamento de edge cases e regras de comportamento do FinEduca.

📄 **Documento:** [`docs/03-prompts.md`](./docs/03-prompts.md)

---

### 4. Aplicação Funcional

Implementação do chatbot utilizando Streamlit, Ollama e integração com a Base de Conhecimento.

📁 **Código-fonte:** [`src/`](./src/)

---

### 5. Avaliação e Métricas

Define os critérios utilizados para avaliar precisão, segurança, personalização e qualidade educacional das respostas.

📄 **Documento:** [`docs/04-metricas.md`](./docs/04-metricas.md)

---

### 6. Pitch

Apresentação resumida do FinEduca, destacando problema, solução, demonstração e impacto.

📄 **Documento:** [`docs/05-pitch.md`](./docs/05-pitch.md)

---

## Estrutura do Repositório

```text
📁 fineduca/
│
├── README.md
│
├── data/
│   ├── historico_atendimento.csv
│   ├── perfil_investidor.json
│   ├── produtos_financeiros.json
│   └── transacoes.csv
│
├── docs/
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
│
├── src/
│   ├── app.py
│   ├── agente.py
│   ├── conhecimento.py
│   ├── validacao.py
│   ├── prompts.py
│   └── config.py
│
├── assets/
│
└── requirements.txt
```
