import streamlit as st
from PIL import Image
import matplotlib.pyplot as plt
import pandas as pd
import io
from datetime import datetime
from fpdf import FPDF
import os
import tempfile
from pathlib import Path

# 🔹 Seletor de idioma
idioma = st.selectbox("🌍 Escolha o idioma | Choose Language", ["Português", "English"])

traducao = {
    "Português": {
        "title": "🌱 Planilha de Emissões Café",
        "subtitle": "Torne sua produção de café mais sustentável!",
        "input_header": "Dados de Entrada",
        "Número de Plantas por Hectare": "Número de Plantas por Hectare",
        "fertilizer_header": "Fertilizantes Sintéticos (N)",
        "dryer_label": "Secador",
        "coffee_dryer": "Secadores de Café",
        "coffee_dryer_type": "Tipo de secador",
        "coffee_dryer_volume": "Volume de lenha utilizada (m³ saca⁻¹ ha⁻¹ ano⁻¹)",
        "direct_fire_drum_dryer": "Secador fogo direto rolo",
        "motor_power": "Potência do motor (cv)",
        "operation_hours": "Horas de operação por ano (saca ha⁻¹ ano⁻¹) ",
        "static_dryer": "Secador Estático",
        "straw_dryer": "Quantidade de palha consumida no secador (kg saca⁻¹ ha⁻¹ ano⁻¹)",
        "fertilizer_label1": "Fertilizante Opção A",
        "fertilizer_label2": "Fertilizante Opção B",
        "fertilizer_quantity1": "Quantidade de fertilizante Opção A ",
        "fertilizer_quantity2": "Quantidade de fertilizante Opção B ",
        "organic_fertilizer": "Fertilizantes Orgânicos",
        "coffee_straw": "Quantidade de palha de café ",
        "chicken_bedding": "Quantidade de cama de frango",
        "composto_comercial": "Composto Orgânico Comercial (kg ha⁻¹ ano⁻¹)",
        "limestone": "Calcário",
        "diesel": "Consumo de Combustíveis Fósseis nas Operações Agrícolas",
        "irrigation": "Irrigação",
        "calculate_button": "Calcular Emissões",
        "result_header": "🌱 Resultados das Emissões ",
        "graph_title": "Distribuição das Emissões de Carbono",
        "productivity_header": "📊 Quantidade de saca/ha produzida?",
        "productivity_label": "🌱 Insira a produtividade em sacas por hectare",
        "emission_per_bag": " Emissão por saca: {:.2f} kg CO₂eq/saca",
        "drone": "Uso de Drone",
        "drone_batteries": "Quantidade de baterias utilizadas por hectare",
        "drone_emission": "🚁 Uso de Drone",
        "diesel_ref_values": {
            "Sulcação": 11.0,
            "Aração": 10.0,
            "Gradagem": 8.0,
            "Calagem": 7.0,
            "Gessagem": 6.0,
            "Aplicação de Fertilizantes": 9.0,
            "Aplicação de Defensivos": 12.0,
            "Aplicação de Herbicidas": 12.0,
            "Aplicação de Adubo Foliar": 12.0,
            "No uso da Trincha": 12.0,
            "No uso da Roçadeira": 12.0,
            "Na Colheita Mecanizada": 12.0,
            "Na Colheita Semimecanizada": 12.0,
        },
        "Nome_produtor": "Nome do Produtor",
        "propriedade": "Nome da Propriedade",
        "email": "Email",
        "localizacao": "Localização Geográfica",
        "area_cafe": "Área de café (ha)",
        "area_propriedade": "Área total da propriedade (ha)",
        "header": "Informações do Usuário"
    },
    "English": {
        "title": "🌱 Emissions Sheet Coffee",
        "subtitle": "Make your coffee production more sustainable!",
        "input_header": "Input Data",
        "Número de Plantas por Hectare": "Number of Plants per Hectare",
        "fertilizer_header": "Synthetic Fertilizers (N)",
        "dryer_label": "Dryer",
        "coffee_dryer": "Coffee Dryers",
        "coffee_dryer_type": "Dryer Type",
        "coffee_dryer_volume": "Firewood volume used (m³ ha⁻¹ year⁻¹)",
        "direct_fire_drum_dryer": "Direct Fire Drum Dryer",
        "motor_power": "Motor power (hp)",
        "operation_hours": "Operating hours per year",
        "static_dryer": "Static Dryer",
        "straw_dryer": "Straw consumed in dryer (kg ha⁻¹ year⁻¹)",
        "composto_comercial": "Commercial Organic Compound (kg ha⁻¹ year⁻¹)",
        "fertilizer_label1": "Fertilizer Option A",
        "fertilizer_label2": "Fertilizer Option B",
        "fertilizer_quantity1": "Amount of fertilizer Option A ",
        "fertilizer_quantity2": "Amount of fertilizer Option B ",
        "organic_fertilizer": "Organic Fertilizers",
        "coffee_straw": "Amount of coffee straw (kg ha⁻¹ year⁻¹)",
        "chicken_bedding": "Amount of chicken bedding (kg ha⁻¹ year⁻¹)",
        "manure": "Amount of cattle manure (kg ha⁻¹ year⁻¹)",
        "limestone": "Limestone",
        "diesel": "Consumption of Fossil Fuels in Agricultural Operations",
        "irrigation": "Irrigation",
        "calculate_button": "Calculate Emissions",
        "result_header": "🌱 Emission Results",
        "graph_title": "Carbon Emission Distribution",
        "productivity_header": "📊 Quantity of bags/ha produced?",
        "productivity_label": "🌱 Enter productivity in bags per hectare",
        "emission_per_bag": "☕ Emission per bag: {:.2f} kg CO₂eq/bag",
        "drone": "Drone Usage",
        "drone_batteries": "Number of drone batteries used per hectare",
        "drone_emission": "Drone Usage: kg CO₂eq",
        "diesel_ref_values": {
            "furrowing": 11.0,
            "Plowing": 10.0,
            "Harrowing": 8.0,
            "Limestone Application": 7.0,
            "Gypsum Application": 6.0,
            "Fertilizer Application": 9.0,
            "Pesticide Application": 12.0,
            "Herbicide Application": 12.0,
            "Foliar Fertilizer Application": 12.0,
            "Brush Cutter Usage": 12.0,
            "Mower Usage": 12.0,
            "Mechanized Harvest": 12.0,
            "Semi-Mechanized Harvest": 12.0,
        },
        "Nome_produtor": "Producer Name",
        "propriedade": "Property Name",
        "email": "Email",
        "localizacao": "Geographic Location",
        "area_cafe": "Coffee Area (ha)",
        "area_propriedade": "Total Property Area (ha)",
        "header": "User Information"
    }
}

# 🔹 Mapeamento de tradução para os tipos de calcário
tipo_calcario_map = {
    "Português": {"Calcítico": "Calcítico", "Dolomítico": "Dolomítico"},
    "English": {"Calcitic": "Calcítico", "Dolomitic": "Dolomítico"}
}


# Funções para calcular emissões
def calcular_emissoes_fertilizante(quantidade_n, tipo_fertilizante):
    # Define o fator de volatilização com base no tipo IPCC
    fatores_volatilizacao = {
        "urea": 0.15,
        "ammonium": 0.08,
        "nitrate": 0.01,
        "ammonium-nitrate": 0.05
    }
    fator_volatilizacao = fatores_volatilizacao.get(tipo_fertilizante, 0.11)  # padrão: 0.11

    return (((quantidade_n * 0.016) +
             (quantidade_n * fator_volatilizacao * 0.014) +
             (quantidade_n * 0.24 * 0.011)) * 44 / 28) * 298


def calcular_emissoes_calcario(quantidade_calcario, tipo_calcario, fase):
    if tipo_calcario == "Calcítico":
        fator = 0.12
    elif tipo_calcario == "Dolomítico":
        fator = 0.124 if fase == "Produção" else 0.13
    else:
        fator = 0.0  # fallback padrão
    return (quantidade_calcario * fator) * (44 / 12)


def calcular_emissoes_palha_cafe(quantidade_n_palha_cafe):
    return (((quantidade_n_palha_cafe * 0.006) +
             (quantidade_n_palha_cafe * 0.21 * 0.014) +
             (quantidade_n_palha_cafe * 0.24 * 0.011)) * 44 / 28) * 298


def calcular_emissoes_cama_frango(quantidade_n_cama_frango):
    return (((quantidade_n_cama_frango * 0.006) +
             (quantidade_n_cama_frango * 0.21 * 0.014) +
             (quantidade_n_cama_frango * 0.24 * 0.011)) * 44 / 28) * 298


def calcular_emissoes_composto(quantidade_n_composto):
    return (((quantidade_n_composto * 0.006) +
             (quantidade_n_composto * 0.21 * 0.014) +
             (quantidade_n_composto * 0.24 * 0.011)) * 44 / 28) * 298


def calcular_emissoes_diesel(litros_diesel):
    emissoes_totais_co2eq = (litros_diesel * 2.604)
    return emissoes_totais_co2eq


def calcular_emissoes_gasolina(litros_gasolina):
    emissoes_totais_co2eq = (litros_gasolina * 2.2126)  # TJ
    return emissoes_totais_co2eq


def calcular_emissoes_irrigacao_diesel(litros_diesel_irrigacao):
    tj_diesel_irrigacao = (litros_diesel_irrigacao * 35.8) / 1_000_000
    emissoes_co2 = tj_diesel_irrigacao * 74100
    emissoes_ch4 = tj_diesel_irrigacao * 4.15 * 25
    emissoes_n2o = tj_diesel_irrigacao * 28.6 * 298
    emissoes_totais_co2eq = emissoes_co2 + emissoes_ch4 + emissoes_n2o
    return emissoes_totais_co2eq


