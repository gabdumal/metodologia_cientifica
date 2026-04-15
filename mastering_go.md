# Mastering the game of Go with deep neural networks and tree search

> Silver, D., Huang, A., Maddison, C. et al. Mastering the game of Go with deep neural networks and tree search. Nature 529, 484–489 (2016). <https://doi-org.ez25.periodicos.capes.gov.br/10.1038/nature16961>

## Porque este artigo foi escolhido? Qual a relação com o seu tema de pesquisa?

- Este artigo revolucionário iniciou uma pesquisa do laboratório Google Deepmind, que propôs uma nova técnica para se construir agentes inteligentes capazes de jogar jogos de tabuleiro.

- Este primeiro se restringiu ao jogo Go, e utilizou treinamento supervisionado em parte do ajuste de pesos, mas a técnica foi posteriormente adaptada para outros jogos. Além disso, o treinamento supervisionado foi abandonado, sendo utilizado apenas o aprendizado por reforço.

- O programa AlphaGo, desenvolvido a partir da técnica proposta, teve um desempenho de vitória de 99,8% contra outros programas de computador jogadores de Go testados.

- Além disso, ele indicou a viabilidade do uso de redes neurais para avaliar estados de jogos, diminuindo a necessidade de simulações de jogadas em tempo de execução.

- O trabalho levou a uma quebra de paradigma.
  - Até então, o método de MCTS era o mais utilizado para lidar com o grande espaço de busca de jogos de tabuleiro.
  - Os resultados do AlphaGo levaram outros pesquisadores a se debruçarem sobre técnicas de avaliação de estados de jogos por meio de redes neurais.

- O artigo se relaciona diretamente com meu tema de pesquisa, que se trata do desenvolvimento de agentes inteligentes para jogos de turnos abstratos.
  - O projeto desenvolvido em etapas posteriores desse mesmo grupo, o AlphaZero, foi capaz de vencer os melhores programas de computador jogadores de Go, Xadrez e Shogi, utilizando a mesma técnica, mas sem treinamento supervisionado.
  - Acredito que possa ser possível adaptar a técnica proposta para o desenvolvimento de agentes inteligentes para jogos de turnos abstratos, o que é de interesse para a área de teste de jogabilidade de protótipos de jogos.

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
    - Os autores ainda utilizam alguns passos de simulação (rollout) aleatórios para complementar a avaliação.
  - Além disso, a cada passo de busca, será selecionado um nó para se expandir em uma nova jogada.
    - O método tradicional seleciona a jogada que melhor balanceia a exploração (exploration) da árvore de busca com o aproveitamento (exploitation) dos nós mais promissores, de acordo com a fórmula de Upper Confidence Bounds for Trees (UCT).
    - A técnica revolucionária, em vez disso, fornece o estado do tabuleiro para a "policy network", que retorna uma distribuição de probabilidades sobre os movimentos válidos a partir daquele estado. Então seleciona algum segundo o método da roleta.

- O conteúdo do artigo se debruça principalmente sobre o treinamento das duas redes neurais profundas propostas.

- Num primeiro estágio, os autores utilizam o método de aprendizado supervisionado para treinar uma "policy network" composta de blocos com camadas convolucionais e funções de ativação ReLU. A saída passa for uma função de softmax.
  - Eles utilizara redes de 13 camadas, treinadas com um banco de dados de 30 milhões de estados de jogos de Go jogados por humanos experts, do KGS Go Server.

- Num segundo estágio, os autores utilizam o método de aprendizado por reforço para melhorar a "policy network".
  - Eles empenharam o método de "self-play" (auto-jogo), em que um agente joga contra uma cópia de si mesmo (utilizado iterações variadas de treinamento).
  - Eles determinam uma função de recompensa para o agente, que é +1 para vitória, -1 para derrota e 0 para demais estados.
  - Os pesos são atualizados pelo método de "stochastic gradient ascent".

