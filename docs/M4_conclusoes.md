# Relatório de Conclusão e Entrega de Valor (Milestone 4)
## 1. Síntese de Resultados e Impacto

**O Problema Resolvido:** O objetivo definido na Milestone 1 era desenvolver um modelo capaz de prever as vendas diárias de cada loja da cadeia Rossmann, com um erro percentual médio (RMSPE) igual ou inferior a 20% e um coeficiente de determinação (R²) igual ou superior a 0,85. Ambos os critérios foram alcançados: o modelo final atingiu um RMSPE de 15,92% e um R² de 0,8677 no conjunto de teste. O problema proposto foi, portanto, resolvido.

| Métrica | Resultado | Interpretação simples |
| :--- | :---: | :--- |
| RMSPE | 15,92% | Em média, a previsão de vendas erra cerca de 16%, abaixo do limite de 20% definido. |
| MAE | 761,68 € | A previsão diária de cada loja afasta-se, em média, cerca de 762 euros do valor real. |
| R² | 0,8677 | O modelo explica cerca de 87% da variação das vendas diárias. |

**Interpretação dos Resultados:** O modelo consegue prever as vendas diárias de uma loja com um erro médio de aproximadamente 762 euros e explica cerca de 87% da variação das vendas. Isto significa que, para a grande maioria dos dias e das lojas, a previsão do modelo fica próxima do valor que realmente se verifica. O modelo aprendeu que fatores como o tipo de loja, a realização de promoções e a variedade de produtos são os que mais influenciam o volume de vendas.

A tabela seguinte mostra a evolução face ao modelo de base, uma simples regressão linear, até ao modelo final otimizado:

| Indicador | Modelo base (Regressão Linear) | Modelo final (XGBoost) |
| :--- | :---: | :---: |
| Erro percentual médio (RMSPE) | 41,42% | 15,92% |
| Erro médio em euros (MAE) | 1929,05 € | 761,68 € |
| Variação das vendas explicada (R²) | 0,2008 | 0,8677 |

O modelo final reduziu o erro médio das previsões de cerca de 1929 euros para 762 euros, uma melhoria de mais de 60%, tornando as previsões muito mais fiáveis. Em termos práticos, se uma loja vende em média 7000 euros por dia, a previsão do modelo erra tipicamente cerca de 762 euros, ou seja, indica um valor entre aproximadamente 6240 e 7760 euros. Para a maioria das decisões de reabastecimento, esta margem é adequada para planear as encomendas com confiança.

**Valor para o Negócio:** Uma previsão fiável das vendas diárias permite à Rossmann planear melhor o reabastecimento de cada loja. Com uma margem de erro controlada, a empresa pode reduzir tanto as situações de rutura de stock, que afastam clientes, como o excesso de mercadoria, que imobiliza capital. Saber antecipadamente que uma loja vai ter um dia de vendas elevado permite reforçar o inventário e a equipa; prever um dia fraco evita encomendas desnecessárias. A previsão transforma-se, assim, numa ferramenta de apoio à decisão para a gestão de inventário.
## 2. Análise Crítica e Limitações
**Limitações dos Dados:** O histórico disponível abrange de janeiro de 2013 a julho de 2015, um período de dois anos e meio. Além disso, o modelo não tem acesso a fatores externos que influenciam as vendas, como as condições meteorológicas ou eventos locais.

* A variável mais associada às vendas era o número de clientes diários, com uma correlação de 0,82 identificada na fase exploratória. Por só ser conhecida no final do dia, não pôde ser usada como preditora, o que representa uma limitação estrutural do modelo.
* O intervalo de dois anos e meio, embora suficiente para o objetivo deste projeto, não captura ciclos económicos mais longos nem tendências de mercado que se manifestam ao longo de vários anos, como mudanças graduais nos hábitos de consumo ou a entrada de novos concorrentes. Por ter sido treinado neste período, o modelo pode também não reagir bem a mudanças estruturais futuras que não estejam representadas no histórico, como uma crise económica.

