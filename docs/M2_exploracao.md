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

* **Customers (0.89):** Correlação positiva muito forte.
* **Open (0.68):** Correlação positiva forte.
* **Promo (0.45):** Correlação positiva moderada.
* **DayOfWeek (-0.46):** Correlação negativa moderada.
* **CompetitionDistance (-0.019):** Correlação negativa muito fraca.

Estes números mostram que o número de clientes (`Customers`) é o grande motor da faturação. O facto de a loja estar aberta (`Open`) e ter descontos ativos (`Promo`) também empurra as vendas para cima. Por outro lado, o dia da semana (`DayOfWeek`) tem um impacto negativo moderado, o que faz sentido, já que as vendas tendem a descer à medida que nos aproximamos do fim de semana, com o domingo habitualmente fechado. A distância para a concorrência (`CompetitionDistance`) tem um valor tão baixo que, de forma isolada, praticamente não afeta as vendas.

### Análise Gráfica das Variáveis Mais Relevantes

Para além dos números da matriz, a visualização dos dados ajudou a perceber como estas relações funcionam na prática no dia a dia das lojas:

* **Customers vs. Sales:** Os gráficos de dispersão formam uma linha ascendente quase perfeita, provando que mais visitantes garantem mais dinheiro em caixa. Contudo, como não sabemos antecipadamente quantas pessoas vão visitar a loja na próxima semana, esta variável serve apenas para compreender o passado, não podendo entrar nas fórmulas de previsão do modelo final.
* **Promo vs. Sales:** A análise gráfica mostra diferenças evidentes. Os dias com campanhas ativas apresentam barras de faturação visivelmente mais altas. Isto comprova que o consumidor da Rossmann reage muito bem aos descontos, sendo a promoção a principal alavanca para prever grandes picos de vendas.
* **Open vs. Sales:** Os gráficos separam de forma cirúrgica duas realidades. Nos 17% dos dias em que as lojas fecham, a linha de vendas fixa no zero. Esta evidência visual reforça que temos de remover estes dias inativos antes de passarmos a matriz para a nossa folha de cálculo, evitando que as regressões fiquem distorcidas.
* **CompetitionDistance vs. Sales:** Neste cruzamento, os gráficos mostram uma grande dispersão de pontos. Há concorrência na mesma rua a faturar tanto como lojas completamente isoladas. Esta falta de padrão visual confirma o valor fraco da matriz e prova que uma boa localização ou o tipo de produtos importam muito mais do que ter concorrentes por perto.

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

**A Ilusão dos Clientes (Fuga de Informação):** A decisão mais crítica na seleção de atributos foi a remoção propositada da coluna dos visitantes (`Customers`). Embora seja a variável matemática com maior impacto nas vendas, na vida real nós não sabemos hoje quantas pessoas vão entrar na loja na próxima semana. Se deixássemos esta coluna na base de dados, o modelo iria fazer batota, aprendendo a prever as vendas com base numa informação que não estará disponível no momento de uso real.

**O Peso do Calendário:** Como o nosso objetivo é antecipar o futuro, as datas ditam as regras do consumo. A partir da coluna original da data, criámos novas variáveis independentes para o dia, o ano e a semana do ano. Esta transformação é vital para que o algoritmo consiga interpretar a sazonalidade do negócio, percebendo de forma clara as épocas de pico e as quebras habituais de faturação.

**Limpeza de Redundâncias:** Na reta final da preparação, removemos as colunas que apenas faziam volume e duplicavam informação. Por exemplo, a variável isolada do mês foi descartada porque esse dado já estava perfeitamente traduzido na coluna da semana do ano, evitando o processamento de dados repetidos. O resultado final é uma matriz limpa, leve e focada apenas nas características que têm um poder real e prático de previsão.

## 3. Engenharia de Atributos (Feature Engineering)
### 3.1. Transformações Realizadas
* **Encoding:** (Ex: "Convertemos a variável 'Género' em numérica usando One-Hot Encoding.")
* **Escalonamento:** (Ex: "Aplicámos o StandardScaler nas variáveis numéricas para que todas
fiquem na mesma escala.")
### 3.2. Criação de Novos Atributos
*Descrevam as variáveis que criaram para ajudar o modelo.*
* **Nova Variável [Nome]:** (Ex: "Criámos a 'Tenure_Per_Year' que divide o tempo de contrato
pela idade do cliente.")
## 4. Dicionário de Dados Final (Pós-Processamento)
*Listagem final das variáveis que serão entregues ao modelo na Fase 3.*
| Atributo | Tipo | Descrição |
| :--- | :--- | :--- |
| `cliente_id` | ID | Removido (não preditivo) |
| `idade_norm` | Float | Idade após normalização |
| `is_premium` | Binary | 1 para clientes com plano superior |
## 5. Conclusões da Fase de Exploração
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes
para avançar para a modelação?*
---
*Data de última atualização: [DD/MM/AAAA]*
