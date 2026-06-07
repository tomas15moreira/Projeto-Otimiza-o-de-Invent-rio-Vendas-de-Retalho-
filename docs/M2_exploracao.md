# Milestone 2: Análise Exploratória e Engenharia de Atributos
## 1. Análise Exploratória de Dados (EDA)
### 1.1. Distribuição da Variável Alvo
**A Variável e o Problema:** A variável alvo de estudo é o volume de vendas diárias ("Sales"). Tratando-se da previsão de um montante financeiro numérico e contínuo, estamos perante um caso claro de regressão supervisionada. O objetivo não é adivinhar uma categoria, mas sim prever um valor exato de faturação para cada estabelecimento.

**Assimetria e Valores a Zero:** A análise revelou que esta variável está longe de seguir uma distribuição normal. Detetámos uma concentração de registos com o valor exato de zero, que refletem os dias em que as lojas estiveram de portas fechadas. Quando isolamos apenas os dias de funcionamento, a curva apresenta uma forte assimetria positiva, onde a grande maioria da faturação se concentra em valores padrão, existindo contudo picos de venda pontuais muito elevados que chegam a ultrapassar os 41 mil euros.

**A Distorção do Modelo:** O comportamento destes dados tem implicações diretas na forma como vamos preparar o modelo. Se os dias de loja fechada passarem para a nossa folha de cálculo de treino, a regressão vai interpretar esses zeros como um desempenho comercial péssimo, enviesando todas as previsões para baixo. A remoção destes registos inativos é, por isso, um passo obrigatório.

**Adoção de Métricas Robustas:** A enorme amplitude entre dias de vendas fracas e dias de pico exige cuidados na avaliação. Medir o desempenho apenas pelo erro em euros seria limitador, dado que um erro de mil euros tem um peso muito diferente numa loja pequena face a uma loja de grande volume. É por isso que a utilização do erro percentual médio (RMSPE) e do coeficiente de determinação (R²) se revela uma abordagem correta e justa para o nosso cenário.

**Necessidade de Estratégias Específicas:** A inclinação dos dados de faturação exige que os valores sejam ajustados antes da aplicação matemática. Para garantir que o modelo de regressão não seja desestabilizado pelos picos de vendas extremas, será essencial aplicar uma transformação logarítmica à variável alvo na próxima etapa, puxando a sua distribuição para um formato muito mais equilibrado e previsível.

### 1.2. Correlações Relevantes
A leitura da matriz de correlação (`reports/figures/matriz_correlacao.png`) permitiu identificar matematicamente quais as variáveis com maior impacto na nossa variável alvo, o volume de vendas diárias (`Sales`). Destacam-se os seguintes valores:

* ***Customers* (0.89):** Correlação positiva muito forte.
* ***Open* (0.68):** Correlação positiva forte.
* ***Promo* (0.45):** Correlação positiva moderada.
* ***DayOfWeek* (-0.46):** Correlação negativa moderada.
* ***CompetitionDistance* (-0.019):** Correlação negativa muito fraca.

Estes números mostram que o número de clientes (`Customers`) é o grande motor da faturação. O facto de a loja estar aberta (`Open`) e ter descontos ativos (`Promo`) também empurra as vendas para cima. Por outro lado, o dia da semana (`DayOfWeek`) tem um impacto negativo moderado, o que faz sentido, já que as vendas tendem a descer à medida que nos aproximamos do fim de semana, com o domingo habitualmente fechado. A distância para a concorrência (`CompetitionDistance`) tem um valor tão baixo que, de forma isolada, praticamente não afeta as vendas.

### Análise Gráfica das Variáveis Mais Relevantes

Para além dos números da matriz, a visualização dos dados ajudou a perceber como estas relações funcionam na prática no dia a dia das lojas:

* ***Customers vs. Sales:*** Os gráficos de dispersão formam uma linha ascendente quase perfeita, provando que mais visitantes garantem mais dinheiro em caixa. Contudo, como não sabemos antecipadamente quantas pessoas vão visitar a loja na próxima semana, esta variável serve apenas para compreender o passado, não podendo entrar nas fórmulas de previsão do modelo final.
* ***Promo vs. Sales:*** A análise gráfica mostra diferenças evidentes. Os dias com campanhas ativas apresentam barras de faturação visivelmente mais altas. Isto comprova que o consumidor da Rossmann reage muito bem aos descontos, sendo a promoção a principal alavanca para prever grandes picos de vendas.
* ***Open vs. Sales:*** Os gráficos separam de forma cirúrgica duas realidades. Nos 17% dos dias em que as lojas fecham, a linha de vendas fixa no zero. Esta evidência visual reforça que temos de remover estes dias inativos antes de passarmos a matriz para a nossa folha de cálculo, evitando que as regressões fiquem distorcidas.
* ***CompetitionDistance vs. Sales:*** Neste cruzamento, os gráficos mostram uma grande dispersão de pontos. Há concorrência na mesma rua a faturar tanto como lojas completamente isoladas. Esta falta de padrão visual confirma o valor fraco da matriz e prova que uma boa localização ou o tipo de produtos importam muito mais do que ter concorrentes por perto.

