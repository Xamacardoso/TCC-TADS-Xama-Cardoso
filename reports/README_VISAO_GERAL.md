# PiLine ROS: Plataforma Robótica Seguidora de Linha com ROS2

**Autor:** Xamã Cardoso Mendes Santos  
**Orientador:** Prof. Me. Francisco Marcelino Almeida de Araújo  
**Curso:** Tecnologia em Análise e Desenvolvimento de Sistemas (TADS)  
**Instituição:** Instituto Federal de Educação, Ciência e Tecnologia do Piauí (IFPI) - Campus Teresina Central  
**Ano:** 2026  

## Visão Geral do Projeto

O **PiLine ROS** é um projeto de Trabalho de Conclusão de Curso (TCC) focado na engenharia de sistemas robóticos que propõe a adaptação de um robô aspirador comercial (Roomba® 614 iRobot) em uma plataforma robótica móvel seguidora de linha autônoma. 

### Objetivos do Projeto

A plataforma foi projetada sob uma filosofia de arquitetura aberta, modular e amplamente documentada, visando atender a dois grandes propósitos de aplicação prática:

1.  **Recurso Didático-Científico:** Servir como um laboratório didático de alta fidelidade e baixo custo para o ensino, experimentação e pesquisa de conceitos avançados de cinemática diferencial, teoria de controle clássico (PID), comunicação serial tolerante a ruídos, computação em borda (*Edge Computing*) e o middleware industrial **ROS 2**. A plataforma é integrada de forma ativa às atividades e pesquisas do Laboratório de Automação, Robótica e Sistemas Inteligentes (**LABIRAS**).
2.  **Solução de Logística de Pequeno/Médio Porte:** Apresentar uma plataforma base de custo acessível e modular aplicável em **ambientes corporativos e industriais de pequeno e médio porte**. O robô serve como um protótipo de veículo guiado automaticamente (AGV) para tarefas de transporte interno de pequenas cargas, monitoramento de estoque ou rastreamento em rotas pré-definidas (através do sensoriamento de linha infravermelho aliado à detecção de marcadores ArUco), preenchendo a lacuna entre as caras soluções industriais proprietárias (caixas-pretas) e a necessidade de flexibilização de layouts operacionais em pequenas empresas.

## Resumo

O desenvolvimento de plataformas robóticas móveis para fins educacionais e de automação industrial de pequeno porte demanda arquiteturas de software flexíveis, robustas e de baixo custo. Este trabalho foca na transformação estrutural e lógica de um Roomba 614 comercial. O sistema utiliza um **Raspberry Pi 4 Model B** (operando com Ubuntu 22.04) como unidade central de processamento e tomada de decisão local, eliminando a necessidade de microcontroladores intermediários. Essa abordagem de *Edge Computing* permite a leitura direta de um módulo seguidor de linha de 5 canais (**BFD-1000**) e a execução do controle **Proporcional-Integral-Derivativo (PID)** em tempo real.

Todo o software de controle e comunicação foi construído de forma modular e altamente desacoplada sobre o ecossistema **ROS 2** (suportando ROS 2 Humble / Jazzy Jalisco), adotando a linguagem **Python** (`rclpy`) e integrando processamento de imagem com **OpenCV** para leitura de marcos topológicos (IDs ArUco). A arquitetura de hardware resultante é simplificada e robusta, empregando comunicação direta via cabo conversor USB-Serial TTL para mitigar ruídos de motor e garantir estabilidade operacional.

Os resultados demonstram a viabilidade da plataforma, validando seu modelo cinemático em ambiente físico e provando seu potencial como sistema móvel versátil para integração de sensores, atuadores, controle inteligente e comunicação em rede.

---
*Para mais detalhes sobre as tecnologias utilizadas e o funcionamento do sistema, consulte o `README_ARQUITETURA.md`.*
