# Milestone 2: Análise Exploratória e Engenharia de Atributos
## 1. Análise Exploratória de Dados (EDA)
### 1.1. Distribuição da Variável Alvo
**A Variável e o Problema:** A variável alvo de estudo é o volume de vendas diárias ("Sales"). Tratando-se da previsão de um montante financeiro numérico e contínuo, estamos perante um caso claro de regressão supervisionada. O objetivo não é adivinhar uma categoria, mas sim prever um valor exato de faturação para cada estabelecimento.

**Assimetria e Valores a Zero:** A análise revelou que esta variável está longe de seguir uma distribuição normal. Detetámos uma concentração de registos com o valor exato de zero, que refletem os dias em que as lojas estiveram de portas fechadas. Quando isolamos apenas os dias de funcionamento, a curva apresenta uma forte assimetria positiva, onde a grande maioria da faturação se concentra em valores padrão, existindo contudo picos de venda pontuais muito elevados que chegam a ultrapassar os 41 mil euros.

**A Distorção do Modelo:** O comportamento destes dados tem implicações diretas na forma como vamos preparar o modelo. Se os dias de loja fechada passarem para o nosso conjunto de dados de treino, a regressão vai interpretar esses zeros como um desempenho comercial péssimo, enviesando todas as previsões para baixo. A remoção destes registos inativos é, por isso, um passo obrigatório.

**Adoção de Métricas Robustas:** A enorme amplitude entre dias de vendas fracas e dias de pico exige cuidados na avaliação. Medir o desempenho apenas pelo erro em euros seria limitador, dado que um erro de mil euros tem um peso muito diferente numa loja pequena face a uma loja de grande volume. É por isso que a utilização do erro percentual médio (RMSPE) e do coeficiente de determinação (R²) se revela uma abordagem correta e justa para o nosso cenário.

**Necessidade de Estratégias Específicas:** A inclinação dos dados de faturação exige que os valores sejam ajustados antes da aplicação matemática. Para garantir que o modelo de regressão não seja desestabilizado pelos picos de vendas extremas, será essencial aplicar uma transformação logarítmica à variável alvo na próxima etapa, puxando a sua distribuição para um formato muito mais equilibrado e previsível.

### 1.2. Correlações Relevantes

A leitura da matriz de correlação (`reports/figures/matriz_correlacao.png`) permitiu identificar quais as variáveis com maior relação com a variável alvo, o volume de vendas diárias (*Sales*). Destacam-se os seguintes valores:

* *Customers* (0,82): correlação positiva muito forte.
* *Promo* (0,37): correlação positiva moderada.
* *DayOfWeek* (-0,18): correlação negativa fraca.
* *CompetitionDistance* (-0,04): correlação praticamente nula.

Estes números mostram que o número de clientes (*Customers*) é o fator mais associado à faturação. Os dias com promoção (*Promo*) também tendem a registar vendas mais altas. Por outro lado, o dia da semana (*DayOfWeek*) tem uma relação negativa fraca, o que faz sentido, já que as vendas tendem a descer à medida que nos aproximamos do fim de semana, com o domingo habitualmente fechado. A distância para a concorrência (*CompetitionDistance*) tem um valor muito baixo que, de forma isolada, praticamente não afeta as vendas.

### Análise Gráfica das Variáveis Mais Relevantes

A análise visual dos boxplots permitiu confirmar e aprofundar as relações sugeridas pela matriz de correlação.

* **Efeito das promoções:** O boxplot das vendas com e sem promoção (`reports/figures/vendas_promo_diasemana.png`) mostra uma diferença. Nos dias com promoção, a mediana das vendas é visivelmente superior à dos dias sem promoção. Isto confirma que as campanhas promocionais têm um efeito real e positivo no volume de vendas, sendo uma das variáveis mais úteis para a previsão.

* **Efeito do dia da semana:** O mesmo gráfico mostra que as vendas se mantêm relativamente estáveis ao longo dos dias úteis, destacando-se o domingo (dia 7) pela sua maior dispersão. Como a maioria das lojas encerra ao domingo, os poucos registos desse dia apresentam um comportamento mais irregular. Esta análise confirma a relação entre o dia da semana e as vendas.

