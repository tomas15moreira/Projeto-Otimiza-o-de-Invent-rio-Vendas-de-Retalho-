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
O setor do retalho exige um planeamento rigoroso para garantir a adequação do inventário à procura diária. A Rossmann, uma cadeia de drogarias europeia com mais de mil estabelecimentos, enfrenta o desafio constante de antecipar o volume de transações em cada local. Uma estimativa incorreta resulta em ruturas nas prateleiras, o que afasta consumidores para a concorrência, ou na acumulação excessiva de mercadoria, o que imobiliza capital financeiro de forma ineficiente. A complexidade deste planeamento reside na elevada volatilidade do comportamento de compra, fortemente influenciado pela vigência de campanhas promocionais, pela ocorrência de feriados e pelas dinâmicas de concorrência local. O propósito deste projeto consiste em conceber um sistema analítico capaz de interpretar estes padrões históricos e prever com rigor a faturação futura, capacitando os decisores para otimizar as rotinas de reabastecimento e maximizar a rentabilidade de toda a operação comercial.
### Objetivos do Projeto
A meta central deste trabalho reside no desenvolvimento de um modelo de regressão vocacionado para a estimativa da faturação diária em cada estabelecimento da cadeia. O sucesso desta solução técnica será medido de forma objetiva, visando alcançar um erro percentual médio igual ou inferior a vinte por cento e um coeficiente de determinação de pelo menos oitenta e cinco por cento. Em paralelo, a análise procura isolar e quantificar o impacto prático de variáveis externas e operacionais, compreendendo de que forma a sazonalidade, as promoções em vigor e o distanciamento face à concorrência afetam os padrões de consumo e o volume transacionado.
### Fonte de Dados
* **Dataset:** [Link para a fonte ou descrição dos ficheiros]
* **Dimensão:** [Ex: 10.000 linhas, 15 colunas]
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
