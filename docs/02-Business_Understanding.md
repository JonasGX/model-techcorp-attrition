# Contexto do Problema
A TechCorp Brasil, uma das maiores empresas de tecnologia do país com mais de **50.000 funcionários**, está enfrentando um problema crítico:
sua **taxa de attrition (rotatividade de funcionários) aumentou 35%** no último ano, gerando **custos estimados em R$ 45 milhões.**

Cada funcionário que deixa a empresa representa não apenas custos de demissão e contratação **(estimados em 1,5x o salário anual)**,
mas também: - Perda de conhecimento institucional - Impacto na produtividade das equipes - Diminuição da moral dos colaboradores - Atrasos em projetos críticos

**Você foi contratado como Cientista de Dados** para desenvolver um sistema preditivo que identifique funcionários com alto risco de deixar a empresa, permitindo que o RH tome ações preventivas.


# Objetivo
Desenvolver um pipeline completo de Machine Learning para prever attrition de funcionários.

## Processamento mensal - sugestão
- Disponibilizar para o RH a probabilidade (score_y) de atrito, priorizando o risco do maior para o menor.
- Exemplo da tabela final disponibilizada para o RH:

| id_cliente | departamento | score_y | risco |
|---|---|---|---|
| 01 | Engenharia | 0.87 | alto |
| 456 | Tecnologia | 0.71 | alto |
| 56 | Produto | 0.45 | medio |
| 05 | Engenharia | 0.87 | alto |
| 891 | Marketing | 0.24 | baixo |

**Observação:** A régua para definição entre baixo, medio e alto será feita depois da análise real de distribuição das probabilidades, geradas pelo modelo treinado. Se o modelo nunca gera probabilidade acima de 60%, definir 70% como "alto" é irrelevante.



# Business Understanding

## Dados do contexto:
| Item | Valor |
|---|---|
| Quantidade de funcionários | 50.000 |
| Aumento na taxa de attrition | 35% |
| Custo total estimado no ano | R$ 45M |
| Custo de saída por funcionário| 1,5 * salário anual |


## Quanto custa o atrittion?

- Salário médio(Suposição): R$ 8.000,00/mês
- Salário anual: R$ 96.000,00/ano (salário médio * 12)
- Custo de saída por funcionário anual: R$ 144.000,00 (salário anual * 1,5)
- Quantidade de funcionários que saíram no ano: 45.000.000 / 144.000 = ~ 312


**O custo de atrittion por funcionário está em ~ R$ 144.000,00**


## Qual impacto financeiro?

### Economia gerada pelo modelo
| Item | Valor |
|---|---|
| Custo total atual | R$ 45.000.000 |
| Redução de 10% anual   | R$ 4.500.000 economizados|
| Redução de 10% mensal   | R$ 375.000 economizados |
| Funcionários retidos | ~31 funcionários |

### Custo do projeto
| Item | Valor |
|---|---|
| Desenvolvimento do modelo | ~ R$ 80.000 |
| Infraestrutura (cloud/deploy)    | ~ R$ 40.000|
| Manutenção anual | ~ R$ 30.000 |
| Total estimado | ~ R$ 150.000 | 

### ROI do projeto
ROI = (Ganho obtido - Custo do investimento) / Custo do investimento

ROI = (4.500.000 - 150.000) / 150.000
ROI = 4.350.000 / 150.000
ROI = 29x  ≈  30x

**Tivemos um retorno de 30x mais do que foi investido**

### O risco da falha

- Métrica de negócio: Qual o real impacto financeiro de reduzir o attrition em 10%

- O pronto mais crítico do projeto é o custo do erro

| Erro | O que significa? | Impacto |
|---|---|---|
| Falso Negativo| Não identificar quem vai sair | 🔴 Alto |
| Falso Positivo | Apontar  quem não vai sair como risco | 🟢 Baixo |


## Tipos de métricas - Classificação

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


# Conclusões

- Concluímos que o Falso Negativo é o mais custoso para a empresa.
- A cada falha que o modelo comete a empresa perde por funcionáiro ~ R$ 144.000,00/ano.
- Queremos que o modelo erre menos em "deixando passar quem vai sar da empresa".
- Podemos utilizar métricas como Recall com priorizaçao em não perder casos reais.

**Observações**: Recall e Precision vivem em tensão, pois ao melhorar um tende a piorar o outro. Na avaliação do modelo, podemos utilizar o F1-Score para a média harmônica, porém tendo um olho especial para o Recall para área de negócio.

### Por que recall ?

#### Cenário 1 — Falso Negativo (Recall baixo)
Funcionário vai sair → modelo não identificou → RH não agiu
→ Funcionário saiu
→ Custo: R$ 144.000 ❌

#### Cenário 2 — Falso Positivo (Precision baixa)
Funcionário não ia sair → modelo apontou como risco → RH conversou com ele
→ Funcionário ficou (ou já ia ficar)
→ Custo: 1 hora do RH ✅

#### Definição de sucesso do modelo
| Métrica   | Mínimo Aceitável | Justificativa                                          |
|---|---|---|
| Recall    | ≥ 75%  | Pegar pelo menos 3 de cada 4 funcionários em risco.   |
| F1-Score  | ≥ 70%  | Garantir equilíbrio mínimo com Precision.             |

**Observação:** Sem aplicação da AUC-ROC pois não conheço muito bem.