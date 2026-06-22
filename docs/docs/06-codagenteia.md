from groq import Groq
import json
import re
import streamlit as st
from datetime import datetime
import os

# ===================== CONFIG =====================
st.set_page_config(page_title="FinEduca", page_icon="📚", layout="wide")

# 🔐 API KEY SEGURA (usar .env ou variável de ambiente)
client = Groq(api_key=os.getenv("GROQ_API_KEY"))

# ===================== SYSTEM PROMPT =====================
SYSTEM_PROMPT = """
Você é o FinEduca, um agente especializado em educação financeira,
economia, investimentos e certificações do mercado financeiro brasileiro.

MISSÃO:
Ensinar finanças de forma simples, prática e estruturada.

ENSINO:
- Sempre explique de forma clara e didática
- Use exemplos práticos quando necessário

GERAÇÃO DE TESTES (IMPORTANTE):
- Só gere teste quando o usuário pedir explicitamente (teste, quiz, simulado, prova)
- Quando gerar teste, responda APENAS em JSON válido

FORMATO OBRIGATÓRIO DO QUIZ:
{
  "quiz": [
    {
      "pergunta": "",
      "alternativas": {
        "A": "",
        "B": "",
        "C": "",
        "D": ""
      },
      "resposta_correta": "A",
      "explicacao": "Explique por que essa é a correta"
    }
  ]
}

- Gere de 3 a 5 perguntas
- Alternativas sempre A, B, C, D
- Apenas uma correta
- Misture dificuldade fácil, média e difícil

REGRAS IMPORTANTES:
- Não recomende compra ou venda de ativos
- Não faça previsões de mercado
- Sempre em português do Brasil
"""

# ===================== STATE INIT =====================
if "conversas" not in st.session_state:
    st.session_state.conversas = {}

if "conversa_atual" not in st.session_state:
    cid = str(datetime.now().timestamp())
    st.session_state.conversas[cid] = {"titulo": "Nova conversa", "mensagens": []}
    st.session_state.conversa_atual = cid

if "quiz_data" not in st.session_state:
    st.session_state.quiz_data = None

if "mostrar_quiz" not in st.session_state:
    st.session_state.mostrar_quiz = False

# ===================== SIDEBAR =====================
with st.sidebar:
    st.title("📚 FinEduca IA")
    st.subheader("🤖 Sobre o agente")

    st.info("""
O FinEduca é um agente de educação financeira.

🎯 Objetivo:
Ensinar finanças de forma simples e estruturada.

📌 Funções:
- Explica conceitos financeiros
- Ensina certificações
- Personaliza com base no usuário
- Gera testes SOMENTE quando solicitado

🚫 Não recomenda investimentos nem prevê mercado
""")

    st.divider()

    st.subheader("🗂 Histórico de conversas")
    if st.session_state.conversas:
        for cid, conv in st.session_state.conversas.items():
            if st.button(conv["titulo"], key=cid):
                st.session_state.conversa_atual = cid
                st.session_state.quiz_data = None
                st.session_state.mostrar_quiz = False

    if st.button("🧹 Nova conversa"):
        cid = str(datetime.now().timestamp())
        st.session_state.conversas[cid] = {"titulo": "Nova conversa", "mensagens": []}
        st.session_state.conversa_atual = cid
        st.session_state.quiz_data = None
        st.session_state.mostrar_quiz = False

# ===================== HEADER =====================
st.title("📚 FinEduca - Educação Financeira")
st.divider()

st.warning("⚠️ Para gerar um teste, use comandos como: 'Crie um teste sobre inflação'")

# ===================== CHAT HISTORY =====================
conversa = st.session_state.conversas[st.session_state.conversa_atual]

for msg in conversa["mensagens"]:
    st.markdown(f"👤 **Usuário:** {msg['pergunta']}")
    st.markdown(f"🤖 **FinEduca:** {msg['resposta']}")

# ===================== INPUT =====================
pergunta = st.chat_input("Digite sua dúvida...")

if pergunta:

    resposta = client.chat.completions.create(
        model="llama-3.3-70b-versatile",
        messages=[
            {"role": "system", "content": SYSTEM_PROMPT},
            {"role": "user", "content": pergunta}
        ],
        temperature=0.2
    )

    texto = resposta.choices[0].message.content
    is_quiz = False

    try:
        data = json.loads(texto)

        if "quiz" in data:
            st.session_state.quiz_data = data["quiz"]
            st.session_state.mostrar_quiz = True
            is_quiz = True
            st.success("Quiz gerado!")

            for i, q in enumerate(st.session_state.quiz_data, 1):
                st.write(f"**{i}. {q['pergunta']}**")
                for k, v in q["alternativas"].items():
                    st.write(f"{k}: {v}")

        else:
            st.markdown(texto)

    except:
        st.markdown(texto)

    conversa["mensagens"].append({
        "pergunta": pergunta,
        "resposta": texto if not is_quiz else "Quiz gerado"
    })

# ===================== QUIZ =====================
if st.session_state.quiz_data and st.session_state.mostrar_quiz:

    st.subheader("✍️ Respostas")

    respostas = st.text_area("Ex: 1.A, 2.B, 3.C")

    if st.button("Corrigir"):
        linhas = respostas.split("\n")
        user_answers = {}

        for linha in linhas:
            match = re.match(r"(\d+)\s*\.?\s*([A-Za-z])", linha.strip())
            if match:
                n, r = match.groups()
                user_answers[int(n)] = r.upper()

        acertos = 0

        for i, q in enumerate(st.session_state.quiz_data, 1):
            correta = q["resposta_correta"]
            user = user_answers.get(i)

            if user == correta:
                st.success(f"{i}. Correto")
                acertos += 1
            else:
                st.error(f"{i}. Errado (correta: {correta})")
                st.info(q["explicacao"])

        st.write(f"Pontuação: {acertos}/{len(st.session_state.quiz_data)}")
