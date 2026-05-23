# EVOKE
Este projeto tem como objetivo principal o desenvolvimento de um dispositivo de entrada adaptativo (Game Controller) com foco total em acessibilidade. O sistema permite que pessoas com mobilidade reduzida controlem jogos ou softwares de computador por meio de comandos de voz, eliminando ou minimizando a necessidade de interação física com botões e joysticks convencionais.

Visão Geral do Projeto
A ideia central é criar uma ponte de acessibilidade de alto desempenho e baixo custo. Ao traduzir comandos sonoros em sinais de teclado padrão, o dispositivo torna-se universalmente compatível com praticamente qualquer jogo ou sistema operacional, sem a necessidade de instalar drivers ou softwares adicionais de terceiros.

Funcionamento Lógico
O fluxo de operação detalha a tradução de sinais acústicos em comandos de software, seguindo a sequência abaixo:

Reconhecimento: O módulo de reconhecimento de voz processa o áudio capturado e identifica palavras-chave previamente treinadas (ex: "pular", "correr", "esquerda").

Processamento: O microcontrolador STM32 recebe o código identificador correspondente à palavra por meio de uma interface de comunicação serial (UART).

Emulação (USB HID): O STM32 opera de forma nativa como um dispositivo USB HID (Human Interface Device). Ao processar o comando de voz, ele envia ao computador um sinal idêntico ao de uma tecla de teclado sendo pressionada.

Feedback Visual: Uma tela OLED fornece retorno em tempo real, exibindo o comando que acabou de ser reconhecido e o status do sistema.

🛠️ Especificações de Hardware
Para esta implementação, optou-se pelo ecossistema STM32 em substituição às plataformas comuns de prototipagem (como Arduino), visando obter maior poder de processamento, periféricos de comunicação nativos mais robustos e flexibilidade de design de firmware.

Microcontrolador: STM32F103C8T6 (Popularmente conhecido como "Blue Pill").

Módulo de Voz: Elechouse Voice Recognition Module V3 (Capacidade para até 80 comandos de voz gravados).

Entradas Físicas (Controle Híbrido): 01 Joystick Analógico e 06 Botões (Push-buttons) para setups mistos de jogabilidade.

Interface de Saída: Conexão USB nativa para emulação direta de periférico.

Diferenciais Técnicos da Implementação com STM32
A transição para a arquitetura ARM Cortex-M do STM32 traz vantagens significativas, mas exige atenção especial a pontos críticos de engenharia de hardware e firmware:

Tolerância e Nível Lógico: O STM32 opera nativamente com nível lógico de 3.3V. A equipe deve garantir que o módulo de voz e as linhas de sinal dos botões estejam referenciados e alimentados corretamente para evitar danos permanentes aos pinos de entrada e saída (GPIOs).

Robustez da USB Stack: Utilizando as bibliotecas nativas de USB do STM32, configuramos o descritor de teclado de forma precisa. Isso garante uma comunicação HID com taxa de atualização superior e menor latência se comparada a soluções emuladas por software.

Priorização via Interrupções (EXTI): O mapeamento de interrupções externas (EXTI) e a configuração do controlador de interrupções (NVIC) garantem que as entradas dos botões físicos e o fluxo serial tenham prioridade absoluta, assegurando uma resposta imediata aos comandos.

Metas de Desenvolvimento
O projeto está estruturado em três fases principais:

[ ] Fase 1: Treinamento e calibração do dicionário de voz (gravação e validação das palavras-chave diretamente na memória do módulo Elechouse).

[ ] Fase 2: Configuração do firmware do STM32 para reconhecimento automático como periférico (Keyboard HID) em sistemas operacionais Windows e Linux.

[ ] Fase 3: Integração fina da lógica "Voz -> Comando USB", calibração de filtros de ruído e montagem final do protótipo físico adaptado.

