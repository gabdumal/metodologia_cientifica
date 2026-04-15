# A general reinforcement learning algorithm that masters chess, shogi, and Go through self-play

> David Silver, Thomas Hubert, Julian Schrittwieser, Ioannis Antonoglou, Matthew Lai, Arthur Guez, Marc Lanctot, Laurent Sifre, Dharshan Kumaran, Thore Graepel, Timothy Lillicrap, Karen Simonyan, Demis Hassabis. A general reinforcement learning algorithm that masters chess, shogi, and Go through self-play. Science362,1140-1144(2018). <https://doi.org/10.1126/science.aar6404>

## Porque este artigo foi escolhido? Qual a relação com o seu tema de pesquisa?

## Faça um resumo do conteúdo metodológico/tecnológico do Artigo Revolucionário

- O artigo apresenta os jogos Xadrex e Shogi como desafios para a inteligência computacional, similarmente ao jogo de Go.
  - O grupo de pesquisa que lançou este trabalho havia previamente desenvolvido uma técnica de desenvolvimento de agentes inteligentes que jogavam Go a nível profissional.
  - Os autores teorizam que é possível criar uma técnica evolucionária que crie agentes inteligentes capazes de jogar Xadrez e Shogi a nível profissional, sem a necessidade de conhecimento prévio específico sobre os jogos.

- Os autores reconhecem como trabalhos relacionados programas específicos para jogar Xadrez e Shogi, mas destacam que esses programas dependem da incorporação de conhecimento e regras específicas para o contexto de cada jogo.
  - Eles citam como técnicas prévias do estado-da-arte o uso de árvores de busca alfa-beta e heurísticas específicas para cada jogo.
  - Eles destacam algoritmos tidos como o estado-da-arte: Stockfish e DeepBlue para Xadrez, e Elmo para Shogi.

- Um interesse aprestado é o de se chegar a uma técnica de desenvolvimento de agentes inteligentes que seja generalista, e permita representar e simular qualquer jogo de tabuleiro.

- Nesse sentido, os autores apresentam o algoritmo AlphaZero, que deve ser capaz de aprender a jogar qualquer jogo de tabuleiro por aprendizado por reforço, dado apenas o conjunto de regras do jogo.

- De forma similar ao AlphaGo, o AlphaZero usa uma rede neural profunda que toma como entrada um estado do jogo e produz como saída: um vetor de probabilidades para cada movimento possível, e um escalar que estima a qualidade do resultado final da partida.

- A seleção de jogadas é feita por meio do método de busca em árvore de Monte Carlo (MCTS), de forma adaptada.
  - O ciclo de busca seleciona um nó da árvore segundo uma diretriz que alinha exploração e aproveitamento, chamada UCT.
    - Para o AlphaZero, essa diretriz também leva em conta as probabilidades estimadas para cada movimento como gerado pela rede neural.
  - Então, todas as jogadas válidas a partir desse estado são geradas na árvore.
  - A rede neural também é usada para avaliar o valor dos estados resultantes, e atualizar a estimativa de qualidade do ramo da árvore.

- O algoritmo do AlphaZero diferente daquele do AlphaGo em alguns aspectos.
  - Ele deve considerar a possibilidade de cenários diversos de fim de jogo, além de apenas vitórias e derrotas.
  - Ele não pode aproveitar de otimizações típicas do Go, como o fato de o tabuleiro ser simétrico.
    - Não se deve fazer rotações ou aumentar o número de amostras por meio de transformações do tabuleiro.

- O treinamento por reforço é realizado a partir do método de self-play, de forma que um agente joga contra si mesmo em diferentes estágios de treinamento.
  - O conjunto de dados de jogadas gerado é usado para atualizar os pesos da rede neural. Para isso, uma função de recompensa aumenta em 1 o valor estimado de vitória quando a partida termina em vitória, e diminui em 1 quando termina em derrota.

- O AlphaZero mantém apenas uma rede neural, em vez de duas, como o AlphaGo. Ela tem a maior parte de sua arquitetura compartilhada, mas apresenta uma saída que estima a qualidade de vitória de um estado ("value head"), e outra saída que estima a probabilidade de qualidade de cada movimento tomado ("policy head").

- O treinamento do AlphaZero foi realizada em instâncias separadas para cada jogo. A duração do treinamento foi de 9 horas para o Xadrez, 12 horas para o Shogi, e 13 dias para o Go.
  - Os autores compararam quanto tempo de treinamento foi necessário para que cada agente superasse os melhores programas de cada jogo.
  - O AlphaZero superou o Stockfish em Xadrez em 4 horas; superou o Elmo em Shogi em 2 horas; e superou o AlphaGo Lee em Go em 30 horas.

- Os autores ainda demonstraram que o AlphaZero treinado para o Go foi capaz de superar o AlphaGo Zero (com otimizações específicas) em 61% das partidas.

- Para o Xadrez, o AlphaZero venceu o Stockfish em 155 partidas, perdeu em 6, e empatou nas restantes em um conjunto de 1000.

- Para o Shogi, o AlphaZero venceu o Elmo em 98,2% das partidas jogando com peças pretas, e em 91,2% das partidas jogando com peças brancas.

- Os autores concluem que o AlphaZero é um algoritmo de aprendizado por reforço geral, capaz de aprender a jogar diferentes jogos de tabuleiro a partir do conhecimento apenas das regras do jogo, e que supera os melhores programas específicos para cada jogo dada uma quantidade suficiente de tempo de treinamento.

## Faça um resumo do conteúdo epistemológico do Artigo Revolucionário