def calcular_emissoes_irrigacao_eletrica(mwh_eletrica):
    emissoes_co2eq_por_mwh = 0.0003785  # valor médio de emissões para geração de eletricidade (em kg CO₂eq/MWh)
    return mwh_eletrica * emissoes_co2eq_por_mwh


def calcular_emissoes_drone_por_bateria(qtd_baterias):
    mwh_por_bateria = 0.006216  # MWh por bateria
    fator_emissao_mwh = 0.0003785  # kg CO₂eq por MWh (média brasileira)
    return qtd_baterias * mwh_por_bateria * fator_emissao_mwh


def calcular_emissao_secador_fogo_direto(lenha_m3, cv, horas):
    emissao_lenha = lenha_m3 * 500 * 0.2
    emissao_motor = cv * 0.7355 * horas * 0.06
    return emissao_lenha + emissao_motor


def calcular_emissao_secador_estatico(cv, horas, palha_kg):
    energia_motor = cv * 0.7355 * horas * 0.06
    n_palha = palha_kg * 0.031
    n2o_palha = n_palha * 0.01
    co2eq_palha = n2o_palha * (44 / 28) * 298
    return energia_motor + co2eq_palha


st.markdown("""
    <style>
    /* Fundo geral */
    body {
        background-color: #f5f7fa !important;
    }

    /* Container principal */
    .block-container {
        max-width: 1100px !important;
        padding: 2rem 2.5rem !important;
        background-color: #ffffff;
        border-radius: 16px;
        box-shadow: 0 12px 24px rgba(0, 0, 0, 0.05);
        margin: 2rem auto;
    }

    /* Títulos principais */
h1, h2, h3, h4, h5, h6 {
    font-family: 'Segoe UI', sans-serif;
    color: #1b5e20 !important;
    font-weight: 700 !important;
    text-align: center;
    margin-top: 0.5rem;
    margin-bottom: 1.8rem;  /* 👈 Espaço sutil entre título e campos */
}


    /* Labels e textos */
    label, div[data-testid="stMarkdownContainer"] > p {
        font-size: 18px !important;
        font-weight: 600;
        color: #333333;
    }

    /* Inputs */
    input[type="text"], input[type="number"], .stNumberInput input, textarea {
        font-size: 17px !important;
        height: 44px !important;
        padding: 10px !important;
        border-radius: 8px !important;
        border: 1px solid #cccccc !important;
        background-color: #fafafa !important;
        box-shadow: inset 0 1px 3px rgba(0,0,0,0.08);
    }

    .stNumberInput, .stSelectbox, .stRadio, .stTextInput {
        margin-bottom: 1.5rem !important;
    }

    /* Botões */
    .stButton>button {
        background-color: #43a047;
        color: #ffffff;
        font-size: 18px;
        font-weight: 600;
        padding: 12px 26px;
        border: none;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(67,160,71,0.2);
        transition: all 0.3s ease-in-out;
    }

    .stButton>button:hover {
        background-color: #388e3c;
        transform: scale(1.03);
        box-shadow: 0 6px 14px rgba(56,142,60,0.3);
        cursor: pointer;
    }

    /* Radio e SelectBox */
    .stRadio label {
        font-size: 17px !important;
        font-weight: 600;
    }

    .stSelectbox div[data-baseweb="select"] > div {
        font-size: 17px !important;
    }

    /* Sidebar */
    section[data-testid="stSidebar"] {
        background-color: #e8f5e9;
        padding: 1.5rem !important;
        border-radius: 12px;
        border: 1px solid #c8e6c9;
    }

    section[data-testid="stSidebar"] * {
        font-size: 16px !important;
        color: #2e7d32 !important;
    }

    section[data-testid="stSidebar"] h1,
    section[data-testid="stSidebar"] h2,
    section[data-testid="stSidebar"] h3 {
        font-size: 22px !important;
        font-weight: bold !important;
        color: #1b5e20 !important;
        margin-top: 1rem !important;
        margin-bottom: 1rem !important;
    }

    /* Colunas */
    .stColumns {
        display: flex !important;
        gap: 2rem !important;
        justify-content: space-between !important;
        width: 100% !important;
    }

    /* Espaço extra após subtítulos do conteúdo */
div[data-testid="stMarkdownContainer"] h2,
div[data-testid="stMarkdownContainer"] h3 {
    margin-bottom: 1.8rem !important;
}


    /* Rolagem suave */
    html {
        scroll-behavior: smooth;
    }
    </style>
""", unsafe_allow_html=True)

# 🔹 Título e Imagem de Capa
st.title(traducao[idioma]["title"])
st.markdown(f"### {traducao[idioma]['subtitle']}")
image = Image.open("Cafe_Robusta_Brasil.jpg")
st.image(image, caption=traducao[idioma]["title"], use_container_width=True)

# Layout em colunas para inputs
st.header(traducao[idioma]["input_header"])

col1, col2 = st.columns(2)

with col1:
    tipo_cultivo = st.radio(
        "🌿 " + ("Tipo de Condução" if idioma == "Português" else "Type of Cultivation"),
        ("Produção", "Plantio") if idioma == "Português" else ("Production", "Planting"),
        key="radio_tipo_cultivo"
    )
    if idioma == "English":
        tipo_cultivo = "Produção" if tipo_cultivo == "Production" else "Plantio"

fase_plantio = tipo_cultivo == "Plantio"
fase_producao = tipo_cultivo == "Produção"

st.sidebar.header(traducao[idioma]["header"])

nome_produtor = st.sidebar.text_input(traducao[idioma]["Nome_produtor"])
propriedade = st.sidebar.text_input(traducao[idioma]["propriedade"])
email = st.sidebar.text_input(traducao[idioma]["email"])
telefone = st.sidebar.text_input("📞 Telefone" if idioma == "Português" else "📞 Phone")
numero_car = st.sidebar.text_input("📄 Número do CAR" if idioma == "Português" else "📄 CAR Number")
regiao_cafeira = st.sidebar.text_input("📍 Região Cafeeira" if idioma == "Português" else "📍 Coffee Region")

# 🔹 Removido: localizacao = st.sidebar.text_input(...)
# 🔹 Substituído por:

ano_referencia = st.sidebar.number_input(
    "📅 Ano de Referência" if idioma == "Português" else "📅 Reference Year",
    min_value=2000,
    max_value=2100,
    value=2025,
    step=1
)

cidade = st.sidebar.text_input("🏙️ Cidade" if idioma == "Português" else "🏙️ City")
estado = st.sidebar.text_input("🗺️ Estado (UF)" if idioma == "Português" else "🗺️ State")

area_cafe = st.sidebar.number_input(traducao[idioma]["area_cafe"], min_value=0.0, format="%.2f")
area_propriedade = st.sidebar.number_input(traducao[idioma]["area_propriedade"], min_value=0.0, format="%.2f")

# Lista de fertilizantes e suas concentrações de N, traduzidos
fertilizantes = {
    "Sulfato de amônio" if idioma == "Português" else "Ammonium Sulfate": {"n": 24, "tipo": "ammonium"},
    "Ureia" if idioma == "Português" else "Urea": {"n": 46.9, "tipo": "urea"},
    "MAP": {"n": 9, "tipo": "ammonium"},
    "20-0-20": {"n": 20, "tipo": "ammonium-nitrate"},
    "20-05-20": {"n": 20, "tipo": "ammonium-nitrate"}
}

# 🔹 Número de plantas por hectare
st.subheader(traducao[idioma]["Número de Plantas por Hectare"])
num_plantas = st.number_input(
    "Digite o número de plantas por hectare",
    min_value=1, format="%d"
)

# 🔹 Rótulos de unidade
unit_label = "Unidade" if idioma == "Português" else "Unit"
kg_label = "kg ha⁻¹ ano⁻¹" if idioma == "Português" else "kg ha⁻¹ year⁻¹"
gpl_label = "g planta⁻¹ ano⁻¹" if idioma == "Português" else "g plant⁻¹ year⁻¹"


def input_quantidade_com_unidade(titulo_markdown, label_qtd, key_prefix, default_unit="kg"):
    """
    Renderiza um bloco no estilo do fertilizante sintético:
      - título (markdown)
      - radio de unidade (kg ha-1 ano-1 | g planta-1 ano-1)
      - number_input de quantidade
    Retorna (quantidade_em_kg_ha_ano, unidade_escolhida_str)
    """
    st.markdown(f"**{titulo_markdown}**")
    unidade = st.radio(
        unit_label, [kg_label, gpl_label],
        key=f"{key_prefix}_unidade",
        index=0 if default_unit == "kg" else 1
    )
    qtd = st.number_input(label_qtd, min_value=0.0, format="%.2f", key=f"{key_prefix}_qtd")

    # Normaliza para kg/ha/ano
    if unidade == gpl_label:
        qtd = (qtd * num_plantas) / 1000.0
    return qtd, unidade


# 🔹 Calcário
st.subheader(traducao[idioma]["limestone"])
col_calc1, col_calc2 = st.columns(2)

with col_calc1:
    tipo_calcario1 = st.selectbox(
        "Tipo de calcário Opção A" if idioma == "Português" else "Limestone type Option A",
        list(tipo_calcario_map[idioma].keys()),
        key="tipo_calc1"
    )
    unidade_calcario1 = st.radio(unit_label, [kg_label, gpl_label], key="uni_calc1")
    quantidade_calcario1 = st.number_input(
        "Quantidade de calcário Opção A" if idioma == "Português" else "Limestone amount Option A",
        min_value=0.0, format="%.2f", key="qtd_calc1"
    )
    if unidade_calcario1 == gpl_label:
        quantidade_calcario1 = (quantidade_calcario1 * num_plantas) / 1000.0