* **Efeito do tipo de loja:** O boxplot das vendas por tipo de loja (`reports/figures/vendas_tipoloja_variedade.png`) revela um padrão interessante. O tipo de loja "b", apesar de ser o menos frequente no conjunto de dados, é o que apresenta vendas medianas mais elevadas, claramente acima dos restantes tipos. Os tipos "a", "c" e "d" apresentam distribuições de vendas semelhantes entre si.

* **Efeito da variedade de produtos:** De forma semelhante, a variedade "b" destaca-se com vendas medianas superiores às das restantes. Tal como no tipo de loja, a categoria menos comum é a que regista maior volume de vendas, o que sugere que estas lojas, embora raras, têm um perfil comercial distinto e mais forte.

**Síntese:** Estas análises mostram que o comportamento das vendas depende de vários fatores em simultâneo, como a presença de promoções, o dia da semana e o perfil da loja. Nenhuma destas variáveis explica as vendas sozinha, o que reforça a necessidade de um modelo capaz de cruzar todas estas dimensões em conjunto.
## 2. Qualidade dos Dados e Limpeza
### 2.1. Tratamento de Dados em Falta (Missing Data)
**O Ponto de Partida:** A nossa tabela principal com o histórico diário de vendas estava completa e sem qualquer falha nos registos. O desafio da limpeza concentrou-se apenas na tabela secundária que descreve o perfil de cada loja.

**Colunas Afetadas e Lógica:** Os valores nulos agrupavam-se nas variáveis de concorrência (`CompetitionDistance`, `CompetitionOpenSinceMonth`, `CompetitionOpenSinceYear`) e de campanhas promocionais contínuas (`Promo2SinceWeek`, `Promo2SinceYear`, `PromoInterval`). Estes espaços vazios não eram erros informáticos, mas sim a ausência real de um evento (como não ter concorrência por perto ou não aderir à campanha).

**Estratégia para as Promoções:** Como os valores estavam vazios porque as lojas não participam nestas campanhas, substituímos os nulos numéricos por zero e a variável de texto (*PromoInterval*) pela categoria "Nenhum". Assim, o modelo percebe que a campanha não se aplica àquela loja.

**Estratégia para a Concorrência:** Na distância para o concorrente (*CompetitionDistance*), apagar as linhas vazias significava perder lojas inteiras. Como apenas 0,26% dos valores estavam em falta, optámos por preencher esses vazios com a mediana da coluna. A mediana foi escolhida em vez da média por ser mais robusta aos valores extremos, uma vez que existem lojas com concorrentes a distâncias muito elevadas que enviesariam a média. Para as datas de abertura da concorrência, preenchemos os espaços com zero, indicando que a informação é desconhecida.

**Lojas Abertas sem Vendas:** Identificámos 54 dias em que as lojas estavam registadas como abertas mas com vendas iguais a zero. No comércio, é invulgar que uma loja aberta passe um dia inteiro sem qualquer venda, pelo que estes registos não refletem o funcionamento normal das lojas. Por se afastarem do comportamento esperado e poderem confundir o modelo, optámos por remover estas 54 linhas.

**Os Picos de Vendas (Outliers Reais):** Por outro lado, encontrámos valores extremos na coluna das vendas que chegam a ultrapassar os 41 mil euros diários. Embora a estatística os classifique como "outliers" por fugirem muito da média, percebemos que não eram erros. Tratam-se de dias reais de enorme consumo, impulsionados por campanhas fortes ou épocas festivas. Se simplesmente apagássemos estes picos para limpar os gráficos, estaríamos a retirar do histórico os dias mais rentáveis da empresa. Por isso, decidimos manter todos estes valores altos intatos, sabendo que o seu peso será equilibrado mais tarde através da transformação logarítmica que vamos aplicar antes do treino.

