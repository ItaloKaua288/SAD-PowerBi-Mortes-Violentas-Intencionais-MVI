# 📊 Análise de Mortes Violentas Intencionais (MVI) – Pernambuco (2004 a 2026)

Este repositório contém o projeto de **Business Intelligence e Engenharia de Dados** focado na análise de dados sobre **Mortes Violentas Intencionais (MVI)** no estado de Pernambuco, abrangendo o período histórico de **janeiro de 2004 a julho de 2026**.

O objetivo principal é transformar dados brutos da segurança pública em conhecimento estratégico por meio de processos de **ETL (Extract, Transform, Load)** e de um painel interativo desenvolvido no **Power BI**.

---

## 🗂️ Estrutura do Repositório

```text
.
├── Dataset/                    # Ficheiro de Dataset
├── Pentaho/                    # Ficheiro de ETL (.ktr) do Pentaho Data Integration
│   └── *.ktr
│
├── Saidas/                     # Ficheiros CSV gerados pelo processo de ETL
│   ├── DIM_LOCALIDADE.csv
│   ├── DIM_OCORRENCIA.csv
│   ├── DIM_TEMPO.csv
│   ├── DIM_VITIMA.csv
│   └── FATO.csv
│
├── SAD_MVI_Pernambuco.pbix     # Dashboard / Relatório do Power BI
└── README.md                   # Documentação do projeto
```

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

| Tecnologia / Ferramenta                     | Utilização                                                            |
| ------------------------------------------- | --------------------------------------------------------------------- |
| **Pentaho Data Integration (PDI / Kettle)** | Extração, tratamento, limpeza e modelagem dos dados brutos            |
| **Power BI**                                | Modelação dimensional, cálculos DAX e criação do dashboard interativo |
| **DAX & M**                                 | Modelação e enriquecimento dos dados no Power BI                      |
| **Git & GitHub**                            | Controlo de versão e documentação do projeto                          |

---

## 🔄 Fluxo do Processo de ETL

O pipeline desenvolvido no **Pentaho Data Integration** executa os seguintes passos principais:

### 1. Leitura dos Dados Brutos

Ingestão do ficheiro CSV contendo os dados históricos de **Mortes Violentas Intencionais (MVI)** de Pernambuco.

### 2. Tratamento de Dados e Limpeza

* Padronização de textos em maiúsculas (**Upper Case**);
* Remoção de espaços extras;
* Tratamento de valores nulos, com atribuição de valores padrão para dados ausentes;
* Mapeamento de categorias utilizando **Value Mapper**;
* Categorização por faixas etárias:

  * Menores;
  * Adultos;
  * Idosos;
  * Idade desconhecida.

### 3. Modelação Dimensional

Criação e separação das tabelas de dimensão:

* `DIM_LOCALIDADE`;
* `DIM_TEMPO`;
* `DIM_VITIMA`;
* `DIM_OCORRENCIA`.

Durante essa etapa também são realizados:

* Remoção de duplicados;
* Geração de **Chaves Substitutas (Surrogate Keys)**;
* Criação de IDs sequenciais.

### 4. Cruzamento e Formação da Tabela Fato

Utilização de **Stream Lookup** para relacionar os registros às respectivas dimensões e agregação dos totais de vítimas para formação da tabela `FATO`.

### 5. Exportação

Os dados tratados e modelados são gravados em formato CSV na pasta `Saidas/`.

---

## 📐 Modelo de Dados — Star Schema

O modelo foi construído seguindo a arquitetura em **Estrela (Star Schema)** para facilitar a análise multidimensional e otimizar o desempenho analítico no Power BI.

### ⭐ FATO

| Campo           | Descrição                       |
| --------------- | ------------------------------- |
| `TOTAL_VITIMAS` | Total de vítimas                |
| `ID_LOCALIDADE` | Chave da dimensão de localidade |
| `ID_TEMPO`      | Chave da dimensão de tempo      |
| `ID_OCORRENCIA` | Chave da dimensão de ocorrência |
| `ID_VITIMA`     | Chave da dimensão de vítima     |

### 📍 DIM_LOCALIDADE

| Campo               | Descrição                   |
| ------------------- | --------------------------- |
| `ID_LOCALIDADE`     | Identificador da localidade |
| `MUNICIPIO`         | Município                   |
| `REGIAO_GEOGRAFICA` | Região geográfica           |

### 📅 DIM_TEMPO

