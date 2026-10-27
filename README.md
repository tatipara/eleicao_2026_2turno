# Eleições 2026 — abstenção e voto por local de votação

Dados abertos do 1.º e do 2.º turno da eleição presidencial de 2026, organizados por **local de votação** (a escola, o ginásio, o posto onde as pessoas votam) e por **urna**, para responder a três perguntas:

- Onde a abstenção mudou entre os turnos, e em quanto?
- Quais seções eleitorais mudaram de local de votação — e isso afetou o comparecimento?
- Onde o voto em cada candidato avançou ou recuou?

Este repositório existe para que qualquer pessoa — programando ou não — possa conferir, reaproveitar e questionar essa análise. Não é preciso saber Python para usar os dados: as planilhas abrem direto no Excel, no Google Sheets ou em qualquer programa de números.

**Site com o mapa interativo e a análise:** https://tatipara.github.io/eleicao_2026_2turno/

---

## O que tem aqui

| Arquivo | O que é |
|---|---|
| `index.html` | A página inicial do site, com os mapas, os números principais e os links para tudo |
| `mapa_T1xT2_BR.html` | O mapa interativo: um ponto para cada local de votação do Brasil, com quatro camadas para alternar (abstenção, urnas transferidas, mudança de liderança, variação do voto) |
| `analise_os_dados.png` | A imagem com os quatro mapas lado a lado, pronta para compartilhar |
| `Eleicao2026_locais_abstencao_IBGE_v2.ipynb` | O notebook: o código que baixa os dados oficiais, confere os totais e monta todas as planilhas abaixo. Roda no Google Colab, sem precisar instalar nada |
| `presidente_2026_T2_por_local.csv` | Votos e abstenção do **2.º turno**, uma linha por local de votação |
| `presidente_2026_T2_por_municipio.csv` | A mesma informação, somada por município, já com o código do IBGE |
| `LICENSE` | Licença MIT: o código pode ser copiado, alterado e reaproveitado livremente, inclusive para fins comerciais, desde que se mantenha o aviso de autoria |

### Arquivos grandes demais para o GitHub

Algumas planilhas — sobretudo a de votos por **urna**, que tem uma linha para cada uma das mais de 470 mil urnas do país — passam do limite de tamanho que o GitHub aceita. Esses arquivos estão disponíveis nesta pasta do Google Drive:

**📁 https://drive.google.com/drive/folders/1CbYko5wnANgWJaNHuDwHWNjQzVRL7mBX**

Lá você encontra, além dos arquivos listados acima:

- `presidente_2026_T1_por_local.csv` e `presidente_2026_T1_por_municipio.csv` — a mesma estrutura do 2.º turno, mas para o **1.º turno**;
- `presidente_T1xT2_por_urna.csv` — uma linha para cada urna, com o local de votação do 1.º e do 2.º turno lado a lado, a distância entre eles e a variação de voto e abstenção;
- `presidente_T1xT2_por_local.csv`, `..._por_municipio.csv` e `..._por_uf.csv` — a mesma comparação entre turnos, somada por local, por município e por UF;
- `auditoria.csv` e `manifesto_fontes.csv` — as planilhas de conferência: os totais calculados ao lado dos totais oficiais do TSE, e o registro (com código de verificação SHA-256) de cada arquivo baixado.

> Se algum desses nomes não bater exatamente com o que está na pasta do Drive, é porque a lista acima foi escrita a partir do notebook — confira o conteúdo real da pasta e ajuste este README se precisar.

---

## De onde vêm os dados

- **TSE — Portal de Dados Abertos** (dadosabertos.tse.jus.br): votação por seção eleitoral e cadastro de eleitores por local de votação, que traz o número de eleitores aptos, o endereço e as coordenadas de cada seção, separadamente para o 1.º e o 2.º turno.
- **IBGE — Censo 2022**: população dos municípios, usada só como contexto demográfico (população não é eleitorado).

