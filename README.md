# === Configuração da página ===
st.set_page_config(
    page_title="Apresentação - Controle de Qualidade Cavaco Biomassa",
    layout="wide",
    page_icon="🌳"
)

# === Dados principais ===
dados_principais = {
    "Data": "07/06/2025",
    "Responsável": "José Moraes Bernardo",
    "Turno": "B",
    "Fornecedor": "JUNGES & BOHN",
    "Densidade (g)": 344.09,
    "Peso da Amostra (g)": 3938.92,
    "% de Casca": 15.40,
    "Umidade (%)": 49.05
}

# === Resumo em texto ===
st.markdown("""
# 🌳 Controle de Qualidade - Cavaco para Biomassa  
### Fornecedor: **JUNGES & BOHN**  
---
#### Resumo da Análise
O controle de qualidade realizado em 07/06/2025, sob responsabilidade de José Moraes Bernardo (Turno B), avaliou uma amostra de cavaco para biomassa.  
A densidade da amostra foi de **344,09 g**, com peso total de **3938,92 g**. A amostra apresentou uma fração de **casca** de **15,40%** e **umidade** de **49,05%**, valores adequados para o processo de geração de energia.  
A granulometria indica predominância de partículas médias e finas, com baixa presença de impurezas como pedra, plástico ou vidro.  
Os resultados demonstram boa qualidade do material fornecido, sendo adequado para uso como biomassa.
""")

# === Exibição dos dados principais em cards ===
st.markdown("## 📋 Dados da Amostra")
col1, col2, col3, col4 = st.columns(4)
col1.metric("Densidade (g)", f"{dados_principais['Densidade (g)']:.2f}")
col2.metric("Peso Amostra (g)", f"{dados_principais['Peso da Amostra (g)']:.2f}")
col3.metric("% de Casca", f"{dados_principais['% de Casca']}%")
col4.metric("Umidade", f"{dados_principais['Umidade (%)']}%")
st.divider()

# === Tabela de Granulometria ===
gran_classes = ["Acima de 50 mm", "37,5 mm", "25 mm", "16 mm", "6,3 mm", "2 mm"]
gran_values = [91.98, 217.07, 1710.63, 1162.54, 712.73, 34.41]
granulometria_df = pd.DataFrame({
    "Classe (mm)": gran_classes,
    "Peso (g)": gran_values
})

# === Gráfico de barras da granulometria ===
st.markdown("## 📊 Distribuição Granulométrica")
colg1, colg2 = st.columns([1, 2])
colg1.dataframe(granulometria_df, hide_index=True)
fig, ax = plt.subplots(figsize=(7, 3))
ax.bar(granulometria_df["Classe (mm)"], granulometria_df["Peso (g)"], color="#007849")
ax.set_xlabel("Granulometria (mm)")
ax.set_ylabel("Peso (g)")
ax.set_title("Distribuição dos Tamanhos de Partículas")
colg2.pyplot(fig)
st.divider()

# === Tabela de Classificação ===
classificacoes = [
    "Prato", "Cavacos", "Finos", "Lasca", "Grosso", "Pedra", "Plástico", "Partículas", "Vidro", "Casca"
]
classificacoes_peso = [9.56, 2850.81, 43.97, 301.65, 91.98, 0.00, 0.00, 34.41, 0.00, 606.54]
classificacoes_pct = [0.24, 72.38, 1.12, 7.66, 2.34, 0.00, 0.00, 0.87, 0.00, 15.40]

classificacoes_df = pd.DataFrame({
    "Classificação": classificacoes,
    "Peso (g)": classificacoes_peso,
    "%": classificacoes_pct
})

# === Gráfico de pizza das classificações ===
st.markdown("## 🥧 Classificação dos Componentes da Amostra")
colp1, colp2 = st.columns([1,2])
colp1.dataframe(classificacoes_df, hide_index=True)

# Gráfico de pizza para componentes com peso > 0
mask = [peso > 0 for peso in classificacoes_peso]
fig2, ax2 = plt.subplots(figsize=(5, 5))
ax2.pie(
    [p for i,p in enumerate(classificacoes_peso) if mask[i]],
    labels=[c for i,c in enumerate(classificacoes) if mask[i]],
    autopct='%1.1f%%',
    startangle=90,
    colors=plt.cm.Greens(range(100, 256, int(156/sum(mask))))
)
ax2.set_title("Composição da Amostra (%)")
colp2.pyplot(fig2)

# === Observações finais ===
st.divider()
st.markdown("""
### 📝 Observações Finais
- **Material bem classificado**, com predominância de cavacos e baixa presença de finos/impurezas.
- **Umidade dentro do esperado** para biomassa de qualidade.
- **Casca em proporção adequada** (15,4%), não comprometendo o rendimento energético.
- **Baixa quantidade de contaminantes** (pedra, plástico, vidro: 0%).

*App gerado automaticamente para apresentação de resultados de controle de qualidade de biomassa.*
""")
