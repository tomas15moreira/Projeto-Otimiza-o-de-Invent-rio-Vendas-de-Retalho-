# Milestone 2: Análise Exploratória e Engenharia de Atributos
## 1. Análise Exploratória de Dados (EDA)
### 1.1. Distribuição da Variável Alvo
**A Variável e o Problema:** A variável alvo de estudo é o volume de vendas diárias ("Sales"). Tratando-se da previsão de um montante financeiro numérico e contínuo, estamos perante um caso claro de regressão supervisionada. O objetivo não é adivinhar uma categoria, mas sim prever um valor exato de faturação para cada estabelecimento.

**Assimetria e Valores a Zero:** A análise revelou que esta variável está longe de seguir uma distribuição normal. Detetámos uma concentração de registos com o valor exato de zero, que refletem os dias em que as lojas estiveram de portas fechadas. Quando isolamos apenas os dias de funcionamento, a curva apresenta uma forte assimetria positiva, onde a grande maioria da faturação se concentra em valores padrão, existindo contudo picos de venda pontuais muito elevados que chegam a ultrapassar os 41 mil euros.

**A Distorção do Modelo:** O comportamento destes dados tem implicações diretas na forma como vamos preparar o modelo. Se os dias de loja fechada passarem para a nossa folha de cálculo de treino, a regressão vai interpretar esses zeros como um desempenho comercial péssimo, enviesando todas as previsões para baixo. A remoção destes registos inativos é, por isso, um passo obrigatório.

**Adoção de Métricas Robustas:** A enorme amplitude entre dias de vendas fracas e dias de pico exige cuidados na avaliação. Medir o desempenho apenas pelo erro em euros seria limitador, dado que um erro de mil euros tem um peso muito diferente numa loja pequena face a uma loja de grande volume. É por isso que a utilização do erro percentual médio (RMSPE) e do coeficiente de determinação (R²) se revela uma abordagem correta e justa para o nosso cenário.

**Necessidade de Estratégias Específicas:** A inclinação dos dados de faturação exige que os valores sejam ajustados antes da aplicação matemática. Para garantir que as fórmulas de regressão no Excel não sejam desestabilizadas pelos picos de vendas extremas, será essencial aplicar uma transformação logarítmica à variável alvo na próxima etapa, puxando a sua distribuição para um formato muito mais equilibrado e previsível.
### 1.2. Correlações Relevantes
*Quais as variáveis que têm maior relação com o problema? Incluam referências a gráficos que
geraram no Kaggle.*
* **Atributo A vs. Alvo:** (Ex: "Notámos que quanto maior a idade, menor a probabilidade de
cancelamento.")
* **Atributo B vs. Alvo:** (Ex: "O tipo de contrato mensal está fortemente ligado à saída de
clientes.")
## 2. Qualidade dos Dados e Limpeza
### 2.1. Tratamento de Dados em Falta (Missing Data)
* **Colunas afetadas:** [Lista de colunas]
* **Estratégia adotada:** (Ex: "Substituímos os nulos da coluna 'Salário' pela mediana para
evitar o impacto de outliers.")
### 2.2. Outliers e Inconsistências
*Descrevam se encontraram valores impossíveis (ex: idade = 200) e como os resolveram.*
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
