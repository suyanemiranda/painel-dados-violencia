# 📊 Pipeline de Tratamento dos Dados do SINAN

## Violência contra Mulheres em Salvador (BA)

Este repositório contém o **pipeline completo de tratamento, filtragem e recodificação** dos dados do **SINAN – Sistema de Informação de Agravos de Notificação**, módulo **Violência Interpessoal e Autoprovocada**, com foco em **mulheres vítimas de violência não autoprovocada no município de Salvador (BA)**.

O projeto foi desenvolvido no contexto de um **Trabalho de Conclusão de Curso (TCC)** em Sistemas de Informação, com o objetivo de subsidiar análises estatísticas e visualizações de dados em dashboards.

---

## 🎯 Objetivo

Construir uma base analítica estruturada para apoiar:

- 👩‍🦰 análise do perfil das vítimas
- ⚠️ compreensão da dinâmica da violência
- 🧑‍🤝‍🧑 estudo da relação entre vítima e provável autor
- 🏥 avaliação da gravidade e das lesões
- 🏛️ análise da resposta institucional
- 🗺️ leitura territorial e temporal das notificações

---

## 📌 Recorte analítico adotado

O pipeline mantém apenas registros que atendem simultaneamente aos seguintes critérios:

- ✔️ vítimas do sexo feminino (`CS_SEXO = "F"`)
- ✔️ notificadas na Bahia
- ✔️ município de Salvador (`ID_MUNICIP = 292740`)
- ❌ exclusão de violência autoprovocada (`LES_AUTOP != "1"`)

Esses filtros garantem consistência territorial e temática com o objetivo do estudo.

---

## 📥 Fonte oficial dos dados

Os dados principais foram obtidos via FTP do DATASUS no diretório:

`ftp://ftp.datasus.gov.br/dissemin/publicos/SINAN/DADOS/FINAIS/`

Os arquivos anuais do agravo de violência são disponibilizados em formato `.DBC`, por exemplo:

- `VIOLBR14.DBC`
- `VIOLBR15.DBC`
- `VIOLBR16.DBC`
- ...
- `VIOLBR24.DBC`

As tabelas auxiliares foram obtidas no diretório:

`ftp://ftp.datasus.gov.br/dissemin/publicos/SINAN/AUXILIAR/`

por meio do arquivo compactado:

- `TAB_SINANNET.zip`

Esses arquivos representam os dados **consolidados e validados**, adequados para uso acadêmico e científico.

> ⚠️ Os arquivos DBC **não são incluídos neste repositório**, pois são volumosos (>100MB) e não devem ser versionados.

---

## 📂 Estrutura do repositório

```
PAINEL-DADOS-VIOLENCIA/
│
├── notebook_sinan.ipynb
├── README.md
├── requirements.txt
├── instruções.md
│
├── dados_brutos/              # gerado automaticamente
├── tabelas_auxiliares/        # gerado automaticamente
└── dados_processados/
```

* `notebook_sinan.ipynb` → pipeline completo de tratamento
* `dados_processados/` → arquivos finais prontos para análise e dashboards
* `requirements.txt` → dependências do projeto

---

## 🔄 Fluxo do pipeline

O notebook executa as seguintes etapas:

📦 instalação das dependências
📁 criação das pastas de trabalho
🌐 download dos arquivos principais via FTP
🗜️ download e extração das tabelas auxiliares
🔄 conversão dos arquivos .DBC para .DBF
🧹 leitura, limpeza e aplicação dos filtros
🧠 recodificação de variáveis categóricas
🔗 integração com tabelas auxiliares úteis
📊 criação da base analítica
💾 exportação das saídas finais

---

## 🧩 Tabelas auxiliares utilizadas

Após inspeção do conteúdo do pacote auxiliar, foram priorizadas as seguintes tabelas:

📌 OCUPANET → ocupação
📌 UNIDTOTAL → unidade + bairro da unidade
📌 UNIDANET → complemento/fallback
📌 UNIDINVEST → complemento/fallback

Tabelas consideradas pouco úteis, vazias ou redundantes para o recorte adotado foram descartadas do pipeline principal.