with col_calc2:
    tipo_calcario2 = st.selectbox(
        "Tipo de calcário Opção B" if idioma == "Português" else "Limestone type Option B",
        list(tipo_calcario_map[idioma].keys()),
        key="tipo_calc2"
    )
    unidade_calcario2 = st.radio(unit_label, [kg_label, gpl_label], key="uni_calc2")
    quantidade_calcario2 = st.number_input(
        "Quantidade de calcário Opção B" if idioma == "Português" else "Limestone amount Option B",
        min_value=0.0, format="%.2f", key="qtd_calc2"
    )
    if unidade_calcario2 == gpl_label:
        quantidade_calcario2 = (quantidade_calcario2 * num_plantas) / 1000.0

# 🔹 Converte tipo p/ PT
tipo_calcario1_pt = tipo_calcario_map[idioma][tipo_calcario1]
tipo_calcario2_pt = tipo_calcario_map[idioma][tipo_calcario2]

# 🔹 Inputs adicionais para fertilizantes sintéticos
st.subheader(traducao[idioma]["fertilizer_header"])
col3, col4 = st.columns(2)

with col3:
    fertilizante1 = st.selectbox(traducao[idioma]["fertilizer_label1"], list(fertilizantes.keys()))
    unidade_fertilizante1 = st.radio("Unidade", ["kg ha⁻¹ ano⁻¹", "g planta⁻¹ ano⁻¹"], key="uni1")
    quantidade_fertilizante1 = st.number_input(traducao[idioma]["fertilizer_quantity1"], min_value=0.0, format="%.2f")
    if unidade_fertilizante1 == "g/planta":
        quantidade_fertilizante1 = (quantidade_fertilizante1 * num_plantas) / 1000  # Convertendo para kg/ha/ano

with col4:
    fertilizante2 = st.selectbox(traducao[idioma]["fertilizer_label2"], list(fertilizantes.keys()))
    unidade_fertilizante2 = st.radio("Unidade", ["kg ha⁻¹ ano⁻¹", "g planta⁻¹ ano⁻¹"], key="uni2")
    quantidade_fertilizante2 = st.number_input(traducao[idioma]["fertilizer_quantity2"], min_value=0.0, format="%.2f")
    if unidade_fertilizante2 == "g/planta":
        quantidade_fertilizante2 = (quantidade_fertilizante2 * num_plantas) / 1000  # Convertendo para kg/ha/ano

# =========================
# Palha de café e Cama de frango
# =========================
st.subheader("🌿 Resíduos Orgânicos" if idioma == "Português" else "🌿 Organic Residues")
col_res1, col_res2 = st.columns(2)

with col_res1:
    st.markdown("🍂 " + traducao[idioma]["coffee_straw"])
    unidade_palha = st.radio(unit_label, [kg_label, gpl_label], key="uni_palha")
    quantidade_palha_cafe = st.number_input(
        "Quantidade" if idioma == "Português" else "Amount",
        min_value=0.0, format="%.2f", key="qtd_palha"
    )
    if unidade_palha == gpl_label:
        quantidade_palha_cafe = (quantidade_palha_cafe * num_plantas) / 1000.0

with col_res2:
    st.markdown("🐔 " + traducao[idioma]["chicken_bedding"])
    unidade_cama = st.radio(unit_label, [kg_label, gpl_label], key="uni_cama")
    quantidade_cama_frango = st.number_input(
        "Quantidade" if idioma == "Português" else "Amount",
        min_value=0.0, format="%.2f", key="qtd_cama"
    )
    if unidade_cama == gpl_label:
        quantidade_cama_frango = (quantidade_cama_frango * num_plantas) / 1000.0

# =========================
# Composto orgânico comercial
# =========================
st.markdown("**" + traducao[idioma]["composto_comercial"] + "**")
col_comp1, col_comp2 = st.columns(2)

with col_comp1:
    unidade_composto = st.radio(unit_label, [kg_label, gpl_label], key="uni_comp")
    quantidade_composto = st.number_input(
        "Quantidade" if idioma == "Português" else "Amount",
        min_value=0.0, format="%.2f", key="qtd_comp"
    )
    if unidade_composto == gpl_label:
        quantidade_composto = (quantidade_composto * num_plantas) / 1000.0

with col_comp2:
    n_composto_percentual = st.number_input(
        "Concentração de N (%)" if idioma == "Português" else "N concentration (%)",
        min_value=0.0, format="%.2f", key="n_comp"
    )

import streamlit as st

# 🔹 Dicionário de tradução
traducoes_diesel = {
    "Português": [],
    "English": [
        "Furrowing", "Subsoiler", "Plow", "Harrow",
        "Pasture Herbicide Application", "Brachiaria Seeding", "Green Manure Seeding",
        "Harrowing", "Limestone Application", "Fertilizer Application",
        "Pesticide Application", "Herbicide Application", "Foliar Fertilizer Application",
        "Brush Cutter", "Mower Usage",
        "Semi-Mechanized Harvest - Shaker", "Semi-Mechanized Harvest - Stationary Engine",
        "Semi-Mechanized Harvest - Tractor", "Mechanized Harvest"
    ]
}


# 🔹 Função para traduzir operações
def traduzir_nome(operacao, idioma):
    todas = list(operacoes_diesel_gerais.keys()) + list(operacoes_colheita.keys())
    if idioma == "Português":
        return operacao
    else:
        idx = todas.index(operacao)
        return traducoes_diesel["English"][idx]


# 🔹 Define as operações com diesel com base no tipo de condução
if tipo_cultivo == "Plantio":
    operacoes_diesel_gerais = {
        "Sulcação": ["Trator"],
        "Subsolador": ["Trator"],
        "Arado": ["Trator"],
        "Grade": ["Trator"],
        "Trincha": ["Trator"],
        "Semeadura de Braquiária": ["Trator", "Manual"],
        "Semeadura de Adubo Verde": ["Trator", "Manual"],
        "Calagem": ["Triciclo", "Trator"],
        "Aplicação de Fertilizantes": ["Triciclo", "Trator", "Manual"],
        "Aplicação de Defensivos": ["Triciclo", "Trator", "Manual"],
        "Aplicação de Herbicidas": ["Triciclo", "Trator", "Manual"],
        "Aplicação de Herbicida no Pasto": ["Triciclo", "Trator", "Manual"],
    }
else:  # Produção
    operacoes_diesel_gerais = {
        "Trincha": ["Trator"],
        "No uso da Roçadeira": ["Roçadeira Motorizada", "Trator"],
        "Aplicação de Defensivos": ["Triciclo", "Trator", "Manual"],
        "Aplicação de Herbicidas": ["Triciclo", "Trator", "Manual"],
        "Aplicação de Adubo Foliar": ["Triciclo", "Trator", "Manual"],
        "Semeadura de Adubo Verde": ["Trator", "Manual"],
        "Calagem": ["Triciclo", "Trator"],
        "Aplicação de Fertilizantes": ["Triciclo", "Trator", "Manual"],
    }

# 🔹 Operações de colheita (mantidas somente para Produção)
operacoes_colheita = {
    "Colheita Semimecanizada - Derriçadora": ["Derriçadora Gasolina"],
    "Colheita Semimecanizada - Estacionária": ["Motor Estacionário"],
    "Colheita Semimecanizada - Trator": ["Trator"],
    "Colheita Mecanizada": ["Trator"]
}

# 🔹 Valores de referência
valores_referencia_diesel = {
    "Português": {
        "Sulcação": {"Trator": 8.0},
        "Calagem": {"Triciclo": 5.0, "Trator": 7.0},
        "Aplicação de Fertilizantes": {"Triciclo": 5.0, "Trator": 9.0, "Manual": 0.0},
        "Aplicação de Defensivos": {"Triciclo": 6.0, "Trator": 12.0, "Manual": 0.0},
        "Aplicação de Herbicidas": {"Triciclo": 6.0, "Trator": 12.0, "Manual": 0.0},
        "Aplicação de Adubo Foliar": {"Triciclo": 5.0, "Trator": 12.0, "Manual": 0.0},
        "No uso da Roçadeira": {"Roçadeira Motorizada": 3.0, "Trator": 12.0},
        "Subsolador": {"Trator": 11.0},
        "Arado": {"Trator": 10.0},
        "Grade": {"Trator": 8.0},
        "Trincha": {"Trator": 12.0},
        "Aplicação de Herbicida no Pasto": {"Triciclo": 6.0, "Trator": 12.0, "Manual": 0.0},
        "Semeadura de Braquiária": {"Trator": 8.0, "Manual": 0.0},
        "Semeadura de Adubo Verde": {"Trator": 7.0, "Manual": 0.0},
        "Colheita Semimecanizada - Derriçadora": {"Derriçadora Gasolina": 6.0},
        "Colheita Semimecanizada - Estacionária": {"Motor Estacionário": 5.0},
        "Colheita Semimecanizada - Trator": {"Trator": 12.0},
        "Colheita Mecanizada": {"Trator": 14.0}
    }
}

# 🔹 Armazenamento das entradas
diesel_inputs = {}

# 🔹 Interface - Operações Gerais
st.subheader("🚜 Operações Agrícolas Gerais" if idioma == "Português" else "🚜 General Agricultural Operations")

