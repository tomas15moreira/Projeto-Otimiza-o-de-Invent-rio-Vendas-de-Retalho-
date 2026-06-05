# Milestone 1: Iniciação e Definição do Projeto
## 1. Descrição Detalhada do Problema
### Contexto e Relevância
A gestão de inventário é um dos maiores desafios operacionais do setor do retalho. Uma loja que tenha produtos a menos arrisca-se a ruturas de stock, perdendo vendas e clientes para a concorrência. Uma loja que tenha produtos a mais imobiliza capital em mercadoria parada e, no caso de bens perecíveis, incorre em desperdício. O equilíbrio entre estes dois extremos depende, em grande medida, da capacidade de antecipar quanto se vai vender. Quanto melhor uma cadeia conseguir prever as suas vendas, melhor consegue dimensionar as encomendas e a reposição de mercadoria.

A relevância deste problema é clara num setor onde as margens são apertadas e onde a procura está sujeita a variações marcadas. As vendas não são estáveis ao longo do tempo. Sobem e descem consoante o dia da semana, a proximidade de feriados, a presença de promoções e padrões sazonais que se repetem ao longo do ano. O verdadeiro desafio técnico está precisamente em conseguir captar estes padrões e traduzi-los numa previsão, distinguindo o que é uma variação normal do que é um pico ou uma quebra excecional.

### Dataset e a Variável Objetivo

Este projeto debruça-se sobre os dados de vendas da Rossmann, uma grande cadeia de retalho europeia, com mais de mil lojas. O conjunto de dados reúne o histórico diário de vendas de 1115 lojas, entre janeiro de 2013 e julho de 2015, num total superior a um milhão de registos. Para cada dia e cada loja, sabe-se o valor das vendas, o número de clientes, se a loja esteve aberta, se havia uma promoção em curso e se o dia coincidiu com um feriado nacional ou escolar. A estes dados juntam-se as características de cada loja, como o tipo de loja, a variedade de produtos disponível, a distância ao concorrente mais próximo e a participação em campanhas promocionais contínuas.

O problema central que se pretende resolver é a previsão do volume de vendas diárias de cada loja. Trata-se de um problema de regressão, uma vez que a variável a prever é um valor numérico contínuo, e não uma categoria. Prever este valor com rigor permitiria à cadeia tomar decisões de reabastecimento mais conscientes e de forma antecipada, reduzindo simultaneamente o risco de rutura e o de excesso de inventário.

## 2. Objetivos SMART

* **Objetivo:** Desenvolver um modelo de regressão para estimar o volume de vendas diárias por loja da cadeia Rossmann, com base no histórico de vendas de janeiro de 2013 a julho de 2015, atingindo um erro percentual médio (RMSPE) igual ou inferior a 20% e um coeficiente de determinação (R²) igual ou superior a 0,85, até dia 14 de junho de 2026.

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

## 4. Metodologia de Desenvolvimento

O planeamento segue a metodologia estruturada CRISP-DM (Cross-Industry Standard Process for Data Mining) e iterativa padrão da indústria analítica, assegurando o alinhamento da vertente tecnológica com as exigências do negócio. O trabalho encontra-se segmentado pelas fases metodológicas e respetivos marcos de entrega que se apresentam na tabela.

| Fase de Desenvolvimento | Descrição da Etapa | Integração no Projeto |
| :--- | :--- | :--- |
| Business Understanding | Definição do problema de gestão, clarificação de objetivos e dos critérios de sucesso. | Milestone 1 |
| Data Understanding | Avaliação inicial da estrutura analítica, qualidade da informação e natureza das variáveis. | Milestone 1 e Milestone 2 |
| Data Preparation | Processamento de valores omissos e criação de novos atributos relevantes para a análise. | Milestone 2 |
| Modeling | Treino comparativo e otimização dos diferentes algoritmos com foco na tarefa de previsão. | Milestone 3 |
| Evaluation | Medição do desempenho do sistema através das métricas estipuladas na fase de definição. | Milestone 3 e Milestone 4 |


## 5. Metodologia de Gestão (PBL)
## Gestão do Projeto e Responsabilidades

O desenvolvimento deste projeto será assegurado de forma inteiramente individual. Assumirei a responsabilidade integral por todas as fases do ciclo de vida analítico, o que engloba a engenharia de dados para a extração e preparação da informação, a modelação estatística para a criação e afinação do algoritmo preditivo e a vertente de visualização e redação do documento final.

A execução individual exige um rigor acrescido na organização das etapas de trabalho. A gestão do código e o planeamento das tarefas associadas a cada entrega serão conduzidos através do GitHub, garantindo um controlo de versões estruturado. Assegurando um repositório robusto e analisável ao longo de todo o processo.

## 6. Análise de Viabilidade dos Dados

### Disponibilidade
O conjunto de dados utilizado é o Rossmann Store Sales, descarregado a partir da plataforma Kaggle através da hiperligação.

