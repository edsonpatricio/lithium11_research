
#Lithium11 - Algoritmo de reconhecimento de atenção


#Índice
* auto-gen TOC:
{:toc}


## Introdução
Nossa atenção é cada vez mais exigida ao longo do tempo. Não só nossos dispositivos computacionais crescem em capacidade de executar múltiplas aplicações, como nós mesmo somos praticamente coagidos a lidar com mais de uma tarefa ao mesmo tempo. A imensa maioria dessas tarefas necessitam de atenção visual: olhar para tela do dispositivo, movimentar os olhos percorrendo a tela. Esses mesmos dispositivos, em sua maioria, possuem câmeras apontadas para o usuário.

Com o uso da câmera do próprio dispositivo podemos analisar aspectos do comportamento do usuário, na tentativa de reconhecer o quanto de atenção está sendo prestada à tela.

A pesquisa atual objetiva servir de base para a implementação de uma sistema de reconhecimento de atenção de usuários de dispositivos como celular, tablets e notebooks. Utilizando de câmeras providas por esses dispositivos, o projeto *Lithium11* visa analisar imagens com a finalidade de coletar e inferir informações através de padrões cognitivos do movimentos dos olhos.

Nos capítulo de Aspectos científicos da medição de atenção serão abordadas fundamentações teóricas que justificam a formulação das análises feitas pelo projeto, atráves de dados consolidados por diversas metodologias e pesquisas cientificas que comprovam ou validam análise de alerta e atenção. 

O capítulo de Aspectos Limitantes aborda limites concluidos atráves da análise do capítulo de Aspectos científicos da medição de atenção com a finalidade de obter melhor cenário de funcionamento para o projeto, definindo parâmetros que garantem uma taxa de acerto viável.

No capítulo de Desenvolvimento aborda uma modelagem de funcionamento do algoritmo atráves de diagramas *UML*, além da descrição do ambiente de desenvolvimento e testes para a integridade dos artefatos gerados em cada plataforma.

## Aspectos científicos da medição de atenção

### Movimentos Sacádicos

Movimentos sacádicos podem ser definidos como movimentos rápidos e abruptos dos olhos que trazem novos objetos para o foco da retina (1). Acontecem ao observar um ambiente estático. Este tipo de movimento depois que iniciado não tem mudança de trajetória. Durante os movimentos sacádicos, existe uma perda de precisão visual (2).  Devido a isso é utilizado um mecanismo associado ao movimento dos olhos conhecido como *RVO*, *Reflexo Vestibulo-Ocular*. O *RVO* utiliza estruturas do ouvido interno para estabilizar o olhar e claridade da visão  (3). Movimentos resultantes do *RVO* acontecem em um tempo muito curto, (~16ms) comparando ao movimento apenas de estimulo visual (~70ms) (4).

Utilizando hardware medianos atuais (novembro de 2014), como base a câmera do celular LG Optimus G, o qual captura imagens a uma taxa de 30 frames por segundo, teríamos em uma captura a cada 33,34ms, impossibilitando analise por quantidade de quadros obtido, já que necessitaria de no mínimo 3 quadros em 60ms para analisar esse movimento. Para realizar rastreamento de movimentos oculares a fim de obter os movimentos sacádicos é necessário uma câmera com lentes sensíveis a infravermelho de alta velocidade (RS-170 ou CCIR) (5), sendo esses hardwares específicos e não encontrado em câmeras atuais de dispositivos móveis.

Dessa forma não está no escopo do algoritmo qualquer forma de análise de movimentos sacádicos com intenção de determinar a atenção do indivíduo analisado.

### Piscar dos olhos

