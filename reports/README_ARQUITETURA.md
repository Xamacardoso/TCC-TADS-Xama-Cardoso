# Arquitetura de Hardware e Software

Este documento detalha as especificações técnicas, organização de hardware e engenharia de software da plataforma **PiLine ROS**, construída para o TCC.

## Arquitetura de Hardware

A arquitetura eletrônica foi organizada em camadas focadas em simplicidade e baixa latência de resposta, eliminando microcontroladores intermediários na leitura dos sensores e centralizando o controle:

*   **Chassi / Base:** Robô aspirador comercial Roomba® 614 iRobot, complementado por partes estruturais modulares impressas em 3D (modeladas no Autodesk Fusion 360).
*   **Camada de Controle e Processamento (*Edge Computing*):** Raspberry Pi 4 Model B. Atua como o cérebro principal do robô, lendo diretamente os sinais e publicando comandos.
*   **Sensoriamento:** Módulo BFD-1000 de 5 canais infravermelhos de alta sensibilidade para detecção precisa da linha na superfície, acoplado na parte frontal inferior. Possui também sensores para prevenção de colisão.
*   **Camada de Comunicação Física:** Conexão direta entre o Raspberry Pi e os atuadores (motores do Roomba) mediada por um cabo conversor USB-Serial TTL, mitigando ruídos e garantindo respostas bidirecionais estáveis.
*   **Alimentação:** Bateria dedicada de 12V do ROOMBA e *powerbank* externo para alimentar o RASPBERRY PI 4.

## Arquitetura de Software

O sistema roda sobre o *middleware* **ROS 2** (suportando tanto o ROS 2 Humble no hardware físico com Ubuntu 22.04 quanto o ROS 2 Jazzy Jalisco em ambiente WSL2 com Ubuntu 24.04). Todo o projeto segue o padrão de *packages* do ROS 2, com diretórios e tópicos normalizados (ex: `/cmd_vel`, `/line_sensor`) para simplificar a integração e interoperabilidade. A linguagem primária de desenvolvimento de todos os nós foi o **Python** (através da biblioteca **rclpy**), facilitando o desenvolvimento rápido e a integração nativa com bibliotecas avançadas de visão computacional.

### Controle de Navegação Autônoma (Algoritmo PID)

A correção de trajetória em tempo real é feita por uma malha fechada baseada no controle Proporcional-Integral-Derivativo (PID):
1.  **Leitura Direta:** O nó ROS 2 lê continuamente os estados lógicos (0 ou 1) gerados pelos 5 sensores do BFD-1000 diretamente nos pinos GPIO do Raspberry Pi 4.
2.  **Cálculo de Erro:** Cada sensor recebe um peso específico (ex: -2, -1, 0, 1, 2). O erro do sistema é calculado a partir de uma média ponderada dos sensores ativos.
3.  **Equação PID Discreta:** A variável de controle angular é processada somando a resposta proporcional, integral (acumulada no tempo) e derivativa (variação de erro) para produzir movimentos suaves e rápidos na malha. A equação matemática implementada é:

    $$u(k) = K_p e(k) + K_i \sum_{i=0}^{k} e(i) \Delta t + K_d \frac{e(k) - e(k-1)}{\Delta t}$$

    Onde:
    *   $u(k)$ é o sinal de controle (correção da velocidade angular);
    *   $e(k)$ é o erro de posição atual;
    *   $\Delta t$ é o intervalo de tempo entre as amostras;
    *   $K_p$, $K_i$ e $K_d$ são os ganhos proporcional, integral e derivativo, respectivamente.
4.  **Publicação de Comando:** O valor corretivo gerado é convertido em velocidades lineares e angulares `geometry_msgs/Twist` publicadas via tópico intermediário `/cmd_vel_pid` para a tomada de decisão antes de serem repassadas ao driver dos motores no tópico `/cmd_vel`.

### Monitoramento por Visão Computacional (ArUco) e Mensagens Padrão

*   **Detecção de Marcadores ArUco (Monitoramento):** O sistema utiliza visão computacional em tempo real para monitorar a localização e coordenar missões em estações físicas na pista. O nó `detector_aruco` consome o fluxo de vídeo `/image_raw` (capturado localmente ou recebido via sockets pela `ponte_camera`), detecta marcadores 4x4 (`DICT_4X4_50`) usando a biblioteca **OpenCV 4.6.0** e publica o ID reconhecido no tópico `/aruco_id`. Além disso, gera a imagem depurada `/aruco_image_debug` exibindo visualmente a detecção no **RViz 2** para monitoramento remoto.
*   **Mensagens Padrão do ROS 2 (Sem Mensagens Customizadas):** O projeto prioriza portabilidade e facilidade de manutenção ao utilizar exclusivamente os tipos de mensagens nativos e padronizados do ROS 2, eliminando pacotes e dependências customizadas:
    *   **`/line_sensor` (`std_msgs/msg/Int32MultiArray`):** Vetor com o estado binário de leitura direta (0 ou 1) dos 5 canais do módulo infravermelho BFD-1000.
    *   **`/cmd_vel` e `/cmd_vel_pid` (`geometry_msgs/msg/Twist`):** Vetores de velocidade linear e angular para acionamento diferencial da base motriz.
    *   **`/image_raw` e `/aruco_image_debug` (`sensor_msgs/msg/Image`):** Frames de imagem brutos e processados para depuração visual da câmera.
    *   **`/aruco_id` (`std_msgs/msg/Int32`):** Identificador numérico da estação ativa para coordenação lógica de estados da missão.

---
*Para ver como a arquitetura se traduziu em resultados concretos, veja o `README_RESULTADOS_FUTURO.md`.*
