# Milestone 1: Iniciação e Definição do Projeto
## 1. Descrição Detalhada do Problema
### Contexto e Relevância
A gestão de inventário é um dos maiores desafios operacionais do setor do retalho. Uma loja que tenha produtos a menos arrisca-se a ruturas de stock, perdendo vendas e clientes para a concorrência. Uma loja que tenha produtos a mais imobiliza capital em mercadoria parada e, no caso de bens perecíveis, incorre em desperdício. O equilíbrio entre estes dois extremos depende, em grande medida, da capacidade de antecipar quanto se vai vender. Quanto melhor uma cadeia conseguir prever as suas vendas, melhor consegue dimensionar as encomendas e a reposição de mercadoria.

A relevância deste problema é clara num setor onde as margens são apertadas e onde a procura está sujeita a variações marcadas. As vendas não são estáveis ao longo do tempo. Sobem e descem consoante o dia da semana, a proximidade de feriados, a presença de promoções e padrões sazonais que se repetem ao longo do ano. O verdadeiro desafio técnico está precisamente em conseguir captar estes padrões e traduzi-los numa previsão, distinguindo o que é uma variação normal do que é um pico ou uma quebra excecional.

### Dataset e a Variável Objetivo

Este projeto debruça-se sobre os dados de vendas da Rossmann, uma grande cadeia de retalho europeia, com mais de mil lojas. O conjunto de dados reúne o histórico diário de vendas de 1115 lojas, entre janeiro de 2013 e julho de 2015, num total superior a um milhão de registos. Para cada dia e cada loja, sabe-se o valor das vendas, o número de clientes, se a loja esteve aberta, se havia uma promoção em curso e se o dia coincidiu com um feriado nacional ou escolar. A estes dados juntam-se as características de cada loja, como o tipo de loja, a variedade de produtos disponível, a distância ao concorrente mais próximo e a participação em campanhas promocionais contínuas.

O problema central que se pretende resolver é a previsão do volume de vendas diárias de cada loja. Trata-se de um problema de regressão, uma vez que a variável a prever é um valor numérico contínuo, e não uma categoria. Prever este valor com rigor permitiria à cadeia tomar decisões de reabastecimento mais conscientes e de forma antecipada, reduzindo simultaneamente o risco de rutura e o de excesso de inventário.

## 2. Objetivos SMART

Desenvolver um modelo de regressão para estimar o volume de vendas diárias por loja da cadeia Rossmann, com base no histórico de vendas de janeiro de 2013 a julho de 2015, atingindo um erro percentual médio (RMSPE) igual ou inferior a 20% e um coeficiente de determinação (R²) igual ou superior a 0,85, até às 18h do dia 14 de junho de 2026.

Este objetivo cumpre os cinco critérios da metodologia SMART:

- Específico: pretende prever o volume de vendas diárias por loja, através de um modelo de regressão.
- Mensurável: o sucesso é avaliado por duas métricas complementares, o erro percentual médio (RMSPE) e o coeficiente de determinação (R²). A primeira mede a dimensão relativa do erro das previsões e a segunda mede a proporção da variação das vendas que o modelo consegue explicar.
- Atingível: o conjunto de dados reúne mais de um milhão de registos e variáveis suficientes para sustentar uma previsão com esta qualidade.
- Relevante: a previsão das vendas responde diretamente ao problema da gestão de inventário, permitindo reduzir o risco de rutura de stock e de excesso de mercadoria.
- Temporal: o objetivo está limitado pela data de entrega do projeto, às 18h do dia 14 de junho de 2026.

## 3. Perguntas de Investigação

1. Quais são os fatores que mais influenciam o volume de vendas diárias das lojas Rossmann?
2. Existe relação entre a realização de promoções e o aumento das vendas?
3. De que forma os dias festivos e os fins de semana afetam o volume de vendas?
4. Quais são as variáveis que mais contribuem para a previsão das vendas no modelo final?
5. O tipo de loja e a variedade de produtos influenciam o comportamento das vendas?
## 3. Metodologia de Gestão (PBL)
* **Divisão de Tarefas:**
 * **Membro A:** Responsável pela Engenharia de Dados.
 * **Membro B:** Responsável pela Modelação Estatística.
 * **Membro C:** Responsável pela Visualização e Documentação.
* **Ferramentas de Colaboração:** [Ex: GitHub Projects para Kanban, reuniões semanais via
Teams/Discord].
## 4. Análise de Viabilidade dos Dados
* **Disponibilidade:** [Os dados já foram descarregados? Estão em base de dados?]
* **Qualidade Inicial:** [Ex: Notámos que faltam dados de datas em algumas colunas, precisaremos
de tratar isso na M2.]
* **Ética:** [Os dados cumprem o RGPD? Estão anonimizados?]
## 5. Cronograma Interno
| Fase | Data Limite | Entregável Esperado |
| :--- | :--- | :--- |
| M1: Iniciação | [Data] | Repositório estruturado e Plano de Projeto. |
| M2: Exploração | [Data] | Notebook de EDA e Dados Processados. |
| M3: Modelação | [Data] | Comparação de algoritmos e métricas. |
| M4: Finalização| [Data] | Pitch e Relatório Final. |
---
*Data de última atualização: [DD/MM/AAAA]*
