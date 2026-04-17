# 📘 Instruções de uso

Este guia descreve como executar o pipeline de dados do SINAN.

## ⚙️ 1. Instalação

```bash
pip install -r requirements.txt

## 📓 2. Abrir o notebook

Você pode usar:

* Jupyter Notebook
* JupyterLab
* VS Code
* Google Colab

## ▶️ 3. Execução

Execute todas as células em ordem.

## 🔄 Etapas do pipeline
1. instalação e imports
2. criação das pastas
3. download dos dados
4. extração das tabelas auxiliares
5. conversão .DBC → .DBF
6. limpeza e filtros
7. recodificação
8. merges
9. criação da base analítica
10. exportação

## 📁 Pastas criadas automaticamente
* dados_brutos/
* tabelas_auxiliares/
* dados_processados/

## 🌐 Sobre os dados
# Arquivos principais

Os arquivos anuais de violência são baixados em formato .DBC.

# Tabelas auxiliares

As tabelas auxiliares são baixadas a partir do arquivo TAB_SINANNET.zip.

## 🧹 Filtros aplicados
* vítimas do sexo feminino
* notificadas na Bahia
* município de Salvador
* exclusão de violência autoprovocada

## 🗺️ Sobre o bairro

A variável BAIRRO_ANALISE representa o bairro associado à unidade notificadora/investigadora.

## 📦 Saídas geradas
#Base completa
.parquet
.csv
#Base analítica
.parquet
.csv

## 💡 Recomendação

Use a base analítica para dashboards e exploração visual.

## ⚠️ Problemas comuns
#Timeout no FTP

Se a conexão cair, execute novamente a célula. Arquivos já baixados são ignorados.

#Arquivos corrompidos

Se necessário, exclua o arquivo problemático e baixe novamente.

## 🧠 Dicas
* execute o notebook por blocos
* acompanhe os logs de download
* revise a base final antes de exportar

### Versão alternativa para download manual do dados

1. Baixe os arquivos DBC do SINAN no site oficial:
   ftp://ftp.datasus.gov.br/dissemin/publicos/SINAN/DADOS/FINAIS/

2. Utilize o TabWin para descompactar os arquivos .dbc para .dbf.

3. Coloque os arquivos na pasta:
   dados_brutos/

4. Execute o notebook do início ao fim.

5. Os dados tratados serão gerados na pasta:
   dados_processados/

Tutorial utilizado para baixar dados do DataSUS: https://www.youtube.com/watch?v=L328u5NqAIo