**Conclusão da Análise:** Os números e os gráficos provam que não podemos olhar para as características de forma isolada. Há uma grande sobreposição nos comportamentos de consumo. Para atingirmos a margem de erro rigorosa que traçámos, temos de levar todas estas variáveis limpas para a folha de cálculo e aplicar uma abordagem multivariável. Só cruzando o calendário, as promoções e o perfil de cada loja em simultâneo através de um modelo multivariável é que conseguiremos prever a verdadeira complexidade das vendas.

## 2. Qualidade dos Dados e Limpeza
### 2.1. Tratamento de Dados em Falta (Missing Data)
**O Ponto de Partida:** A nossa tabela principal com o histórico diário de vendas estava completa e sem qualquer falha nos registos. O desafio da limpeza concentrou-se apenas na tabela secundária que descreve o perfil de cada loja.

**Colunas Afetadas e Lógica:** Os valores nulos agrupavam-se nas variáveis de concorrência (`CompetitionDistance`, `CompetitionOpenSinceMonth`, `CompetitionOpenSinceYear`) e de campanhas promocionais contínuas (`Promo2SinceWeek`, `Promo2SinceYear`, `PromoInterval`). Estes espaços vazios não eram erros informáticos, mas sim a ausência real de um evento (como não ter concorrência por perto ou não aderir à campanha).

**Estratégia para as Promoções:** Como os valores estavam vazios simplesmente porque as lojas não participam nestas campanhas, substituímos os nulos numéricos por zero e a variável de texto por "N/A". Assim, o modelo percebe de forma matemática que a campanha não se aplica àquele espaço.

**Estratégia para a Concorrência:** Na distância para o concorrente (`CompetitionDistance`), apagar as linhas vazias significava perder o histórico de lojas inteiras. A opção foi substituir estes vazios por um valor numérico muito acima da maior distância já registada, indicando ao modelo que a loja rival está tão longe que não tem impacto nas vendas. Para as datas de abertura da concorrência, seguimos a mesma lógica e preenchemos os espaços com zero.
### 2.2. Outliers e Inconsistências

**Lojas Abertas sem Faturação:** A maior anomalia que detetámos no histórico foi a existência de 54 dias em que as lojas estavam dadas como abertas, mas o valor das vendas era exatamente zero. No mundo real do comércio, é altamente improvável que um espaço com as portas abertas passe um dia inteiro sem fazer uma única transação. Concluímos que isto se tratou de um erro no sistema de registo das caixas ou de uma falha de comunicação. Como estes dados eram ilógicos e iriam confundir o modelo, a nossa decisão foi apagar estas 54 linhas da base de dados.

**Os Picos de Vendas (Outliers Reais):** Por outro lado, encontrámos valores extremos na coluna das vendas que chegam a ultrapassar os 41 mil euros diários. Embora a estatística os classifique como "outliers" por fugirem muito da média, percebemos que não eram erros. Tratam-se de dias reais de enorme consumo, impulsionados por campanhas fortes ou épocas festivas. Se simplesmente apagássemos estes picos para limpar os gráficos, estaríamos a retirar do histórico os dias mais rentáveis da empresa. Por isso, decidimos manter todos estes valores altos intatos, sabendo que o seu peso será equilibrado mais tarde através da transformação logarítmica que vamos aplicar antes do treino.

### 2.3. Verificação de Duplicados e Seleção de Atributos
**Registos Duplicados:** Antes de avançarmos para a escolha das colunas finais, fizemos um teste de segurança para garantir a integridade da base de dados. Confirmámos que não existia nenhuma linha duplicada no histórico, o que nos deu luz verde para moldar a informação final.

