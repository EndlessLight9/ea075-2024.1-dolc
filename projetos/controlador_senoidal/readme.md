# `Controlador de MOSFETs`
# `MOSFETs Controller`

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de graduação *EA075 - Introdução ao Projeto de Sistemas Embarcados*, 
oferecida no primeiro semestre de 2024, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).


 |Nome  | RA | Curso|
 |--|--|--|
| Gabriel Tavares Coluccini Francisco  | 239640  | Eng. Elétrica|
| João Pedro Souza Pascon  | 239733  | Eng. Elétrica|

## Descrição do Projeto

Um Gate Driver de MOSFET está associado ao controle de forma específica e coordenada de chaves MOSFETs, como por exemplo um inversor de potência. No atual contexto econômico global, podemos destacar as enormes possibilidades de mercados que esses dispositivos encontrarão em transição energética e eletrificação veicular, sem contar as indústrias já bem consolidadas que utilizam esses equipamentos para controle de motores.

Os usuários são vastos e variados. Podemos citar usinas sucroalcooleiras,  celulose (muito fortes aqui na região sudeste com nomes como Raízen, São Martinho) geradoras, transmissoras e distribuidoras (que cada vez mais adotam a eletrônica de potência, como CPFL e NeoEnergia). Além das novas áreas, como novos parques eólicos (muito proeminentes nas regiões sul e nordeste); parques solares; novas usinas de etanol de milho; além da eletrificação veicular que traz uma grande gama de novos compradores em potencial.

No cenário macroeconômico global, estamos diante de um ciclo de cortes nas taxas de juros, tanto do FED quanto BC, fazendo com que investimentos em reservas nacionais com baixíssimo risco ofereçam um retorno mais baixo. Como consequência, cria-se um cenário em que investidores precisam tomar mais riscos a fim de obter bons retornos, tornando o ciclo de 2024 um momento embrionário de novos projetos e empreendimentos.
 
Apesar dos bons panoramas, é preciso ter em mente que os desafios de mercado ainda são enormes, uma vez que este setor é dominado por gigantes multinacionais (como Siemens; RockWell; ABB e até mesmo WEG) com produtos muito bem estabelecidos e alto poder de investimento. Além disso, muitas vezes essas companhias possuem linhas completas, isto é: motores, controladores, sensores e softwares plug and play, e vendem esse pacote como um conjunto, conseguindo assim um maior poder de barganha frente a um concorrente de menor tamanho.

O game changer desse mercado, estaria nas novas tecnologias de MOSFETs utilizando os materiais WBG – como SiC e GaN –  uma vez que essa tecnologia ainda não está bem consolidada e em constante avanço. O aprofundamento em nuances destes materiais alongaria o corpo deste texto para o projeto específico desta disciplina, mas, é preciso ter consciência da importância destes materiais para a viabilização econômica do dispositivo.

Dessa forma, para o escopo desta disciplina, propõe-se um modelo rudimentar de gate driver. Com o objetivo de criar uma curva de aprendizado e familiarização com o sistema, atuando em um half-bridge com carga indutiva para onda senoidal. Chamamos o modelo de “rudimentar” porque um dispositivo deste, em estado da arte, é um trabalho para um time de vários engenheiros e com anos de experiência, sendo impossível trazer um dispositivo deste para o escopo desta disciplina


 ## Descrição Funcional
 
Embora seja fato que diversos gate drivers comerciais já existentes e bem estabelecidos comercialmente propõem inúmeras funções como: operações do MOSFET em regiões de triodo; adaptabilidade de Vgs; e, nos mais avançados, controle ativo. O projeto proposto não visa chegar em tal ponto de adaptabilidade e escalabilidade, mas sim, permitir um modelo base para criar o início de uma curva de aprendizado e, se possível, futuras melhorias.

Dessa forma, como funcionalidade do projeto, propomos a implementação de um algoritmo em um DSP, que realiza a leitura de potenciômetros, a fim controlar sua frequência de operação e amplitude da onda senoidal resultante de uma fonte de corrente contínua.

Também, é muito importante, em dispositivos de tal forma, monitorar alguns parâmetros de qualidade e falhas e feedback do que acontece. Usaremos alguns leds para isso e espicificaremos essas falhas à medida que o projeto caminha.

Duas configurações de leitrua devem ser feitas: 
- Ler o potênciometro de frequência
- Ler o potênciometro de amplitude
- Detectar se algum dos potenciômetros desconectar
- Detectar sobrecorrentes
- Monitoramento da temperatura

 ### Tratamento de Eventos

Não esperamos que o dispositivo seja inteligente o suficiente para tratar de falhas, mas que pelo menos passe um feedback de qual falha ocorreu.

- LED vermelho - Temperatura alta
- LED verde - Potênciometros desconectados
- LED Azul - Problemas com a corrente

## Descrição Estrutural do Sistema:

(TRABALHAR EM UM DIAGRAMA SOBRE O ALGORITMO EM C) REFERENCIAS DO RASHID

Explicando o circuito: Temos uma fonte de corrente contúnua, que será estabilizada por dois capacitores. O circuito em verde, mostra a lógica do algoritmo SPWM em hardware. Depois, temos um push-pull. Após isso, temos um filtro.

