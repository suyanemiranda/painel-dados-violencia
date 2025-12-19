# 📊 Pipeline de Tratamento dos Dados do SINAN

## Violência contra Mulheres em Salvador (BA)

Este repositório contém o **pipeline completo de tratamento, filtragem e recodificação** dos dados do **SINAN – Sistema de Informação de Agravos de Notificação**, módulo **Violência Interpessoal e Autoprovocada**, com foco em **mulheres vítimas de violência não autoprovocada no município de Salvador (BA)**.

O projeto foi desenvolvido no contexto de um **Trabalho de Conclusão de Curso (TCC)** em Sistemas de Informação, com o objetivo de subsidiar análises estatísticas e visualizações de dados em dashboards.

---

## 📥 Fonte oficial dos dados

Os dados utilizados são provenientes do **repositório oficial do DATASUS**, no formato DBF, versão **FINAIS**:

```
ftp://ftp.datasus.gov.br/dissemin/publicos/SINAN/DADOS/FINAIS/
```

Os arquivos seguem o padrão:

```
VIOLBR14.dbf  → 2014
VIOLBR15.dbf  → 2015
...
VIOLBR24.dbf  → 2023
```

Esses arquivos representam os dados **consolidados e validados**, adequados para uso acadêmico e científico.

> ⚠️ Os arquivos DBF **não são incluídos neste repositório**, pois são volumosos (>100MB) e não devem ser versionados.

---

## 📂 Estrutura do repositório

```
PAINEL-DADOS-VIOLENCIA/
│
├── dados_brutos/
│   └── instruções.txt
│
├── dados_processados/
│   ├── sinan_SSA_mulheres_2014.parquet
│   ├── sinan_SSA_mulheres_2015.parquet
│   ├── ...
│   ├── sinan_final_SSA_mulheres.csv
│   └── sinan_final_SSA_mulheres.parquet
│
├── notebook_sinan.ipynb
├── requirements.txt
└── README.md
```

* `notebook_sinan.ipynb` → pipeline completo de tratamento
* `dados_processados/` → arquivos finais prontos para análise e dashboards
* `requirements.txt` → dependências do projeto

---

## ☁️ Como os dados são processados

O processamento é realizado **no Google Colab**, utilizando o sistema de arquivos temporário (`/content`):

1. Os arquivos DBF são **enviados manualmente** para o Colab durante a sessão.
2. O notebook lê cada DBF **ano a ano**, evitando estouro de memória.
3. Os dados são filtrados, tratados e recodificados.
4. Os arquivos finais são exportados em formato **Parquet** e **CSV**.
5. Apenas os dados tratados são enviados para este repositório.

---

## 🔍 Filtros aplicados

Os registros mantidos no dataset final atendem **simultaneamente** aos seguintes critérios:

```python
CS_SEXO == "F"                 # Mulheres
SG_UF_NOT == 29 ou "BA"        # Bahia
ID_MUNICIP == 292740           # Salvador (município de notificação)
LES_AUTOP != "1"               # Violência NÃO autoprovocada
```

Esses filtros garantem consistência territorial e temática com o objetivo do estudo.

---

## 🔄 Tratamento da idade

A variável `NU_IDADE_N` é convertida corretamente para idade em anos (`IDADE_ANOS`), considerando sua codificação oficial:

* 1xxx → horas
* 2xxx → dias
* 3xxx → meses
* 4xxx → anos

Também é criada a variável:

* `FAIXA_ETARIA` (0–9, 10–14, 15–19, ..., 60+)

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

* Arquivos Parquet **por ano**
* Um arquivo final consolidado:

```
sinan_final_SSA_mulheres.parquet
sinan_final_SSA_mulheres.csv
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

## 🎓 Contexto acadêmico

Este pipeline foi desenvolvido como parte de um **Trabalho de Conclusão de Curso (TCC)** em Sistemas de Informação, com foco na **visualização de dados sobre violência contra mulheres em Salvador (BA)**, utilizando dados públicos do SINAN.

---

## 📌 Observação final

O notebook disponível neste repositório é a **versão pública e reprodutível** do pipeline.
A versão operacional completa foi executada no Google Colab para geração dos arquivos finais aqui disponibilizados.

---

