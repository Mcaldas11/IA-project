# Relatório Técnico Detalhado - Projeto IA 2025/2026 (Grupo 1)

Este relatório apresenta uma análise exaustiva e detalhada do desenvolvimento do projeto, célula por célula, justificando as decisões técnicas, interpretando os resultados visuais e discutindo alternativas arquiteturais.

---

## 1. Importação de Bibliotecas e Configurações (Célula 23)
*   **O que faz:** Importa as ferramentas essenciais (`pandas`, `numpy`, `matplotlib`, `seaborn`) e define a precisão decimal para visualização de dados.
*   **Porquê:** Estas são as bibliotecas padrão da indústria para ciência de dados em Python. O Pandas permite manipulação tabular, o NumPy lida com operações matemáticas, e Matplotlib/Seaborn são usados para visualização.
*   **Alternativas:** Poderíamos usar o `Plotly` para gráficos interativos, mas o Seaborn é preferível para relatórios estáticos devido à sua estética limpa e integração direta com o Pandas.

## 2. Carregamento e Inspeção Inicial (Células 24 - 27)
*   **Célula 24:** Carrega o CSV e mostra as primeiras 5 linhas (`head()`). Serve para confirmar que os dados foram lidos corretamente.
*   **Célula 25:** O comando `df.info()` revela os tipos de dados (int64 e object). Vemos que temos 35 colunas e 1249 registos.
*   **Célula 26:** O `df.describe()` fornece estatísticas como média, desvio padrão e quartis. Aqui notamos, por exemplo, que a idade média é 37 anos.
*   **Célula 27:** Verifica a existência de valores nulos (`isnull().sum()`).
    *   **Resultado:** O dataset não tem valores em falta.
    *   **Justificação:** Caso houvesse nulos, teríamos de usar técnicas de imputação (como preencher com a média ou mediana). Como está limpo, avançamos diretamente.

## 3. Análise Exploratória de Dados (EDA) (Células 28 - 35)

### 3.1. Histogramas (Célula 28)
*   **O que mostra:** A distribuição de variáveis como Age, MonthlyIncome e YearsAtCompany.
*   **Interpretação:** O `MonthlyIncome` está deslocado para a esquerda (muitos ganham pouco, poucos ganham muito), o que é comum em estruturas corporativas.

### 3.2. Boxplots e Outliers (Célula 29)
*   **O que mostra:** Identifica valores atípicos no salário e anos de carreira.
*   **Decisão Técnica:** Decidimos manter os outliers. 
*   **Porquê:** Na gestão de RH, "Diretores" ou "Séniores" que ganham muito não são erros de dados, mas sim casos reais importantes. Removê-los faria o modelo de regressão falhar em prever salários elevados.

### 3.3. Relação Bivariada (Célula 30 e 31)
*   **Gráfico JobLevel vs MonthlyIncome:** Mostra uma correlação quase linear perfeita. Quanto maior o nível do cargo, maior o salário.
*   **Gráfico Attrition vs Outras:** Vemos que quem sai do emprego tende a ser mais jovem e a ganhar menos do que quem fica.

### 3.4. Mapa de Correlação (Célula 34)
*   **O que mostra:** A força da relação entre todas as variáveis numéricas.
*   **Insights:** Confirmamos estatisticamente que `JobLevel` e `MonthlyIncome` têm uma correlação altíssima (perto de 1.0).

---

## 4. Pré-processamento com Pipelines (Célula 36)
*   **O que faz:** Cria uma estrutura automatizada para tratar os dados.
    *   **StandardScaler:** Normaliza variáveis numéricas (coloca-as na mesma escala).
    *   **OneHotEncoder:** Converte variáveis de texto (como Department) em colunas binárias (0 e 1).
