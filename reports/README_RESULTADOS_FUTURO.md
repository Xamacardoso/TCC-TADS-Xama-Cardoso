# Resultados, Conclusão e Trabalhos Futuros

Este documento detalha os resultados experimentais obtidos com a construção e teste da plataforma **PiLine ROS**, as conclusões da pesquisa de TCC e o direcionamento para expansões futuras do projeto em cenários educacionais e de logística empresarial.

---

## 📊 Resultados Experimentais e Discussões

A validação da plataforma envolveu testes tanto em ambiente simulado quanto no protótipo físico, focando na estabilidade da comunicação serial, frequência operacional da malha de controle e na precisão do seguimento de trajetória.

### 1. Desempenho da Malha de Controle PID Discreto

A sintonia dos ganhos proporcional ($K_p$), integral ($K_i$) e derivativo ($K_d$) foi realizada de forma empírica, iniciando com testes na simulação virtual e refinando os valores no protótipo físico para compensar o atrito dinâmico e a inércia rotacional do chassi:

*   **Ajuste Fino de Ganhos:** O melhor comportamento de navegação autônoma foi alcançado com os coeficientes $K_p = 0.8$, $K_i = 0.0$ e $K_d = 0.2$. O ganho proporcional de $0.8$ provê correções de trajetória rápidas e enérgicas em curvas acentuadas, enquanto o ganho derivativo de $0.2$ atua eficientemente no amortecimento do sobressinal (*overshoot*), evitando que o robô perca o alinhamento da linha devido a movimentos oscilatórios descontrolados.
*   **Frequência da Malha:** A leitura das GPIOs do Raspberry Pi e o processamento do algoritmo PID operam de forma extremamente estável a **20 Hz** (intervalo temporal $\Delta t = 0.05\text{ s}$). Essa frequência garante que a tomada de decisão ocorra a tempo do robô realizar pequenos ajustes de correção em sua velocidade linear nominal de $0.15\text{ m/s}$.

### 2. Estabilidade Eletrônica e Conectividade do Hardware

Os testes físicos de bancada e pista revelaram alta estabilidade mecânica e de sinal na plataforma modificada:

*   **Mitigação de Ruídos com USB-Serial TTL:** O uso de um cabo conversor dedicado conectado à porta `/dev/ttyUSB0` com taxa de transmissão de `115200 bps` eliminou por completo os ruídos eletromagnéticos e perdas de frame serial antes presentes quando se utilizava conversores de nível lógico avulsos.
*   **Alimentação Isolada e Segurança:** A separação física dos sistemas de energia — mantendo a bateria original de 12V do Roomba dedicada exclusivamente à base motriz e um *powerbank* USB de 5V alimentando o Raspberry Pi 4 — resolveu a ocorrência de episódios de *under-voltage* e travamentos do barramento serial em curvas apertadas (quando o consumo de corrente dos motores atinge picos de demanda).
*   **Inicialização Automatizada via Software:** O pino GPIO 18 da Raspberry Pi conectado ao pino *Device Detect* (DD) do Roomba enviou com sucesso pulsos lógicos estáveis para acordar a base robótica de seu estado de suspensão (*Sleep Mode*), viabilizando uma rotina de inicialização 100% autônoma pelo software ROS 2.

### 3. Validação da Detecção de Visão Computacional (Windows Bridge e Sockets)

*   A arquitetura distribuída via socket TCP/IP comprovou-se viável para realizar processamento distribuído (*Edge Computing*): a transmissão de frames da webcam pelo Windows na porta `9999` foi recebida estavelmente pelo Linux (WSL2) e publicada no `/image_raw`.
*   O nó `detector_aruco` identificou com sucesso marcadores fiduciais 4x4 (`DICT_4X4_50`) usando **OpenCV 4.6.0** no WSL2, gerando o tópico `/aruco_id` para fins de controle e disponibilizando o `/aruco_image_debug` no **RViz 2** para monitoramento.

---

## 🎓 Conclusão do TCC

A engenharia aplicada ao projeto **PiLine ROS** confirmou a **total viabilidade** de adaptar bases robóticas de consumo comercial em sistemas avançados de robótica móvel inteligente. O projeto se consolida com sucesso em duas principais áreas de impacto:

1.  **Potencial Didático e Reprodutibilidade:** O robô serve como um laboratório modular itinerante completo para disciplinas de computação e controle. A arquitetura em **Python** facilita a programação orientada a eventos para estudantes, enquanto a integração com o ROS 2 provê familiaridade direta com o padrão da indústria robótica.
2.  **Viabilidade de Logística Corporativa e Industrial:** A validação física demonstra que é perfeitamente factível projetar AGVs (Veículos Guiados Automaticamente) altamente funcionais de baixo custo. Para pequenas e médias empresas, o PiLine serve como prova de conceito de um ecossistema livre de "caixas-pretas", permitindo que layouts de pista de retalho, almoxarifados ou escritórios sejam facilmente mapeados e atendidos sem investimentos milionários em infraestrutura proprietária.

**Limitações Atuais:**
Nesta fase, a condução física no mundo real baseia-se exclusivamente nos sensores de infravermelho de piso (BFD-1000). A transição completa para a navegação combinada com visão computacional de campo (ArUco) foi mantida em ambiente simulado e computação paralela nesta etapa devido a limitações físicas temporárias para a fixação estável do suporte da câmera no chassi móvel real.

---

## 🚀 Trabalhos Futuros

Tendo validado com sucesso o modelo cinemático diferencial, a potência de tração e a estabilidade da malha fechada, os próximos passos do projeto compreendem:

1.  **Montagem e Integração Física da Câmera:** Concluir o suporte mecânico impresso em 3D para acoplar uma câmera física à estrutura do Roomba, ativando fisicamente o nó de visão e habilitando a navegação híbrida real (PID + Marcadores ArUco).
2.  **Testes de Logística Interna (AGV):** Validar as tarefas de transporte interno em rotas logísticas simulando almoxarifados corporativos de pequeno porte, testando o controle de paradas programadas por sensores baseados em ArUco com o `mission_control_pi`.
3.  **Implementação de Sistemas Cooperativos (Multi-Robôs):** Desenvolver testes de rede descentralizada ROS 2 com múltiplos robôs operando na mesma pista, investigando evasão de colisão entre unidades de transporte internas.
