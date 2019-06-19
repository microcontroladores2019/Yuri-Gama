>__Redução de Ruído Digital__

>__Descrição do conceito com função e motivação__

Processamento digital de sinal (Digital Signal Processing - DSP) de áudio para receptor de rádio com recursos de redução de ruído digital (Digital Noise Reduction-DNR) baseados em subtração espectral, com emprego dos recursos de FreeRTOS da "Real Time Engineers".

O objetivo desse projeto é utilizar a arquitetura de DSP do microprocessador ARM Cortex M4 presente na placa de desenvolvimento STM32F407 empregando os recursos de FreeRTOS da "Real Time Engineers", a fim de possibilitar uma maior economia de energia e eficiência no processamento. Devido às extensões DSP é possível processar FFT em tempo real (bin) de 256 elementos garantindo uma boa resposta nas funcionalidades de DNR, filtragem e visualização do sinal de áudio.

Para realizar a pré-filtragem do sinal de entrada ruidoso, quatro elementos filtrantes do tipo resposta ao impulso finita (_Finite Impulse Response - FIR_) serão implementados, com as seguintes características:
- Filtro 0: passa-alta (> 12kHz);
- Filtro 1: passa-baixa para AM (<6kHz);
- Filtro 2: passa-baixa para SSB (<3kHz);
- Filtro 3: passa-banda para CW (500 Hz).

Será implementado também um Filtro Adaptativo (_Least Mean Square_ - LMS). Na filtragem convencional do tipo resposta ao impulso finita (FIR) ou do tipo resposta ao impulso infinita (IIR), geralmente assume-se que os parâmetros do processo estocástico de entrada são conhecidos, tais parâmetros determinam as características do sinal de entrada. No entanto, em nosso problema prático, alguns parâmetros de entrada podem mudar, ou ainda, o sistema pode não conhecer exatamente o comportamento destes parâmetros. Desse modo, é altamente desejável um filtro que possua o atributo de auto-aprendizado, de maneira que este possa ajustar-se automaticamente de acordo com cada situação. O algoritmo clássico LMS utilizado constitui uma das técnicas mais conhecidas para a implementação de um filtro adaptativo. O LMS utiliza um algoritmo de gradiente estocástico para adaptar a carga de um filtro. Esta adaptação consiste no ajuste contínuo (e adaptativo) dos valores dos coeficientes do filtro, tendo como métrica a minimização do erro no sentido da média dos quadrados.

Além disso,  será utilizada a funcionalidade da FFT (Direta e Inversa), pois a partir do valor de módulo (magnitude) do sinal pode-se aplicar a técnica de subtração espectral de magnitude, a fim de restaurar o espectro de magnitude do sinal em presença do ruído aditivo. Em seguida, a informação de fase contaminada com o ruído é novamente combinada ao sinal, e então passada para o domínio do tempo, via FFT inversa. Este processo está representado no diagrama de blocos de DNR.

Funções finais:
- DNR com dois estágios (Filtros de subtração espectral + filtro adaptativo LMS);
- Filtros BPF / LPF (12, 6, 3, 0,5 kHz);
- Decodificador Morse CW.

Hardware a ser utilizado:
- Placa STM32F407;



- Display TFT SPI 2";
- Encoder-Decoder KY-040;
- Módulo mínimo de áudio;
- ST-Link V2 (conforme tabela de pinagem).

![ST-LinkV2](https://user-images.githubusercontent.com/37374766/59639155-bb661d00-9130-11e9-893d-06e34e660bf2.JPG)

>__Diagrama de Blocos do DNR__

![Diagrama_Blocos_DNR](https://user-images.githubusercontent.com/37374766/59629492-581cc080-9119-11e9-888f-9a164f1cd51c.jpg)

>__Diagrama e Tabela de pinagem__

![Diagrama da Eletrônica com pinagem](https://user-images.githubusercontent.com/37374766/59636471-fb75d180-9129-11e9-84a0-c89447238805.jpg)

|ST-Link V2|JTAG na STM32F407|
|----------|-----------------|
|SWDIO     |Pin7 (TMS)       |
|GND       |Pin4 (GND)       |
|SWCLK     |Pin9 (TCLK)      |
|3.3V      |Pin1 (VCC)       |

>__Firmware__

Firmware para o ST-LinkV2 (Interface PC - stm32f407): 
https://www.st.com/en/development-tools/stsw-link004.html

>__Referências__

- Application note AN3997 : "Audio Playback and Recording using ST32F4Discovery", encontrado no site da ST;
- Reference manual RM0090 - Manual de Referência da Discovery;
- Repositório RadioDSP-DNR-Stm32f407 disponível em <https://github.com/gcallipo/RadioDSP-DNR-Stm32f407> acesso em 17 de junho de 2019;
- Repositório STM32F4-FreeRTOS disponível em <https://github.com/wangyeee/STM32F4-FreeRTOS> acesso em 17 de junho de 2019.

