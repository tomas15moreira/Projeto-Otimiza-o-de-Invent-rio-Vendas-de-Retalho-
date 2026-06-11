# Q&A - Preparação para Questões Técnicas

Este documento antecipa algumas questões técnicas sobre o modelo e apresenta as respetivas respostas, servindo de preparação para a defesa do projeto.

## 1. Porque é que a divisão entre treino e teste foi temporal e não aleatória?

Porque os dados são uma série temporal e o objetivo é prever vendas futuras. Uma divisão aleatória colocaria dias de 2015 no treino e dias de 2013 no teste, ou seja, o modelo estaria a usar o futuro para prever o passado, uma situação irrealista que causa fuga de informação (data leakage). Ao ordenar os dados cronologicamente e reservar os 20% mais recentes para teste, simula-se a situação real: o modelo aprende com o passado e é avaliado sobre o futuro. Esta escolha torna a avaliação mais honesta, embora também mais exigente, pois prever o futuro é mais difícil do que preencher lacunas no meio dos dados.

## 2. Porque é que o número de clientes não foi usado, se era a variável mais correlacionada com as vendas?

O número de clientes tinha de facto a correlação mais forte com as vendas (0,82), mas não pode ser usado como variável preditora por uma razão prática: só é conhecido no final do dia, ao mesmo tempo que as próprias vendas. Usá-lo seria fuga de informação, porque o modelo estaria a usar uma informação que, no momento real da previsão, ainda não existe. Quando se quer prever as vendas de amanhã, não se sabe quantos clientes vão entrar amanhã. Por isso a variável foi removida, e a sua ausência é assumida como uma limitação estrutural do modelo.

## 3. O modelo apresenta overfitting. Isso não é um problema?

O modelo apresenta um sobreajuste (overfitting) ligeiro, com um R² de 0,96 no treino e de 0,87 no teste. A diferença existe, mas é moderada e controlada. O que importa é que o desempenho no conjunto de teste, ou seja, em dados que o modelo nunca viu, cumpre os objetivos definidos (R² acima de 0,85 e erro percentual abaixo de 20%). Um overfitting ligeiro é comum em modelos baseados em árvores e não compromete a utilidade do modelo, desde que o desempenho em dados novos se mantenha forte, como é o caso. A curva de aprendizagem mostrou ainda que o modelo beneficiaria de mais dados, o que ajudaria a reduzir esta diferença.

## 4. Porque é que escolheram o modelo com 500 árvores e não o de 900, se a segunda pesquisa testou valores maiores?

A segunda pesquisa, que explorou até 1200 árvores, não trouxe melhoria significativa. A diferença de R² entre as duas configurações foi de apenas 0,0017, um valor inferior ao desvio padrão entre as dobras da validação cruzada (0,0021). Isto significa que as duas configurações são estatisticamente equivalentes, ou seja, a pequena diferença observada está dentro da margem de variação normal e não permite afirmar que uma é verdadeiramente melhor. Perante este empate técnico, optou-se pela configuração mais simples, com 500 árvores, por ser mais rápida a treinar e menos propensa a sobreajuste, sem qualquer perda de desempenho.
