1. Importação de Bibliotecas


import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from sklearn.linear_model import LinearRegression
import plotly.graph_objects as go


* Objetivo: Importar as bibliotecas necessárias para manipulação de dados, visualização gráfica e modelagem preditiva.
   * pandas: Manipulação e análise de dados.
   * matplotlib: Criação de gráficos estáticos.
   * numpy: Operações matemáticas e manipulação de arrays.
   * sklearn.linear_model.LinearRegression: Regressão linear para previsões.
   * plotly.graph_objects: Criação de tabelas e gráficos interativos.
________________


2. Carregamento dos Dados


file_path = 'boletim.xls'
df = pd.read_excel(file_path, engine='xlrd', header=1, sheet_name=1)
print(df.columns)


* Descrição: O arquivo de dados é carregado usando o Pandas com o motor xlrd.
   * header=1: Indica que o cabeçalho está na segunda linha.
   * sheet_name=1: Seleciona a segunda aba do arquivo.
   * O comando print(df.columns) exibe os nomes das colunas para análise inicial.
________________


3. Pré-visualização e Filtragem dos Dados
python
CopiarEditar
df.sample(10)
df['id_uf'] == 'SP'
df_sp = df[df['id_uf'] == 'SP']


* Objetivo:
   * df.sample(10): Visualiza 10 amostras aleatórias do dataset.
   * df['id_uf'] == 'SP': Filtra apenas os registros do estado de São Paulo.
   * df_sp: Cria um subconjunto contendo somente os dados de São Paulo.
________________


4. Análise para o Ano de 2024
python
CopiarEditar
df_sp_2024 = df_sp[df_sp['ANO2'] == 2024]
meses_presentes_2024 = df_sp_2024['MÊS'].unique()
df_mes_presentes = df_sp_2024[df_sp_2024['MÊS'].isin(meses_presentes_2024)]
df_mes_presentes_agrupados = df_mes_presentes.groupby('MÊS')['va_outros_tributos_ipva'].sum().reset_index()
print(df_mes_presentes_agrupados)


* Descrição:
   * Filtra os dados de São Paulo para o ano de 2024.
   * Identifica os meses disponíveis e soma os valores de IPVA para cada mês.
   * Exibe os dados agrupados.
________________


5. Análise Anual e Identificação de Anos Completos


df_sp_ano_mes = df_sp.groupby(['ANO2', 'MÊS'])
anos_com_meses_completos = df_sp_ano_mes.groupby('ANO2')['MÊS'].count().reset_index()
anos_completos = anos_com_meses_completos[anos_com_meses_completos['MÊS'] == 12]['ANO2']
df_ano_ipva = df_sp[df_sp['ANO2'].isin(anos_completos)].groupby('ANO2')['va_outros_tributos_ipva'].sum().reset_index()
print(df_ano_ipva)


* Objetivo:
   * Verifica quais anos possuem registros completos (12 meses).
   * Filtra os anos completos e calcula o total de IPVA arrecadado por ano.
________________


6. Projeção de Dados com Regressão Linear


X = df_ano_ipva_clean['ANO2'].values.reshape(-1, 1)
y = df_ano_ipva_clean['va_outros_tributos_ipva'].values
modelo = LinearRegression()
modelo.fit(X, y)
anos_futuros = np.array([2024, 2025, 2026]).reshape(-1, 1)
previsao_futura = modelo.predict(anos_futuros)


* Descrição:
   * Treina um modelo de regressão linear para prever valores de IPVA para os anos de 2024, 2025 e 2026.
________________


7. Visualização dos Resultados


plt.plot(df_ano_ipva_clean['ANO2'], df_ano_ipva_clean['va_outros_tributos_ipva'], marker='o', color='g', linestyle='-', label='Valor Absoluto do IPVA (Histórico)')
plt.plot([2024, 2025, 2026], previsao_futura, marker='x', color='r', linestyle='--', label='Projeção')
plt.title('Projeção do IPVA - Estado de São Paulo')
plt.show()


* Objetivo: Gera um gráfico que combina os dados históricos com as previsões futuras.