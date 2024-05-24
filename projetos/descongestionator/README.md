# `Rodovias Fluidas: Sistema de Redução de Congestionamentos`
# `Smooth Highways: Trafic Jam Reduction System`

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de graduação *EA075 - Introdução ao Projeto de Sistemas Embarcados*, 
oferecida no primeiro semestre de 2024, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).

 |Nome  | RA | Curso|
 |--|--|--|
 | Kevin Caio Marques dos Santos  | 247218  | Eng. de Computação|
 | Thiago Maximo Pavão  | 247381  | Eng. de Computação|


## Descrição do Projeto

O projeto foi idealizado para abordar um problema comum enfrentado pelos motoristas hoje em dia: congestionamentos em rodovias. Muitas vezes, atribui-se esse problema à ocorrência de acidentes que bloqueiam uma das vias, forçando o tráfego a se concentrar na outra via. No entanto, em muitos casos de congestionamento, não há acidente aparente ou outra explicação clara, o que levanta questões para os motoristas mais perspicazes.

Durante uma investigação sobre essa questão, foi encontrado um [vídeo](https://www.youtube.com/watch?v=iHzzSao6ypE) que explicava a causa de muitos congestionamentos. O principal fator identificado foi a grande disparidade de velocidades entre os veículos na mesma via, juntamente com ultrapassagens realizadas por veículos mais lentos. Isso faz com que outros motoristas tenham que reduzir sua velocidade, criando uma espécie de "onda de lentidão" que se propaga ao longo da rodovia. Eventualmente, essa onda de lentidão pode levar alguns carros a pararem completamente para evitar colisões.

Diante desse contexto, o objetivo do projeto é reduzir a propagação dessa onda de lentidão, diminuindo assim a extensão dos congestionamentos, especialmente nos pontos onde os motoristas são obrigados a parar completamente. O objetivo final é evitar que o congestionamento se agrave.

A prejudicialidade dos congestionamentos está muito além do atraso que gera para os motoristas em suas trajetórias diárias. Alguns exemplos de consequências são:

 - Estresse dos motoristas
 - Acidentes
 - Maior consumo de combustível
 - Maior poluição
 - Maior prejuízo para a saúde dos motoristas expostos a poluição gerada pelos carros

Nesse sentido, o Rodovias Fluídas® visa solucionar todos esses problemas ao monitorar o surgindo desses engarrafamentos e enviar informações aos motoristas que vem atrás para reduzirem a velocidade, de modo ao chegarem ao local do suposto congestionamento, ele ja tenha desaparecido. Com a implementação desse sistema, prevê-se que haja um significativo retorno econômico ao reduzir o consumo de combustível e a prevenção de acidentes.

## Descrição Funcional

O objetivo do projeto é identificar congestionamentos nas rodovias e avisar os motoristas que trafegam na mesma rodovia para reduzirem a velocidade, de modo que ao chegarem ao local em que existia o engarrafamento ele já tenha se resolvido.

### Funcionalidades

- Medir a velocidade do veículo
- Medir a posição do veículo na rodovia
- Mostrar mensagens em um display sobre congestionamentos e informar qual velocidade o motorista deve trafegar.
- Reproduzir avisos sonoros quando novas informações estão disponíveis
- Detectar a localização e o tamanho de congestionamentos
- Determinar a velocidade adequada em reação a um congestionamento à frente
- Capacidade de distribuição das informações sobre congestionamentos para os carros trafegando na rodovia 

### Configurabilidade

Como configuração, o usuário que possuir o dispositivo em seu carro pode optar por desabilitar o aviso sonoro e ao invés disso o display de informações deve piscar algumas vezes para chamar a atenção do motorista.

### Eventos

#### Eventos esperados em fluxo normal

1. Recebimento de informação de posição e velocidade de carro na via (periódico, 30 segundos)
2. Disparo de verificação das informações recebidas para detecção de congestionamentos (periódico, 30 segundos)
3. Detecção do surgimento de um congestionamento
4. Detecção do fim de um congestionamento, volta do fluxo normal
5. Recebimento de informações sobre congestionamentos na via

#### Eventos de erro, inesperados

1. Falha na obtenção da velocidade do veículo
2. Falha na obtenção da posição do veículo
3. Falha na comunicação com o sistema (envio/recebimento de mensagens) 

### Tratamento de Eventos

#### Eventos esperados em fluxo normal

1. Armazena as informações de velocidade e posição recebidas em uma tabela para futura consulta.
2. Roda um algoritmo que utiliza as informações na tabela de velocidade e posição e verifica se há congestionamentos.
3. Envia a informação do surgimento de um engarrafamento e a sua localização para os motoristas que trafegam na rodovia.
4. Envia a informação da finalização do engarrafamento para os motoristas que trafegam na rodovia.
5. Calcula a velocidade ideal para o carro trafegar antes de encontrar o congestionamento e informa ao motorista essa velocidade, através de um display luminoso e efeitos sonoros.
 
#### Eventos de erro, inesperados

* Itens 1, 2 e 3: Avisa ao usuário que ocorreu um problema, qual foi e que não terá mais a assistência do sistema até resolver o infortúnio.


## Descrição Estrutural do Sistema

O sistema será composto por dois tipos de dispositivos: Módulos móveis, que estão presentes nos carros dos usuários do sistema e módulos fixos, presentes ao longo das rodovias, principalmente em trechos em que congestionamentos são comuns.

Os módulos móveis tem como objetivo monitorar a velocidade do carro e informar ao motorista quando deve reduzir sua velocidade. Ele baseia essa decisão em sinais recebidos pelo módulos fixo mais próximo. Além disso, tem o papel de informar a velocidade do carro para o módulo fixo mais próximo.

Os dispostivos fixos são torres colocadas ao longo da via, e tem como objetivo detectar congestionamentos, recebendo informações de velocidade dos motoristas mais próximos, e repassar a informação para torres anteriores na via, que por sua vez avisam sobre as condições ao módulos móveis proxímos a elas.

![Alt text here](diagramas/diagrama-estrutural.png)

## Referências

- Vídeo motivador do projeto: https://www.youtube.com/watch?v=iHzzSao6ypE
