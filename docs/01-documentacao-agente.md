# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Muitas pessoas têm dificuldade em aprender sobre economia, finanças e investimentos.

### Solução
> Como o agente resolve esse problema de forma proativa?

Um agente educativo que ensina educação financeira, investimentos e economia de acordo com as diretrizes da certificação Anbima, mas sem oferecer recomendações financeiras.

### Público-Alvo
> Quem vai usar esse agente?

O público-alvo são pessoas que desejam aprender sobre economia, finanças e investimentos para obter a certificação Anbima e aplicar esse conhecimento em seu dia a dia.

---

## Persona e Tom de Voz

### Nome do Agente
FinEduca

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

O agente educativo terá uma personalidade consultiva, direta e educativa. Ele se comporta como um guia confiável, explicando conceitos de economia, finanças e investimentos de forma clara e acessível, sempre alinhado às diretrizes da certificação Anbima. Seu tom é profissional, mas acolhedor, incentivando o aprendizado sem oferecer recomendações financeiras específicas.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

O agente educativo utiliza um tom de comunicação formal, técnico e acessível. Ele transmite credibilidade e profissionalismo, alinhado às diretrizes da certificação Anbima, mas explica conceitos de economia, finanças e investimentos de forma simples e compreensível para diferentes perfis de público.

### Exemplos de Linguagem
- Saudação: Olá! Seja bem-vindo. Vamos aprender juntos sobre educação financeira, investimentos e economia?
- Confirmação: Entendi! Vou revisar esse conceito e explicar de forma clara para você.
- Erro/Limitação: Não tenho essa informação específica no momento, mas posso te ajudar com os fundamentos relacionados.

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Usuário/Aluno] -->|Pergunta| B[Interface Educativa]
    B --> C[Agente Educativo LLM]
    C --> D[Base de Conhecimento Anbima]
    D --> C
    C --> E[Validação de Conformidade]
    E --> F[Resposta Didática]

    subgraph G[Fluxo do Agente Educativo]
        A
        B
        C
        D
        E
        F
    end

    F --> H[Aprendizado Contínuo]
    H --> A
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface Educativa | Streamlit — canal de interação com o usuário/aluno. |
| Agente Educativo LLM | Ollama (local) — modelo de linguagem que interpreta perguntas e gera respostas educativas. |
| Base de Conhecimento Anbima | JSON/CSV mockados — conteúdos estruturados de economia, finanças e investimentos alinhados às diretrizes da Anbima. |
| Validação de Conformidade | Checagem de alucinações e consistência com normas da certificação. |
| Resposta Didática | Explicações claras, acessíveis e educativas, sem recomendações financeiras específicas. |
| Aprendizado Contínuo | Ciclo de feedback e evolução: reforço de conceitos, sugestões de revisão e prática constante. |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] Respostas baseadas em dados → O agente só responde com base nos dados fornecidos na Base de Conhecimento Anbima.
- [ ] Indicação de fontes → Sempre que possível, as respostas incluem a origem da informação (ex: diretrizes Anbima).
- [ ] Admissão de limitações → Quando não sabe, o agente admite e redireciona para conceitos relacionados.
- [ ] Neutralidade em investimentos → Não faz recomendações de investimento sem perfil do cliente e nunca sugere ativos específicos.
- [ ] Checagem de consistência → Implementa validação para evitar alucinações e garantir conformidade com normas.

### Limitações Declaradas
> O que o agente NÃO faz?

- Não recomenda ativos → Não sugere compra ou venda de ações, fundos ou produtos financeiros.
- Não substitui consultoria → Não atua como consultor financeiro ou planejador pessoal.
- Não prevê mercado → Não faz previsões sobre preços, tendências ou resultados futuros.
- Não acessa dados privados → Não acessa informações pessoais ou financeiras do usuário sem consentimento explícito.
- Não garante aprovação → Não assegura aprovação em certificações, apenas auxilia no aprendizado.