**A Ilusão dos Clientes (Fuga de Informação):** A decisão crítica na seleção de atributos foi a remoção da coluna dos visitantes (`Customers`). Embora seja a variável matemática com maior impacto nas vendas, na vida real nós não sabemos hoje quantas pessoas vão entrar na loja na próxima semana. Se deixássemos esta coluna na base de dados, o modelo iria fazer batota, aprendendo a prever as vendas com base numa informação que não estará disponível no momento de uso real.

**O Peso do Calendário:** Como o nosso objetivo é antecipar o futuro, as datas ditam as regras do consumo. A partir da coluna original da data, criámos novas variáveis independentes para o dia, o ano e a semana do ano. Esta transformação é importante para que o algoritmo consiga interpretar a sazonalidade do negócio, percebendo de forma clara as épocas de pico e as quebras habituais de faturação.

**Limpeza de Redundâncias:** Na reta final da preparação, removemos as colunas que apenas faziam volume e duplicavam informação. Por exemplo, a variável isolada do mês foi descartada porque esse dado já estava traduzido na coluna da semana do ano, evitando o processamento de dados repetidos. O resultado final é uma matriz limpa, leve e focada apenas nas características que têm um poder real e prático de previsão.

## 3. Engenharia de Atributos (Feature Engineering)
### 3.1. Transformações Realizadas
**Encoding:** Transformámos as variáveis categóricas de texto, como o tipo de estabelecimento (`StoreType`) e a variedade de produtos (`Assortment`), em formato numérico. Uma vez que as fórmulas matemáticas não conseguem interpretar letras, mapeámos as categorias originais (a, b, c, d) para números. Esta conversão foi um passo para garantir que o modelo consigua processar o perfil exato de cada loja sem descartar a informação original.

**Escalonamento e Transformações Matemáticas:** O ajuste que preparámos incidiu sobre a variável alvo, as vendas (`Sales`). Devido à enorme diferença entre os dias de vendas fracas e os picos de faturação extrema, aplicámos uma transformação logarítmica. Esta operação comprime a escala dos dados, puxando a distribuição para um formato muito mais equilibrado e evitando que o modelo fique desorientado pelos dias atípicos. Para variáveis contínuas com valores absolutos muito altos, como a distância para a concorrência (`CompetitionDistance`), o escalonamento coloca todas as métricas na mesma ordem de grandeza. Isto garante que o algoritmo não atribua um peso desproporcional a uma característica apenas por ter números naturalmente maiores.
### 3.2. Criação de Novos Atributos
Com o objetivo de explorar a informação disponível para o algoritmo e de captar a lógica de negócio associada ao calendário, criámos novas variáveis derivadas das colunas originais do nosso conjunto de dados: as componentes da data e a variável *Promo2Ativa*.

**Componentes do Calendário (*Year*, *WeekOfYear*, *Day*):**
A coluna original *Date* estava num formato de texto (Ano-Mês-Dia) que o modelo não consegue interpretar matematicamente. Para resolver isto, desconstruímos a data e criámos novas colunas independentes para o ano (*Year*), a semana do ano (*WeekOfYear*) e o dia do mês (*Day*).
Esta extração permite ao modelo avaliar ciclos temporais e a sazonalidade do retalho, ajudando a identificar situações em que as vendas disparam, como o início do mês (quando os clientes recebem o salário) ou as semanas que antecedem o Natal.

**A Variável *Promo2Ativa*:**
Foi também criada a variável *Promo2Ativa*, que cruza a data de cada venda com as colunas do histórico de campanhas contínuas (*Promo2SinceWeek* e *Promo2SinceYear*).
Esta nova variável transforma a complexidade do calendário num simples valor binário (1 ou 0). Assim, o modelo consegue identificar de forma imediata e direta se, naquele dia específico, a loja tinha ou não a promoção de longo prazo a decorrer, sem ter de fazer cruzamentos de datas em tempo real.

### 3.2.1 Análise de Correlação com as Novas Variáveis

A análise da matriz de correlação atualizada permitiu validar o impacto destes novos atributos no comportamento da variável alvo, as vendas diárias (*Sales*).

Observa-se que a variável *WeekOfYear* consegue capturar os ciclos de faturação de forma muito mais eficaz do que a coluna de data inteira, mostrando picos de correlação nas épocas festivas.
A variável derivada *Promo2Ativa* apresenta também uma correlação relevante com as *Sales*, embora o seu comportamento seja distinto da campanha diária regular (*Promo*). De forma geral, a introdução destas variáveis trouxe uma nova camada ao projeto, ligando o tempo e as campanhas ao volume de negócios de forma muito mais clara.

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

---
*Data de última atualização: [07/06/2026]*
