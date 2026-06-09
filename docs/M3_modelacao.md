# Milestone 3: Modelação e Avaliação
## 1. Estratégia de Modelação
**Divisão do Dataset:** Ao contrário das divisões estatísticas comuns, a separação dos dados não foi feita de forma aleatória. Como o objetivo é prever o volume de vendas(*Sales*) no futuro, optou-se por uma divisão temporal para evitar qualquer fuga de informação (data leakage). O histórico foi ordenado de forma cronológica, reservando os 80% de registos mais antigos para treinar o algoritmo e guardando os 20% mais recentes para a fase de teste. Desta forma, simulou-se o cenário real do retalho, obrigando o modelo a usar apenas o passado para tentar prever o futuro.

**Métricas de Sucesso:** Por se tratar da resolução de um problema de regressão contínua, os modelos foram avaliados com três indicadores complementares. É importante referir que, antes do cálculo das métricas, a transformação logarítmica aplicada à variável *Sales_log* foi revertida, garantindo que a avaliação final fosse feita e interpretada na escala original de euros:
* A métrica principal escolhida foi o **RMSPE** (Root Mean Square Percentage Error). A escolha recaiu sobre este indicador porque avalia o erro em percentagem, o que permite uma comparação entre lojas de dimensões muito diferentes (um erro de 100 euros é muito grave numa loja de bairro pequena, mas quase insignificante num hipermercado).
* Como complemento de fácil interpretação para o negócio, utilizou-se o **MAE** (Erro Médio Absoluto) para avaliar, em euros, quanto é que as previsões falhavam face à realidade diária.
* Por fim, analisou-se o **R²** (Coeficiente de Determinação) para validar a eficácia estatística, medindo que proporção das oscilações das vendas o algoritmo conseguia efetivamente explicar.
## 2. Experiências Realizadas
### 2.1. Modelo Baseline
**O Ponto de Partida Simples:** Para estabelecer um patamar mínimo de desempenho, treinou-se um modelo inicial básico. O objetivo não era encontrar a solução definitiva, mas sim criar uma métrica base de comparação. Todos os modelos mais avançados terão de superar obrigatoriamente estes valores para que a sua complexidade adicional se justifique.

**Algoritmo:** Regressão Linear

**Resultados no Conjunto de Teste:**
* **RMSPE:** 41,42%
* **MAE:** 1929,05 euros
* **R²:** 0,2008

**Análise do Baseline:** Os valores obtidos foram fracos, um resultado que já era esperado. O R² revela que este algoritmo inicial apenas consegue explicar cerca de 20% da variação das vendas(*Sales*), apresentando um erro médio nas previsões a rondar os 1929 euros. Este desempenho vem confirmar as conclusões da fase de análise exploratória: as relações entre as características das lojas e a faturação não são lineares. Por natureza, um modelo linear é incapaz de captar essas dinâmicas mais complexas, validando assim a necessidade de avançar para modelos baseados em árvores de decisão nas etapas seguintes.
## 2.2. Modelos Candidatos

**Justificação da Escolha:** Após os resultados do baseline, avançou-se para algoritmos da família *ensemble*, que combinam várias árvores de decisão para tentar captar as relações complexas e não lineares nos dados de *Sales*. Os dois algoritmos selecionados para teste foram a Random Forest e o XGBoost.

**Comparação de Desempenho e Diagnóstico:**

| Algoritmo | Parâmetros Base | RMSPE (Treino) | RMSPE (Teste) | Diagnóstico de Overfitting | Notas |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Random Forest | `n_estimators=50`, `max_depth=15` | 27,93% | 25,08% | *Overfitting* ligeiro. A proximidade dos valores prova que o modelo não decorou o histórico. | Boa generalização, mas superado pelo concorrente. |
| XGBoost | `n_estimators=100`, `max_depth=8`, `learning_rate=0.1` | 27,04% | 22,72% | *Overfitting* ligeiro. Margem de erro estável perante dados novos. | Melhor desempenho global. Escolhido para avançar. |

**Análise dos Resultados:**
* **Random Forest:** Este algoritmo conseguiu reduzir de forma drástica os erros face à Regressão Linear, passando a explicar cerca de 64% da variação das vendas (*Sales*) no conjunto de teste (R² de 0,6428). Contudo, os seus números globais ficaram um passo atrás.
* **XGBoost:** Revelou o melhor equilíbrio geral da experiência. Para além da excelente capacidade de generalização atestada na tabela, apresentou o menor erro médio absoluto nas previsões diárias (1163,49 euros) e a maior capacidade de explicar a variação da variável vendas (*Sales*) (R² de 0,6926).

**Diagnóstico overfitting/underfitting:** A integração da análise de *overfitting* na tabela confirmou o sucesso da abordagem. Como o erro no teste não disparou em relação ao treino em nenhum dos modelos, a proximidade dos valores indica que os modelos generalizam bem.

**Conclusão da Seleção:** Tendo provado ser um algoritmo robusto e por ter vencido a Random Forest em todas as métricas de avaliação no conjunto de teste, o XGBoost foi o algoritmo selecionado de forma isolada para avançar para a fase de otimização final.
## 3. Otimização (Tuning)