| Campo        | Descrição             |
| ------------ | --------------------- |
| `ID_TEMPO`   | Identificador da data |
| `DATA`       | Data                  |
| `ANO`        | Ano                   |
| `MES`        | Mês                   |
| `DIA_MES`    | Dia do mês            |
| `DIA_SEMANA` | Dia da semana         |
| `TRIMESTRE`  | Trimestre             |

### 👤 DIM_VITIMA

| Campo          | Descrição               |
| -------------- | ----------------------- |
| `ID_VITIMA`    | Identificador da vítima |
| `SEXO`         | Sexo                    |
| `IDADE`        | Idade                   |
| `FAIXA_ETARIA` | Faixa etária            |

### ⚖️ DIM_OCORRENCIA

| Campo               | Descrição                   |
| ------------------- | --------------------------- |
| `ID_OCORRENCIA`     | Identificador da ocorrência |
| `NATUREZA_JURIDICA` | Natureza jurídica           |

---

## 🚀 Como Executar o Projeto

### 📋 Pré-requisitos

Antes de executar o projeto, certifique-se de possuir:

* **Pentaho Data Integration (Spoon)** versão **8.0 ou superior**;
* **Power BI Desktop**;
* **Git**, caso deseje clonar o repositório.

### 1. Clonar o Repositório

```bash
git clone https://github.com/ItaloKaua288/SAD-PowerBi-Mortes-Violentas-Intencionais-MVI.git
```

Entre na pasta do projeto:

```bash
cd SAD-PowerBi-Mortes-Violentas-Intencionais-MVI
```

### 2. Executar o ETL — Opcional

Caso seja necessário reproduzir ou atualizar o processo de tratamento dos dados:

1. Abra o **Pentaho Data Integration (Spoon)**;
2. Abra a transformação localizada na pasta `Pentaho/`;
3. Verifique e, se necessário, ajuste o caminho do ficheiro CSV de entrada;
4. Execute o pipeline de ETL;
5. Os ficheiros CSV de saída serão gerados/atualizados na pasta `Saidas/`.

### 3. Visualizar o Dashboard

Abra o ficheiro:

```text
SAD_MVI_Pernambuco.pbix
```

no **Power BI Desktop**.

Caso necessário, atualize a fonte de dados para apontar para os ficheiros localizados na pasta:

```text
Saidas/
```

---

## 📊 Dashboard

O projeto utiliza o **Power BI** para disponibilizar uma visão interativa dos dados de Mortes Violentas Intencionais em Pernambuco.

O modelo permite realizar análises considerando diferentes perspectivas, como:

* Evolução temporal das ocorrências;
* Distribuição por município;
* Distribuição por região geográfica;
* Perfil das vítimas;
* Sexo;
* Faixa etária;
* Natureza jurídica;
* Quantidade total de vítimas;
* Comparações entre diferentes períodos.

---

## 🔗 Fluxo da Solução

```text
                  ┌─────────────────────┐
                  │   Dados Brutos CSV  │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │       Pentaho       │
                  │   ETL / Tratamento  │
                  └──────────┬──────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │        Modelo Estrela        │
              │                              │
              │  DIM_LOCALIDADE              │
              │  DIM_TEMPO                   │
              │  DIM_VITIMA                  │
              │  DIM_OCORRENCIA              │
              │            │                 │
              │            ▼                 │
              │          FATO                │
              └──────────────┬───────────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │      Power BI       │
                  │     Dashboard       │
                  └─────────────────────┘
```

---

## 🎯 Objetivo do Projeto

O projeto busca demonstrar a aplicação prática de conceitos de:

* **Engenharia de Dados**;
* **Processos ETL**;
* **Modelagem Dimensional**;
* **Business Intelligence**;
* **Data Analytics**;
* **Visualização de Dados**;
* **Power BI**;
* **DAX**.

A partir de dados históricos, o objetivo é estruturar informações de segurança pública de forma que possam ser exploradas e interpretadas por meio de análises multidimensionais.

---

## 👨‍💻 Autor

Desenvolvido por **Ítalo Kauã**.

Sinta-se à vontade para enviar sugestões, relatar problemas ou contribuir com melhorias para o projeto.

---

## 📄 Licença

Este projeto está disponível para fins acadêmicos, educacionais e de demonstração.

Consulte a documentação e as condições de uso dos dados públicos utilizados como fonte antes de realizar qualquer redistribuição dos dados originais.