**Limitações do Modelo:** O modelo, embora cumpra os objetivos, tem fronteiras que devem ser reconhecidas.

* O modelo apresenta um sobreajuste (*overfitting*) ligeiro, com um R² de 0,96 no treino e de 0,87 no teste. A diferença é moderada e o desempenho no teste mantém-se forte, mas indica que o modelo se ajusta um pouco melhor aos dados que já viu do que a dados novos.
* A análise de resíduos revelou um padrão em funil: o modelo é mais preciso nas lojas de vendas baixas e médias e perde precisão nas lojas de vendas muito elevadas, onde o erro absoluto tende a ser maior. As previsões para as lojas de maior volume devem, por isso, ser interpretadas com mais cautela.
* O modelo tende a subestimar muito ligeiramente as vendas, com um resíduo médio de cerca de 258 euros. Embora pequeno, este desvio sistemático significa que, na dúvida, o modelo erra mais por defeito do que por excesso.
* Sendo um modelo baseado em árvores de decisão, capta bem as relações entre as variáveis, mas não foi concebido para tirar partido explícito da sequência temporal das vendas. Padrões de tendência ao longo do tempo podem, assim, não ser totalmente aproveitados.

**Contextos de Falha:** Há situações específicas em que as previsões do modelo são menos fiáveis e devem ser usadas com mais cautela.

* O modelo tem maior dificuldade em prever as vendas nos dias de fim de semana, sobretudo ao domingo. A análise de resíduos confirmou-o: os dias de fim de semana representam cerca de 24% das piores previsões, contra 17% no conjunto geral. Isto deve-se ao comportamento irregular das vendas nesses dias e ao encerramento de muitas lojas ao domingo.
* O modelo não está preparado para prever vendas em situações que não constam no histórico de treino, como um tipo de promoção nunca antes realizado ou a abertura de lojas em mercados muito diferentes dos existentes. Nestes casos, o modelo extrapola para além do que aprendeu, e as previsões tornam-se pouco fiáveis.
* As previsões para os dias de feriado são também menos seguras, uma vez que estes dias são pouco frequentes no histórico e oferecem menos exemplos para o modelo aprender o seu padrão.


## 3. Considerações Éticas e de Viés
### **Privacidade** 
O conjunto de dados utilizado não contém qualquer informação pessoal de clientes. Os dados referem-se a vendas agregadas por loja e por dia, e não a transações individuais ou a dados identificáveis de pessoas. A variável que representava o número de clientes foi, removida da modelação, por só ser conhecida no final do dia. O modelo analisa, assim, apenas padrões de venda das lojas, sem implicações para a privacidade individual.

### **Transparência e Explicabilidade** 
Para tornar o modelo mais transparente e perceber o que está por trás das suas previsões, foi feita uma análise da importância das variáveis, que mostra quais os fatores que mais pesam nas previsões. Esta explicabilidade permite compreender a razão das decisões do modelo, em vez de aceitar números sem justificação. Saber, por exemplo, que o tipo de loja e as promoções são os fatores que mais influenciam as vendas torna o modelo transparente e ajuda os gestores a confiar nas suas previsões e a interpretá-las com sentido crítico.

### **Viés de Negócio**
Importa notar que um modelo treinado sobre o histórico tende a reproduzir os padrões do passado. Se a empresa basear todas as decisões de reabastecimento apenas nas previsões, corre o risco de reforçar os padrões existentes, dando menos atenção a lojas ou períodos com potencial de crescimento ainda não refletido nos dados. O modelo deve, por isso, apoiar a decisão humana, e não substituí-la por completo.

## 4. Roadmap e Trabalhos Futuros

### 4.1. Melhorias Técnicas Imediatas

