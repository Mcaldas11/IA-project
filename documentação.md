# Documentação Técnica - IA Project 2025/2026

Este documento explica detalhadamente o fluxo de trabalho realizado no `notebook.ipynb`, a lógica por trás das decisões e o que os gráficos nos dizem.

---

## 1. Carregamento e Inspeção (Células 1 a 5)
*   **O que faz:** Carregamos o dataset e verificamos o tipo de dados e se existem valores nulos.
*   **Porquê:** É o primeiro passo para garantir que os dados estão "limpos". Verificámos que não existem valores nulos, o que facilita o trabalho.

## 2. Análise Exploratória (EDA) - Gráficos
### 2.1. Histogramas (Distribuições)
*   **O que mostra:** A idade média ronda os 35-40 anos. O salário (`MonthlyIncome`) está muito concentrado em valores baixos, com poucos funcionários a ganhar salários de topo.
*   **Lógica:** Ajuda a perceber se existem anomalias nos dados.

### 2.2. Boxplots (Outliers)
*   **O que mostra:** Identificamos valores "outliers" no salário e nos anos de carreira. São funcionários com salários muito acima da média.
*   **Lógica:** Não os removemos porque são casos reais (diretores/gestores séniores) que o modelo de regressão precisa de aprender.

### 2.3. Matriz de Correlação (Heatmap)
*   **O que mostra:** Existe uma correlação fortíssima (perto de 1.0) entre o `JobLevel` e o `MonthlyIncome`. 
*   **Conclusão:** Isto diz-nos que o cargo ocupado é o fator que mais influencia o salário, o que faz todo o sentido no mundo real.

## 3. Pré-processamento e Pipeline (Célula 14)
*   **Transformadores:**
    *   **StandardScaler:** Normaliza os números para que variáveis com escalas grandes (ex: Salário) não "abafem" variáveis pequenas (ex: Idade).
    *   **OneHotEncoder:** Transforma palavras (ex: "Sales", "Research") em números que a IA consegue processar.
*   **Pipeline:** Usamos Pipelines para garantir que o pré-processamento é aplicado de forma automática e igual tanto nos dados de treino como nos de teste, evitando erros.

## 4. Regressão - Salário (Ponto 6)
*   **Modelos:** Testámos Linear Regression, Ridge e Random Forest.
*   **Resultado:** O **Random Forest** foi o melhor. Ao contrário da Regressão Linear, que assume uma linha reta, o Random Forest consegue capturar relações complexas entre variáveis.

## 5. Classificação - Attrition (Ponto 7)
### 5.1. O Uso do SMOTE
*   **Problema:** O dataset estava desequilibrado (poucas pessoas saíam).
*   **Solução:** Usámos o **SMOTE** para criar dados sintéticos de pessoas que saem.
*   **Porquê:** Sem o SMOTE, o modelo ignorava quem saía. Com o SMOTE, o modelo tornou-se muito mais atento a casos de risco.

### 5.2. Comparação Final
*   O **Random Forest Classifier (com SMOTE)** foi o escolhido como modelo final.
*   **Resultado:** Alcançou um **F1-score global de 0.85**. É o modelo mais equilibrado entre precisão e capacidade de deteção.

## 6. Exportação (Pickle)
*   **O que faz:** Guarda os modelos treinados em ficheiros `.pkl`.
*   **Lógica:** Permite que os modelos sejam usados no futuro ou numa aplicação web sem ter de treinar tudo de novo.
