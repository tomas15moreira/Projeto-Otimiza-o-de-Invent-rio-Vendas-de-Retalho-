# Q&A - Preparação para Questões Técnicas

Este documento antecipa algumas questões técnicas sobre o modelo e apresenta as respetivas respostas, servindo de preparação para a defesa do projeto.

## 1. Porque é que a divisão entre treino e teste foi temporal e não aleatória?

Porque os dados são uma série temporal e o objetivo é prever vendas futuras. Uma divisão aleatória colocaria dias de 2015 no treino e dias de 2013 no teste, ou seja, o modelo estaria a usar o futuro para prever o passado, uma situação irrealista que causa fuga de informação (data leakage). Ao ordenar os dados cronologicamente e reservar os 20% mais recentes para teste, simula-se a situação real: o modelo aprende com o passado e é avaliado sobre o futuro.

Em termos de negócio, esta escolha é o que garante que os resultados são de confiança. Uma divisão aleatória daria métricas mais bonitas, mas enganadoras. A empresa pensaria ter um modelo melhor do que realmente tem, e as decisões de reabastecimento baseadas nele falhariam quando aplicadas a dados reais futuros. Preferimos uma avaliação mais exigente mas honesta, que reflete o desempenho que a empresa pode mesmo esperar.

## 2. Porque é que o número de clientes não foi usado, se era a variável mais correlacionada com as vendas?

O número de clientes tinha a correlação mais forte com as vendas (0,82), mas não pode ser usado como variável preditora por uma razão prática: só é conhecido no final do dia, ao mesmo tempo que as próprias vendas. Usá-lo seria fuga de informação, porque o modelo estaria a usar uma informação que, no momento real da previsão, ainda não existe.

O ponto de negócio é precisamente este: quando um gestor precisa de decidir a encomenda para amanhã, não sabe quantos clientes vão entrar amanhã. Um modelo que dependesse dessa variável teria resultados excelentes nos testes, mas seria completamente inútil no dia a dia da loja, porque pediria uma informação que ainda não existe no momento da decisão. Removê-la foi o que tornou o modelo realista e utilizável.

## 3. O modelo apresenta overfitting. Isso não é um problema?

O modelo apresenta um sobreajuste (overfitting) ligeiro, com um R² de 0,96 no treino e de 0,87 no teste. A diferença existe, mas é moderada e controlada. O que importa é que o desempenho no conjunto de teste, ou seja, em dados que o modelo nunca viu, cumpre os objetivos definidos. A curva de aprendizagem mostrou ainda que o modelo beneficiaria de mais dados, o que ajudaria a reduzir esta diferença.

Traduzindo para o impacto no negócio: o que interessa à Rossmann não é o desempenho no treino, é o desempenho em dias futuros que o modelo nunca viu, e esse mantém-se forte (87% da variação explicada, erro médio de 762 euros). Na prática, cada previsão errada traduz-se numa encomenda mal dimensionada, que leva a uma rutura de stock (vendas perdidas e clientes insatisfeitos) ou a excesso de mercadoria (capital imobilizado). Como o erro em dados novos é controlado, o risco destas duas situações mantém-se baixo, que é o que importa para a operação.

## 4. Porque é que escolheram o modelo com 500 árvores e não o de 900, se a segunda pesquisa testou valores maiores?

A segunda pesquisa, que explorou até 1200 árvores, não trouxe melhoria significativa. A diferença de R² entre as duas configurações foi de apenas 0,0017, um valor inferior ao desvio padrão entre as dobras da validação cruzada (0,0021). Isto significa que as duas configurações são estatisticamente equivalentes: a pequena diferença está dentro da margem de variação normal e não permite afirmar que uma é verdadeiramente melhor.

Perante este empate técnico, optou-se pela configuração mais simples, com 500 árvores. Além de evitar complexidade desnecessária, há uma vantagem prática: um modelo mais simples treina mais depressa e é mais barato e mais fácil de manter quando colocado em produção. Para a empresa, escolher 900 árvores para obter o mesmo resultado seria pagar mais por nada.
