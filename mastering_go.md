# Mastering the game of Go with deep neural networks and tree search

> Silver, D., Huang, A., Maddison, C. et al. Mastering the game of Go with deep neural networks and tree search. Nature 529, 484–489 (2016). <https://doi-org.ez25.periodicos.capes.gov.br/10.1038/nature16961>

## Porque este artigo foi escolhido? Qual a relação com o seu tema de pesquisa?

Este artigo revolucionário iniciou uma pesquisa do laboratório Google Deepmind, que propôs uma nova técnica para se construir agentes inteligentes capazes de jogar jogos de tabuleiro.

Este primeiro se restringiu a

 <!--TODO-->

## Faça um resumo do conteúdo metodológico/tecnológico do Artigo Revolucionário

- O jogo Go é desafiador para agentes de inteligência computacional, dado seu grande espaço de busca.

- O artigo propõe a tecnologia de "value networks" para avaliar uma configuração das peças no tabuleiro (chamada de estado) e "policy networks" para selecionar entre os movimentos disponíveis naquele estado.

- Na técnica desenvolvida, essas redes neurais profundas são treinadas em uma combinação de aprendizado supervisionado por jogadores humanos experts, e de aprendizado por reforço de acordo com o método de "self-play".

- Os agentes treinados tem performance comparável àqueles que utilizam a técnica de "Busca em Árvore de Monte Carlo (MCTS)", mas sem realizar a etapa de simulação de jogadas durante o ciclo de busca. Isso resulta num custo computacional menor.

- Esse novo ciclo de busca desenvolvido pelos autores apresentou uma taxa de vitória de 99,8% contra outros programas jogadores de Go testados, e venceu um jogador humano profissional em todas as cinco partidas disputadas.
  - Até então, um programa de computador nunca havia vencido um jogador humano profissional em uma partida de Go com as regras completas.

- Os autores tomam como premissa que todo estado de um jogo que seja de turnos e de informação completa pode ser avaliado por uma função de valor (value).
  - É possível obter a função de valor ótima para jogos nessas condições.
  - Esse jogos podem ser resolvidos (solved) quando se computa recursivamente a função de valor ótima para cada estado do jogo em uma árvore de busca.

- As árvores de buscas de jogos de turnos apresentam como:
  - largura: a quantidade de movimentos válidos a partir de qualquer estado do jogo; e
  - profundidade: a quantidade de jogadas necessárias para chegar a um estado terminal do jogo.

- Os autores assumem que, para jogos grandes, o espaço de busca é tão grande que é inviável fazer uma busca exaustiva.
  - Eles assumem aproximadamente uma largura de 250 movimentos válidos e uma profundidade de 150 jogadas para o jogo de Go.
  - Também assumem aproximadamente uma largura de 35 movimentos válidos e uma profundidade de 80 jogadas para o jogo de Xadrez.

- Uma técnica comum para lidar com esse espaço de busca é a MCTS. Ela atribui a cada nó da árvore uma estimativa do valor do ramo de busca a partir daquele nó.
  - Conforme passos da busca são realizados, as estimativas de valor dos nós são atualizadas com base nos resultados das simulações de jogadas realizadas a partir daquele nó.
  - Assintoticamente, a MCTS converge para a função de valor ótima.

- O novo ciclo de busca desenvolvido pelos autores é uma adaptação da MCTS.
  - O método tradicional de atribuição de valor a um nó é realizar simulações do seu estado até o fim da partida e avaliar seu resultado (se vitória, derrota ou empate)
  - A técnica revolucionária, em vez de simular a partida, solicita a avaliação do estado do nó para a "value network", uma rede neural profunda de classificação de imagens.
    - Ela recebe como entrada o tabuleiro do jogo no estado atual representado no tamanho de 19x19 (para o jogo de Go) e retorna uma estimativa do valor do estado, em relação à probabilidade esperada de vitória (ou derrota) para o jogador atual.
  - Além disso, a cada passo de busca, será selecionado um nó para se expandir em uma nova jogada.
    - O método tradicional seleciona a jogada que melhor balanceia a exploração (exploration) da árvore de busca com o aproveitamento (exploitation) dos nós mais promissores, de acordo com a fórmula de Upper Confidence Bounds for Trees (UCT).
    - A técnica revolucionária, em vez disso, fornece o estado do tabuleiro para a "policy network", que retorna uma distribuição de probabilidades sobre os movimentos válidos a partir daquele estado. Então seleciona algum segundo o método da roleta.

## Faça um resumo do conteúdo epistemológico do Artigo Revolucionário
