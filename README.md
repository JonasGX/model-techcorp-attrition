# TechCorp Attrition — Predição de Rotatividade de Funcionários

Pipeline completo de Machine Learning para prever attrition de funcionários em uma empresa de tecnologia, cobrindo desde a análise exploratória até a comparação de quatro algoritmos de classificação.

---

## Contexto do Problema

A **TechCorp Brasil**, empresa de tecnologia com mais de **50.000 funcionários**, registrou um aumento de **35% na taxa de attrition** no último ano, gerando um **custo estimado de R$ 45 milhões**.

Cada saída de funcionário representa:
- Custo de demissão e recontratação equivalente a **1,5× o salário anual** (~R$ 144.000/funcionário)
- Perda de conhecimento institucional
- Impacto na produtividade e moral das equipes
- Atrasos em projetos críticos

O projeto desenvolve um sistema preditivo que entrega ao RH um **score de risco por funcionário**, permitindo ações preventivas antes da saída.

### ROI do Projeto

| Item | Valor |
|------|-------|
| Custo total atual de attrition | R$ 45.000.000 |
| Economia com redução de 10% | R$ 4.500.000/ano |
| Custo total do projeto | R$ 150.000 |
| **ROI estimado** | **~30x** |

---

## Estrutura do Projeto

```
techcorp_attrition/
│
├── data/
│   ├── raw/
│   │   └── ibm_hr_analytics_sintetico.csv      # Dataset sintético (1M registros)
│   ├── processed/
│   │   └── ibm_hr_analytics_feature_engineering.csv  # Dataset pós-feature engineering
│   └── gerador_de_massa_rh.ipynb               # Script de geração dos dados sintéticos
│
├── notebooks/
│   ├── 01_analise_exploratoria.ipynb           # EDA completa + análise estatística
│   ├── 02_feature_engineering.ipynb            # Criação de 10 novas features
│   └── 03_pre_processing_modelagem.ipynb       # Pré-processamento + 4 modelos ML
│
├── reports/
│   ├── comparacao_modelos.png                  # Gráfico comparativo final
│   ├── curva_roc_comparativa.png               # Curva ROC — 4 modelos
│   ├── confusion_matrix_*.png                  # Matrizes de confusão individuais
│   ├── curva_roc_*.png                         # Curvas ROC individuais
│   ├── distribuicao_variaveis_continuas/       # Histogramas e boxplots
│   ├── distribuicao_variaveis_ordinais/        # Distribuição variáveis ordinais
│   └── distribuicao_variaveis_categoricas/     # Distribuição variáveis categóricas
│
├── docs/
│   ├── 01_metadados.md                         # Dicionário de dados completo
│   ├── 02_Business_Understanding.md            # Contexto e análise de negócio
│   ├── 03_tipos_de_metricas_de_modelos.md      # Guia de métricas de classificação
│   ├── hr_analytics_trabalho.pdf               # Documentação acadêmica
│   └── referencias.txt                         # Referências e links utilizados
│
├── requirements.txt                            # Dependências do projeto
└── README.md
```

---

## Dataset

| Propriedade | Valor |
|-------------|-------|
| Nome | IBM HR Analytics Employee Attrition & Performance (sintético) |
| Registros | 1.000.000 |
| Features originais | 35 |
| Variável alvo | `Attrition` (Yes / No) |
| Taxa de attrition | ~16% (desbalanceado: 84% No / 16% Yes) |
| Tipo de problema | Classificação Binária |

O dataset é uma versão **sintética de larga escala** baseada no IBM HR Analytics, gerada para simular um ambiente corporativo real com 1 milhão de registros. A distribuição das variáveis é uniforme e independente da variável alvo — característica intencional para validar o rigor metodológico do pipeline.

### Variáveis Originais (após remoção de constantes)

| Grupo | Variáveis |
|-------|-----------|
| Demográficas | `Age`, `Gender`, `MaritalStatus`, `Education`, `EducationField` |
| Trabalho atual | `Department`, `JobRole`, `JobLevel`, `JobInvolvement`, `BusinessTravel`, `OverTime`, `DistanceFromHome` |
| Compensação | `MonthlyIncome`, `DailyRate`, `HourlyRate`, `MonthlyRate`, `PercentSalaryHike`, `StockOptionLevel` |
| Satisfação | `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction`, `WorkLifeBalance` |
| Performance | `PerformanceRating`, `TrainingTimesLastYear` |
| Histórico | `NumCompaniesWorked`, `TotalWorkingYears`, `YearsAtCompany`, `YearsInCurrentRole`, `YearsSinceLastPromotion`, `YearsWithCurrManager` |

