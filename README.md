# Repositório de Escrita do TCC - Xamã Cardoso

Este repositório contém a documentação e escrita do Trabalho de Conclusão de Curso (TCC) para o curso de Tecnologia em Análise e Desenvolvimento de Sistemas (TADS).

O projeto foca no desenvolvimento de um robô seguidor de linha utilizando a base do **iRobot Roomba 614**, controlado por um **Raspberry Pi 4**, utilizando controle **PID** e o módulo de sensores **BFD-1000**.

## Estrutura do Repositório

O repositório está organizado em três versões principais do trabalho:

- **[escrita_monografia](./escrita_monografia/)**: Contém a monografia completa do TCC, com todos os elementos pré-textuais, textuais (introdução, fundamentação, metodologia, resultados) e pós-textuais.
- **[escrita_artigo](./escrita_artigo/)**: Contém a versão em formato de artigo científico submetido/preparado para publicação.
- **[escrita_artigo_reduzido](./escrita_artigo_reduzido/)**: Uma versão compacta do artigo, focada nos resultados e implementação técnica.

## Tecnologias Utilizadas na Escrita

- **LaTeX**: Sistema de composição de textos de alta qualidade.
- **BibTeX**: Para gerenciamento de referências bibliográficas.
- **Draw.io/Mermaid**: Para diagramas de arquitetura e fluxo.

## Como Compilar

Para gerar os PDFs a partir dos arquivos `.tex`, recomenda-se o uso de uma distribuição LaTeX (como MiKTeX ou TeX Live) com o comando:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Ou utilizando o `latexmk`:

```bash
latexmk -pdf main.tex
```

E ainda, usando a extensao Latex Workshop do VS Code, basta clicar em "Build LaTeX project" -> "Build with latexmk"

---
*Xamã Cardoso - ADS*