Descrição de como o melhor modelo foi refinado e validado face aos objetivos de negócio.

### 3.1. Otimização de Hiperparâmetros (RandomizedSearchCV)

**Técnica Utilizada:** Aplicou-se o `RandomizedSearchCV` sobre o algoritmo XGBoost. Dada a grande dimensão do conjunto de dados, o processo foi dividido em duas fases de pesquisa sobre uma amostra representativa de 100 mil registos, testando diferentes combinações de forma mais ágil. A grelha explorada concentrou-se nos hiperparâmetros com maior impacto no desempenho do algoritmo:

| Hiperparâmetro | Valores Testados (Fase 1 e 2) | Objetivo |
| :--- | :--- | :--- |
| `max_depth` | [6, 8, 10, 12] | Limitar a profundidade das árvores para evitar a memorização de padrões específicos (*overfitting*). |
| `learning_rate` | [0.05, 0.1, 0.2, 0.3] | Controlar a velocidade da aprendizagem, tornando o modelo mais gradual e robusto. |
| `n_estimators` | [100, 200, 300, 500, 700, 900, 1200] | Definir o número ideal de árvores de decisão a combinar sequencialmente. |
| `subsample` | [0.8, 1.0] | Controlar a percentagem de dados fornecida a cada árvore para melhorar a capacidade de generalização. |

**Melhoria Obtida:** A pesquisa identificou a configuração ideal (`n_estimators=500`, `max_depth=8`, `learning_rate=0.2`, `subsample=1.0`). O erro percentual (RMSPE) no conjunto de teste desceu de 22,72% para 15,92% após o ajuste, um ganho de 6,8 pontos percentuais. A validação cruzada K-Fold (K=5) confirmou a estabilidade absoluta do modelo, registando um R² médio de 0,92 com um desvio padrão de apenas 0,0021 entre as 5 dobras, provando que os resultados generalizam de forma consistente.

| Fase | RMSPE (Teste) | R² (Teste) | Δ RMSPE |
| :--- | :--- | :--- | :--- |
| XGBoost Base | 22,72% | 0,6926 | — |
| XGBoost Otimizado | 15,92% | 0,8677 | Melhoria de 6,8 p.p. |

### 3.2. Validação Orientada ao Negócio (Objetivos SMART)

**Porquê o RMSPE como métrica principal:** Num problema de regressão como este, não basta olhar para o erro estatístico, é preciso garantir que o erro é aceitável para lojas de dimensões diferentes. Se usássemos apenas o erro em euros, o modelo daria mais peso às lojas de maior volume e trataria com menos rigor as lojas mais pequenas, já que um mesmo erro em euros pesa de forma diferente conforme o tamanho da loja. Por isso, a métrica principal foi o RMSPE, que mede o erro em percentagem. Desta forma, o modelo trata todas as lojas com a mesma importância, independentemente do seu volume habitual de vendas.

**Resultado face à meta:** O modelo otimizado cumpriu os dois critérios definidos no objetivo SMART. O R² de 0,8677 significa que o modelo explica cerca de 87% da variação das vendas diárias. O RMSPE ficou nos 15,92%, abaixo do limite de 20% que tinha sido estabelecido. Ambos os critérios foram cumpridos com margem.

| Critério | Meta SMART | Resultado Final | Cumprido |
| :--- | :---: | :---: | :---: |
| Margem de erro (RMSPE) | ≤ 20% | 15,92% | Sim |
| Poder explicativo (R²) | ≥ 0,85 | 0,8677 | Sim |
## 4. Avaliação do Modelo Final
### 4.1. Análise de Resíduos e Diagnóstico de Erros

Tratando-se de um problema de regressão, a avaliação dos erros foi feita através da análise de resíduos, e não de uma matriz de confusão, que se aplica apenas a problemas de classificação. Um resíduo é a diferença entre o valor real das vendas e o valor previsto pelo modelo.

**Distribuição dos erros:** O gráfico de resíduos [`reports/figures/analise_residuos.png`](reports/figures/analise_residuos.png) mostra que os erros se concentram em torno do zero, o que indica que o modelo acerta na maioria das previsões. A dispersão dos resíduos aumenta à medida que as vendas previstas crescem, formando um padrão em funil. Isto significa que o modelo é mais preciso nas lojas de vendas baixas e médias e tende a errar mais, em valor absoluto, nas lojas de vendas elevadas. O resíduo médio é de cerca de 258 euros, ligeiramente positivo, o que indica que o modelo tende a subestimar muito ligeiramente as vendas reais.

**Onde o modelo mais falha:** Para perceber em que situações o modelo erra mais, isolaram-se os 5% de previsões com maior erro percentual, que apresentam um erro médio de 41%. A análise destas falhas revelou um padrão claro: os dias de fim de semana estão sobre-representados nas piores previsões, correspondendo a 24% dos casos, contra 17% no conjunto geral.