> **Removidas no pré-processamento:** `EmployeeCount`, `StandardHours`, `Over18` (constantes) e `EmployeeNumber` (identificador único — risco de data leakage).

---

## Pipeline de Machine Learning

O projeto segue a metodologia CRISP-DM adaptada, com os seguintes notebooks em sequência:

### Notebook 01 — Análise Exploratória (EDA)

**Objetivo:** Entender os dados e levantar hipóteses sobre o Attrition.

**Metodologia de 3 camadas:**

1. **Análise Visual** — Histogramas, boxplots e gráficos de taxa de attrition por categoria para todas as 30 variáveis (contínuas, ordinais e categóricas).

2. **Validação Estatística**
   - Variáveis numéricas/ordinais: **Teste U de Mann-Whitney** (não paramétrico, baseado em ranks)
   - Variáveis categóricas: **Qui-Quadrado (Chi-Square)**
   - Limiar de significância: p < 0,05

3. **Análise de Correlação** — Matriz de Spearman (adequada para variáveis ordinais e distribuições não-normais) com mapa de calor e ranking de correlação com a target.

**Principais achados:**

| Análise | Resultado |
|---------|-----------|
| EDA Visual | Nenhuma variável mostrou diferença visual entre Attrition Yes/No |
| Testes estatísticos | 29 de 30 variáveis sem significância (p > 0,05) |
| Exceção | `YearsInCurrentRole` (p = 0,039), porém correlação com target = 0,002 (desprezível) |
| Correlação com target | Todas entre -0,002 e +0,002 — praticamente zero |
| Multicolinearidade | Bloco de correlação moderada entre variáveis de tempo (YearsAtCompany ↔ TotalWorkingYears ↔ YearsInCurrentRole ↔ YearsSinceLastPromotion) |

**Conclusão:** Nenhuma variável individual explica o Attrition. O Feature Engineering é essencial para capturar padrões via interações e proporções entre variáveis.

---

### Notebook 02 — Feature Engineering

**Objetivo:** Criar variáveis derivadas com base em hipóteses de negócio para tentar capturar padrões que as features originais não revelam isoladamente.

#### 10 novas features criadas:

**Grupo: Satisfação**

| Feature | Cálculo | Interpretação |
|---------|---------|---------------|
| `SatisfactionScore` | `(JobSatisfaction + EnvironmentSatisfaction + RelationshipSatisfaction) / 3` | Score geral de satisfação com a empresa (1.00–4.00). Captura o nível de satisfação que nenhuma das três variáveis representa isoladamente. |
| `CriticalSatisfactionFlag` | `1 se SatisfactionScore ≤ 2.0` | Indicador binário de satisfação crítica. ~31% dos funcionários classificados como críticos. |

**Grupo: Carreira e Tempo**

| Feature | Cálculo | Interpretação |
|---------|---------|---------------|
| `RatioCareerCompany` | `YearsAtCompany / (TotalWorkingYears + 1)` | Proporção da carreira feita na empresa. Alto = fez carreira aqui (mais estável). Baixo = passou por várias empresas. |
| `RoleStagnation` | `YearsInCurrentRole / (YearsAtCompany + 1)` | Proporção do tempo no mesmo cargo. Próximo de 1.0 = nunca mudou de função. |
| `YearsWithoutPromotion` | `YearsSinceLastPromotion / (YearsAtCompany + 1)` | Proporção do tempo sem promoção. Alto = estagnação de carreira. Complementar ao RoleStagnation (um funcionário pode mudar de cargo mas não de nível). |
| `AgeGroup` | `Jovem (≤30) / Adulto (31–45) / Senior (>45)` | Faixas de comportamento do mercado de trabalho. Distribuição: Senior 40,4%, Adulto 32,0%, Jovem 27,6%. |

**Grupo: Compensação**

| Feature | Cálculo | Interpretação |
|---------|---------|---------------|
| `IncomePerLevel` | `MonthlyIncome / JobLevel` | Salário por nível hierárquico. Valor baixo = funcionário subvalorizado para o nível que ocupa. |
| `IncomeMedianJob` | `MonthlyIncome / mediana do cargo` | Razão entre salário e mediana dos pares. < 1.0 = ganha menos que a mediana do cargo. |

**Grupo: Risco e Comportamento**

| Feature | Cálculo | Interpretação |
|---------|---------|---------------|
| `RiskOvertimeDistance` | `OverTime_encoded × DistanceFromHome` | Acúmulo de desgaste entre hora extra e distância casa-trabalho. 0 = sem hora extra (independente da distância). |
| `RiskWorklifeOvertime` | `(5 - WorkLifeBalance) × OverTime_encoded` | Acúmulo de sobrecarga: equilíbrio ruim + hora extra. Valor 4 = risco crítico. Valor 0 = sem hora extra. |

