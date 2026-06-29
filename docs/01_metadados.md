# 📊 Dicionário de Dados - HR Analytics Employee Attrition

## 📋 Informações Gerais do Dataset

| **Propriedade** | **Valor** |
|-----------------|-----------|
| **Nome do Dataset** | IBM HR Analytics Employee Attrition & Performance |
| **Número de Registros** | 1,000,000 |
| **Número de Features** | 35 |
| **Variável Alvo** | Attrition (Yes/No) |
| **Taxa de Attrition** | ~16% |
| **Tipo de Problema** | Classificação Binária |

---

## 🔍 Descrição Detalhada das Variáveis

### 1. Variáveis de Identificação

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **EmployeeCount** | Integer | Contagem de funcionários | Sempre 1 | Constante - remover no preprocessing |
| **EmployeeNumber** | Integer | ID único do funcionário | 1-2068 | Identificador único |
| **Over18** | String | Se o funcionário tem mais de 18 anos | Sempre 'Y' | Constante - remover no preprocessing |
| **StandardHours** | Integer | Horas padrão de trabalho | Sempre 80 | Constante - remover no preprocessing |

### 2. Variáveis Demográficas

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **Age** | Integer | Idade do funcionário | 18-60 | Distribuição normal, média ~37 |
| **Gender** | String | Gênero do funcionário | 'Male', 'Female' | ~60% Male, ~40% Female |
| **MaritalStatus** | String | Estado civil | 'Single', 'Married', 'Divorced' | Importante para análise de attrition |
| **Education** | Integer | Nível de educação | 1-5 | 1='Below College', 2='College', 3='Bachelor', 4='Master', 5='Doctor' |
| **EducationField** | String | Área de formação | 'Life Sciences', 'Medical', 'Marketing', 'Technical Degree', 'Human Resources', 'Other' | Distribuição variada |

### 3. Variáveis de Trabalho Atual

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **Department** | String | Departamento atual | 'Sales', 'Research & Development', 'Human Resources' | R&D tem maior população |
| **JobRole** | String | Função/Cargo atual | 9 diferentes roles | 'Sales Executive', 'Research Scientist', 'Laboratory Technician', etc. |
| **JobLevel** | Integer | Nível hierárquico | 1-5 | 1=Entry, 5=Senior |
| **JobInvolvement** | Integer | Nível de envolvimento no trabalho | 1-4 | 1='Low', 2='Medium', 3='High', 4='Very High' |
| **BusinessTravel** | String | Frequência de viagens | 'Non-Travel', 'Travel_Rarely', 'Travel_Frequently' | Forte correlação com attrition |
| **OverTime** | String | Se faz hora extra | 'Yes', 'No' | ~28% fazem hora extra |
| **DistanceFromHome** | Integer | Distância casa-trabalho (km) | 1-29 | Impacta satisfação e attrition |

### 4. Variáveis de Compensação

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **DailyRate** | Integer | Taxa diária | 102-1499 | Cálculo interno da empresa |
| **HourlyRate** | Integer | Taxa por hora | 30-100 | Cálculo interno da empresa |
| **MonthlyRate** | Integer | Taxa mensal | 2094-26999 | Cálculo interno da empresa |
| **MonthlyIncome** | Integer | Salário mensal | 1009-19999 | Principal indicador de compensação |
| **PercentSalaryHike** | Integer | % do último aumento | 11-25 | Porcentagem do último aumento salarial |
| **StockOptionLevel** | Integer | Nível de stock options | 0-3 | 0=Sem opções, 3=Mais opções |

### 5. Variáveis de Satisfação

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **EnvironmentSatisfaction** | Integer | Satisfação com ambiente | 1-4 | 1='Low', 2='Medium', 3='High', 4='Very High' |
| **JobSatisfaction** | Integer | Satisfação com trabalho | 1-4 | 1='Low', 2='Medium', 3='High', 4='Very High' |
| **RelationshipSatisfaction** | Integer | Satisfação com relacionamentos | 1-4 | 1='Low', 2='Medium', 3='High', 4='Very High' |
| **WorkLifeBalance** | Integer | Equilíbrio trabalho-vida | 1-4 | 1='Bad', 2='Good', 3='Better', 4='Best' |

### 6. Variáveis de Performance

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **PerformanceRating** | Integer | Avaliação de performance | 3-4 | 3='Excellent', 4='Outstanding' |
| **TrainingTimesLastYear** | Integer | Treinamentos no último ano | 0-6 | Número de treinamentos realizados |

### 7. Variáveis de Histórico

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **NumCompaniesWorked** | Integer | Número de empresas anteriores | 0-9 | Histórico de mudanças |
| **TotalWorkingYears** | Integer | Anos totais de experiência | 0-40 | Experiência profissional total |
| **YearsAtCompany** | Integer | Anos na empresa atual | 0-40 | Tempo de casa |
| **YearsInCurrentRole** | Integer | Anos na função atual | 0-18 | Tempo na posição atual |
| **YearsSinceLastPromotion** | Integer | Anos desde última promoção | 0-15 | Indicador de estagnação |
| **YearsWithCurrManager** | Integer | Anos com o gestor atual | 0-17 | Estabilidade da gestão |

### 8. Variável Alvo

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **Attrition** | String | Se o funcionário deixou a empresa | 'Yes', 'No' | Target - ~16% 'Yes' |

---