*   **Porquê usar Pipelines?** É a melhor prática para evitar o **Data Leakage** (fuga de dados). O pipeline garante que as transformações aprendidas no treino sejam aplicadas exatamente da mesma forma no teste.
*   **Alternativas:** Poderíamos fazer as transformações manualmente com `get_dummies`, mas isso tornaria o código frágil e difícil de exportar para uma aplicação real.

---

## 5. Comparação de Modelos (Prós e Contras)

Antes da escolha final, avaliámos diversos algoritmos. A tabela abaixo resume os pontos fortes e fracos de cada um no contexto deste projeto:

| Modelo | Prós | Contras |
| :--- | :--- | :--- |
| **Regressão Linear / Ridge** | Simples, muito rápido e fácil de interpretar. | Assume relações lineares; falha em capturar a complexidade salarial real. |
| **Regressão Logística** | Eficiente para classificação binária; coeficientes explicáveis. | Fronteira de decisão linear; dificuldade com padrões complexos de saída (attrition). |
| **KNN (K-Neighbors)** | Não assume distribuição de dados; intuitivo. | Muito sensível à escala e ao ruído; lento em datasets maiores. |
| **SVM (Support Vector Machine)** | Eficaz em espaços de alta dimensão. | Difícil de afinar parâmetros; treino lento; menos intuitivo. |
| **XGBoost / Gradient Boosting** | Performance altíssima; lida com valores em falta. | Muito complexo de configurar; alto risco de decorar os dados (overfitting). |
| **Random Forest (Escolhido)** | **Excelente com dados não-lineares; robusto a outliers; evita overfitting; lida bem com categorias.** | Menos interpretável que um modelo linear simples; ligeiramente mais pesado. |

---

## 6. Justificação da Escolha: Porquê o Random Forest?

Após testarmos as alternativas (visíveis nas pastas do diretório `Caldas`), o **Random Forest** foi selecionado como o modelo final tanto para Regressão como para Classificação pelos seguintes motivos:

1.  **Superioridade na Regressão (Salário):** Ao contrário da Regressão Linear, o Random Forest percebeu que o salário não aumenta de forma constante. Ele conseguiu mapear que certas combinações de "Cargo" e "Anos de Empresa" geram saltos salariais que os modelos lineares ignoravam. O resultado foi um **RMSE (Erro Médio)** drasticamente menor.
2.  **Eficácia com o SMOTE (Attrition):** Na classificação, o Random Forest provou ser o algoritmo que melhor "aprendeu" com os dados sintéticos gerados pelo SMOTE. Conseguiu um equilíbrio quase perfeito entre identificar quem realmente sai e não dar alarmes falsos, resultando num **F1-Score Global de 0.85**.
3.  **Robustez:** Como decidimos manter os *outliers* (diretores com altos salários), precisávamos de um modelo que não fosse "puxado" por esses valores extremos. O Random Forest, por ser um conjunto de muitas árvores, é naturalmente resistente a este problema.
4.  **Integração em Pipelines:** Funciona harmoniosamente com os nossos transformadores de dados, garantindo que o modelo é estável e fiável para novas previsões.

---

## 7. Exploração Adicional e Conclusão
Embora o XGBoost tenha mostrado resultados promissores, a sua complexidade não justificava o ganho marginal de performance em relação ao **Random Forest**, que se revelou muito mais estável e equilibrado para este dataset de RH.

---

## 8. Exportação dos Modelos (Célula 42)
*   **O que faz:** Guarda os objetos finais em ficheiros `.pkl` (Pickle).
*   **Porquê:** Permite que estes modelos sejam carregados instantaneamente em qualquer aplicação Python (como um servidor web ou script de automação) sem necessidade de reprocessar os dados originais ou treinar novamente, o que poupa tempo e recursos computacionais.

---
**Conclusão:** O fluxo de trabalho adotado priorizou a robustez (Pipelines), a honestidade dos dados (manutenção de outliers) e a eficácia em dados desequilibrados (SMOTE), resultando em modelos prontos para produção.
