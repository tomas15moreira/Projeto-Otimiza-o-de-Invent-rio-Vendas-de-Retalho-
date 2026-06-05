# Aplicação de Modelos de Regressão na Previsão de Vendas e Gestão de Stocks da Rossmann
## Identificação da Equipa
* **Membros:**
 * Tomás Moreira - 2023143375

## Organização do Repositório
A estrutura deste projeto segue as boas práticas de Ciência de Dados e Engenharia de Software:
* **`data/`**: Armazenamento de dados (dados brutos em `raw/` e processados em `processed/`).
* **`docs/`**: Documentação técnica detalhada dividida por Milestones (M1, M2 e M3).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (Milestone 1)
### Contexto e Problema de Negócio
O setor do retalho exige um planeamento rigoroso para garantir a adequação do inventário à procura diária. A Rossmann, uma cadeia de retalho especializada com mais de mil estabelecimentos, enfrenta o desafio constante de antecipar o volume de transações em cada local. Uma estimativa incorreta pode resultar em ruturas nas prateleiras, o que afasta consumidores para a concorrência, ou na acumulação excessiva de mercadoria, o que imobiliza capital financeiro de forma ineficiente. A complexidade deste planeamento reside na elevada volatilidade do comportamento de compra, fortemente influenciado por campanhas promocionais, pela ocorrência de feriados e pelas dinâmicas de concorrência local. O propósito deste projeto consiste em conceber um sistema analítico capaz de interpretar estes padrões históricos e prever com rigor a faturação futura, capacitando os decisores para otimizar as rotinas de reabastecimento e maximizar a rentabilidade de toda a operação comercial.

### Objetivos do Projeto
**Objetivo SMART:** Desenvolver um modelo de regressão para estimar o volume de vendas diárias por loja da cadeia Rossmann, com base no histórico de vendas de janeiro de 2013 a julho de 2015, atingindo um erro percentual médio (RMSPE) igual ou inferior a 20% e um coeficiente de determinação (R²) igual ou superior a 0,85, até dia 14 de junho de 2026.

### Perguntas de Investigação
1. Quais são os fatores que mais influenciam o volume de vendas diárias das lojas Rossmann?
2. Existe relação entre a realização de promoções e o aumento das vendas?
3. De que forma os dias festivos e os fins de semana afetam o volume de vendas?
4. Quais são as variáveis que mais contribuem para a previsão das vendas no modelo final?
5. O tipo de loja e a variedade de produtos influenciam o comportamento das vendas?

### Fonte de Dados
| Característica | Detalhe Analítico |
| :--- | :--- |
| Dataset | Rossmann Store Sales |
| Fonte principal | Kaggle |
| Link | https://www.kaggle.com/datasets/shahpranshu27/rossman-store-sales |
| Dimensão | 1 017 209 observações no histórico de treino e 1 115 registos no perfil das lojas |
| Variável Alvo | Sales (Volume numérico e contínuo da faturação diária) |
| Descrição | O conjunto de dados documenta o histórico transacional diário e as características comerciais dos estabelecimentos da cadeia de retalho, suportando a criação do modelo preditivo para a otimização de inventário. |
## 2. Exploração (Milestone 2)
### Limpeza e Preparação
* [Breve resumo das ações de limpeza tomadas. Detalhes em `docs/M2_exploracao.md`]
### Principais Conclusões (EDA)
> *Dica: Insere aqui o gráfico mais importante do projeto.*
* **Ponto-chave:** [Ex: Identificámos que o fator X influencia em 40% o resultado Y, por aplicação
do método ganho de informação]
## 3. Modelação (Milestone 3)
### Abordagem Técnica
* **Modelos:** [Ex: Random Forest e XGBoost]
* **Métrica Principal:** [Ex: F1-Score ou RMSE]
## 4. Finalização (Milestone 4)
### Resposta ao Problema
[Resumo da solução e como ela gera valor para o negócio.]
### Recomendações de Inovação
1. [Sugestão prática baseada nos resultados]
## Como Reproduzir este Projeto
1. Clone o repositório: `git clone [url-do-repo]`
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na pasta `notebooks/` seguindo a ordem numérica.
**Instituição:** Coimbra Business School | ISCAC
**Curso:** Licenciatura em Ciência de Dados para a Gestão
**Unidade Curricular:** Projeto em Ciência de Dados
**Professor Responsável:** Dora Melo (dmelo@iscac.pt)