O tempo e a frequência do piscar dos olhos de forma espontânea é influenciado por processos cognitivos ou neurofisiológicos , porém como esses processos ainda são incertos (6) para que possam ser analisados com precisão suficiente.  Muitos estudos visam associar a frequência do piscar de olhos (*EBR* - *EyeBlinking Rate*) com a mudança de estados cognitivos (8), estendendo essa pesquisa para associar os níveis de dopamina, o neurotransmissor responsável por diversas funções, incluindo atenção(9), com *EBR*(10). O aumento do *EBR* em adulto está ligado a atuação de estímulos, como por exemplo, a taxa de *EBR* é maior durante a fala e leitura do que em repouso. Esta frequência distribuída por tempo, mostra-se não uniforme e intimamente ligado ao estimulo(11)(12). A figura 1, mostra o resultado da pesquisa de (6), a qual foi aplicado testes utilizando o efeito *Stroop*(interferência no tempo de reação para uma dada tarefa) em um ambiente controlado para medição. Esse experimento refere-se a incidência de piscada  de acordo com estimulo visual e tarefa realizada pelo analisado.

![Visual Stroop] (http://www.plosone.org/article/fetchObject.action?uri=info:doi/10.1371/journal.pone.0034871.g002&representation=PNG_M)



Durante a execução de tarefas no experimento, houve uma média de piscadas de 30.7 ps/min com uma faixa de erro de 13.0 ps/min para mais ou menos e no estado repouso 20.7 ps/min com uma faixa de erro de 11.2 ps/min. Foi admitido que o estado de repouso é olhar para um ponto fixo por 2 minutos.

####Visão x Dispositivos digitais

Uma das funções do piscar de olhos é manter úmido o globo ocular, com a finalidade de lubrificar e nutrir a córnea (10), mas com uma leitura prolongada em telas digitais, os olhos necessitam adaptar-se a saturação da tela, tendendo a piscar menos. Dessa forma o liquido que lubrifica a cornea irá evaporar com maior rapidez, deixando os olhos secos(11). Esse fator artificial reduz o *EBR* espontâneo(12) para cerca de 10.5 pis/min com imprecisão de 6.5 durante atividade de leitura e 7.6 pis/min com imprecisão de 6.7 durante atividades com longas durações relacionadas a computadores (13). Aplicativos como *EyeGuardian* utilizam um limite de frequência mínima de 13.5pis/min para notificar e prevenir esses fatores(11).

####Alerta e fadiga


 Fadiga pode ter origem apenas mental, sem necessidade de desgastes físicos, nesses casos tem como consequência a sonolência e diminuição geral de atenção, podendo ter causas diversas, como trabalho em excesso, estresse, super-estimulação, sub-estimulação, dentre outros. (17)

 Estudos sobre medidas de sonolência utilizam mecanismos de análise através do *eletroencefalograma* (*ECG*), o qual mede atividade e frequências cerebrais que através de comparações com medições de referencia é possível calibrar o algoritmo e inferir informações sobre a fadiga e sonolência (14). Não adotaremos esse mecanismo devido a incompatibilidade dos dispositivos alvos(descritos no mapa mental) ao sensores necessários.

 Segundo o modelo de análise de fadiga utilizando *redes de Bayesian*(16), foi criado um gráfico acíclico (figura 2) que demonstra causas e resultados da fadiga. 

![BN Model](http://i1381.photobucket.com/albums/ah236/Alano_Martins/CapturadeTela2014-11-19a3000s150126_zpsa463344a.png)
Figura 2: Modelo de fadiga humana

 O projeto *Lithium11* visa analisar frames para inferir nível de atenção do usuário, logo a sub-árvore de interesse da aplicação será apenas as consequências representada pela figura 3.

![BN Model Part](http://i1381.photobucket.com/albums/ah236/Alano_Martins/subArvore_zps9c0d09f6.png)
Figura 3: Características da fadiga

 Utilizando a árvore da figura 3 como áreas de pesquisa relevantes para medição de fadiga, está fora do escopo do projeto a análise o olhar fixo (*Gaze*), movimentos da cabeça e faciais. 

 Análise utilizando olhar fixo utiliza-se de movimentos sacádicos, os quais já tiveram a impossibilidade de aplicação justificada no capítulo sobre Movimentos Sacádicos.

 Para mensurar movimentos realizados pela cabeça, é necessário a fixação da câmera em relação ao observador e isso é impossível garantir no uso de dispositivos móveis que estão previstos na lista de premissas do projeto.

 Dessa forma o projeto Lithium11 utiliza a média de abertura e fechamento dos olhos (*AECS*) e percentagem de tempo com pálpebras fechadas (*PERCLOS*) (15) como indicadores de fadiga ou alerta. Segundo experimentos realizados por (18), uma media desses indicadores pode ser observado nas figuras 4 e 5.

![AECS](http://i1381.photobucket.com/albums/ah236/Alano_Martins/AECS_zps5f5787f0.jpg "AECS")
Figura 4. Média de AECS

![PERCLOS](http://i1381.photobucket.com/albums/ah236/Alano_Martins/PERCLOS_zps553b8312.jpg "PERCLOS")
Figura 5. Média de PERCLOS

Analises resultantes da média de *PERCLOS* (figura 5) apontam que abaixo de 30% é considerado estado de atenção normal e acima de 40% alto índice de sonolência, considerando um estado de fadiga. (19)
Na figura 4 podemos observar o aumento acentuado de *AECS* no intervalo entre 150.000ms e 200.000ms,  os quais podem ser associados a crescente sonolência. 
De acordo com essas análises, uma correlação é feita entre *PERCLOS* e *AECS* indica com maior precisão um estado de fadiga, demonstrando ser um medidor confiável sobre níveis de alerta do usuário.

### Atenção instantânea
 A mensuração de atenção instantânea no projeto *Lithium11* utilizará de identificação e rastreamento de pupila para inferir o local de interesse visual em uma dado momento (frame) e assim verificar se o indivíduo está olhando em direção a tela. Não está no escopo dessa analise nenhuma inferência sobre nível de atenção, pois não haverá entrada de *frames* suficiente para análise. 

## Aspectos limitantes

### Angulação
Utilizando uma implementação do algoritmo de identificação e rastreamento de pupila de sugerido no artigo de (20) (figura 6), testes realizados utilizando uma câmera de 1.3mpx, demonstram uma queda de precisão acentuada com angulação da cabeça em relação a câmera de 50°, como demonstrado pelas imagens.

![enter image description here](http://i1381.photobucket.com/albums/ah236/Alano_Martins/AngCen_zps0bf3306e.png)
Figura 6: Captura correta da irís

![enter image description here](http://i1381.photobucket.com/albums/ah236/Alano_Martins/Ang1_zps4346ee0f.png)
Figura 7: Captura incorreta da irís esquerda

 Acima dessa angulação, o reconhecimento da pupila torna-se pouco previsível nesse algoritmo.

### Frequência mínima de quadros

Para o calculo de frequência minima de quadros necessários para realização de uma análise, utiliza a frequência *AECS* de 80ms como mínimo tempo analisado, caso a piscada tenha velocidade abaixo de 80ms, será inferido nível de atenção maxima nesse quesito. Afim de garantir que durante o intervalo entre quadros é possível capturar uma piscada na frequência mínima, obtem a taxa mínima de quadros de 12.5fps. Seguindo a análise feita no capítulo Visão x Dispositivos Digitais, a frequência de piscada (ERB) no pior caso é em torno de 7.6pis/min, porém para um ganho de performance e otimização de recursos o projeto adotará a frequência sugerida por (11) de 13.5 pis/min. Com esse resultado, pode-se inferir que uma piscada a cada 4,44s, considerando uma margem de erro referente a uma piscada, o intervalo passa para 8,88s, assim utiliza-se aproximação para 10s para um intervalo onde deve haver uma piscada. 
 Como resultado do cálculos acima, o projeto *Lithium* deverá ter como entrada minima para análise de 125 frames em uma taxa de 12.5 frames/sec.


## Desenvolvimento

##Diagrama do algoritmo


```flow
st=>start: Obtenção de sequência de frames
e=>end:>
op1=>operation: Detecção de rosto
op2=>operation: Detecção de pupila
op3=>operation: Detecção de piscada
op4=>operation: Nível de atenção
io=>inputoutput: Entrada de dados insuficiente
cond1=>condition: Sucesso?
cond2=>condition: Sucesso?
cond3=>condition: Sucesso?


st->op1->cond1
cond1(no)->io->e
cond1(yes)->op2(right)->e
op2->cond2
cond2(no)->io->e
cond2(yes)->op3(right)
op3->cond3
cond3(no)->io->e
cond2(yes)->op4->e
```
Figura 8: Análise de atenção

```flow
st=>start: Obtenção de frame
e=>end:>
op1=>operation: Detecção de rosto
op2=>operation: Detecção de pupila
op3=>operation: Análise de atenção
io=>inputoutput: Entrada de dados insuficiente
cond1=>condition: Sucesso?
cond2=>condition: Sucesso?


st->op1->cond1
cond1(no)->io->e
cond1(yes)->op2(right)->e
op2->cond2
cond2(no)->io->e
cond2(yes)->op3(right)
op3->e
```
Figura 9: Atenção instantânea

## Desenvolvimento

### Ambiente de desenvolvimento 
 O projeto *Lithium11* será desenvolvido com a linguagem *C++*, para garantir uma portabilidade entre as plataformas destinadas segundo a lista de premissas. Será utilizado um framework open source de visão computacional chamado de *OpenCV* na versão 2.4.9 para prover implementações de algoritmos que auxiliarão no projeto. O computador onde será desenvolvido é um *MacBook Pro*, i5 2,4Ghz com 8Gb de RAM e a câmera HD nativa *iSight* na resolução de 640x480px com taxa de 30fps. Para validar os artefatos gerados para dispositivos móveis, será utilizado o celular LG Optimus G E977, com a câmera frontal nativa de 1.3mpx e Android 4.1 (Jelly Bean).

### Compilação cruzada (cross-building)

 Objetivando a criação de um ambiente de desenvolvimento multi-plataforma, será adotada a ferramenta open source de controle de sistemas de *build* e empacotamento, *CMake*. Esta ferramenta utiliza de compiladores nativos para realizar o *build* na plataforma local ou  um arquivos de configuração para identificar compiladores e formas de empacotar o código fonte. O *build* multi plataforma será utilizado para gerar artefatos *.so* compilados para *ARM7v* através dos compiladores providos pelo *NDK (Native Development Kit)* da plataforma *Android*. 
 Para a compilação no ambiente *Windows* será utilizado o sistema operacional *Windows 7* com o compilador *Visual C++ Compiler* com a finalidade de gerar biblioteca dinâmica *.dll* para essa plataforma. Para o sistema *IOS*, o *MAC OS Maverick 10.9.5* com o compilador *G++*  será responsável por compilar para o formato *.dylib*.
 Como mencionado no capitulo  "Ambiente de desenvolvimento", o projeto possui uma dependência com o framework *OpenCV* e isso deve ser sinalizado durante o processo de compilação, para isso utilizaremos o *toolchain* criado pela comunidade que mantêm o *OpenCV*, o qual também utiliza o *CMake*.

### Ambiente de Integração 

 Assim como definido no capítulo anterior, o projeto será compilado para cada plataforma utilizando o compilador nativo em cada sistema operacional e o compilador provido pelo *NDK* para *Android*. Para testar a integridade de cada artefato gerado, o projeto utilizará de aplicações-testes automatizadas que verificam a integridade da biblioteca gerada em cada sistema operacional e um projeto *Android* para com o artefato *.so* *ARM7v*.
	

##Bibliografia

1. Carpenter, R. H. S. (1988). Movements of the eyes. London: Pion Limited.

2. Volkman, F. C., Schick, A. M. L., & Riggs, L. A. (1968). Time course of visual inhibition during voluntary saccades. Journal of Optical Society of America, 58(4), 562-569.

3. Laurutis, V. P., & Robinson, D. A. (1986). The vestibulo-ocular reflex during human saccadic eye movements. Journal of Physiology (London), 373, 209-233.

4. Lee, J. R., & Zeigh. D. S. (1991). The neurology of eye movements. Philadelphia: F.A. Davies.

5. Covre, Priscilla et. Al. (2005). Eye movements and scan patterns in mental rotation tasks

6. Oh J, Han M, Peterson BS, Jeong J (2012) Spontaneous Eyeblinks Are Correlated with Responses during the Stroop Task. PLoS ONE 7(4): e34871. doi:10.1371/journal.pone.0034871

7. von Cramon D, Schuri U (1980) Blink frequency and speed motor activity. Neuropsychologia 18: 603–606.

8. Bacher LF, Smotherman WP (2004) Spontaneous eye blinking in human infants: a review. Dev Psychobiol 44: 95–102.

9. Artigo online – Ver como referenciar: http://www.news-medical.net/health/Dopamine-Functions-(Portuguese).aspx

10. http://www.aaco.com.br/plastica_ocular.htm

11. EyeGuardian: A Framework of Eye Tracking and Blink
Detection for Mobile Device Users
Seongwon Han, Sungwon Yang, Jihyoung Kim and Mario Gerla
Dept. of Computer Science, UCL

12. Yan, Z., Hu, L., Chen, H., and Lu, F. Computer vision
syndrome: A widely spreading but largely unknown
epidemic among computer users.Comput. Hum. Behav. 24
(September 2008), 2026{2042.

13. Tsubota, K. Tear dynamics and dry eye. Progress in
Retinal and Eye Research 17, 4 (1998), 565 { 596

14. QiangJi, PeilinLan, Carl Looney “A Probabilistic Framework for Modeling and Real-Time Monitoring Human Fatigue‖, IEEE Transactions on Systems, Man, and Cybernetics—part A: Systems a

15. Fabian Friedrichs and Bin Yang, “Camera- based Drowsiness Reference for Driver State Classification under Real Driving Conditions”, 2010 IEEE Intelligent Vehicles Symposium, June 21-24, 2010

16. Jordan, M. 1.1999, editor. Learning in Graphical Models, MIT press

17. Ver como referências ou buscar outra fonte: http://www.copacabanarunners.net/cansaco-fadiga.html

18. Real Time Visual Cues Extraction for Monitoring Driver Vigilance
Qiang Ji, Xiaojie Yang

19. Real-Time Eye, Gaze, and Face Pose Tracking for Monitoring Driver Vigilance # 2002 Elsevier Science Ltd. All rights reserved.
Qiang Ji and Xiaojie Yang 
Department of Electrical, Computer,
and System Engineering Rensselaer Polytechnic Institute,

20. ACCURATE EYE CENTRE LOCALISATION BY MEANS OF
GRADIENTS
Fabian Timm and Erhardt Barth
Institute for Neuro- and Bioinformatics, University of Lubeck, Ratzeburger Allee 160, D-23538 L ¨ ubeck, Germany ¨
Pattern Recognition Company GmbH, Innovations Campus Lubeck, Maria-Goeppert-Strasse 1, D-23562 L ¨ ubeck, Germany ¨
{timm, barth}@inb.uni-luebeck.de

21. PERCLOS Threshold for Drowsiness Detection during Real Driving Sheng Tong Lin, Ying Ying Tan, Pei Ying Chua, Lian Kheng Tey and Chie Hui Ang

22. Magee, J. J., Scott, M. R., Waber, B. N., and Betke, M. Eyekeys: A real-time vision interface based on gaze detection from a low-grade video camera. In In Proceedings of the IEEE Workshop on Real-Time Vision for Human-Computer Interaction (RTV4HCI (2004),
pp. 159–166.

23. A drowsiness and point of attention monitoring system for driver vigilance. Batista, J. ; FCTUC -Univ. of Coimbra, Coimbra

24. Miluzzo, E., Wang, T., and Campbell, A. T. Eyephone: activating mobile phones with your eyes. In Proceedings of the second ACM SIGCOMM workshop on Networking, systems, and applications on mobile handhelds (New York, NY, USA, 2010), MobiHeld ’10, ACM, pp. 15–20.