Este resultado é coerente com a análise exploratória da fase anterior, que já tinha identificado um comportamento mais irregular das vendas ao fim de semana, sobretudo ao domingo, devido ao encerramento de muitas lojas. O modelo tem, assim, mais dificuldade em prever as vendas nestes dias atípicos. A principal limitação do modelo encontra-se, portanto, nos dias de fim de semana e nas lojas de vendas muito elevadas.
### 4.2. Importância dos Atributos (Feature Importance)

O XGBoost permite medir quanto cada variável contribuiu para as previsões do modelo. Esta análise responde diretamente às perguntas de investigação sobre quais os fatores que mais influenciam as vendas e quais as variáveis que mais contribuem para a previsão. As variáveis mais importantes foram as seguintes:

1. **Tipo de loja "b" (*StoreType_b*)**, com uma importância de 0,24, de longe a mais determinante.
2. **Promoção (*Promo*)**, com 0,18, confirmando o forte efeito das campanhas promocionais.
3. **Adesão à promoção contínua (*Promo2*)**, com 0,07.
4. **Variedade de produtos do tipo "c" (*Assortment_c*)**, com 0,07.
5. **Distância à concorrência (*CompetitionDistance*)**, com 0,06.

**Análise:** A variável mais influente é, com larga vantagem, o tipo de loja "b". Este resultado confirma de forma clara o padrão identificado na análise exploratória, em que o tipo de loja "b", apesar de ser o menos frequente, era o que apresentava vendas medianas mais elevadas. O modelo veio confirmar que esta é a característica mais determinante para prever as vendas. A promoção surge como o segundo fator mais importante, o que está de acordo com o efeito já observado na fase exploratória. No conjunto, são sobretudo o perfil da loja (tipo e variedade) e as promoções que determinam o volume de vendas.

**Um resultado a destacar:** A variável *FimDeSemana*, criada na fase de preparação, apresentou uma importância nula. Isto não significa que o fim de semana não afete as vendas, mas sim que essa informação já está contida na variável *DayOfWeek*, que distingue os sete dias da semana de forma mais detalhada. Sendo o *FimDeSemana* derivado do *DayOfWeek*, o modelo optou por usar a variável original, tornando a criada redundante. De forma semelhante, a variável *Dia* do mês revelou uma importância reduzida. Esta análise mostra que nem todas as variáveis criadas se revelaram úteis, o que é em si um resultado válido, ao distinguir as características que realmente contribuem para a previsão daquelas que são redundantes.
## 5. Conclusão da Fase de Modelação

A fase de modelação cumpriu o objetivo definido para o projeto. Partindo de um modelo de base simples, foram testados e comparados modelos progressivamente mais sofisticados, com uma melhoria clara do desempenho em cada etapa.

### Evolução do Desempenho

| Modelo | RMSPE (Teste) | MAE (Teste) | R² (Teste) |
| :--- | :---: | :---: | :---: |
| Regressão Linear (Baseline) | 41,42% | 1929,05 € | 0,2008 |
| Random Forest | 25,08% | 1238,39 € | 0,6428 |
| XGBoost (base) | 22,72% | 1163,49 € | 0,6926 |
| XGBoost (otimizado) | 15,92% | 761,68 € | 0,8677 |

A progressão é clara: o R² subiu de 0,20 para 0,87 e o erro médio das previsões desceu de 1929 para 762 euros, uma redução de mais de 60%.

### Porque está o modelo pronto a ser apresentado como solução

O modelo final cumpre os dois critérios definidos no objetivo SMART, com um RMSPE de 15,92% (abaixo do limite de 20%) e um R² de 0,8677 (acima do mínimo de 0,85), ambos medidos no conjunto de teste, ou seja, em dados que o modelo nunca viu durante o treino.

Para além de cumprir a meta, o modelo foi validado de forma rigorosa. A divisão temporal dos dados garantiu que não houve fuga de informação. A validação cruzada com cinco dobras confirmou a estabilidade dos resultados, com um desvio padrão de apenas 0,0021. A análise de resíduos e da importância das variáveis permitiu compreender como o modelo decide e onde tem limitações. Por estas razões, considera-se que o modelo está pronto para ser apresentado como solução do projeto.

### Limitações a ter em conta

Apesar de cumprir os objetivos, o modelo apresenta limitações que devem ser reconhecidas. Mostra um sobreajuste (*overfitting*) ligeiro, com um desempenho um pouco superior no treino do que no teste, embora a diferença seja moderada e o desempenho no teste se mantenha forte. Tem também maior dificuldade em prever as vendas nos dias de fim de semana e nas lojas de vendas muito elevadas, como ficou evidente na análise de resíduos. Por fim, os dias de feriado, sendo pouco frequentes no histórico, oferecem menos informação ao modelo, o que pode tornar as previsões menos fiáveis nesses períodos.

Estas limitações não comprometem a utilidade do modelo, mas indicam onde as suas previsões devem ser usadas com mais cuidado. A sua resolução constitui uma via natural de melhoria futura, a abordar na fase final do projeto.

---
*Data de última atualização: [10/06/2026]*
