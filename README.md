# Arbitragem-import streamlit as st
import pandas as pd

st.set_page_config(page_title="Detecção de Arbitragem 1X2", layout="centered")

st.title("Analisador de Arbitragem - Mercado 1X2 (Futebol)")

# Dados simulados (você pode depois substituir por dados reais)
dados = [
    {
        "partida": "Flamengo x Palmeiras",
        "casa1": {"nome": "Bet365", "1": 2.10, "X": 3.30, "2": 3.60},
        "casa2": {"nome": "Pinnacle", "1": 2.05, "X": 3.50, "2": 3.70},
    },
    {
        "partida": "São Paulo x Corinthians",
        "casa1": {"nome": "Bet365", "1": 2.40, "X": 3.10, "2": 2.90},
        "casa2": {"nome": "Pinnacle", "1": 2.35, "X": 3.20, "2": 3.00},
    },
]

def calcular_arbitragem(odds):
    inv_total = sum([1/o for o in odds])
    retorno_percentual = (1 / inv_total - 1) * 100
    return inv_total < 1, round(retorno_percentual, 2)

for jogo in dados:
    st.subheader(jogo["partida"])

    odds_1 = max(jogo["casa1"]["1"], jogo["casa2"]["1"])
    odds_X = max(jogo["casa1"]["X"], jogo["casa2"]["X"])
    odds_2 = max(jogo["casa1"]["2"], jogo["casa2"]["2"])

    arbitragem, lucro = calcular_arbitragem([odds_1, odds_X, odds_2])

    col1, col2, col3 = st.columns(3)
    col1.metric("1", f"{odds_1}")
    col2.metric("X", f"{odds_X}")
    col3.metric("2", f"{odds_2}")

    if arbitragem:
        st.success(f"Oportunidade de arbitragem! Lucro garantido de {lucro}%")
    else:
        st.info("Sem arbitragem nesse jogo.")

    st.markdown("---")