**Resultado:** Mesmo as features derivadas apresentaram correlação próxima de zero com a target, confirmando que a limitação está na natureza do dataset sintético (distribuição uniforme e independente do Attrition).

---

### Notebook 03 — Pré-processamento e Modelagem

#### Pré-processamento

| Etapa | Decisão | Justificativa |
|-------|---------|---------------|
| Separação X / y | `X` = 40 features, `y` = `AttritionFlag` | `AttritionFlag` é a versão numérica (0/1) da target |
| Remoção de redundância | `OverTime_encoded` removido | Redundante com `OverTime` após OneHotEncoder |
| Split treino/teste | 80% / 20% com `stratify=y` | Mantém proporção 84%/16% em ambos os conjuntos |
| Escalonamento numérico | `StandardScaler` | Centraliza média em 0 e desvio padrão em 1 — essencial para Regressão Logística |
| Encoding categórico | `OneHotEncoder(drop='first')` | Converte variáveis categóricas evitando multicolinearidade (dummy trap) |
| Desbalanceamento | `class_weight='balanced'` / `scale_pos_weight` | Penaliza mais os erros na classe minoritária (Yes) |

Dimensões após split:
- `X_train`: 800.000 × 40
- `X_test`: 200.000 × 40

#### Modelos Avaliados

Todos os modelos foram implementados em **scikit-learn Pipeline**, aplicando o preprocessador automaticamente antes do treinamento.

| Modelo | Tratamento do Desbalanceamento | Tempo de Treino |
|--------|-------------------------------|-----------------|
| Regressão Logística | `class_weight='balanced'`, `max_iter=1000` | 8,76s |
| Random Forest | `class_weight='balanced'`, `n_estimators=100` | 140,95s |
| XGBoost | `scale_pos_weight` (razão entre classes) | 25,06s |
| LightGBM | `is_unbalance=True`, `n_estimators=100` | 22,46s |

#### Resultados Comparativos

| Modelo | Recall (Yes) | Precision (Yes) | F1-Score (Yes) | ROC-AUC |
|--------|:------------:|:---------------:|:--------------:|:-------:|
| Regressão Logística | 0.5001 | 0.1594 | 0.2417 | 0.4976 |
| LightGBM | 0.4888 | 0.1610 | 0.2422 | 0.4995 |
| XGBoost | 0.4337 | 0.1585 | 0.2322 | 0.4983 |
| Random Forest | 0.0000 | 0.0000 | 0.0000 | 0.4977 |

> **Linha de referência aleatória:** ROC-AUC = 0.50

**Análise dos resultados:**

- Todos os modelos operam na faixa aleatória (ROC-AUC ~0,50)
- A **Regressão Logística** apresentou o melhor Recall (0,50), chutando 50/50 para ambas as classes
- O **Random Forest** previu "No" para todos os 200.000 registros — Recall = 0, Accuracy = 84%
- A Precision de ~0,16 em todos os modelos é exatamente a proporção de Yes no dataset, confirmando previsões sem poder preditivo real
- A curva ROC comparativa mostra todos os 4 modelos colados à diagonal (nenhum conseguiu se descolar do acaso)

**Por que nenhum modelo funcionou?** O dataset sintético foi gerado com distribuições uniformes e **sem relação programada entre as features e o Attrition**. Quando os dados não possuem sinal preditivo, nenhum algoritmo — linear ou não-linear — consegue aprender padrões. Em dados reais de RH, espera-se que variáveis como `OverTime`, `JobSatisfaction` e `MonthlyIncome` apresentem correlação significativa com a target.

---

## Métricas e Justificativas

### Por que priorizar Recall?

No contexto de negócio do Attrition, os erros têm custos assimétricos:

| Tipo de Erro | Significado | Impacto |
|--------------|-------------|---------|
| **Falso Negativo** | Funcionário vai sair → modelo não identificou → RH não agiu → custo R$ 144.000 | Alto |
| **Falso Positivo** | Funcionário não ia sair → modelo apontou como risco → RH fez uma conversa | Baixo (custo: ~1h do RH) |

**Minimizar Falsos Negativos = maximizar Recall.**

### Critérios de Sucesso

| Métrica | Mínimo Aceitável | Justificativa |
|---------|:----------------:|---------------|
| **Recall** | ≥ 75% | Capturar pelo menos 3 de cada 4 funcionários em risco |
| **F1-Score** | ≥ 70% | Garantir equilíbrio mínimo com Precision |

> **Nota sobre Accuracy:** Em datasets desbalanceados (84%/16%), um modelo que prevê sempre "No" atinge 84% de accuracy. Por isso, Accuracy não foi utilizada como métrica de avaliação.

