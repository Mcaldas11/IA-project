# IA Project 2025/2026 - Employee Data Analysis

Este projeto utiliza técnicas de Machine Learning para analisar dados de funcionários e resolver dois problemas principais:
1.  **Regressão**: Prever o rendimento mensal (`MonthlyIncome`).
2.  **Classificação**: Prever a saída de funcionários (`Attrition`).

## 🚀 Estrutura do Projeto

*   `notebook.ipynb`: Notebook principal com toda a análise exploratória (EDA), pré-processamento e treino de modelos.
*   `employee_data/`: Pasta contendo o dataset `employee_data.csv`.
*   `AI-Project-2526.pdf`: Enunciado/documentação do projeto.
*   `check_results.py`: Script auxiliar para validação rápida dos resultados.

## 📊 Resumo dos Resultados

### 1. Previsão de Salário (Regressão)
Testamos modelos de Regressão Linear, Ridge e Random Forest.
*   **Melhor Modelo**: Random Forest Regressor
*   **Erro Médio (RMSE)**: ~1027.29
*   **Conclusão**: O Random Forest apresentou o erro mais baixo, sendo o mais robusto para prever variações salariais.

### 2. Previsão de Saída (Classificação)
Comparamos Regressão Logística e Random Forest.
*   **Melhor Modelo**: Logistic Regression
*   **Accuracy**: 86%
*   **F1-Score (Classe Attrition=Yes)**: 0.49
*   **Conclusão**: Embora a Accuracy seja igual, a Regressão Logística identificou melhor os funcionários em risco de sair (maior Recall e F1-score para a classe positiva).

## 🛠️ Como Executar

1.  Certifique-se de ter o Python instalado.
2.  Instale as dependências necessárias:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn
    ```
3.  Abra o `notebook.ipynb` num ambiente Jupyter ou VS Code e execute as células sequencialmente.

---
*Projeto desenvolvido no âmbito da disciplina de Inteligência Artificial.*