1. Testar outros algoritmos de boosting, como o LightGBM ou o CatBoost, que costumam ser mais rápidos em grandes conjuntos de dados e poderiam captar padrões complementares aos do XGBoost.
2. Reduzir o ligeiro sobreajuste (*overfitting*) identificado, através de um ajuste mais fino dos hiperparâmetros de regularização ou do treino com uma maior quantidade de dados, uma vez que a curva de aprendizagem sugeriu que o modelo ainda beneficiaria de mais exemplos.
3. Aplicar valores SHAP (SHapley Additive exPlanations), uma técnica que explica cada previsão individualmente. Enquanto a análise atual mostra a importância geral das variáveis, o SHAP permitiria perceber o que pesou em cada previsão específica.

### 4.2. Expansão dos Dados e Novas Variáveis

1. Criar variáveis de histórico, como a média de vendas ou de clientes por loja e por dia da semana, calculadas apenas sobre dados passados para evitar fuga de informação. Como o número de clientes era a variável mais associada às vendas (correlação de 0,82) mas não pôde ser usado diretamente, criar estas médias seria uma forma de recuperar parte do valor preditivo dos clientes, sem usar dados do próprio dia.
2. Integrar variáveis externas relevantes, como dados meteorológicos, eventos locais ou os períodos de férias escolares, que ajudariam a explicar melhor as variações das vendas mas que o modelo atual não conhece.
3. Explorar modelos específicos de séries temporais, que tirem partido da ordem cronológica das vendas, algo que o modelo atual, baseado em árvores de decisão, não aproveita de forma direta.

### 4.3. Escalabilidade e Deployment

1. Desenvolver uma interface web interativa (com ferramentas como o *Streamlit* ou o *Dash*) que permita aos gestores das lojas inserir uma data e obter a previsão de vendas correspondente, sem necessidade de conhecimentos técnicos.
2. Implementar um processo de monitorização que detete automaticamente quando os dados mais recentes começam a afastar-se dos dados de treino, sinalizando a necessidade de treinar o modelo de novo com informação atualizada.
3. Integrar o modelo no sistema de gestão de inventário da empresa, de forma a que as previsões possam apoiar diretamente as decisões de reabastecimento de cada loja.

## 5. Reflexão Final

Este projeto percorreu o ciclo de Ciência de Dados, desde a definição do problema até à entrega de uma solução com utilidade prática. Mais do que o resultado final, foi o percurso que deu sentido ao trabalho: cada fase preparou a seguinte, e as decisões tomadas numa etapa só se compreendem à luz das anteriores.

A exploração inicial dos dados revelou que nenhuma variável, de forma isolada, explicava as vendas, o que orientou tanto a criação de novas variáveis como a escolha de modelos capazes de cruzar vários fatores. Essa escolha confirmou-se acertada na fase de modelação, em que os modelos baseados em árvores mostraram-se mais eficazes do que uma abordagem linear simples. E a interpretação final do modelo veio confirmar, com dados, aquilo que a exploração já tinha sugerido: que o perfil da loja e as promoções são os fatores decisivos nas vendas. Esta coerência entre as várias fases é um dos pontos mais sólidos do trabalho.

Ao longo do trabalho, procurou-se privilegiar decisões fundamentadas em vez de resultados impressionantes. A divisão temporal dos dados, mais exigente do que uma divisão aleatória, foi escolhida por refletir a realidade do problema. A escolha do modelo final recaiu sobre a configuração mais simples entre as equivalentes, em vez da que apresentava o número ligeiramente mais alto. E as limitações foram identificadas de forma aberta, em vez de escondidas. Estas opções refletem uma preocupação com o rigor e a honestidade que se considera mais valiosa do que qualquer métrica isolada.

O modelo desenvolvido não é uma solução fechada, mas um ponto de partida. Cumpre os objetivos a que se propôs e oferece valor real para a gestão de inventário, ao mesmo tempo que deixa em aberto caminhos claros de evolução. Cumpre o seu propósito, reconhece as suas limitações mas é uma base sólida e honesta sobre a qual abre caminho para evoluções futuras.

---
**Data de Conclusão:** 11 de Junho de 2026

**Versão do Projeto:** v4.0 Final

--- 
