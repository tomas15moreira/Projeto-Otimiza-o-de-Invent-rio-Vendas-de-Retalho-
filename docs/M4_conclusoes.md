# Relatório de Conclusão e Entrega de Valor (Milestone 4)
## 1. Síntese de Resultados e Impacto

**O Problema Resolvido:** O objetivo definido na Milestone 1 era desenvolver um modelo capaz de prever as vendas diárias de cada loja da cadeia Rossmann, com um erro percentual médio (RMSPE) igual ou inferior a 20% e um coeficiente de determinação (R²) igual ou superior a 0,85. Ambos os critérios foram alcançados: o modelo final atingiu um RMSPE de 15,92% e um R² de 0,8677 no conjunto de teste. O problema proposto foi, portanto, resolvido.

| Métrica | Resultado | Interpretação simples |
| :--- | :---: | :--- |
| RMSPE | 15,92% | Em média, a previsão de vendas erra cerca de 16%, abaixo do limite de 20% definido. |
| MAE | 761,68 € | A previsão diária de cada loja afasta-se, em média, cerca de 762 euros do valor real. |
| R² | 0,8677 | O modelo explica cerca de 87% da variação das vendas diárias. |

**Interpretação dos Resultados:** Em palavras simples, o modelo consegue prever as vendas diárias de uma loja com um erro médio de aproximadamente 762 euros e explica cerca de 87% da variação das vendas. Isto significa que, para a grande maioria dos dias e das lojas, a previsão do modelo fica próxima do valor que realmente se verifica. O modelo aprendeu que fatores como o tipo de loja, a realização de promoções e a variedade de produtos são os que mais influenciam o volume de vendas.

A tabela seguinte mostra a evolução face ao modelo de base, uma simples regressão linear, até ao modelo final otimizado:

| Indicador | Modelo base (Regressão Linear) | Modelo final (XGBoost) |
| :--- | :---: | :---: |
| Erro percentual médio (RMSPE) | 41,42% | 15,92% |
| Erro médio em euros (MAE) | 1929,05 € | 761,68 € |
| Variação das vendas explicada (R²) | 0,2008 | 0,8677 |

O modelo final reduziu o erro médio das previsões de cerca de 1929 euros para 762 euros, uma melhoria de mais de 60%, tornando as previsões muito mais fiáveis. Em termos práticos, se uma loja vende em média 7000 euros por dia, a previsão do modelo erra tipicamente cerca de 762 euros, ou seja, indica um valor entre aproximadamente 6240 e 7760 euros. Para a maioria das decisões de reabastecimento, esta margem é suficientemente precisa para planear as encomendas com confiança.

**Valor para o Negócio:** Uma previsão fiável das vendas diárias permite à Rossmann planear melhor o reabastecimento de cada loja. Com uma margem de erro controlada, a empresa pode reduzir tanto as situações de rutura de stock, que afastam clientes, como o excesso de mercadoria, que imobiliza capital. Saber antecipadamente que uma loja vai ter um dia de vendas elevado permite reforçar o inventário e a equipa; prever um dia fraco evita encomendas desnecessárias. A previsão transforma-se, assim, numa ferramenta de apoio à decisão para a gestão de inventário.
## 2. Análise Crítica e Limitações
> **Nota:** Identificar de forma honesta as fronteiras do projeto e onde o modelo pode falhar.
* **Limitações dos Dados:** * (Ex: "O volume de dados para a classe X era reduzido, o que pode
afetar a precisão em cenários específicos.")
* **Limitações do Modelo:** * (Ex: "O modelo de associação foca-se em relações de co-ocorrência,
mas não prova causalidade direta entre as variáveis.")
* **Contextos de Falha:** * (Ex: "O modelo não é recomendado para situações de [Cenário Y], uma
vez que os dados de treino não contemplavam essa variável externa.")
## 3. Considerações Éticas e de Viés
* **Privacidade:** (Ex: "Todos os identificadores pessoais foram removidos, garantindo que o
modelo analisa apenas padrões de comportamento anónimos.")
* **Transparência:** (Ex: "Utilizámos técnicas de 'Feature Importance' para garantir que as
decisões do modelo são explicáveis e não operam como uma 'caixa negra'.")
## 4. Roadmap e Trabalhos Futuros
> **Nota:** Sugestões concretas para quem quiser continuar ou escalar este projeto.
1. **Melhoria Técnica:** (Ex: "Implementar técnicas de reamostragem (SMOTE) para lidar melhor
com o desequilíbrio das classes.")
2. **Novas Variáveis:** (Ex: "Integrar dados meteorológicos ou sazonais para refinar as previsões
de venda.")
3. **Escalabilidade (Deployment):** (Ex: "Desenvolver uma interface web (Streamlit) para que o
modelo possa ser utilizado por utilizadores não-técnicos em tempo real.")
---
**Data de Conclusão:** [Inserir Data]
**Versão do Projeto:** v4.0 Final--- 