- O último estágio é o treinamento da "value network", utilizando o método de aprendizado por reforço. Ela apresenta uma arquitetura similar à "policy network", mas com uma camada de saída diferente, que retorna um valor escalar entre -1 e +1.
  - Os pesos são atualizados pelo método de "stochastic gradient descent", utilizando o erro quadrático médio entre a previsão da rede e o resultado efetivo da partida previamente simulada por self-play (vitória, derrota ou empate).

- A avaliação da técnica revolucionária foi realizada por uma competição entre ela e outros programas de computador jogadores de Go que utilizam a técnica de MCTS, e técnicas no estado da arte anteriores.
  - Em partidas comuns, o AlphaGo venceu quaisquer outros programas de computador jogadores de Go testados, com uma taxa de vitória de 99,8%.
  - Mesmo criando estados iniciais com uma vantagem para o adversário, o AlphaGo venceu 77% das partidas contra o programa Crazy Stone (o mais forte).

- Mesmo quando o ciclo de busca proposto não fazia nenhuma simulação de jogadas (rollout), o AlphaGo supero o desempenho de outros programas. Isso indica que a maior contribuição do trabalho é a "value network", que é capaz de avaliar um estado do jogo com alta precisão, sem a necessidade de simulações de jogadas.

## Faça um resumo do conteúdo epistemológico do Artigo Revolucionário

- O artigo se encaixa no domínio da Ciência da Computação, mais especificamente na subárea de Inteligência Artificial.

- Ele trata acerca de jogos de tabuleiro de uma perspectiva de complexidade computacional, e não a partir de preocupações sobre jogabilidade ou design de jogos. Isso ressalta o aspecto soberano do domínio da Ciência da Computação.

- Além disso, a seleção desse único aspecto do domínio de jogos de tabuleiro caracteriza uma abstração realizada sobre o jogo Go.
  - Os aspectos físicos do jogo, como o tabuleiro e as peças, são abstraídos para uma representação matricial de 19x19.
  - O aspecto de interação entre os jogadores é abstraído para um modelo de turnos, em que cada jogador tem um turno para realizar uma jogada, e o jogo termina quando um estado é avaliado como final pelo sistema de regras do jogo.

- Em específico, destaca-se a necessidade de representar um jogo como um sistema de elementos e regras.
  - Ele é composto pelos jogadores, por um conjunto de movimentos válidos, por uma representação do estado da partida, e por um conjunto de funções que levam de um estado a outros segundo as regras.

- Os autores tomam a premissa de que todo estado de um jogo de turnos e de informação completa pode ser avaliado por uma função de valor, e que é possível obter a função de valor ótima para jogos nessas condições.

- Essa premissa domina o paradigma até então vigente, em que o método de MCTS era o mais utilizado para lidar com o grande espaço de busca de jogos de tabuleiro.

- O artigo propõe uma quebra de paradigma, ao apresentar uma técnica de avaliação de estados de jogos por meio de redes neurais, diminuindo a necessidade de simulações de jogadas em tempo de execução.

- Em específico, a hipótese tomada pelos autores é que a "value network" é um método capaz de chegar mais próximo da função de valor ótima do que a técnica de MCTS, e que a "policy network" é um método capaz de selecionar movimentos mais promissores do que a técnica de UCT comum.

- Para comprovar essa hipótese, os autores realizaram experimentos comparando a técnica revolucionária com a técnica de MCTS, e outras técnicas no estado da arte anteriores.

- A reprodutibilidade do artigo é garantida pela descrição: do dataset de treinamento utilizado; das equações matemáticas utilizadas para avaliar os estados; da arquitetura das redes neurais montadas; do método de treinamento utilizado; dos componentes algorítmicos utilizados.

- O treinamento foi realizado com objetividade, tendo sido descrito os componentes de hardware, como a quantidade de CPUs e GPUs, e threads utilizados.

- Por fim, os resultados do experimento foram apresentados com métricas tomadas acerca da acurácia, da taxa de vitórias e do tempo gasto.
  - Como formas de identificar cada técnica, os autores listaram todos os parâmetros relevantes para montar os agentes de AlphaGo, além da versão específica dos softwares adversários.