### 2.3. Verificação de Duplicados e Seleção de Atributos
**Registos Duplicados:** Antes de avançarmos para a escolha das colunas finais, fizemos um teste de segurança para garantir a integridade da base de dados. Confirmámos que não existia nenhuma linha duplicada no histórico, o que nos deu luz verde para moldar a informação final.

**A Ilusão dos Clientes (Fuga de Informação):** A decisão crítica na seleção de atributos foi a remoção da coluna dos visitantes (`Customers`). Embora seja a variável matemática com maior impacto nas vendas, na vida real nós não sabemos hoje quantas pessoas vão entrar na loja na próxima semana. Se deixássemos esta coluna na base de dados, o modelo iria fazer batota, aprendendo a prever as vendas com base numa informação que não estará disponível no momento de uso real.

**O Peso do Calendário:** Como o nosso objetivo é antecipar o futuro, as datas ditam as regras do consumo. A partir da coluna original da data, criámos novas variáveis independentes para o dia, o ano e a semana do ano. Esta transformação é importante para que o algoritmo consiga interpretar a sazonalidade do negócio, percebendo de forma clara as épocas de pico e as quebras habituais de faturação.

**Limpeza de Redundâncias:** Na reta final da preparação, removemos as colunas que apenas faziam volume e duplicavam informação. Por exemplo, a variável isolada do mês foi descartada porque esse dado já estava traduzido na coluna da semana do ano, evitando o processamento de dados repetidos. O resultado final é uma matriz limpa, leve e focada apenas nas características que têm um poder real e prático de previsão.

## 3. Engenharia de Atributos (Feature Engineering)
### 3.1. Transformações Realizadas
**Codificação das Variáveis Categóricas (Encoding):** Transformámos as variáveis categóricas de texto, como o tipo de loja (*StoreType*), a variedade de produtos (*Assortment*) e o tipo de feriado (*StateHoliday*), em formato numérico. Como os algoritmos não interpretam texto, aplicámos One-Hot Encoding, que cria uma coluna binária por cada categoria. Esta abordagem é adequada a variáveis sem ordem natural, pois não impõe qualquer hierarquia entre as categorias.

**Transformações previstas para a fase de modelação:** Duas transformações importantes foram planeadas, mas serão aplicadas apenas na fase de modelação, e não nesta fase. A primeira é a transformação logarítmica da variável alvo (*Sales*), que servirá para equilibrar a assimetria identificada na análise exploratória. A segunda é o escalonamento das variáveis numéricas. Optou-se por deixar estas transformações para a fase seguinte por dois motivos: o escalonamento deve ser ajustado apenas com os dados de treino, para evitar a fuga de informação do conjunto de teste, e o modelo principal previsto baseia-se em árvores de decisão, que não são sensíveis à escala das variáveis.
### 3.2. Criação de Novos Atributos
Com o objetivo de explorar a informação disponível para o algoritmo e de captar a lógica de negócio associada ao calendário, criámos novas variáveis derivadas das colunas originais do nosso conjunto de dados: as componentes da data e a variável *Promo2Ativa*.

**Componentes do Calendário (*Ano*, *SemanaDoAno*, *Dia*):**
A coluna original *Date* foi convertida para o formato de data e, a partir dela, criámos novas colunas independentes para o ano (*Ano*), a semana do ano (*SemanaDoAno*) e o dia do mês (*Dia*). Esta extração permite ao modelo avaliar os ciclos temporais e a sazonalidade do retalho, ajudando a identificar épocas em que as vendas sobem, como as semanas que antecedem o Natal, ou quando descem.

**A Variável *Promo2Ativa*:**
Foi também criada a variável *Promo2Ativa*, que cruza o mês de cada registo com o intervalo de meses de promoção contínua de cada loja (*PromoInterval*). Esta nova variável transforma uma informação textual pouco utilizável num valor binário (1 ou 0), indicando de forma direta se, naquele dia, a loja tinha ou não a promoção de longo prazo a decorrer.

### 3.2.1 Análise de Correlação com as Novas Variáveis

