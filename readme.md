# Projeto de Previsão de Preços de Imóveis na Califórnia

Este projeto consiste na construção de um modelo de Machine Learning para prever o valor médio de imóveis em distritos da Califórnia, utilizando dados do censo de 1990. O fluxo de trabalho abrange desde a análise exploratória e pré-processamento dos dados até o treinamento, otimização e avaliação de múltiplos modelos de regressão.

O modelo final, um `RandomForestRegressor` otimizado, foi capaz de prever os preços com um erro médio de **$46,885.11**.

---

## Sumário

- [Visão Geral do Projeto](#-visão-geral-do-projeto)
- [O Dataset](#-o-dataset)
- [Fluxo de Trabalho](#-fluxo-de-trabalho)
  - [1. Análise Exploratória de Dados (EDA)](#1-análise-exploratória-de-dados-eda)
  - [2. Pré-processamento e Engenharia de Atributos](#2-pré-processamento-e-engenharia-de-atributos)
  - [3. Treinamento e Seleção de Modelos](#3-treinamento-e-seleção-de-modelos)
  - [4. Otimização do Modelo](#4-otimização-do-modelo)
- [Resultados Finais](#-resultados-finais)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Como Executar o Projeto](#-como-executar-o-projeto)

---

## Visão Geral do Projeto

O objetivo principal é desenvolver um sistema que estime o valor mediano de uma casa (`median_house_value`) em um determinado distrito da Califórnia com base em um conjunto de características, como localização, idade média dos imóveis e demografia. Este é um problema clássico de regressão.

## O Dataset

O conjunto de dados utilizado é o "California Housing Prices", baseado em dados do censo de 1990.

- **Alvo:** `median_house_value`
- **Características:** Incluem `longitude`, `latitude`, `housing_median_age`, `total_rooms`, `total_bedrooms`, `population`, `households`, `median_income` e `ocean_proximity`.

---

## Fluxo de Trabalho

O projeto foi estruturado em quatro fases principais:

### 1. Análise Exploratória de Dados (EDA)

- **Inspeção Inicial:** Foi realizada uma análise para entender a estrutura dos dados, identificar valores ausentes (encontrados em `total_bedrooms`) e tipos de dados (identificando `ocean_proximity` como categórico).
- **Análise de Distribuições:** Histogramas foram gerados para visualizar a distribuição de cada atributo, revelando valores limitados (capped) no preço das casas e escalas muito diferentes entre as características.
- **Divisão Estratificada:** Para garantir uma avaliação justa, os dados foram divididos em conjuntos de treino e teste (80/20) usando amostragem estratificada baseada na categoria de renda (`median_income`), evitando viés de amostragem.
- **Análise Geográfica e de Correlações:** Visualizações geográficas revelaram que os preços são mais altos em áreas costeiras e densamente povoadas. Uma matriz de correlação (heatmap) confirmou que `median_income` é o preditor linear mais forte do preço das casas.

### 2. Pré-processamento e Engenharia de Atributos

- **Engenharia de Atributos:** Foram criadas novas características baseadas em rácios (`rooms_per_household`, `bedrooms_per_room`, `population_per_household`) que se mostraram mais informativas do que os atributos originais.
- **Pipeline Numérico:** Foi construído um `Pipeline` do Scikit-Learn para tratar as colunas numéricas, realizando duas etapas:
    1. **Imputação:** Preenchimento de valores ausentes com a mediana (`SimpleImputer`).
    2. **Escalonamento:** Padronização de todas as características numéricas (`StandardScaler`).
- **Processamento Categórico:** A coluna `ocean_proximity` foi convertida em um formato numérico usando `OneHotEncoder`.
- **Junção Final:** Os dados numéricos e categóricos processados foram combinados em uma única matriz NumPy, pronta para a modelagem.

### 3. Treinamento e Seleção de Modelos

Diferentes modelos de regressão foram treinados e avaliados usando Validação Cruzada para encontrar o mais promissor:
- **`LinearRegression`:** Serviu como linha de base e apresentou alto erro (subajuste).
- **`DecisionTreeRegressor`:** Apresentou sobreajuste (overfitting) severo, com desempenho inferior ao modelo linear na validação cruzada.
- **`RandomForestRegressor`:** Obteve o melhor desempenho inicial, com um RMSE significativamente menor (~$50k), sendo escolhido para a fase de otimização.

### 4. Otimização do Modelo

- **Grid Search:** O `GridSearchCV` foi utilizado para ajustar os hiperparâmetros do `RandomForestRegressor`, testando sistematicamente várias combinações e encontrando a configuração ótima que melhorou ainda mais o desempenho do modelo.

---

## Resultados Finais

Após o processo de otimização, o modelo final foi avaliado no conjunto de teste que foi mantido separado durante todo o projeto.

- **Modelo Vencedor:** `RandomForestRegressor`
- **Melhores Hiperparâmetros:** `{'max_features': 6, 'n_estimators': 50}`
- **Desempenho Final (RMSE no Conjunto de Teste):** **$46,885.11** 

Este valor representa o erro de previsão típico do modelo em dados novos, indicando uma performance robusta e consistente com o que foi observado durante a validação.

---

## Tecnologias Utilizadas

- **Python 3.x**
- **Pandas:** Para manipulação e análise de dados.
- **NumPy:** Para cálculos numéricos eficientes.
- **Matplotlib & Seaborn:** Para visualização de dados.
- **Scikit-Learn:** Para pré-processamento, treinamento de modelos e avaliação.
- **Jupyter Notebook:** Como ambiente de desenvolvimento.

---

##  Como Executar o Projeto

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/seu-usuario/nome-do-repositorio.git](https://github.com/seu-usuario/nome-do-repositorio.git)
    cd nome-do-repositorio
    ```

2.  **Crie um ambiente virtual (recomendado):**
    ```bash
    python3 -m venv env
    source env/bin/activate
    ```

3.  **Instale as dependências:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Você precisará criar um arquivo `requirements.txt` com as bibliotecas listadas acima)*

4.  **Estrutura de Arquivos:**
    Certifique-se de que o dataset (`housing.csv`) e as imagens estejam nas pastas corretas, conforme especificado no notebook.
    ```
    .
    ├── dataset/
    │   └── housing.csv
    ├── images/
    │   └── california.png
    ├── Housing - LinearRegression.ipynb
    └── README.md
    ```

5.  **Execute o Notebook:**
    Abra o Jupyter Notebook e execute as células do arquivo `Housing - LinearRegression.ipynb` em ordem.
    ```bash
    jupyter notebook
    ```