![image](https://github.com/jppascon/ea075-2024.1/assets/163413469/6c2f74a3-67c1-4651-83fb-c9299a4416da)

### Simulações

Antes de especificar o que devemos comprar para o projeto, foi proposto uma primeira bateria de simulações. Do próprio circuito mostrado acima, juntamente com o algoritmo de controle SPWM. Nessa primeira simulação, os valores foram ajustados por eurística.
Validamos as simulacoes da seguinte maneira, em regime permanente:

![image](https://github.com/jppascon/ea075-2024.1/assets/163413469/31154cb5-75fb-454c-b4cd-275f42ed491e)

Observe que, validando com o que era esperado pelo livro do RASHID, temos uma bom modelo por hora.

![image](https://github.com/jppascon/ea075-2024.1/assets/163413469/62213e9e-1e95-45f3-ba51-8c04969d2dce)

Controlando a amplitude e frequência da onda carrier, nós controlamos a amplitude e a frequência da onda de saída. Pretendemos fazer isso via software e utiizar a região de modulação linear.



## Especificações

### Especificações Estrutruais

Vamos nos interessar bastante em quais seriam as vantagens e desvantagens de cada componente. Dando enfoque nos motivos que acarretaram na escolha final do dispositivo. Para os objetos que serão necessários termos:

- Potenciômetros
- Microcontrolador
- Capacitores
- MOSFETs
- Sensores de Temperatura, Corrente
- LEDs RGB

### Microcontrolador

Nesse caso, temos, juntamento com o MOSFET, o componente mais importante do projeto. Dentre os inúmeros microcontroladores disponíveis, optou-se pelo uso do MKL25Z da freescale. A maior justificativa para essa decisão foi um trade off entre tempo de desenvolvimento somado a eficiencia contra custos. Com certeza poderíamos ter optado por um arduino que pudesse fornecer e tratar os dados que precisamos, mas, como em matérias como EA871 aprendemos a comandar o dispositivo MKL25Z, é nítida sua escolha como microcontrolador ideal, visto que na atual disciplina de EA075, o tempo de projeto e disponibilidade de componentes (ja possuimos um MKL na FEEC) são as variáveis de maior importância em qualquer trade-off que devemos fazer.


Em relação aos LEDs, como mostra a imagem abaixo, podemos nos embasar nos LEDs imbutidos na própria placa. Usaremos os pinos PTB18, PTB19, PTD1.
![image](https://github.com/jppascon/ea075-2024.1/assets/163413469/86a1ed5c-e399-4828-b749-14b842f9e5fd)

Para o output, devemos utilizar um pino GPIO ou PWM. A decisão ainda está em aberto, para manter conformidade com o código.

### Sensor de Temperatura
  
Para a escolha do sensor de temperatura para o circuito que estamos planejando implementar alguns requisitos prévios precisavam ser contemplados, como a contabilidade com o case do MOSFET a ser utilizado, medição precisa e rápida de temperatura, facilidade na medição, ou seja, não necessidade de circuitos externos para mensurar e, por conseguinte, garantir a complexidade e preço necessários para a implementação no projeto.

Destarte, foram analisadas três opções de sensores: Termistor NTC, Termopar e Termorresistência de Platina (PT100). Assim, optou-se pelo Termistor NTC devido a sua compatibilidade com a case do MOSFET, sua conveniência de montagem através de furos (through-hole), simplicidade, custo relativamente baixo, resposta rápida e adequação para medições de temperatura em dispositivos eletrônicos como MOSFETs.

Ademais, a priorização do sensor supracitado em relação aos outros supramencionados está associada a diversos fatores, visto que o Termopar, por exemplo, apesar de ser robusto e capaz de medir temperaturas extremas, necessita de circuitos de amplificação e compensação de junção fria para assegurar a precisão, além de ser mais complexo e, possivelmente, mais caro que o necessário. A Termorresistência de Platina (PT100), por outro lado, embora ofereça alta previsão e estabilidade a longo prazo, seu custo elevado, necessidade de circuitos de excitação mais complexos e resposta mais lenta, a tornaram menos atraente para esta aplicação específica.

Portanto, o Termistor NTC foi a escolha ideal, proporcionando simplicidade, custo relativamente baixo, resposta rápida e adequação para medições de temperatura em dispositivos eletrônicos como MOSFETs. Dito isso, foi optado pelo Termistor NTC. O modelo escolhido, que ainda pode estar sujeito a mudanças, EPCOS B57560G103F.

### Sensor de Corrente

Para os sensores de corrente, vamos optar por um resistor shunt. Acreditamos que ele apresenta como sendo mais simples que um resistor por campo magnético e mais acessível.

### Capacitores

Em relação aos capacitores, para as demandas do projeto foram escolhidos dois capacitores de 100 uF, devido ao fato de apresentarem uma boa estabilidade de tensão, ou seja, são capazes de suportar flutuações de tensão, garantindo uma alimentação estável para o MOSFET, evitando oscilações e garantindo o funcionamento adequado do dispositivo.

 Além disso, são capazes de realizar a filtragem de ruídos e picos de tensão indesejados que possam ser introduzidos na linha de alimentação .E, também são comumente encontrados, relativamente compactos em tamanho e disponíveis em uma ampla variedade de faixas de tensão. Logo, são adequados para a aplicação e facilmente integrados ao projeto eletrônico. O capacitor escolhido, visando uma tensão de 32V iniciais para testes foi ECW-F2105JB, mas ainda podendo estar sujeito a mudanças.



## Referencias

RASHID, M. H. (2001) Power Electronics Handbook

J. F. Guerreiro, H. Guillardi and J. A. Pomilio, "Design Procedures and Prototyping of a
Full-Bridge High Frequency Power Inverter," 2019 IEEE 15th Brazilian Power Electronics
Conference and 5th IEEE Southern Power Electronics Conference (COBEP/SPEC), Santos, Brazil,
2019, pp. 1-6, doi: 10.1109/COBEP/SPEC44138.2019.9065318.
keywords: {MOSFET;Logic gates;Capacitors;Gate
drivers;Oscillators;Inverters;Switches;MOSFETs;Power Converter;Inverter;High Switching
Frequency}