## 🛠️ Feature Engineering - Notebook 02

### Resumo das Novas Features

| # | Feature | Tipo | Grupo | Descrição Resumida |
|---|---|---|---|---|
| 1 | SatisfactionScore | Numérica contínua | Satisfação | Média geral de satisfação do funcionário |
| 2 | CriticalSatisfactionFlag | Binária | Satisfação | Indicador de satisfação crítica (≤ 2.0) |
| 3 | RatioCareerCompany | Numérica contínua | Carreira/Tempo | Proporção da carreira na empresa atual |
| 4 | RoleStagnation | Numérica contínua | Carreira/Tempo | Proporção de tempo no mesmo cargo |
| 5 | YearsWithoutPromotion | Numérica contínua | Carreira/Tempo | Proporção de tempo sem promoção |
| 6 | AgeGroup | Categórica | Carreira/Tempo | Faixa etária (Jovem, Adulto, Senior) |
| 7 | IncomePerLevel | Numérica contínua | Compensação | Salário por nível hierárquico |
| 8 | IncomeMedianJob | Numérica contínua | Compensação | Salário vs mediana do cargo |
| 9 | RiskOvertimeDistance | Numérica contínua | Risco | Interação entre hora extra e distância |
| 10 | RiskWorklifeOvertime | Numérica contínua | Risco | Interação entre equilíbrio vida-trabalho e hora extra |


### 9. Features de Satisfação

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **SatisfactionScore** | Float | Média de satisfação geral do funcionário calculada a partir de JobSatisfaction, EnvironmentSatisfaction e RelationshipSatisfaction | 1.00 - 4.00 | Média: 2.50. Captura o nível geral de satisfação que nenhuma das três variáveis representa isoladamente |
| **CriticalSatisfactionFlag** | Integer | Indicador binário de satisfação crítica baseado no SatisfactionScore | 0 ou 1 | 1 se SatisfactionScore ≤ 2.0, senão 0. ~31% dos funcionários em estado crítico |

### 10. Features de Carreira e Tempo

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **RatioCareerCompany** | Float | Proporção de anos trabalhados na empresa em relação ao total de anos de carreira | 0.00 - 0.97 | Cálculo: YearsAtCompany / (TotalWorkingYears + 1). Valor alto = fez carreira na empresa, mais estável. Valor baixo = passou por muitas empresas |
| **RoleStagnation** | Float | Proporção de tempo no cargo atual em relação ao tempo total na empresa | 0.00 - 0.95 | Cálculo: YearsInCurrentRole / (YearsAtCompany + 1). Valor próximo de 1.0 = nunca mudou de cargo. Valor baixo = mudou recentemente |
| **YearsWithoutPromotion** | Float | Proporção de tempo sem promoção em relação ao tempo total na empresa | 0.00 - 0.94 | Cálculo: YearsSinceLastPromotion / (YearsAtCompany + 1). Valor alto = estagnado, possível frustração. Valor baixo = promovido recentemente |
| **AgeGroup** | String | Faixa etária do funcionário baseada em padrões de comportamento do mercado de trabalho | 'Jovem', 'Adulto', 'Senior' | Jovem ≤ 30 (27,63%), Adulto 31-45 (31,95%), Senior > 45 (40,42%). Jovens tendem a ter mais mobilidade, seniores buscam estabilidade |

### 11. Features de Compensação

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **IncomePerLevel** | Float | Salário mensal dividido pelo nível hierárquico do funcionário | 200.00 - 19999.00 | Cálculo: MonthlyIncome / JobLevel. Valor baixo = funcionário subvalorizado para o nível que ocupa |
| **IncomeMedianJob** | Float | Razão entre o salário do funcionário e a mediana salarial do seu cargo | 0.09 - 1.91 | Abaixo de 1.0 = ganha menos que os pares do mesmo cargo. Acima de 1.0 = ganha mais que os pares |

### 12. Features de Risco e Comportamento

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **RiskOvertimeDistance** | Float | Interação entre fazer hora extra e distância de casa ao trabalho. Captura o acúmulo de desgaste físico e mental | 0.00 - 29.00 | Cálculo: OverTime_encoded × DistanceFromHome. Valor 0 = sem hora extra (independente da distância). Valor alto = faz hora extra e mora longe |
| **RiskWorklifeOvertime** | Float | Interação entre equilíbrio vida-trabalho ruim e hora extra. Captura o acúmulo de sobrecarga percebida | 0.00 - 4.00 | Cálculo: (5 - WorkLifeBalance) × OverTime_encoded. Valor 4 = risco crítico (equilíbrio péssimo + hora extra). Valor 0 = sem hora extra |

### 13. Variáveis Encoded (versões numéricas)

| **Campo** | **Tipo** | **Descrição** | **Valores** | **Observações** |
|-----------|----------|---------------|-------------|-----------------|
| **OverTime_encoded** | Integer | Versão numérica da variável OverTime | 0 ou 1 | 0 = 'No' (não faz hora extra), 1 = 'Yes' (faz hora extra). Criada para possibilitar o cálculo das features RiskOvertimeDistance e RiskWorklifeOvertime |
| **AttritionFlag** | Integer | Versão numérica da variável alvo Attrition | 0 ou 1 | 0 = 'No' (permaneceu), 1 = 'Yes' (saiu). Criada na EDA para facilitar cálculos estatísticos e de correlação |

---