---

## Outputs do Modelo para o RH

A proposta de entrega mensal ao time de RH é uma tabela ordenada por score de risco:

| id_funcionario | departamento | score_attrition | risco |
|----------------|-------------|:---------------:|-------|
| 01 | Engenharia | 0.87 | alto |
| 456 | Tecnologia | 0.71 | alto |
| 56 | Produto | 0.45 | médio |
| 891 | Marketing | 0.24 | baixo |

> A régua de classificação (baixo / médio / alto) é definida com base na distribuição real das probabilidades geradas pelo modelo treinado, não por thresholds arbitrários.

---

## Como Executar

### Pré-requisitos

- Python 3.14+
- Ambiente virtual configurado

### Instalação

```bash
# Criar e ativar o ambiente virtual
python -m venv .venv
.venv\Scripts\activate   # Windows
# source .venv/bin/activate  # Linux/Mac

# Instalar dependências
pip install -r requirements.txt

# Registrar kernel no Jupyter (necessário para rodar os notebooks no VSCode)
pip install ipykernel
python -m ipykernel install --user --name=.venv --display-name "Python 3.14 (techcorp)"
```

### Execução dos Notebooks

Execute os notebooks **em sequência**, pois cada um depende dos outputs do anterior:

```
1. notebooks/01_analise_exploratoria.ipynb
   → Lê:    data/raw/ibm_hr_analytics_sintetico.csv
   → Salva: reports/ (gráficos EDA)

2. notebooks/02_feature_engineering.ipynb
   → Lê:    data/raw/ibm_hr_analytics_sintetico.csv
   → Salva: data/processed/ibm_hr_analytics_feature_engineering.csv

3. notebooks/03_pre_processing_modelagem.ipynb
   → Lê:    data/processed/ibm_hr_analytics_feature_engineering.csv
   → Salva: reports/ (gráficos de modelagem)
```

---

## Dependências Principais

| Biblioteca | Versão | Uso |
|-----------|--------|-----|
| `pandas` | 3.0.3 | Manipulação de dados |
| `numpy` | 2.4.6 | Operações numéricas |
| `scikit-learn` | 1.8.0 | Pipeline, modelos, métricas |
| `xgboost` | — | Gradient Boosting |
| `lightgbm` | — | Gradient Boosting otimizado |
| `matplotlib` | 3.10.9 | Visualizações |
| `seaborn` | 0.13.2 | Visualizações estatísticas |
| `scipy` | 1.17.1 | Testes estatísticos (Mann-Whitney, Chi-Square) |
| `statsmodels` | 0.14.6 | Modelagem estatística |

> Lista completa em [requirements.txt](requirements.txt)

---

## Conclusões e Lições Aprendidas

### O que o projeto demonstra

Apesar dos modelos não terem atingido poder preditivo real (consequência da natureza sintética dos dados), o projeto evidencia domínio completo do pipeline de Machine Learning:

1. **Rigor metodológico na EDA** — Três camadas de evidência (visual, estatística, correlacional) convergindo para a mesma conclusão em 96,7% das variáveis
2. **Feature Engineering fundamentado em negócio** — 10 novas features com justificativa de hipótese de negócio, cálculo documentado e análise de resultado
3. **Pipeline reproduzível** — scikit-learn Pipeline com preprocessador integrado, evitando data leakage
4. **Comparação honesta de algoritmos** — 4 modelos com tratamento adequado do desbalanceamento e métricas alinhadas ao objetivo de negócio
5. **Pensamento crítico sobre resultados** — Identificação correta de que a limitação está nos dados, não nos algoritmos

### Próximos passos com dados reais

Com um dataset real de RH (onde as features têm relação causal com o Attrition), espera-se:
- `OverTime`, `JobSatisfaction` e `MonthlyIncome` com correlações significativas com a target
- Recall acima de 75% atingível com modelos de gradient boosting e tuning de threshold
- Possibilidade de explicabilidade via SHAP values para comunicação com o RH

---

## Documentação Adicional

| Arquivo | Conteúdo |
|---------|----------|
| [docs/01_metadados.md](docs/01_metadados.md) | Dicionário completo de todas as variáveis originais e features criadas |
| [docs/02_Business_Understanding.md](docs/02_Business_Understanding.md) | Contexto do problema, cálculo de ROI e definição de métricas de negócio |
| [docs/03_tipos_de_metricas_de_modelos.md](docs/03_tipos_de_metricas_de_modelos.md) | Guia de Accuracy, Precision, Recall e F1-Score |

---

## Autor

**Jonas Gomes Xavier**
Projeto Final — RH Analytics · 2025
