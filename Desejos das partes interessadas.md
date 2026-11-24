# Sistema de Gerenciamento de Simulações FLUKA (LHCb)

Projeto da disciplina de Engenharia de Software (UFRJ) — André e Gabriel.

Sistema web para fazer upload de simulações de radiação feitas com o **FLUKA** no detector do **LHCb (CERN)**, catalogá-las e visualizá-las em gráficos.

## Desejos das partes interessadas (classificação MoSCoW)

Para cada parte interessada que **usa** o sistema (conforme a planilha de partes interessadas), foram levantados 5 desejos. Cada desejo foi classificado com o método MoSCoW para a **primeira versão (MVP)** do software.

| Classe | Significado |
|---|---|
| **M** – Must have | Obrigatório: sem isso o sistema não atende à necessidade ou não é aceito. |
| **S** – Should have | Importante, mas há solução de contorno provisória; entra se houver prazo. |
| **C** – Could have | Desejável; só entra se sobrar tempo e custo. |
| **W** – Won't have (this time) | Fora desta versão; pode ser considerado no futuro. |

A coluna **Requisito** liga cada desejo ao escopo do projeto (RF = requisito funcional, RNF = não funcional).

### 1. Pesquisador de Radiação (usuário principal)

| # | Desejo | MoSCoW | Requisito | Justificativa |
|---|---|---|---|---|
| P1 | Enviar o arquivo de saída do FLUKA de forma simples, informando nome, descrição e região do detector. | **M** | RF02, RF03 | É a função central do sistema. |
| P2 | Ver os resultados em gráficos 2D e 1D (dose/fluência), com escala linear ou logarítmica. | **M** | RF05 | Objetivo principal: analisar rápido, sem scripts. |
| P3 | Comparar duas ou mais simulações no mesmo gráfico. | **S** | RF06 | Muito útil para análises, mas dá para comparar em gráficos separados no começo. |
| P4 | Listar e buscar simulações por nome, data, autor ou região do detector. | **S** | RF07 | Evita perder simulações; a lista simples já resolve o início. |
| P5 | Exportar o gráfico (PNG) e os dados (CSV) para relatórios e publicações. | **C** | RF08 | Facilita relatórios, mas pode-se usar captura de tela por enquanto. |

### 2. Coordenador do Grupo de Radiação do LHCb

| # | Desejo | MoSCoW | Requisito | Justificativa |
|---|---|---|---|---|
| C1 | Ter uma visão consolidada de todas as simulações do grupo, sem depender de cada pesquisador. | **M** | RF01, RF07 | Base para decisões de proteção radiológica. |
| C2 | Saber quem enviou cada simulação, quando e com quais metadados (rastreabilidade). | **M** | RF02, RF04 | Necessário para relatórios oficiais e conformidade. |
| C3 | Acessar o sistema com perfil próprio, somente para consulta. | **S** | RF01 | Reduz o risco de alteração acidental; pode começar com perfil comum. |
| C4 | Ver um painel-resumo (nº de simulações por região do detector e por período). | **C** | — | Ajuda na gestão, mas não bloqueia o uso. |
| C5 | Gerar relatório em PDF para reuniões e prazos oficiais. | **W** | — | Alto esforço; o PNG/CSV (P5) cobre a necessidade inicial. |

### 3. Oficial de Proteção Radiológica (RP Officer) do CERN

| # | Desejo | MoSCoW | Requisito | Justificativa |
|---|---|---|---|---|
| R1 | Ter garantia de que o dado exibido é idêntico ao enviado (arquivo original preservado, com verificação de integridade). | **M** | RF04 | Decisões de segurança dependem de dados confiáveis. |
| R2 | Ver unidades, eixos e escalas sempre explícitos nos gráficos. | **M** | RF05 | Evita interpretação errada de dose/fluência. |
| R3 | Ter registro de quem enviou, alterou ou excluiu simulações (auditoria). | **S** | RF07 | Reforça a confiança, mas não impede o uso inicial. |
| R4 | Acessar o sistema apenas para leitura. | **S** | RF01 | Mesmo perfil de consulta do coordenador (C3). |
| R5 | Ver limites regulatórios de dose sobrepostos ao gráfico. | **W** | — | Exige validação física e normativa, fora do escopo do projeto. |

### 4. Administrador de TI / Infraestrutura

| # | Desejo | MoSCoW | Requisito | Justificativa |
|---|---|---|---|---|
| T1 | Implantar o sistema com Docker, de forma reproduzível. | **M** | RNF04 | Simplifica instalação e manutenção. |
| T2 | Exigir autenticação e comunicação segura (HTTPS), com senhas protegidas por hash. | **M** | RNF02 | Requisito mínimo de segurança. |
| T3 | Limitar o tamanho dos arquivos enviados e o espaço por usuário. | **S** | RNF01 | Protege o servidor e o desempenho. |
| T4 | Ter backup automático do banco e dos arquivos. | **S** | — | Evita perda de dados; backup manual cobre o começo. |
| T5 | Integrar o login ao SSO do CERN. | **W** | — | Fora do escopo; depende de infraestrutura do CERN. |

### 5. Físico Colaborador de Outra Instituição

| # | Desejo | MoSCoW | Requisito | Justificativa |
|---|---|---|---|---|
| F1 | Consultar simulações do LHCb que forem compartilhadas com ele. | **C** | RF01, RF07 | Colaboração externa é secundária no MVP. |
| F2 | Comparar suas próprias simulações com as do LHCb no mesmo gráfico. | **C** | RF06 | Depende de P3 e de compartilhamento (F1). |
| F3 | Usar a interface em inglês. | **S** | RNF03 | Usuários internacionais; texto traduzido tem baixo custo. |
| F4 | Compartilhar um link direto para um gráfico. | **W** | — | Exige regras de permissão adicionais. |
| F5 | Usar uma API pública para obter os dados. | **W** | — | Fora do escopo desta versão. |

## Resumo da classificação

| Classe | Quantidade | Desejos |
|---|---|---|
| **Must** | 8 | P1, P2, C1, C2, R1, R2, T1, T2 |
| **Should** | 8 | P3, P4, C3, R3, R4, T3, T4, F3 |
| **Could** | 4 | P5, C4, F1, F2 |
| **Won't** | 5 | C5, R5, T5, F4, F5 |

> Observação: foram 25 desejos no total, 5 por parte interessada.

## Documentos do projeto

- Planilha de partes interessadas e planejamento (escopo, EAP, rede, cronograma, recursos e BAC): `planejamento_projeto_fluka_lhcb.xlsx`
- Diagrama de redes: `diagrama_de_redes.png` · EAP: `eap.png`