for operacao, veiculos in operacoes_diesel_gerais.items():
    nome_traduzido = traduzir_nome(operacao, idioma)
    col1, col2 = st.columns([2, 1])

    with col1:
        veiculo_escolhido = st.selectbox(
            f"{nome_traduzido} - {'Escolha o veículo' if idioma == 'Português' else 'Choose the vehicle'}:",
            veiculos,
            key=f"veiculo_{operacao}"
        )

    with col2:
        usar_referencia = st.checkbox(
            f"{'Usar valor de referência para' if idioma == 'Português' else 'Use reference value for'} {nome_traduzido}?",
            key=f"checkbox_{operacao}"
        )

    valor_padrao = valores_referencia_diesel["Português"].get(operacao, {}).get(veiculo_escolhido,
                                                                                0.0) if usar_referencia else 0.0

    if usar_referencia:
        st.markdown(f"""
            <div style="background-color: #f0f0f0; color: #333; font-weight: bold;
                        padding: 6px; border: 1px solid #ccc; border-radius: 4px; text-align: center;">
                {nome_traduzido} - {veiculo_escolhido}: <b>{valor_padrao:.2f}</b> l ha⁻¹
            </div>
        """, unsafe_allow_html=True)
    else:
        valor_padrao = st.number_input(
            f"{'Consumo de diesel na' if idioma == 'Português' else 'Diesel consumption in'} {nome_traduzido} ({veiculo_escolhido}) (l ha⁻¹ ano⁻¹)",
            min_value=0.0,
            format="%.2f",
            value=valor_padrao,
            key=f"input_{operacao}"
        )

    diesel_inputs[operacao] = {veiculo_escolhido: valor_padrao}

# 🔹 Interface - Operação de Colheita agrupada (somente se for Produção)
if tipo_cultivo == "Produção":
    st.subheader(" Operação de Colheita" if idioma == "Português" else "🌾 Harvest Operation")

    tipos_colheita = list(operacoes_colheita.keys())
    tipos_colheita_traduzidos = [traduzir_nome(op, idioma) for op in tipos_colheita]

    tipo_colheita_escolhida = st.selectbox(
        "Tipo de colheita" if idioma == "Português" else "Harvest type",
        tipos_colheita_traduzidos,
        key="tipo_colheita"
    )

    indice_colheita = tipos_colheita_traduzidos.index(tipo_colheita_escolhida)
    operacao = tipos_colheita[indice_colheita]
    veiculos = operacoes_colheita[operacao]

    veiculo_escolhido = st.selectbox(
        "Escolha o veículo" if idioma == "Português" else "Choose the vehicle",
        veiculos,
        key=f"veiculo_colheita"
    )

    usar_referencia = st.checkbox(
        "📌 Usar valor de referência?" if idioma == "Português" else "📌 Use reference value?",
        key=f"checkbox_colheita"
    )

    valor_padrao = valores_referencia_diesel["Português"].get(operacao, {}).get(veiculo_escolhido,
                                                                                0.0) if usar_referencia else 0.0

    if usar_referencia:
        st.markdown(f"""
            <div style="background-color: #fff3cd; color: #664d03; font-weight: bold;
                        padding: 6px; border: 1px solid #ffecb5; border-radius: 4px; text-align: center;">
                {tipo_colheita_escolhida} - {veiculo_escolhido}: <b>{valor_padrao:.2f}</b> l ha⁻¹
            </div>
        """, unsafe_allow_html=True)
    else:
        valor_padrao = st.number_input(
            f"{'Consumo de diesel na' if idioma == 'Português' else 'Diesel consumption in'} {tipo_colheita_escolhida} ({veiculo_escolhido}) (l ha⁻¹ ano⁻¹)",
            min_value=0.0,
            format="%.2f",
            value=valor_padrao,
            key="input_colheita"
        )

    diesel_inputs[operacao] = {veiculo_escolhido: valor_padrao}

col1, col2 = st.columns([3, 1])
with col1:
    st.subheader("♻️ Substituição de Diesel por Biodiesel")

with col2:
    percentual_biogas = st.number_input(
        "Percentual (%)",
        min_value=0,
        max_value=100,
        value=0,
        step=1
    )

percentual_biogas = st.slider(
    "Ajuste o percentual de substituição de diesel por biogás",
    min_value=0,
    max_value=100,
    value=percentual_biogas,
    step=1
)

# 🔹 Inputs adicionais para uso do drone (traduzidos)
st.subheader("🚁 " + ("Uso de Drone na Aplicação" if idioma == "Português" else "Drone Usage in Application"))

# Entrada da quantidade de baterias por hectare
quantidade_baterias_drone = st.number_input(
    "🔋 " + (
        "Quantidade de baterias utilizadas por hectare" if idioma == "Português" else "Number of drone batteries used per hectare"),
    min_value=0.0,
    format="%.2f"
)

st.subheader("☀️ Energia Solar")

energia_solar = st.radio(
    "A fazenda utiliza energia solar que abastece a irrigação e o secador?" if idioma == "Português"
    else "Does the farm use solar energy for irrigation and dryer?",
    options=["Sim", "Não"] if idioma == "Português" else ["Yes", "No"],
    index=1,  # padrão em "Não"
    horizontal=True
)

# 🔹 Converte para booleano
usa_energia_solar = energia_solar == "Sim" if idioma == "Português" else energia_solar == "Yes"

st.subheader("🔥 " + traducao[idioma]["coffee_dryer"])

tipo_secador = st.selectbox(
    "" + traducao[idioma]["coffee_dryer_type"],
    [traducao[idioma]["direct_fire_drum_dryer"], traducao[idioma]["static_dryer"]]
)

potencia_motor_cv = st.number_input(
    "⚙️ " + traducao[idioma]["motor_power"],
    min_value=0.0,
    format="%.1f"
)

tempo_operacao_horas = st.number_input(
    "⏳ " + traducao[idioma]["operation_hours"],
    min_value=0.0,
    format="%.1f"
)

if tipo_secador == traducao[idioma]["direct_fire_drum_dryer"]:
    volume_lenha_m3 = st.number_input(
        "🪵 " + traducao[idioma]["coffee_dryer_volume"],
        min_value=0.0,
        format="%.2f"
    )

elif tipo_secador == traducao[idioma]["static_dryer"]:
    palha_secador_kg = st.number_input(
        " " + traducao[idioma]["straw_dryer"],
        min_value=0.0,
        format="%.2f"
    )

# 🔹 Inputs adicionais para irrigação (traduzidos)
st.subheader("💧 " + (traducao[idioma]["irrigation"]))

# 🔹 Opções de tipo de irrigação
tipos_irrigacao = ["Aspersor", "Gotejamento", "Pivô Central", "Microgotejamento"]

# 🔹 Valores de referência por tipo de irrigação e energia
valores_referencia_irrigacao = {
    "Aspersor": {"Diesel": 8.0, "Elétrica": 10.0},
    "Gotejamento": {"Diesel": 6.0, "Elétrica": 8.0},
    "Pivô Central": {"Diesel": 12.0, "Elétrica": 14.0},
    "Microgotejamento": {"Diesel": 5.0, "Elétrica": 7.0},
}

# 🔹 Pergunta se há irrigação
irrigacao = st.radio(
    "Há irrigação na área?" if idioma == "Português" else "Is there irrigation on the area?",
    ("Sim", "Não") if idioma == "Português" else ("Yes", "No"),
    key="radio_irrigacao"
)

# 🔹 Se houver irrigação, mostra os campos adicionais
if irrigacao == ("Sim" if idioma == "Português" else "Yes"):
    col_irri1, col_irri2 = st.columns(2)

    with col_irri1:
        tipo_irrigacao = st.selectbox(
            "Tipo de irrigação" if idioma == "Português" else "Irrigation type",
            tipos_irrigacao,
            key="tipo_irrigacao"
        )

    with col_irri2:
        tipo_geracao = st.selectbox(
            "⚡ Tipo de geração de energia para irrigação" if idioma == "Português" else "⚡ Energy generation type for irrigation",
            ["Diesel", "Elétrica"] if idioma == "Português" else ["Diesel", "Electric"],
            key="tipo_geracao_irrigacao"
        )

    usar_referencia = st.checkbox(
        f"📌 {'Usar valor de referência para' if idioma == 'Português' else 'Use reference value for'} {tipo_irrigacao} ({tipo_geracao})",
        key="checkbox_irrigacao"
    )

    # Converter tipo_geracao se idioma for inglês
    tipo_geracao_pt = tipo_geracao if idioma == "Português" else (
        "Elétrica" if tipo_geracao == "Electric" else "Diesel")

    if usar_referencia:
        emissoes_irrigacao = valores_referencia_irrigacao[tipo_irrigacao][tipo_geracao_pt]

    else:
        if tipo_geracao_pt == "Diesel":
            litros_diesel_irrigacao = st.number_input(
                "⛽ Consumo de diesel para irrigação (l ha⁻¹ ano⁻¹)" if idioma == "Português" else "⛽ Diesel consumption (l/ha/year)",
                min_value=0.0, format="%.2f", key="input_diesel_irrigacao"
            )

            if litros_diesel_irrigacao > 0:
                emissoes_totais_co2eq, emissoes_co2, emissoes_ch4, emissoes_n2o = calcular_emissoes_diesel(
                    litros_diesel_irrigacao)
                emissoes_irrigacao = emissoes_totais_co2eq
            else:
                emissoes_irrigacao = 0.0

        elif tipo_geracao_pt == "Elétrica":
            horas_por_dia = st.number_input(
                "⏳ Horas de operação da irrigação por dia" if idioma == "Português" else "⏳ Irrigation operating hours per day",
                min_value=0.0, format="%.2f", key="input_horas_dia_irrigacao"
            )

            meses_por_ano = st.number_input(
                "📆 Meses de operação por ano" if idioma == "Português" else "📆 Months of operation per year",
                min_value=0, max_value=12, format="%d", key="input_meses_irrigacao"
            )

            cv_motor_irrigacao = st.number_input(
                "⚙️ Potência do motor elétrico (cv)" if idioma == "Português" else "⚙️ Electric motor power (hp)",
                min_value=0.1, format="%.2f", key="input_cv_motor"
            )

            usa_energia_solar = st.checkbox(
                "🔋 Usa energia solar?" if idioma == "Português" else "🔋 Uses solar energy?",
                key="checkbox_energia_solar"
            )

            eficiencia_motor_padrao = 0.9
            dias_por_ano = meses_por_ano * 30
            horas_uteis_por_ano = dias_por_ano * horas_por_dia

            potencia_kw = cv_motor_irrigacao * 0.7355
            consumo_kwh_total = (potencia_kw * horas_uteis_por_ano) / eficiencia_motor_padrao

            fator_emissao_kwh = 0.0385  # kg CO2eq por kWh

            emissoes_irrigacao = 0.0 if usa_energia_solar else consumo_kwh_total * fator_emissao_kwh