A análise da correlação das novas variáveis com a variável alvo (*Sales*) mostrou valores fracos no seu conjunto. A variável *FimDeSemana* foi a mais expressiva (-0,15), confirmando que as vendas tendem a ser inferiores ao fim de semana. As restantes, como a *SemanaDoAno* e a *Promo2Ativa*, apresentaram correlações próximas de zero.

Importa interpretar estes valores com cuidado. A correlação linear apenas mede relações de proporcionalidade direta e não capta relações não lineares nem interações entre variáveis. Uma variável como a *SemanaDoAno*, por exemplo, pode ter um efeito relevante nas vendas sem que esse efeito seja linear, já que certas alturas do ano ocorrem picos de procura. Os modelos baseados em árvores, que serão usados na modelação, conseguem aproveitar este tipo de relação, pelo que a baixa correlação linear não invalida a utilização destas variáveis.

### 3.2.2 Multicolinearidade

Para perceber se a criação das novas colunas gerou variáveis relacionadas entre si, analisámos a matriz de correlação entre todas as variáveis numéricas, identificando os pares com correlação mais elevada.

A análise revelou uma correlação muito forte, de 0,97, entre as variáveis *Mes* e *SemanaDoAno*. Este resultado era esperado, uma vez que ambas medem essencialmente a mesma informação: a posição do registo ao longo do ano. Manter as duas seria redundante e não acrescentaria valor ao modelo. Por esse motivo, optou-se por remover a variável *Mes* e manter a *SemanaDoAno*, por esta oferecer um maior nível de detalhe temporal (cinquenta e duas semanas, em vez de doze meses).

Foi ainda identificada uma correlação elevada, de 0,79, entre as variáveis *DayOfWeek* e *FimDeSemana*. Esta relação também é compreensível, já que a variável *FimDeSemana* é construída a partir do dia da semana. Neste caso, optou-se por manter ambas, uma vez que captam níveis de informação diferentes: o *DayOfWeek* distingue os sete dias individualmente, enquanto o *FimDeSemana* agrupa e faz a distinção entre dias úteis e fim de semana. Por estarem abaixo do limiar habitualmente considerado problemático, a sua manutenção não compromete a fase de modelação.

Concluída esta verificação, as restantes variáveis não apresentam relações que comprometam a estabilidade do conjunto de dados, ficando este preparado para a fase de treino do modelo preditivo.
## 4. Dicionário de Dados Final (Pós-Processamento)

A tabela seguinte apresenta as variáveis que constituem o conjunto de dados final, preparado para a fase de modelação. As variáveis numéricas mantêm-se na escala original, uma vez que o escalonamento será aplicado apenas na fase de modelação, junto da divisão entre treino e teste.

| Variável | Tipo Estatístico | Domínio | Definição Operacional | Papel Analítico |
| :--- | :--- | :--- | :--- | :--- |
| *Store* | Numérica discreta | 1 a 1115 | Identificador único de cada loja | Identificador |
| *DayOfWeek* | Numérica discreta | 1 a 7 | Dia da semana do registo | Temporal |
| *Sales* | Numérica contínua | 0 a 41551 | Volume de vendas diário | Variável alvo |
| *Promo* | Binária | {0, 1} | Indica se havia promoção nesse dia | Comercial |
| *SchoolHoliday* | Binária | {0, 1} | Indica feriado escolar | Calendário |
| *CompetitionDistance* | Numérica contínua | Escala original (metros) | Distância ao concorrente mais próximo | Concorrência |
| *CompetitionOpenSinceMonth* | Numérica discreta | 0 a 12 | Mês de abertura do concorrente (0 se desconhecido) | Concorrência |
| *CompetitionOpenSinceYear* | Numérica discreta | 0 ou ano | Ano de abertura do concorrente (0 se desconhecido) | Concorrência |
| *Promo2* | Binária | {0, 1} | Adesão da loja à promoção contínua | Comercial |
| *Promo2SinceWeek* | Numérica discreta | 0 a 52 | Semana de início da Promo2 (0 se não aderiu) | Comercial |
| *Promo2SinceYear* | Numérica discreta | 0 ou ano | Ano de início da Promo2 (0 se não aderiu) | Comercial |
| *Ano* | Numérica discreta | 2013 a 2015 | Ano do registo | Variável derivada (Feature Engineering) |
| *Dia* | Numérica discreta | 1 a 31 | Dia do mês do registo | Variável derivada (Feature Engineering) |
| *SemanaDoAno* | Numérica discreta | 1 a 52 | Semana do ano do registo | Variável derivada (Feature Engineering) |
| *FimDeSemana* | Binária | {0, 1} | 1 se sábado ou domingo | Variável derivada (Feature Engineering) |
| *Promo2Ativa* | Binária | {0, 1} | 1 se a loja está num mês de promoção contínua | Variável derivada (Feature Engineering) |
| *StoreType_b, _c, _d* | Binária | {0, 1} | Tipo de loja, após One-Hot Encoding | Característica da loja |
| *Assortment_b, _c* | Binária | {0, 1} | Variedade de produtos, após One-Hot Encoding | Característica da loja |
| *StateHoliday_a, _b, _c* | Binária | {0, 1} | Tipo de feriado, após One-Hot Encoding | Calendário |
## 5. Conclusões da Fase de Exploração

