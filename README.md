# Projeto tema 11 Aprendizagem por Reforço
Grupo: Lucas Venancio, Luiz Matheus, José Rafael

# Dyna-Q e Dyna-Q com Prioritized Sweeping

## Q-Learning
Definição:
Q-Learning é um algoritmo model-free que aprende uma política ótima ao estimar valores de ação-estado (valores Q).
Como funciona: 
- Q-Table: Armazena valores Q para cada par (estado, ação).
- Atualização dos Valores Q: Usa a equação de Bellman:
$$
Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]
$$

Sendo:
- $α$: Taxa de aprendizado 
- $γ$: Fator de desconto (valor futuro)
- $r$: Recompensa imediata
- $s′$: Próximo estado.

### Observação importante:
> $Q-Learning$ é model-free

# Dyna-Q
O Dyna-Q é um algoritmo de aprendizado por reforço de aprendizado baseado em modelo para otimizar o processo de tomada de decisão em ambientes complexos. Ele se destaca por sua capacidade de aprender tanto com a experiência real no ambiente quanto com um modelo interno simulado, permitindo uma aprendizagem mais rápida e eficiente.

Em um agente de planejamento existem pelo menos dois papeis para experimentos reais: pode ser usado para melhorar um modelo (para deixa-lo mais compatível com o ambiente real) e pode ser usado diretamente para melhorar o valor da função e da política usando métodos de Aprendizagem por Reforço. O primeiro chamamos de Aprendizagem de Modelo, e o segundo chamamos de Aprendizagem por Reforço Direta. As possiveis relações entre experiência, modelo, valores e política estão resumidos no diagrama abaixo.

![diagrama circular com setas sobre as relações do modelo](https://github.com/Luv4as/projeto11_RL/blob/main/images/Captura%20de%20tela%202025-02-24%20213459.png)

DIAGRAMA DE SETAS DO LIVRO

Dyna-Q inclui todos os processos mostrados no diagrama acima, planejamento, ação, Aprendizagem por Modelo, e Aprendizagem por Reforço Direta, todos ocorrendo continuamente. Durante o planejamento, o algotimo Q-planning vai aleatoriamente pegar apenas um par de estado-ação que já foi previamente experiênciado, para que o modelo nunca use um par que ele não tem nenhuma informação.

### Componentes Principais do Dyna-Q
Aprendizado sem Modelo (Q-learning): O Dyna-Q utiliza o algoritmo Q-learning para aprender a função de valor Q, que estima a recompensa total esperada ao tomar uma determinada ação em um estado específico. O Q-learning é um método de aprendizado por reforço que não requer um modelo do ambiente, aprendendo diretamente com a experiência.

- Aprendizado Baseado em Modelo: O Dyna-Q mantém um modelo interno do ambiente, que é uma representação das dinâmicas do ambiente. Esse modelo é aprendido a partir da experiência real do agente no ambiente. O modelo permite que o agente simule experiências e aprenda com elas, além de aprender com a experiência real.

- Planejamento: O Dyna-Q utiliza o modelo interno para planejar ações futuras. Ele simula sequências de ações e usa as informações do modelo para atualizar a função de valor Q. Esse processo de planejamento permite que o agente aprenda com experiências simuladas, acelerando o aprendizado.

## Funcionamento do Dyna-Q
- Inicialização: O Dyna-Q inicializa a função de valor Q e o modelo interno do ambiente.

- Interação com o Ambiente: O agente interage com o ambiente, escolhendo uma ação com base na função de valor Q atual e observando o próximo estado e a recompensa recebida.

- Atualização do Q-learning: O Dyna-Q utiliza o algoritmo Q-learning para atualizar a função de valor Q com base na experiência real.

- Atualização do Modelo: O Dyna-Q atualiza o modelo interno do ambiente com base na experiência real.

- Planejamento: O Dyna-Q utiliza o modelo interno para simular experiências e atualizar a função de valor Q com base nessas experiências simuladas.

- Repetição: Os passos 2 a 5 são repetidos até que o agente aprenda a tomar decisões ótimas no ambiente.

### Vantagens do Dyna-Q
 - Aprendizado Mais Rápido: Ao combinar aprendizado com modelo e sem modelo, o Dyna-Q geralmente aprende mais rápido do que os algoritmos que utilizam apenas um dos métodos.
 - Eficiência: O Dyna-Q é eficiente em termos de uso de recursos computacionais, pois o modelo interno permite que o agente aprenda com experiências simuladas, que são mais baratas do que as experiências reais.
 - Generalização: O modelo interno permite que o Dyna-Q generalize o conhecimento aprendido para novas situações, o que pode melhorar o desempenho em ambientes complexos.
### Desvantagens do Dyna-Q
 - Complexidade: O Dyna-Q é mais complexo do que os algoritmos que utilizam apenas aprendizado sem modelo, pois requer a manutenção de um modelo interno.
 - Dependência do Modelo: O desempenho do Dyna-Q depende da precisão do modelo interno. Se o modelo for impreciso, o Dyna-Q pode aprender a tomar decisões subótimas.

# Prioritized Sweeping
Prioritized sweeping é uma técnica poderosa utilizada em algoritmos de aprendizado por reforço baseados em modelo para acelerar o processo de aprendizado. Sua principal vantagem reside na capacidade de priorizar a atualização de estados e ações com base em seu potencial de impacto no aprendizado, tornando o processo mais eficiente.

## Como Funciona o Prioritized Sweeping?
Manutenção de uma Fila de Prioridade: O algoritmo mantém uma fila de prioridade que contém estados e ações que precisam ser atualizados. A prioridade de cada item na fila é determinada por um critério específico, como o erro de previsão do modelo ou a magnitude da mudança na função de valor.

- Inicialização da Fila: Inicialmente, a fila pode conter estados e ações aleatórios ou ser preenchida com base em algum conhecimento prévio do ambiente.

- Seleção e Atualização: O algoritmo seleciona o item de maior prioridade na fila e o utiliza para atualizar o modelo e/ou a função de valor.

- Propagação de Mudanças: Após a atualização, o algoritmo identifica outros estados e ações que foram afetados pela mudança e os adiciona à fila de prioridade, recalculando suas prioridades.

- Repetição: Os passos 3 e 4 são repetidos até que a fila esteja vazia ou um critério de parada seja atingido.

### Vantagens do Prioritized Sweeping
- Aprendizado Mais Rápido: Ao priorizar a atualização de estados e ações mais relevantes, o prioritized sweeping acelera o processo de aprendizado, permitindo que o agente aprenda mais rapidamente a tomar decisões ótimas.

- Eficiência: O prioritized sweeping é mais eficiente do que a atualização uniforme, pois concentra os recursos computacionais nas áreas do espaço de estados e ações que mais precisam ser aprendidas.

- Convergência Aprimorada: Ao atualizar os estados e ações com maior potencial de impacto, o prioritized sweeping pode levar a uma convergência mais rápida para a solução ótima.

### Desvantagens do Prioritized Sweeping
- Complexidade: O prioritized sweeping adiciona complexidade ao algoritmo, pois requer a manutenção de uma fila de prioridade e o cálculo das prioridades.

- Overfitting: Se a prioridade for baseada em informações ruidosas ou incompletas, o prioritized sweeping pode levar a overfitting, onde o agente aprende a tomar decisões subótimas em situações específicas.


## Fontes 
[1] Reinforcement Learning: An Introduction Richard S. Sutton and Andrew G. Barto

