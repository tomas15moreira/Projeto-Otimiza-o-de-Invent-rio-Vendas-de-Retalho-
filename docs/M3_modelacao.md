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
| Random Forest | `n_estimators=50`, `max_depth=15` | 27,93% | 25,08% | Sem *overfitting*. A proximidade dos valores prova que o modelo não decorou o histórico. | Boa generalização, mas superado pelo concorrente. |
| XGBoost | `n_estimators=100`, `max_depth=8`, `learning_rate=0.1` | 27,04% | 22,72% | Sem *overfitting*. Margem de erro estável perante dados novos. | Melhor desempenho global. Escolhido para avançar. |

**Análise dos Resultados:**
* **Random Forest:** Este algoritmo conseguiu reduzir de forma drástica os erros face à Regressão Linear, passando a explicar cerca de 64% da variação das vendas (*Sales*) no conjunto de teste (R² de 0,6428). Contudo, os seus números globais ficaram um passo atrás.
* **XGBoost:** Revelou o melhor equilíbrio geral da experiência. Para além da excelente capacidade de generalização atestada na tabela, apresentou o menor erro médio absoluto nas previsões diárias (1163,49 euros) e a maior capacidade de explicar a variação da variável vendas (*Sales*) (R² de 0,6926).

**Diagnóstico overfitting/underfitting:** A integração da análise de *overfitting* na tabela confirmou o sucesso da abordagem. Como o erro no teste não disparou em relação ao treino em nenhum dos modelos, provou-se matematicamente que ambos os algoritmos aprenderam as regras reais do retalho, em vez de apenas memorizarem os dados.

**Conclusão da Seleção:** Tendo provado ser um algoritmo robusto e por ter vencido a Random Forest em todas as métricas de avaliação no conjunto de teste, o XGBoost foi o algoritmo selecionado de forma isolada para avançar para a fase de otimização final.
## 3. Otimização (Tuning)

Descrição de como o melhor modelo foi refinado e validado face aos objetivos de negócio.

### 3.1. Otimização de Hiperparâmetros (RandomizedSearchCV)

**Técnica Utilizada:** Aplicou-se o `RandomizedSearchCV` sobre o algoritmo XGBoost. Dada a grande dimensão do conjunto de dados, o processo foi dividido em duas fases de pesquisa sobre amostras representativas (100 mil e 60 mil registos), testando diferentes combinações de forma mais ágil. A grelha explorada concentrou-se nos hiperparâmetros com maior impacto no desempenho do algoritmo:

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

Descrição da adaptação matemática do modelo à realidade financeira da operação retalhista.

**Técnica Utilizada:** Num problema de regressão comercial, a otimização puramente estatística não é suficiente; é necessário garantir que o erro financeiro é aceitável para todas as lojas. Utilizar o erro absoluto em euros iria enviesar o algoritmo para dar primazia aos hipermercados e ignorar as lojas de bairro. Por isso, a otimização foi orientada para o impacto no negócio através do RMSPE (Root Mean Square Percentage Error). Esta métrica penaliza os desvios em percentagem, forçando o algoritmo a tratar as quebras de *Sales* com a mesma importância, independentemente da dimensão ou do volume habitual de faturação de cada loja Rossmann.

**Melhoria Obtida:** A aplicação do modelo otimizado e a avaliação rigorosa das métricas ditaram o sucesso global do projeto. Os resultados superaram largamente as restrições financeiras e operacionais impostas pelos Objetivos SMART na fase inicial. Com um R² de 0,8677, o modelo passou a conseguir explicar quase 87% de todas as flutuações das *Sales* diárias. O ganho mais crítico para o negócio observou-se na margem de erro (RMSPE), que estabilizou nos 15,92%, garantindo que as previsões orçamentais da empresa têm agora um risco de falha perfeitamente controlado e abaixo da linha vermelha dos 20%.

| Critério de Negócio | Meta SMART | Resultado Final | Cumprido |
| :--- | :---: | :---: | :---: |
| Margem de Erro (RMSPE) | $\le$ 20% | 15,92% | Sim |
| Poder Explicativo (R²) | $\ge$ 0,85 | 0,8677 | Sim |
## 4. Avaliação do Modelo Final
### 4.1. Matriz de Confusão / Erros
*Analisem onde o modelo mais falha.*
> **Análise:** (p/ex.: "O modelo ainda confunde a Classe A com a Classe B em 10% dos casos devido
à semelhança nos atributos X e Y.")
### 4.2. Importância dos Atributos (Feature Importance)
*Quais as variáveis que o modelo considerou mais importantes para decidir?*
1. [Variável X]
2. [Variável Y]
## 5. Conclusão da Fase de Modelação
*Justifiquem por que razão este modelo está pronto (ou não) para ser apresentado como solução
final.*
---
*Data de última atualização: [DD/MM/AAAA]*