Nenhum dado é digitado à mão: o notebook baixa tudo direto dos servidores oficiais, no momento em que é executado.

## Como a abstenção é calculada

| Grandeza | Cálculo |
|---|---|
| Aptos | eleitores habilitados a votar naquela seção, naquele turno |
| Comparecimento | soma de todos os votos para presidente — nominais, brancos e nulos |
| Abstenção | Aptos − Comparecimento |
| % de abstenção | 100 × Abstenção ÷ Aptos |

É a mesma definição usada pelo TSE. A população do Censo entra apenas como referência de contexto, nunca como denominador da abstenção.

## Por que a comparação entre turnos é feita por urna

O local de votação pode receber seções diferentes em cada turno — no 2.º turno de 2026, o TSE transferiu 4.352 seções para outro endereço. Comparar local com local misturaria a mudança de comportamento dos eleitores com a simples troca de quem vota ali.

A **urna**, ao contrário, carrega sempre o mesmo grupo de eleitores nos dois turnos. Por isso, a comparação urna a urna é a unidade mais confiável para medir variação — e é a que este projeto usa.

## O que esses dados não dizem

Os números são agregados por urna ou por local, nunca por eleitor individual. Se o voto em um candidato cresceu numa região, isso pode vir de quem mudou de voto, de quem antes se absteve ou de quem votou em outro candidato no 1.º turno — os dados agregados não distinguem essas situações. Atribuir uma causa única a partir desses números é o que se chama de **falácia ecológica**, e as leituras deste projeto evitam esse erro.

---

## Como reproduzir tudo do zero

1. Abra o notebook `Eleicao2026_locais_abstencao_IBGE_v2.ipynb` no [Google Colab](https://colab.research.google.com) (clique em *Abrir com* → *Google Colaboratory*, ou use o botão de abrir no Colab se o GitHub mostrar um).
2. Rode as células em ordem, de cima para baixo (*Ambiente de execução → Executar tudo*).
3. O notebook baixa os arquivos oficiais, confere os totais contra os números divulgados pelo TSE e salva todas as planilhas numa pasta local.
4. No fim, um arquivo `.zip` com tudo fica disponível para download.

Cada etapa do notebook tem uma célula de texto explicando a variável usada e por que aquela biblioteca foi escolhida — a ideia é que alguém sem experiência em programação consiga acompanhar o raciocínio, mesmo sem rodar o código.

## Como usar as planilhas sem programar

Todas abrem direto no Excel ou no Google Sheets:

1. Baixe o arquivo (clique nele no GitHub e depois em **Download raw file**, ou baixe direto da pasta do Drive).
2. Abra no Excel: se os acentos aparecerem estranhos, use *Dados → Obter dados → De texto/CSV* e escolha a codificação **UTF-8**.
3. As colunas mais úteis para começar:
   - `PCT_ABSTENCAO` (ou `PCT_ABST_T1` / `PCT_ABST_T2` nas planilhas comparativas) — percentual de abstenção;
   - `DELTA_ABST_PP` — quanto a abstenção mudou, em pontos percentuais, entre os turnos;
   - `DELTA_LULA_2C_PP` — quanto a participação de Lula no confronto direto mudou entre os turnos.

## Limitações e avisos

- O mapa interativo (`mapa_T1xT2_BR.html`) reúne cerca de 94 mil pontos numa única página. Ele é pesado e pode demorar para abrir, especialmente no celular ou em conexões lentas.
- Esta é uma análise independente, feita a partir de dados públicos. Não tem vínculo com o TSE, com partidos ou com campanhas.
- Encontrou um erro ou uma inconsistência? Abra uma [issue neste repositório](https://github.com/tatipara/eleicao_2026_2turno/issues) — correções e sugestões são bem-vindas.

## Licença

Código sob licença MIT (veja `LICENSE`). Os dados eleitorais são públicos, de titularidade do TSE; os dados de população, do IBGE. Ao reaproveitar este material, cite a fonte original e, se puder, este repositório.
