## Metadados do Dataset - RH Analytics

| Coluna | Descrição |
|---|---|
| Age | Idade do funcionário |
| Attrition | Indica se o funcionário saiu da empresa (variável alvo) |
| BusinessTravel | Frequência de viagens de trabalho |
| DailyRate | Taxa salarial diária em USD |
| Department | Departamento em que o funcionário trabalha |
| DistanceFromHome | Distância da residência ao escritório em km |
| Education | Nível de escolaridade em escala ordinal de 1 a 5 |
| EducationField | Área de formação acadêmica |
| EmployeeCount | Contagem de funcionários, coluna constante sem variabilidade |
| EmployeeNumber | Identificador único do funcionário |
| EnvironmentSatisfaction | Satisfação com o ambiente de trabalho em escala de 1 a 4 |
| Gender | Gênero do funcionário |
| HourlyRate | Taxa salarial por hora em USD |
| JobInvolvement | Grau de envolvimento com o trabalho em escala de 1 a 4 |
| JobLevel | Nível hierárquico do cargo em escala de 1 a 5 |
| JobRole | Cargo ou função do funcionário |
| JobSatisfaction | Satisfação com o cargo em escala de 1 a 4 |
| MaritalStatus | Estado civil do funcionário |
| MonthlyIncome | Renda mensal em USD |
| MonthlyRate | Taxa mensal associada ao funcionário em USD |
| NumCompaniesWorked | Número de empresas anteriores na carreira |
| Over18 | Indica se o funcionário é maior de 18 anos, coluna constante |
| OverTime | Indica se o funcionário realiza horas extras |
| PercentSalaryHike | Percentual de aumento salarial no último ciclo de avaliação |
| PerformanceRating | Avaliação de desempenho em escala de 1 a 4 |
| RelationshipSatisfaction | Satisfação com relacionamentos no trabalho em escala de 1 a 4 |
| StandardHours | Horas padrão de trabalho, coluna constante sem variabilidade |
| StockOptionLevel | Nível de opções de ações concedidas em escala de 0 a 3 |
| TotalWorkingYears | Total de anos de experiência profissional acumulados |
| TrainingTimesLastYear | Número de treinamentos realizados no último ano |
| WorkLifeBalance | Percepção do equilíbrio entre vida pessoal e profissional em escala de 1 a 4 |
| YearsAtCompany | Anos de trabalho na empresa atual |
| YearsInCurrentRole | Anos no cargo atual dentro da empresa |
| YearsSinceLastPromotion | Anos desde a última promoção |
| YearsWithCurrManager | Anos trabalhando com o gestor atual |


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

## 🔧 Features Engineered (Criadas no Pipeline)

| **Nova Feature** | **Fórmula** | **Descrição** | **Objetivo** |
|------------------|-------------|---------------|--------------|
| **IncomePerYear** | `MonthlyIncome / (TotalWorkingYears + 1)` | Renda normalizada por experiência | Medir eficiência salarial |
| **TotalSatisfaction** | `Mean(Environment, Job, Relationship Satisfaction)` | Satisfação média geral | Indicador composto de satisfação |
| **PromotionRate** | `YearsSinceLastPromotion / (YearsAtCompany + 1)` | Taxa de promoção | Velocidade de crescimento |
| **LongTimeNoPromotion** | `YearsSinceLastPromotion > 5` | Flag de estagnação | Identificar funcionários estagnados |
| **HighPerformer** | `PerformanceRating >= 4` | Flag de alta performance | Identificar top performers |
| **CompanyChangeRate** | `NumCompaniesWorked / (TotalWorkingYears + 1)` | Taxa de mudança de empresa | Medir estabilidade profissional |
| **AgeStartedWorking** | `Age - TotalWorkingYears` | Idade que começou a trabalhar | Perfil de carreira |
| **YearsInOtherCompanies** | `TotalWorkingYears - YearsAtCompany` | Anos em outras empresas | Experiência externa |
| **AttritionRiskScore** | `Weighted combination of risk factors` | Score de risco composto | Predição simplificada |

---

## 📈 Estatísticas Importantes

### Distribuição da Variável Alvo

Attrition:

- No:  1233 (83.9%)
- Yes: 237 (16.1%)

### Correlações Importantes com Attrition

| **Variável** | **Correlação** | **Insight** |
|--------------|----------------|-------------|
| **OverTime** | Alta | Funcionários com overtime têm maior attrition |
| **JobSatisfaction** | Negativa | Menor satisfação = maior attrition |
| **Age** | Negativa | Funcionários mais jovens têm maior attrition |
| **YearsAtCompany** | Negativa | Menos tempo na empresa = maior attrition |
| **MonthlyIncome** | Negativa | Salários menores = maior attrition |
| **DistanceFromHome** | Positiva | Maior distância = maior attrition |

---

## 🚨 Considerações para Modelagem

### Variáveis a Remover
- `EmployeeCount` - Sempre 1
- `Over18` - Sempre 'Y'
- `StandardHours` - Sempre 80
- `EmployeeNumber` - ID único (não usar para predição)

### Tratamento de Variáveis Categóricas
- **One-Hot Encoding**: Department, EducationField, JobRole, MaritalStatus, Gender
- **Label Encoding**: BusinessTravel, OverTime (binárias ordenadas)
- **Manter como numérica**: Education, JobLevel (ordinais)

### Possíveis Interações
- Age × MaritalStatus
- MonthlyIncome × JobLevel
- YearsAtCompany × YearsSinceLastPromotion
- JobSatisfaction × EnvironmentSatisfaction

### Validação de Dados
```python
# Regras de validação
assert df['Age'].between(18, 65).all()
assert df['Education'].between(1, 5).all()
assert df['PerformanceRating'].isin([3, 4]).all()
assert df['YearsInCurrentRole'] <= df['YearsAtCompany']
assert df['YearsSinceLastPromotion'] <= df['YearsAtCompany']
assert df['YearsWithCurrManager'] <= df['YearsAtCompany']
```