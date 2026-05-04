import streamlit as st
import random
from datetime import datetime
import csv

st.set_page_config(page_title="Laboratório de Sólidos", layout="wide")

st.title("🧪 Laboratório Virtual de Sólidos")

try:
    st.image("logo.png", width=130)
except:
    st.warning("Logo não encontrada")

nome = st.text_input("Nome do aluno")
disciplina = st.text_input("Disciplina")

if "dados" not in st.session_state:
    st.session_state.dados = []

if st.button("🎲 Gerar Exercícios"):
    st.session_state.dados = []

    for i in range(5):
        v = round(random.uniform(117.9, 118.5), 4)
        s = round(v + random.uniform(0.4, 0.8), 4)
        m = round(v + random.uniform(0.2, 0.6), 4)

        f = round(random.uniform(1.1, 1.3), 4)
        sst = round(f + random.uniform(0.1, 0.3), 4)
        ssf = round(f + random.uniform(0.05, 0.2), 4)

        st.session_state.dados.append([v, s, m, f, sst, ssf])

    st.success("Exercícios gerados!")

resultados = []

if st.session_state.dados:

    for i, d in enumerate(st.session_state.dados):

        st.subheader(f"Exercício {i+1}")

        v = st.number_input(f"Vazia {i+1}", value=d[0])
        s = st.number_input(f"Seco {i+1}", value=d[1])
        m = st.number_input(f"Mufla {i+1}", value=d[2])

        f = st.number_input(f"Filtro {i+1}", value=d[3])
        sst = st.number_input(f"SST {i+1}", value=d[4])
        ssf = st.number_input(f"SSF {i+1}", value=d[5])

        ST = s - v
        STF = m - v
        STV = ST - STF

        SST = sst - f
        SSF = ssf - f
        SSV = SST - SSF

        SDT = ST - SST
        SDF = STF - SSF
        SDV = STV - SSV

        st.write(f"ST={ST:.2f} | STF={STF:.2f} | STV={STV:.2f}")
        st.write(f"SST={SST:.2f} | SSF={SSF:.2f} | SSV={SSV:.2f}")
        st.write(f"SDT={SDT:.2f} | SDF={SDF:.2f} | SDV={SDV:.2f}")

        resultados.append([ST, STF, STV, SST, SSF, SSV, SDT, SDF, SDV])

if st.button("💾 Salvar Resultado"):

    if nome == "":
        st.error("Digite o nome do aluno")
    else:
        with open("resultados.csv", "a", newline="") as file:
            writer = csv.writer(file)

            for i, r in enumerate(resultados):
                writer.writerow([nome, disciplina, datetime.now(), i+1] + r)

        st.success("Resultado salvo!")
