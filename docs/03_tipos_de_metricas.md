# Tipos de métricas

## Modelos de Classificação

- **Accuracy - "Quantos acertei no total?"**

*Accuracy = (VP + VN) / Total*

**Observação**: Em datasets desbalanceados, a alta acurácia pode criar uma falsa percepção de desempenho, pois o modelo pode simplesmente aprender a prever a classe majoritária e ignorar a classe minoritária. Por isso, métricas como Recall, Precisão e F1-Score são essenciais para uma avaliação mais confiável.

- **Precision - "Dos que eu apontei como risco, quantos realmente eram?"**

*Precision = VP / (VP + FP)*

**Foco:** Não queremos acionar o RH à toa. Prioriza qualidade dos alertas e tempo gasto dos funcionários no departamento de RH.

- **Recall - "Dos que realmente vão sair, quantos eu peguei?"**

*Recall = VP / (VP + FN)*

**Foco:** Não queremos deixar ninguém escapar. Prioriza não perder casos reais.

- **F1-Score - "Qual é o equilíbrio entre Precision e Recall?"**

*F1-Score = 2 × (Precision × Recall) / (Precision + Recall)*

**Foco:** Encontrar um equilíbrio entre acertar os casos apontados (Precision) e capturar o máximo de casos reais (Recall).

**Quando usar:** É especialmente útil em datasets desbalanceados, pois considera simultaneamente os erros de falsos positivos e falsos negativos. Um modelo só terá um F1-Score alto se apresentar bons valores tanto de Precision quanto de Recall.

## Modelos de Regressão Linear