### O que se aprendeu nesta fase

No final do Milestone 1 conhecia-se a estrutura geral dos dados, mas não o seu comportamento. A análise exploratória permitiu perceber vários aspetos que não eram visíveis na fase inicial.

Aprendeu-se que a variável alvo tem uma forte assimetria positiva, com a maioria dos dias a registar vendas moderadas e um conjunto menor de dias com vendas muito elevadas. Esta constatação levou à decisão de aplicar uma transformação logarítmica na fase de modelação.

Percebeu-se também que o número de clientes é o fator mais associado às vendas, mas que não pode ser utilizado na previsão por não estar disponível antecipadamente, o que obrigou à sua remoção. Entre as variáveis utilizáveis, a promoção e o dia da semana revelaram-se as mais relevantes, confirmando que as vendas sobem em dias de promoção e descem ao fim de semana.

Quanto à qualidade dos dados, confirmou-se que os valores em falta se concentravam nas características das lojas e correspondiam a ausências de evento, e não a erros, o que orientou a estratégia de imputação. Identificaram-se ainda cinquenta e quatro registos invulgares, de lojas abertas que não venderam nada, os quais foram removidos.

Por fim, a análise das correlações mostrou que as variáveis originais têm, uma relação fraca com as vendas. Foi por isso que se criaram novas variáveis, de calendário e de combinação de atributos, para ajudar o modelo a encontrar padrões que as variáveis originais não mostram sozinhas.

### Os dados são suficientes para avançar para a modelação?

Sim. Após a limpeza, a imputação dos valores em falta, a criação de novas variáveis e a codificação das variáveis categóricas, o conjunto de dados encontra-se coerente, sem valores em falta e com as variáveis num formato adequado aos algoritmos. O volume de dados, com mais de um milhão de registos, é suficiente para treinar e avaliar os modelos com segurança.

Importa, ainda assim, registar uma limitação que deverá ser tida em conta. Os dias de feriado são pouco frequentes no conjunto de dados, pelo que o modelo terá menos informação para aprender o comportamento das vendas nessas alturas, o que poderá afetar a fiabilidade das previsões nesses períodos específicos. Apesar desta limitação, os dados estão prontos para avançar para a modelação.

## 6. Referências Bibliográficas

McKinney, W. (2017). *Python for Data Analysis: Data Wrangling with Pandas, NumPy, and IPython* (2.ª ed.). O'Reilly Media.

Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., et al. (2011). Scikit-learn: Machine Learning in Python. *Journal of Machine Learning Research*, 12, 2825-2830.

Tukey, J. W. (1977). *Exploratory Data Analysis*. Addison-Wesley.

Rossmann Store Sales. (2015). Conjunto de dados alojado na plataforma Kaggle. Recuperado a partir de https://www.kaggle.com/datasets/shahpranshu27/rossman-store-sales

---
*Data de última atualização: [07/06/2026]*