else:
    emissoes_irrigacao = 0

st.subheader(traducao[idioma]["productivity_header"])

quantidade_sacas_ha = st.number_input(
    traducao[idioma]["productivity_label"],
    min_value=0.0, format="%.2f")

# Botão para calcular as emissões
if st.button(traducao[idioma]["calculate_button"]):
    # Cálculo das emissões para cada operação
    emissoes_totais_diesel = 0
    emissoes_diesel_por_operacao = {}

    for operacao, consumo in diesel_inputs.items():
        veiculo = st.session_state.get(f"veiculo_{operacao}", "")

        # Pega o valor específico do veículo selecionado
        consumo_valor = consumo.get(veiculo, 0.0)

        if veiculo in ["Triciclo", "Roçadeira Motorizada"]:
            # 🔹 Usa gasolina (sem substituição)
            emissao = calcular_emissoes_gasolina(consumo_valor)


        else:

            # 🔹 Usa diesel – considerar substituição por biogás

            percentual_fossil = (100 - percentual_biogas) / 100

            percentual_biog = percentual_biogas / 100

            fator_diesel = 2.64  # kg CO₂eq/litro

            fator_biogas = 0.4  # kg CO₂eq/litro diesel equivalente

            # 🔸 Extrai o valor numérico do dicionário

            if isinstance(consumo, dict):

                consumo_valor = list(consumo.values())[0]  # Pega o primeiro (e único) valor

            else:

                consumo_valor = consumo

            emissao_fossil = consumo_valor * percentual_fossil * fator_diesel

            emissao_biogas = consumo_valor * percentual_biog * fator_biogas

            emissao = emissao_fossil + emissao_biogas

        emissoes_totais_diesel += emissao

        emissoes_diesel_por_operacao[operacao] = emissao

    # Cálculo das emissões dos fertilizantes e resíduos
    concentracao_n1 = fertilizantes[fertilizante1]["n"]
    tipo_n1 = fertilizantes[fertilizante1]["tipo"]
    quantidade_n_fertilizante1 = (concentracao_n1 / 100) * quantidade_fertilizante1
    emissao_fertilizante1 = calcular_emissoes_fertilizante(quantidade_n_fertilizante1, tipo_n1)

    concentracao_n2 = fertilizantes[fertilizante2]["n"]
    tipo_n2 = fertilizantes[fertilizante2]["tipo"]
    quantidade_n_fertilizante2 = (concentracao_n2 / 100) * quantidade_fertilizante2
    emissao_fertilizante2 = calcular_emissoes_fertilizante(quantidade_n_fertilizante2, tipo_n2)

    quantidade_n_palha_cafe = quantidade_palha_cafe * 0.015  # 1,5 # % de N na palha de café
    quantidade_n_cama_frango = quantidade_cama_frango * 0.031  # 3,1 % de N na cama de frango
    quantidade_n_composto = quantidade_composto * (n_composto_percentual / 100)

    emissao_palha_cafe = calcular_emissoes_palha_cafe(quantidade_n_palha_cafe)
    emissao_cama_frango = calcular_emissoes_cama_frango(quantidade_n_cama_frango)
    emissao_composto = calcular_emissoes_composto(quantidade_n_composto)

    emissao_calcario1 = calcular_emissoes_calcario(quantidade_calcario1, tipo_calcario1_pt, tipo_cultivo)
    emissao_calcario2 = calcular_emissoes_calcario(quantidade_calcario2, tipo_calcario2_pt, tipo_cultivo)

    emissoes_totais_fertilizantes = (emissao_fertilizante1 + emissao_fertilizante2)
    emissoes_totais_calcario = (emissao_calcario1 + emissao_calcario2)

    emissoes_drone = calcular_emissoes_drone_por_bateria(quantidade_baterias_drone)

    if tipo_secador == traducao[idioma]["direct_fire_drum_dryer"]:
        energia_motor = 0.0 if usa_energia_solar else potencia_motor_cv * 0.7355 * tempo_operacao_horas * 0.06
        emissao_lenha = volume_lenha_m3 * 500 * 0.2
        emissao_secador = emissao_lenha + energia_motor

    elif tipo_secador == traducao[idioma]["static_dryer"]:
        energia_motor = 0.0 if usa_energia_solar else potencia_motor_cv * 0.7355 * tempo_operacao_horas * 0.06
        n_palha = palha_secador_kg * 0.031
        n2o_palha = n_palha * 0.01
        co2eq_palha = n2o_palha * (44 / 28) * 298
        emissao_secador = energia_motor + co2eq_palha

    else:
        emissao_secador = 0.0

    # Calcular total de emissões sem diesel e sem irrigação
    emissoes_totais = (emissao_fertilizante1 + emissao_fertilizante2 +
                       emissao_calcario1 + emissao_calcario2 +
                       emissao_palha_cafe + emissao_cama_frango +
                       emissoes_totais_diesel + emissoes_irrigacao + emissoes_drone + emissao_secador + emissao_composto)

    # 🔹 Cálculo de emissão por saca
    if quantidade_sacas_ha > 0:
        emissao_saca = emissoes_totais / quantidade_sacas_ha


        def limpar_texto_pdf(texto):
            if texto is None:
                return "-"
            texto = str(texto)
            substituicoes = {
                "CO₂": "CO2", "⁻¹": "-1", "⁻²": "-2", "°": "o",
                "🚁": "", "🐔": "", "🌱": "", "☕": "", "📄": "",
                "📅": "", "🚜": "", "🔋": "", "💧": "", "⚡": "",
                "⛽": "", "🔌": "", "⏳": "", "📆": "", "📊": "",
                "🔹": "", "✅": "", "📌": "",
            }
            for chave, valor in substituicoes.items():
                texto = texto.replace(chave, valor)
            return texto


        def gerar_pdf_emissoes():
            class PDF(FPDF):
                def header(self):
                    self.set_font("Arial", "B", 14)
                    self.set_fill_color(230, 230, 230)
                    self.cell(0, 12, limpar_texto_pdf(traducao[idioma]['title']), ln=True, align="C", fill=True)
                    self.ln(5)

                def chapter_title(self, title):
                    self.set_font("Arial", "B", 12)
                    self.set_text_color(0)
                    self.set_fill_color(245, 245, 245)
                    self.cell(0, 10, limpar_texto_pdf(title), ln=True, fill=True)
                    self.ln(2)

                def add_table(self, data, col_widths, align='L'):
                    line_height = 9
                    for row in data:
                        if self.get_y() + line_height > self.page_break_trigger:
                            self.add_page()
                        for i, datum in enumerate(row):
                            self.set_font("Arial", "B" if self.get_y() <= self.t_margin + 15 else "", 10)
                            self.cell(col_widths[i], line_height, limpar_texto_pdf(str(datum)), border=1, align=align)
                        self.ln()

            pdf = PDF()
            pdf.set_auto_page_break(auto=True, margin=15)
            pdf.add_page()

            # 🔹 Dados do usuário
            pdf.chapter_title(traducao[idioma]["header"])
            user_data = [
                [traducao[idioma]['Nome_produtor'], nome_produtor],
                [traducao[idioma]['propriedade'], propriedade],
                [traducao[idioma]['email'], email],
                ["Telefone" if idioma == "Português" else "Phone", telefone],
                ["Nº do CAR" if idioma == "Português" else "CAR number", numero_car],
                ["Região Cafeira" if idioma == "Português" else "Coffee Region", regiao_cafeira],
                ["Ano de Referência" if idioma == "Português" else "Reference Year", str(ano_referencia)],
                ["Cidade" if idioma == "Português" else "City", cidade],
                ["Estado (UF)" if idioma == "Português" else "State", estado],
                [traducao[idioma]['area_cafe'], f"{area_cafe:.2f} ha"],
                [traducao[idioma]['area_propriedade'], f"{area_propriedade:.2f} ha"]
            ]

            pdf.set_font("Arial", "", 10)
            pdf.add_table(user_data, [70, 110])

            # 🔹 Resumo de emissões
            pdf.chapter_title(traducao[idioma]["result_header"])
            resumo = [
                [traducao[idioma]['fertilizer_label1'], f"{emissao_fertilizante1:.2f}"],
                [traducao[idioma]['fertilizer_label2'], f"{emissao_fertilizante2:.2f}"],
                [traducao[idioma]['coffee_straw'], f"{emissao_palha_cafe:.2f}"],
                [traducao[idioma]['chicken_bedding'], f"{emissao_cama_frango:.2f}"],
                [traducao[idioma]['composto_comercial'], f"{emissao_composto:.2f}"],
                [f"{traducao[idioma]['limestone']} A", f"{emissao_calcario1:.2f}"],
                [f"{traducao[idioma]['limestone']} B", f"{emissao_calcario2:.2f}"],
                [traducao[idioma]['drone_emission'], f"{emissoes_drone:.2f}"],
                [traducao[idioma]['irrigation'], f"{emissoes_irrigacao:.2f}"],
                [f"{traducao[idioma]['dryer_label']} ({tipo_secador})", f"{emissao_secador:.2f}"],
                ["Diesel Total", f"{emissoes_totais_diesel:.2f}"],
                ["Total", f"{emissoes_totais:.2f}"],
                ["Por Saca", f"{emissao_saca:.2f}"],
                ["Por kg de café", f"{emissao_kg_cafe:.4f}"]
            ]
            pdf.add_table([["Fonte", "Emissão (kg CO2eq/ha/ano)"]] + resumo, [100, 80], align='C')

            # 🔹 Diesel por operação (em nova página)
            pdf.add_page()
            pdf.chapter_title("Diesel por Operação" if idioma == "Português" else "Diesel by Operation")
            diesel_data = [["Operação", "Emissão (kg CO2eq/ha/ano)"]]
            for op, val in emissoes_diesel_por_operacao.items():
                nome_op = op if idioma == "Português" else traducoes_diesel["English"][
                    list(traducoes_diesel["Português"]).index(op)]
                diesel_data.append([nome_op, f"{val:.2f}"])
            pdf.add_table(diesel_data, [100, 80], align='C')

            # 🔹 Parâmetros de entrada (em nova página)
            pdf.add_page()
            pdf.chapter_title("Parâmetros de Entrada" if idioma == "Português" else "Input Parameters")

            dados_parametros = {
                "Parâmetro": [
                    "Fertilizante 1", "Concentração N Fertilizante 1 (%)", "Quantidade Fertilizante 1 (kg/ha)",
                    "Fertilizante 2", "Concentração N Fertilizante 2 (%)", "Quantidade Fertilizante 2 (kg/ha)",
                    "Palha de café (kg/ha)", "Cama de frango (kg/ha)",
                    "Concentração de N no composto (%)", "Quantidade do composto (kg/ha)",
                    "Tipo de Calcário A", "Quantidade de Calcário A (kg/ha)",
                    "Tipo de Calcário B", "Quantidade de Calcário B (kg/ha)",
                    "Drone (nº baterias)", "Irrigação", "Tipo de geração de energia"
                ],
                "Valor": [
                    fertilizante1, fertilizantes[fertilizante1]["n"], quantidade_fertilizante1,
                    fertilizante2, fertilizantes[fertilizante2]["n"], quantidade_fertilizante2,
                    quantidade_palha_cafe, quantidade_cama_frango,
                    n_composto_percentual, quantidade_composto,
                    tipo_calcario1_pt, quantidade_calcario1,
                    tipo_calcario2_pt, quantidade_calcario2,
                    quantidade_baterias_drone, irrigacao, tipo_geracao
                ]
            }

            # 🔹 Secador: adiciona dados conforme o tipo
            if tipo_secador == traducao[idioma]["direct_fire_drum_dryer"]:
                dados_parametros["Parâmetro"] += [
                    "Secador: Tipo", "Volume de Lenha (m³/saca/ha/ano)", "Potência do Motor (cv)",
                    "Horas de Operação (sacas/ha/ano)"
                ]
                dados_parametros["Valor"] += [
                    tipo_secador, volume_lenha_m3, potencia_motor_cv, tempo_operacao_horas
                ]
            elif tipo_secador == traducao[idioma]["static_dryer"]:
                dados_parametros["Parâmetro"] += [
                    "Secador: Tipo", "Palha Consumida (kg/saca/ha/ano)", "Potência do Motor (cv)",
                    "Horas de Operação (sacas/ha/ano)"
                ]
                dados_parametros["Valor"] += [
                    tipo_secador, palha_secador_kg, potencia_motor_cv, tempo_operacao_horas
                ]

            # 🔹 Potência do motor ou diesel na irrigação
            dados_parametros["Parâmetro"] += [
                "Potência do motor elétrico (cv)" if tipo_geracao in ["Elétrica", "Electric"] else "Litros de diesel",
                "Sacas/ha"
            ]
            dados_parametros["Valor"] += [
                cv_motor_irrigacao if tipo_geracao in ["Elétrica", "Electric"] else litros_diesel_irrigacao,
                quantidade_sacas_ha
            ]

            # 🔹 Monta tabela e adiciona ao PDF
            df_parametros = pd.DataFrame(dados_parametros)
            parametros_data = df_parametros.values.tolist()
            pdf.add_table([["Parâmetro", "Valor"]] + parametros_data, [100, 80], align='L')

            import tempfile
            import os
            import time
            import matplotlib.pyplot as plt

            # 🔹 Gráfico de Pizza
            with tempfile.NamedTemporaryFile(delete=False, suffix=".png") as temp_pizza:
                temp_pizza_path = temp_pizza.name

                # Aumentar fonte das porcentagens no gráfico
                for text in fig.axes[0].texts:
                    text.set_fontsize(40)

                # Aumentar fonte da legenda
                legenda = fig.axes[0].get_legend()
                if legenda:
                    legenda.set_title("Fontes", prop={"size": 40})
                    for text in legenda.get_texts():
                        text.set_fontsize(40)

                # Aumentar título do gráfico se houver
                if fig.axes[0].get_title():
                    fig.axes[0].title.set_fontsize(50)

                # Aumentar ticks do eixo (se existirem)
                fig.axes[0].tick_params(labelsize=40)

                # Salvar imagem com alta qualidade
                fig.savefig(temp_pizza_path, bbox_inches="tight", dpi=400)

            # Adicionar gráfico ao PDF
            pdf.add_page()
            pdf.image(temp_pizza_path, x=10, y=30, w=195)

            # Remover arquivo temporário
            time.sleep(0.5)
            try:
                os.remove(temp_pizza_path)
            except PermissionError:
                pass

            # 🔹 Gráfico de Balanço de Carbono
            with tempfile.NamedTemporaryFile(delete=False, suffix=".png") as temp_barra:
                temp_barra_path = temp_barra.name

                # Aumentar título e ticks
                fig_barra.axes[0].title.set_fontsize(30)
                fig_barra.axes[0].tick_params(labelsize=18)

                # Aumentar textos dentro das barras (valores)
                for text in fig_barra.axes[0].texts:
                    text.set_fontsize(16)

                # Corrigir sobreposição: aumenta o espaçamento à esquerda dos labels
                fig_barra.subplots_adjust(left=0.20)

                # Salvar imagem
                fig_barra.savefig(temp_barra_path, bbox_inches="tight", dpi=400)
                plt.close(fig_barra)

            # Inserir no PDF
            pdf.add_page()
            pdf.set_font("Helvetica", size=22, style='B')
            pdf.cell(0, 16, "", ln=True, align="C")
            pdf.ln(6)
            pdf.image(temp_barra_path, x=7, w=195)

            # Remover imagem temporária
            time.sleep(0.5)
            try:
                os.remove(temp_barra_path)
            except PermissionError:
                pass

            # 🔹 Download
            with tempfile.NamedTemporaryFile(delete=False, suffix=".pdf") as tmp_pdf:
                pdf.output(tmp_pdf.name)
                with open(tmp_pdf.name, "rb") as f:
                    st.download_button(
                        label="📄 Baixar PDF com Resultados" if idioma == "Português" else "📄 Download PDF Results",
                        data=f,
                        file_name=f"emissoes_cafe{datetime.now():%Y%m%d_%H%M%S}.pdf",
                        mime="application/pdf",
                        key="download_pdf"
                    )


        def gerar_excel_e_botao_download():
            import os
            import io
            import pandas as pd
            import streamlit as st
            from datetime import datetime

            dados_usuario = {
                "Nome do Produtor": [nome_produtor],
                "Nome da Propriedade": [propriedade],
                "Email": [email],
                "CAR": [numero_car],
                "Telefone": [telefone],
                "Região Cafeira": [regiao_cafeira],
                "Ano de Referência": [ano_referencia],
                "Cidade": [cidade],
                "Estado (UF)": [estado],
                "Área de café (ha)": [area_cafe],
                "Área total da propriedade (ha)": [area_propriedade],
            }

            df_usuario = pd.DataFrame(dados_usuario)

            categorias_resumo = [
                traducao[idioma]['fertilizer_label1'],
                traducao[idioma]['fertilizer_label2'],
                traducao[idioma]['coffee_straw'],
                traducao[idioma]['chicken_bedding'],
                traducao[idioma]['composto_comercial'],
                f"{traducao[idioma]['limestone']} A",
                f"{traducao[idioma]['limestone']} B",
                traducao[idioma]['drone_emission'],
                traducao[idioma]['irrigation'],
                f"{traducao[idioma]['dryer_label']} ({tipo_secador})",
                "Diesel Total",
                "Total Emissões",
                "Emissão por saca"
            ]
            valores = [
                round(emissao_fertilizante1, 2),
                round(emissao_fertilizante2, 2),
                round(emissao_palha_cafe, 2),
                round(emissao_cama_frango, 2),
                round(emissao_composto, 2),
                round(emissao_calcario1, 2),
                round(emissao_calcario2, 2),
                round(emissoes_drone, 2),
                round(emissoes_irrigacao, 2),
                round(emissao_secador, 2),
                round(emissoes_totais_diesel, 2),
                round(emissoes_totais, 2),
                round(emissao_saca, 2)
            ]
            df_resumo = pd.DataFrame({
                "Categoria": categorias_resumo,
                "Emissão (kg CO₂eq/ha/ano)": valores
            })

            if emissoes_diesel_por_operacao:
                df_diesel = pd.DataFrame(list(emissoes_diesel_por_operacao.items()),
                                         columns=["Operação", "Emissão (kg CO₂eq/ha/ano)"])
            else:
                df_diesel = pd.DataFrame(columns=["Operação", "Emissão (kg CO₂eq/ha/ano)"])

            dados_parametros = {
                "Parâmetro": [
                    "Fertilizante 1", "Concentração N Fertilizante 1 (%)", "Quantidade Fertilizante 1 (kg/ha)",
                    "Fertilizante 2", "Concentração N Fertilizante 2 (%)", "Quantidade Fertilizante 2 (kg/ha)",
                    "Palha de café (kg/ha)", "Cama de frango (kg/ha)",
                    "Concentração de N no composto (%)", "Quantidade do composto (kg/ha)",
                    "Tipo de Calcário A", "Quantidade de Calcário A (kg/ha)",
                    "Tipo de Calcário B", "Quantidade de Calcário B (kg/ha)",
                    "Drone (nº baterias)", "Irrigação", "Tipo de geração de energia"
                ],
                "Valor": [
                    fertilizante1, fertilizantes[fertilizante1]["n"], quantidade_fertilizante1,
                    fertilizante2, fertilizantes[fertilizante2]["n"], quantidade_fertilizante2,
                    quantidade_palha_cafe, quantidade_cama_frango,
                    n_composto_percentual, quantidade_composto,
                    tipo_calcario1_pt, quantidade_calcario1,
                    tipo_calcario2_pt, quantidade_calcario2,
                    quantidade_baterias_drone, irrigacao, tipo_geracao
                ]
            }

            # 🔹 Secador: adiciona dados conforme o tipo
            if tipo_secador == traducao[idioma]["direct_fire_drum_dryer"]:
                dados_parametros["Parâmetro"] += [
                    "Secador: Tipo", "Volume de Lenha (m³/saca/ha/ano)", "Potência do Motor (cv)",
                    "Horas de Operação (sacas/ha/ano)"
                ]
                dados_parametros["Valor"] += [
                    tipo_secador, volume_lenha_m3, potencia_motor_cv, tempo_operacao_horas
                ]
            elif tipo_secador == traducao[idioma]["static_dryer"]:
                dados_parametros["Parâmetro"] += [
                    "Secador: Tipo", "Palha Consumida (kg/saca/ha/ano)", "Potência do Motor (cv)",
                    "Horas de Operação (sacas/ha/ano)"
                ]
                dados_parametros["Valor"] += [
                    tipo_secador, palha_secador_kg, potencia_motor_cv, tempo_operacao_horas
                ]

            # 🔹 Potência do motor ou diesel da irrigação + produtividade
            dados_parametros["Parâmetro"] += [
                "Potência do motor elétrico (cv)" if tipo_geracao in ["Elétrica", "Electric"] else "Litros de diesel",
                "Sacas/ha"
            ]
            dados_parametros["Valor"] += [
                cv_motor_irrigacao if tipo_geracao in ["Elétrica", "Electric"] else litros_diesel_irrigacao,
                quantidade_sacas_ha
            ]

            df_parametros = pd.DataFrame(dados_parametros)

            excel_buffer = io.BytesIO()
            with pd.ExcelWriter(excel_buffer, engine='xlsxwriter') as writer:
                df_usuario.to_excel(writer, sheet_name="Informações do Usuário", index=False)
                df_resumo.to_excel(writer, sheet_name="Resumo das Emissões", index=False)
                df_diesel.to_excel(writer, sheet_name="Diesel por Operação", index=False)
                df_parametros.to_excel(writer, sheet_name="Parâmetros de Entrada", index=False)

                workbook = writer.book
                worksheet = writer.sheets["Resumo das Emissões"]
                formato_num = workbook.add_format({'num_format': '#,##0.00', 'align': 'center'})
                worksheet.set_column("A:A", 30)
                worksheet.set_column("B:B", 25, formato_num)

                worksheet.conditional_format("B2:B12", {
                    'type': '3_color_scale',
                    'min_color': "#63BE7B", 'mid_color': "#FFEB84", 'max_color': "#F8696B"
                })

                # 🔹 Inserir dados agregados nas linhas 14 a 19 da aba "Resumo das Emissões"
                categorias_agregadas = [
                    "Fertilizante Sintético (N)",
                    "Calcário",
                    "Palha de Café",
                    "Cama de Frango",
                    "Secador",
                    "Consumo de Combustíveis Fósseis",
                    "Irrigação"
                ]

                valores_agregados = [
                    round(emissao_fertilizante1 + emissao_fertilizante2, 2),
                    round(emissao_calcario1 + emissao_calcario2, 2),
                    round(emissao_palha_cafe, 2),
                    round(emissao_cama_frango, 2),
                    round(emissao_secador, 2),
                    round(emissoes_totais_diesel, 2),
                    round(emissoes_irrigacao, 2)
                ]
                linha_inicial = 13  # Linha 14 (índice 0)
                for i, (cat, val) in enumerate(zip(categorias_agregadas, valores_agregados)):
                    worksheet.write(linha_inicial + i, 0, cat)
                    worksheet.write(linha_inicial + i, 1, val)

                # 🔹 Gráfico de Pizza na aba "Resumo das Emissões"
                chart = workbook.add_chart({'type': 'pie'})
                chart.add_series({
                    'name': 'Fontes de Emissão',
                    'categories': ['Resumo das Emissões', linha_inicial, 0, linha_inicial + 5, 0],
                    'values': ['Resumo das Emissões', linha_inicial, 1, linha_inicial + 5, 1],
                    'data_labels': {
                        'percentage': True,
                        'font': {'bold': True, 'size': 14},
                        'leader_lines': True
                    },
                    'points': [
                        {'fill': {'color': '#FF4C4C'}, 'explode': True},
                        {'fill': {'color': '#FF7F50'}, 'explode': True},
                        {'fill': {'color': '#FFD700'}, 'explode': True},
                        {'fill': {'color': '#FFA07A'}, 'explode': True},
                        {'fill': {'color': '#FF69B4'}, 'explode': True},
                        {'fill': {'color': '#DA70D6'}, 'explode': True}
                    ]
                })
                chart.set_title({'name': 'Fontes de Emissão de Carbono'})
                chart.set_style(10)
                worksheet.insert_chart("D2", chart, {'x_scale': 2.2, 'y_scale': 2.2})

                # 🔹 Balanço de Carbono
                df_balanco = pd.DataFrame({
                    "Indicador": [
                        "Emissão de CO₂ (kg CO₂eq/ha/ano)",
                        "Sequestro de CO₂ na planta (kg CO₂eq/ha/ano)",
                        "Balanço de CO₂ (kg CO₂eq/ha/ano)"
                    ],
                    "Valor": [
                        round(emissoes_totais, 1),
                        round(-carbono_armazenado, 1),
                        round(-carbono_armazenado + emissoes_totais, 1)
                    ]
                })
                df_balanco.to_excel(writer, sheet_name="Balanço de Carbono", index=False)

                sheet_balanco = writer.sheets["Balanço de Carbono"]
                sheet_balanco.set_column("A:A", 50)
                sheet_balanco.set_column("B:B", 30, formato_num)

                chart_bar = workbook.add_chart({'type': 'bar'})
                chart_bar.add_series({
                    'name': 'Balanço de Carbono',
                    'categories': ['Balanço de Carbono', 1, 0, 3, 0],
                    'values': ['Balanço de Carbono', 1, 1, 3, 1],
                    'data_labels': {'value': True, 'font': {'bold': True}},
                    'points': [
                        {'fill': {'color': '#F94C4C'}},
                        {'fill': {'color': '#B2F3E5'}},
                        {'fill': {'color': '#006400'}}
                    ]
                })
                chart_bar.set_title({'name': 'Balanço de Carbono'})
                chart_bar.set_x_axis({'name': 'kg CO₂eq/ha/ano'})
                chart_bar.set_y_axis({'reverse_categories': True})
                chart_bar.set_style(10)
                sheet_balanco.insert_chart("D2", chart_bar, {'x_scale': 2.2, 'y_scale': 2.0})

            st.download_button(
                label="📅 Baixar Excel com Resultados" if idioma == "Português" else "📅 Download Excel Results",
                data=excel_buffer.getvalue(),
                file_name=f"emissoes_cafe{datetime.now():%Y%m%d_%H%M%S}.xlsx",
                mime="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
                key="download_excel"
            )

    # 🔹 Exibir resultados das emissões (Traduzido)
    st.subheader(traducao[idioma]["result_header"])

    st.write(
        f"{traducao[idioma]['fertilizer_label1']}: {str(f'{emissao_fertilizante1:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['fertilizer_label2']}: {str(f'{emissao_fertilizante2:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['coffee_straw']}: {str(f'{emissao_palha_cafe:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['chicken_bedding']}: {str(f'{emissao_cama_frango:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['composto_comercial']}: {str(f'{emissao_composto:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['limestone']} A: {str(f'{emissao_calcario1:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['limestone']} B: {str(f'{emissao_calcario2:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['drone_emission']}: {str(f'{emissoes_drone:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")

    # 🔹 Exibir emissões de Diesel (Traduzido)
    for operacao, emissao in emissoes_diesel_por_operacao.items():
        nome_operacao = operacao if idioma == "Português" else traducoes_diesel["English"][
            list(traducoes_diesel["Português"]).index(operacao)]
        st.write(f"{nome_operacao}: {str(f'{emissao:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")

    # 🔹 Emissões da irrigação
    st.write(
        f"{traducao[idioma]['irrigation']}: {str(f'{emissoes_irrigacao:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹")
    st.write(
        f"{traducao[idioma]['coffee_dryer']} ({traducao[idioma]['direct_fire_drum_dryer']}): {str(f'{emissao_secador:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹"
    )

    # 🔹 Emissões Totais
    st.markdown(f"""
        <div style="
            background-color:#e6ffed;
            border-left: 6px solid #2e7d32;
            padding: 16px;
            margin-top: 24px;
            font-size: 26px;
            font-weight: bold;
            color: #1b5e20;
            border-radius: 6px;
        ">
             {traducao[idioma]['result_header']}: {str(f'{emissoes_totais:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹ ano⁻¹
        </div>
    """, unsafe_allow_html=True)

    # 🔹 Emissões da Produtividade (por saca)
    st.markdown(f"""
        <div style="
            background-color:#e6f0ff;
            border-left: 6px solid #3366cc;
            padding: 16px;
            margin-top: 16px;
            font-size: 24px;
            font-weight: bold;
            color: #003366;
            border-radius: 6px;
        ">
             {traducao[idioma]["emission_per_bag"].split(":")[0]}: {str(f'{emissao_saca:.2f}').replace('.', ',')} kg CO₂eq saca⁻¹
        </div>
    """, unsafe_allow_html=True)

    # 🔹 Emissões por kg de café
    emissao_kg_cafe = emissoes_totais / (quantidade_sacas_ha * 60) if quantidade_sacas_ha > 0 else 0
    st.markdown(f"""
        <div style="
            background-color:#fff4e6;
            border-left: 6px solid #ff9800;
            padding: 16px;
            margin-top: 16px;
            font-size: 24px;
            font-weight: bold;
            color: #e65100;
            border-radius: 6px;
        ">
             {"☕ Emissão por kg de café" if idioma == "Português" else "☕ Emissions per kg of coffee"}: {str(f'{emissao_kg_cafe:.4f}').replace('.', ',')} kg CO₂eq kg⁻¹
        </div>
    """, unsafe_allow_html=True)

    # 🔹 Carbono Armazenado
    carbono_armazenado = num_plantas * 2.06
    st.markdown(f"""
        <div style="
            background-color:#f0f0f0;
            border-left: 6px solid #00695c;
            padding: 16px;
            margin-top: 16px;
            font-size: 24px;
            font-weight: bold;
            color: #003d33;
            border-radius: 6px;
        ">
             {"🌳 Sequestro de CO₂ na planta " if idioma == "Português" else "🌳 Carbon Stored per Hectare"}: {str(f'{carbono_armazenado:.2f}').replace('.', ',')} kg CO₂eq ha⁻¹
        </div>
    """, unsafe_allow_html=True)

    import matplotlib.pyplot as plt

    import numpy as np

    # 🔹 Lista de emissões (agora com o secador)
    emissoes_graficos = [
        emissoes_totais_fertilizantes,
        emissoes_totais_calcario,
        emissao_palha_cafe,
        emissao_cama_frango,
        emissoes_totais_diesel,
        emissoes_irrigacao,
        emissoes_drone,
        emissao_secador  # ✅ NOVO
    ]

    total_emissoes = sum(emissoes_graficos)

    categorias = {
        "Português": [
            "Fertilizante Sintético (N)",
            "Calcário",
            "Palha de Café",
            "Cama de Frango",
            "Consumo de Combustíveis Fósseis",
            "Irrigação",
            "Drone",
            "Secador"  # ✅ NOVO
        ],
        "English": [
            "Synthetic Fertilizer (N)",
            "Limestone",
            "Coffee Straw",
            "Chicken Bedding",
            "Fossil Fuel Consumption",
            "Irrigation",
            "Drone",
            "Dryer"  # ✅ NOVO
        ]
    }

    # 🔹 Filtrar fontes relevantes
    emissoes_filtradas = []
    categorias_filtradas = []

    for nome, valor in zip(categorias[idioma], emissoes_graficos):
        percentual = (valor / total_emissoes) * 100 if total_emissoes > 0 else 0
        if percentual > 0.01:
            emissoes_filtradas.append(valor)
            categorias_filtradas.append(f"{nome} ({percentual:.1f}%)")

    # 🔹 Paleta de cores personalizada
    paleta_cores = [
        "#FF4C4C", "#FF7F50", "#FFB347", "#FFD700", "#FF69B4",
        "#DA70D6", "#CD5C5C", "#FF6347", "#DB7093"
    ]
    cores_ordenadas = [paleta_cores[i] for i in range(len(emissoes_filtradas))]

    # 🔹 Explode
    explode_values = [0.2] * len(emissoes_filtradas)

    # 🔹 Criar gráfico
    fig, ax = plt.subplots(figsize=(24, 20))

    wedges, _ = ax.pie(
        emissoes_filtradas,
        labels=None,
        startangle=200,
        explode=explode_values,
        colors=cores_ordenadas,
        wedgeprops=dict(width=0.42, edgecolor='white')
    )

    # 🔹 Desenhar os textos manualmente dentro das fatias explodidas
    for i, (wedge, explode) in enumerate(zip(wedges, explode_values)):
        ang = (wedge.theta2 + wedge.theta1) / 2
        ang_rad = np.deg2rad(ang)

        # Ajuste do raio levando em conta o explode
        base_raio = wedge.r - wedge.width / 2  # Centro do anel
        raio_ajustado = base_raio + explode  # Considera deslocamento

        x = np.cos(ang_rad) * raio_ajustado
        y = np.sin(ang_rad) * raio_ajustado

        percentual = (emissoes_filtradas[i] / total_emissoes) * 100
        ax.text(
            x, y, f"{percentual:.1f}%",
            ha='center', va='center',
            fontsize=28, fontweight='bold', color='black'
        )

    # 🔹 Título
    ax.set_title(
        "Fontes de Emissões de Carbono" if idioma == "Português" else "Carbon Emission Sources",
        fontsize=36, weight='bold', pad=50
    )

    # 🔹 Legenda
    ax.legend(
        wedges,
        categorias_filtradas,
        title="Fontes" if idioma == "Português" else "Sources",
        loc="center left",
        bbox_to_anchor=(1.1, 0.5),
        fontsize=32,
        title_fontsize=34,
        labelspacing=1.2,
        frameon=True,
        borderpad=1.5
    )

    # 🔹 Layout geral
    plt.subplots_adjust(left=0.05, right=0.8)

    # 🔹 Espaçamento visual maior antes do gráfico de pizza
    st.markdown("<br><br><br><br>", unsafe_allow_html=True)
    st.pyplot(fig, use_container_width=True)

    # 🔹 Gráfico final com ordem: Balanço → Sequestro → Emissão (de cima pra baixo)
    balanco_carbono = carbono_armazenado - emissoes_totais

    labels_barra = [
        "Balanço de CO₂" if idioma == "Português" else "CO₂ Balance",
        "Sequestro de CO₂ na planta" if idioma == "Português" else "CO₂ Sequestration in Plant",
        "Emissão de CO₂" if idioma == "Português" else "CO₂ Emissions",
    ]
    valores_barra = [
        -balanco_carbono,
        -carbono_armazenado,
        emissoes_totais,
    ]
    cores_barra = ["#006400", "#A7F3D0", "#FF3333"]

    import numpy as np

    fig_barra, ax_barra = plt.subplots(figsize=(14, 6))

    # 🔹 Criação das barras
    y_pos = np.arange(len(labels_barra))
    barras = ax_barra.barh(y_pos, valores_barra, color=cores_barra, height=0.5, edgecolor='black')

    # 🔹 Rótulos do eixo Y com fonte moderna, grande, espaçamento perfeito
    from matplotlib import rcParams

    rcParams['axes.unicode_minus'] = False  # Evita problemas com sinais negativos

    # Define os ticks verticais manualmente
    ax_barra.set_yticks(y_pos)

    # Aplica fonte moderna e destaque visual
    ax_barra.set_yticklabels(
        labels_barra,
        fontname='DejaVu Sans',
        fontsize=35,
        fontweight='heavy',
        color='black',
        verticalalignment='center'
    )

    # Remove as marcas dos ticks e refina estilo
    ax_barra.tick_params(
        axis='y',
        labelsize=26,
        labelcolor='black',
        length=0,  # Remove o "tracinho"
        pad=4,  # Espaço entre rótulo e barra
        direction='out'  # Direção do texto
    )

    # (Opcional) Aumenta o espaçamento geral entre as barras
    ax_barra.set_ylim(-0.5, len(labels_barra) - 0.5)

    # 🔹 Título do gráfico
    ax_barra.set_title(
        "Balanço de Carbono" if idioma == "Português" else "Carbon Balance",
        fontsize=24, fontweight='bold', pad=20
    )

    # 🔹 Adiciona os valores das barras de forma clara (fora das barras)
    for i, barra in enumerate(barras):
        largura = barra.get_width()
        alinhamento = 'left' if largura > 0 else 'right'
        deslocamento = 150 if largura > 0 else -150
        ax_barra.text(
            largura + deslocamento,
            barra.get_y() + barra.get_height() / 2,
            f"{largura:,.1f}".replace(".", ","),
            ha=alinhamento,
            va='center',
            fontsize=16,
            fontweight='bold',
            color='black'
        )

    # 🔹 Estilo dos eixos e visual clean
    ax_barra.set_xlabel(
        "Sequestro e Emissões (kg CO₂eq ha⁻¹ ano⁻¹)" if idioma == "Português"
        else "Sequestration and Emissions (kg CO₂eq ha⁻¹ year⁻¹)",
        fontsize=16,
        labelpad=15
    )
    ax_barra.set_xlim(min(-carbono_armazenado * 1.3, -1000), emissoes_totais * 1.3)
    ax_barra.axvline(0, color='gray', linewidth=1.2, linestyle='--')
    ax_barra.grid(axis='x', linestyle=':', alpha=0.4)
    ax_barra.spines[['top', 'right', 'left']].set_visible(False)
    ax_barra.tick_params(axis='both', labelsize=13)

    # ✅ Ordem correta: Balanço em cima, Sequestro no meio, Emissão embaixo
    ax_barra.invert_yaxis()

    # 🔹 Mostrar no Streamlit
    st.markdown("<br><br><br><br>", unsafe_allow_html=True)
    st.pyplot(fig_barra)

    # 🔹 Espaço entre o gráfico e os botões
    st.markdown("<br><br><br>", unsafe_allow_html=True)

    # ✅ Agora sim, fig está definido
    gerar_pdf_emissoes()

    # Chamada da função (por exemplo, após clicar em "Calcular Emissões")
    gerar_excel_e_botao_download()