> https://www.kaggle.com/datasets/shahpranshu27/rossman-store-sales.

A informação encontra-se distribuída por ficheiros no formato CSV, com destaque para o histórico de treino e para o descritivo das lojas. Esta estrutura adequa-se à dimensão do projeto, que engloba mais de um milhão de registos, permitindo a leitura e a manipulação direta com bibliotecas analíticas dentro de um ambiente de cadernos interativos, dispensando a necessidade de uma base de dados relacional.

### Qualidade Inicial dos Dados
Uma inspeção inicial aos ficheiros revelou uma excelente integridade na tabela principal de treino, não se registando qualquer valor omisso na totalidade das observações diárias. Os tipos de dados apresentam-se globalmente coerentes com a sua natureza, embora a variável temporal exija conversão para o formato de data adequado. O principal desafio analítico prende-se com os valores omissos detetados na tabela descritiva das lojas. A análise confirmou que estas lacunas não representam falhas de recolha, mas sim uma ausência de evento real, como a inexistência de um concorrente próximo ou a não adesão a campanhas promocionais contínuas. Esta característica exigirá uma estratégia de imputação metodológica para preservar o sinal analítico dessas ausências. Adicionalmente, a variável alvo apresenta uma forte assimetria positiva, o que sugere a necessidade futura de transformações matemáticas para estabilizar o modelo de regressão.

### Dicionário de Variáveis
A tabela seguinte consolida as variáveis mais relevantes presentes nos dois ficheiros de dados, definindo a sua natureza e o papel que desempenharão na modelação estatística.

| Variável | Ficheiro | Tipo | Definição Operacional |
| :--- | :--- | :--- | :--- |
| Store | Ambos | Numérica discreta | Identificador único de cada estabelecimento |
| DayOfWeek | Train | Numérica discreta | Dia da semana correspondente à transação |
| Date | Train | Data | Data exata do registo de atividade |
| Sales | Train | Numérica contínua | Volume de vendas diário gerado (Variável Alvo) |
| Customers | Train | Numérica discreta | Número total de clientes contabilizados na loja |
| Open | Train | Binária | Indica se o estabelecimento operou nesse dia |
| Promo | Train | Binária | Indica a vigência de uma promoção local |
| StateHoliday | Train | Categórica | Assinala a ocorrência de um feriado estatal |
| SchoolHoliday | Train | Binária | Assinala a ocorrência de um feriado escolar |
| StoreType | Store | Categórica | Modelo comercial e diferenciador de cada loja |
| Assortment | Store | Categórica | Nível de profundidade e variedade do sortido |
| CompetitionDistance | Store | Numérica contínua | Distância em metros ao concorrente mais próximo |
| Promo2 | Store | Binária | Indica a adesão a uma campanha promocional contínua |

### Ética e Conformidade
O conjunto de dados não contém dados pessoais ou comportamentais associados a clientes individuais. Todas as variáveis referem-se exclusivamente a métricas de desempenho operacional e às características físicas ou comerciais dos próprios estabelecimentos da cadeia de retalho. Tratando-se de informação de matriz corporativa, previamente anonimizada e disponibilizada publicamente para efeitos de investigação analítica, o seu uso encontra-se em total conformidade com o Regulamento Geral sobre a Proteção de Dados e respeita os mais estritos princípios éticos aplicáveis à Ciência de Dados.

## 7. Planeamento e Cronograma Interno

A evolução das atividades encontra-se organizada de acordo com datas limite, garantindo o cumprimento do prazo regulamentar e permitindo uma evolução faseada e controlada do desenvolvimento analítico.

| Fase de Execução | Data Limite | Entregável Esperado |
| :--- | :--- | :--- |
| Milestone 1: Iniciação | 5 de junho de 2026 | Repositório estruturado e plano de projeto concluído. |
| Milestone 2: Exploração | 9 de junho de 2026 | Caderno de análise exploratória de dados e matrizes processadas. |
| Milestone 3: Modelação | 12 de junho de 2026 | Treino comparativo de algoritmos e relatório de métricas obtidas. |
| Milestone 4: Finalização | 14 de junho de 2026 | Elaboração do documento final e suporte visual para apresentação. |

## 8. Referências Bibliográficas

Doran, G. T. (1981). There's a S.M.A.R.T. way to write management's goals and objectives. Management Review, 70(11), 35-36.

Rossmann Store Sales. (2015). Conjunto de dados alojado na plataforma Kaggle. Recuperado a partir da hiperligação https://www.kaggle.com/datasets/shahpranshu27/rossman-store-sales.

Wirth, R., & Hipp, J. (2000). CRISP-DM: Towards a standard process model for data mining. Proceedings of the 4th International Conference on the Practical Applications of Knowledge Discovery and Data Mining.

---
*Data de última atualização: [05/06/2026]*
