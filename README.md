# IA Project 2025/2026 - Employee Data Analysis

Este projeto utiliza técnicas de Machine Learning para analisar dados de funcionários e resolver dois problemas principais:
1.  **Regressão**: Prever o rendimento mensal (`MonthlyIncome`).
2.  **Classificação**: Prever a saída de funcionários (`Attrition`).

## 🚀 Estrutura do Projeto

*   `G1_notebook.ipynb`: Notebook principal com toda a análise exploratória (EDA), pré-processamento e treino de modelos.
*   `G1_pipeline_regression.pkl`: Pipeline do modelo de regressão treinado.
*   `G1_pipeline_classification.pkl`: Pipeline do modelo de classificação treinado.
*   `employee_data/`: Pasta contendo o dataset `employee_data.csv`.
*   `AI-Project-2526.pdf`: Enunciado/documentação do projeto.
*   `G1_relatório.md`: Documentação técnica detalhada.

## 📊 Resumo dos Resultados

### 1. Previsão de Salário (Regressão)
Testamos modelos de Regressão Linear, Ridge e Random Forest.
*   **Melhor Modelo**: Random Forest Regressor
*   **Erro Médio (RMSE)**: ~1027.29
*   **Conclusão**: O Random Forest apresentou o erro mais baixo, sendo o mais robusto para prever variações salariais.

### 2. Previsão de Saída (Classificação)
Comparamos Regressão Logística e Random Forest, ambos utilizando a técnica **SMOTE** para balanceamento de dados.
*   **Melhor Modelo**: Random Forest Classifier (com SMOTE)
*   **Accuracy**: 86%
*   **F1-Score Final (Weighted)**: **0.85**
*   **Conclusão**: O Random Forest com SMOTE provou ser o modelo mais equilibrado, mantendo uma precisão elevada e melhorando significativamente a identificação de funcionários em risco de sair.

## 📄 Documentação Detalhada
Para uma explicação detalhada de cada célula do código, gráficos e decisões técnicas, consulte o ficheiro:
👉 `G1_relatório.md`

## 🛠️ Como Executar

1.  Certifique-se de ter o Python instalado.
2.  Instale as dependências necessárias:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn xgboost
    ```
3.  Abra o `G1_notebook.ipynb` num ambiente Jupyter ou VS Code e execute as células sequencialmente.

---
*Projeto desenvolvido no âmbito da disciplina de Inteligência Artificial.*