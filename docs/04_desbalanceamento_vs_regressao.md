## Por que desbalanceamento não se aplica na Regressão Linear?

O conceito de desbalanceamento **só existe em classificação** — e o motivo é simples:

---

### No problema de classificação

O modelo precisa **aprender a distinguir classes**:

```
Yes → 13% dos dados  (minoria)
No  → 87% dos dados  (maioria)
```

Se você treinar assim, o modelo "aprende" que a resposta quase sempre é **No** — porque
87% das vezes ele acerta chutando No. Ele nunca aprende a identificar o **Yes**.

> O desbalanceamento é um problema porque o modelo **conta votos** — e a classe
> majoritária sempre ganha.

---

### Na Regressão Linear

O modelo não conta votos — ele **ajusta uma reta** para minimizar o erro entre o valor
previsto e o valor real:

```
Funcionário A:  valor real = 0.73,  modelo previu = 0.68  →  erro = 0.05
Funcionário B:  valor real = 0.12,  modelo previu = 0.15  →  erro = 0.03
Funcionário C:  valor real = 0.45,  modelo previu = 0.41  →  erro = 0.04
```

Não importa se a maioria dos valores está concentrada perto de 0 — o modelo vai tentar
minimizar o erro de **todos os pontos igualmente**. Não existe "classe minoritária" para ignorar.

---

### Analogia para fixar

**Classificação** é como uma **eleição** — se 87% votam No, No sempre ganha. A minoria não tem voz.

**Regressão** é como uma **média ponderada** — cada ponto contribui com seu erro
individualmente, independente de quantos pontos estão em cada região.

---

### Mas tem um detalhe importante ⚠️

Na Regressão Linear você pode ter outro problema parecido — **distribuição assimétrica do target**:

```
Se 87% dos funcionários tem AttritionProbability próximo de 0.1
→ o modelo vai ser muito bom para prever valores baixos
→ e menos preciso para valores altos (quem realmente vai sair)
```

Isso **não se chama desbalanceamento** — se chama **distribuição assimétrica do target**
— e tratamos de forma diferente, verificando o histograma do `AttritionProbability_hidden`.

---

### Resumo

| Conceito | Classificação | Regressão Linear |
|---|---|---|
| Desbalanceamento | ⚠️ Problema real | ✅ Não se aplica |
| Distribuição assimétrica do target | Tratado via balanceamento | Verificar histograma do target |
| Como o modelo aprende | Contando classes | Minimizando erro contínuo |

> Desbalanceamento é um problema de **contagem de classes**.
> Na regressão não existem classes — existem valores contínuos —
> então o conceito simplesmente não se aplica.