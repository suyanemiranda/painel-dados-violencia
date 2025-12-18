# 📘 **README — Pipeline de Tratamento dos Dados do SINAN (Violência contra a Mulher — Salvador/BA)**

Este repositório contém o notebook e o pipeline completo para processamento dos arquivos do **SINAN — Violência Interpessoal e Autoprovocada**, com foco em:

* **Mulheres (CS_SEXO = “F”)**
* **Violência Autoprovocada (LES_AUTOP != “1”)**
* **Município de Salvador (ID_MUNICIP = 292740)**
* **Estado da Bahia (SG_UF_NOT = “BA”)**
* Anos **2014 a 2023**, ou qualquer conjunto de DBFs disponíveis.

O objetivo é produzir um banco de dados padronizado e limpo para uso em análises estatísticas, estudos acadêmicos e visualização em dashboards (Power BI, Tableau, Metabase ou aplicações web).

---

# 📂 **Estrutura de Pastas**

```
seu-repo/
│
├── dados_brutos/
│    ├── VIOLBR14.dbf
│    ├── VIOLBR15.dbf
│    ├── ...
│    └── VIOLBR24.dbf
│
├── dados_processados/
│    ├── sinan_SSA_mulheres_2014.parquet
│    ├── ...
│    └── sinan_final_SSA_mulheres.parquet
│
├── notebook_sinan.ipynb
└── README.md
```

---

# ⚙️ **Requisitos**

O notebook utiliza:

* Python 3.8+
* Pandas
* dbfread
* unidecode
* pyarrow (para salvar Parquet)

Para instalar:

```bash
pip install pandas dbfread unidecode pyarrow
```

---

# 🧠 **Sobre a Estrutura do SINAN (Variáveis Importantes)**

### ✔ Sexo (CS_SEXO)

* “M” = Masculino
* “F” = Feminino
* “I” = Ignorado

### ✔ Raça/Cor (CS_RACA)

| Código | Significado |
| ------ | ----------- |
| 1      | Branca      |
| 2      | Preta       |
| 3      | Amarela     |
| 4      | Parda       |
| 5      | Indígena    |
| 9      | Ignorado    |

### ✔ Idade (NU_IDADE_N)

Formato composto:

| Dígito 1 | Unidade |
| -------- | ------- |
| 1        | Horas   |
| 2        | Dias    |
| 3        | Meses   |
| 4        | Anos    |

Exemplos:

* **4018 → 18 anos**
* **3009 → 9 meses**

O notebook converte automaticamente para **IDADE_ANOS** e **FAIXA_ETARIA**.

### ✔ Tipo de Violência (ex.: VIOL_FISIC, VIOL_PSICO)

* “1” = Sim
* “2” = Não
* “9” = Ignorado

Também é criada uma coluna **TIPO_VIOLENCIA** consolidada.

---

# 🔍 **Filtros Aplicados Pelo Pipeline**

O notebook mantém apenas registros que atendam *simultaneamente*:

### 1. Mulheres

```python
CS_SEXO == "F"
```

### 2. Estado da Bahia

```python
SG_UF_NOT == "BA"
```

### 3. Município de Salvador

```python
ID_MUNICIP == 292740
```

### 4. Violência Autoprovocada

```python
LES_AUTOP != "1"
```

---

# 🧼 **Etapas do Pipeline**

O notebook realiza automaticamente:

1. **Leitura de cada arquivo DBF individualmente** (tamanho reduzido → sem estourar RAM).
2. **Aplicação dos filtros** (sexo, UF, município, tipo de violência).
3. **Seleção das colunas relevantes** (`KEEP_COLS_BASE`).
4. **Criação da coluna `ano`** a partir do nome do arquivo.
5. **Conversão de idade NU_IDADE_N → idade em anos**.
6. **Geração de um arquivo Parquet por ano**.
7. **Concatenação de todos os anos tratados**.
8. **Padronização das variáveis categóricas** (sexo, raça, violências).
9. **Criação da variável consolidada TIPO_VIOLENCIA**.
10. **Exportação do dataset final** em Parquet e CSV.

---

# 📊 **Variáveis Derivadas Criadas**

| Variável         | Descrição                                      |
| ---------------- | ---------------------------------------------- |
| `IDADE_ANOS`     | Conversão numérica correta da idade            |
| `FAIXA_ETARIA`   | Idade agrupada em faixas (0–9, 10–14, ... 60+) |
| `CS_SEXO_DESC`   | Sexo por extenso                               |
| `CS_RACA_DESC`   | Raça/cor por extenso                           |
| `TIPO_VIOLENCIA` | Lista consolidada de tipos de violência        |

---

# 📁 **Saídas Geradas**

Os seguintes arquivos aparecem em `dados_processados/`:

### Dataset final consolidado:

```
sinan_final_SSA_mulheres.parquet
sinan_final_SSA_mulheres.csv
```

---

# ▶️ **Como Executar o Notebook**

### No Google Colab:

1. Suba os arquivos DBF na pasta `dados_brutos/` do Google Drive ou do ambiente local.
2. Rode todas as células do notebook.
3. Os arquivos processados aparecerão em `dados_processados/`.

### Localmente (Jupyter, VS Code):

1. Crie as pastas conforme a estrutura indicada.
2. Coloque os DBFs em `dados_brutos/`.
3. Execute o notebook.
4. Os resultados aparecerão na pasta `dados_processados/`.

---

# 📈 **Uso em Dashboards**

O arquivo final pode ser usado diretamente em:

* Power BI
* Tableau
* Qlik
* Metabase
* Dash/Plotly
* Streamlit
* Superset

Recomendação: use sempre o arquivo **Parquet**, pois é mais leve e rápido.

---

# ✨ **Créditos e Observações**

Este pipeline foi desenvolvido para um Trabalho de Conclusão de Curso na área de **Sistemas de Informação**, com foco na visualização de dados sobre **violência contra a mulher na cidade de Salvador**, utilizando dados brutos do SINAN (Ministério da Saúde).