---
## ⚠️ Observação importante sobre o bairro

A variável BAIRRO_ANALISE não representa necessariamente o bairro exato da ocorrência da violência.

Na base final, ela foi construída prioritariamente a partir da unidade notificadora/investigadora, funcionando como uma aproximação territorial da notificação.

Portanto, esse campo deve ser interpretado como:

    bairro associado à unidade notificadora/investigadora, e não necessariamente ao local exato da violência.

---

## 🧾 Recodificação das variáveis

Grande parte das variáveis do SINAN é codificada numericamente.
Para permitir **análises e gráficos interpretáveis**, o pipeline cria **colunas descritivas (`*_DESC`)**.

### Exemplos de variáveis recodificadas:

### Dados sociodemográficos

* `CS_SEXO_DESC`
* `CS_RACA_DESC`
* `CS_ESCOL_DESC`
* `SIT_CONJUG_DESC`
* `CS_GESTANT_DESC`

### Tipos de violência

* `VIOL_FISIC_DESC`
* `VIOL_PSICO_DESC`
* `VIOL_SEXU_DESC`
* `VIOL_NEGLI_DESC`
* `VIOL_FINAN_DESC`
* entre outras

### Meio de agressão

* `AG_FORCA_DESC`
* `AG_CORTE_DESC`
* `AG_AMEACA_DESC`
* etc.

### Encaminhamentos e rede de proteção

* `ENC_SAUDE_DESC`
* `ENC_DEAM_DESC`
* `ENC_CREAS_DESC`
* `DEFEN_PUBL_DESC`
* etc.

### Relação autor–vítima

Além das variáveis individuais (`REL_CONJ`, `REL_PAI`, etc.), é criada a variável consolidada:

* **`RELACAO_AUTOR`** → texto único com o vínculo identificado

---

## 📦 Arquivos gerados

Na pasta `dados_processados/` encontram-se:

* Base completa:
```
sinan_SSA_mulheres_2014_2024_final.parquet
sinan_SSA_mulheres_2014_2024_final.csv
```
* Base analítica:

```
sinan_SSA_mulheres_2014_2024_analitico.parquet
sinan_SSA_mulheres_2014_2024_analitico.csv
```

Este arquivo é o **dataset principal** para análises e dashboards.

---

## 📈 Uso em dashboards e análises

Os arquivos `.parquet` e `.csv` podem ser utilizados diretamente em:

* Power BI
* Tableau
* Metabase
* Apache Superset
* Streamlit
* Dash / Plotly

Recomenda-se utilizar **Parquet**, por ser mais leve e eficiente.

---

## ❌ Por que os DBFs não estão no GitHub?

* Arquivos muito grandes (100–300MB)
* Limite de tamanho do GitHub
* Boas práticas de ciência de dados
* Evita versionar dados brutos sensíveis

O repositório distribui apenas:

✔ código
✔ documentação
✔ dados tratados e anonimizados

---

## ⚠️ Limitações

📍 o bairro utilizado é uma aproximação territorial associada à unidade notificadora/investigadora
📉 algumas variáveis do SINAN apresentam alto grau de incompletude
🧾 determinados campos exigem interpretação cuidadosa devido à natureza administrativa e epidemiológica da notificação
🔎 o pipeline foi otimizado para o recorte Salvador–BA e mulheres, podendo exigir ajustes para outros recortes

---

## 📘 Instruções de uso

As instruções resumidas são:

1. instalar as dependências
2. executar o notebook em ordem
3. aguardar o download dos arquivos do SINAN
4. permitir a conversão .DBC → .DBF
5. acompanhar a geração das bases final e analítica

---

## 🎓 Contexto acadêmico

Este pipeline foi desenvolvido como parte de um **Trabalho de Conclusão de Curso (TCC)** em Sistemas de Informação, com foco na **visualização de dados públicos sobre violência contra mulheres em Salvador (BA)**, a partir de registros do SINAN.

---

## 📌 Observação final

O notebook disponível neste repositório é a **versão pública e reprodutível** do pipeline.
A versão operacional completa foi executada no Google Colab para geração dos arquivos finais aqui disponibilizados.

---

