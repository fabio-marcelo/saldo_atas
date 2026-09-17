# Gerador de Relatórios de Saldo de empenhos não disponíveis

Automação em **R** com disparador em lote (**Batch/Windows**) para processamento de planilhas de saldo de empenho/atas e compilação direta de relatórios consolidados em **PDF (via LaTeX)**.

---

## 📋 Sumário
- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades Principais](#funcionalidades-principais)
- [Requisitos de Software](#requisitos-de-software)
- [Estrutura de Arquivos do Repositório](#estrutura-de-arquivos-do-repositório)
- [Como Baixar e Preparar a Planilha CSV](#como-baixar-e-preparar-a-planilha-csv)
- [Como Executar](#como-executar)
- [Estrutura dos Relatórios Gerados](#estrutura-dos-relatórios-gerados)

---

## 📌 Sobre o Projeto

Este projeto automatiza a leitura, validação e compilação dos saldos e percentuais adquiridos de itens registrados em **Atas de Registro de Preços**. A partir de uma exportação bruta em CSV (SALDO DE EMPENHO-SOLICITACAO e CONTROLE  - FOR_DLAB_021 E FOR_DLAB_007(Saldo não disponível )), a ferramenta organiza os registros por cada par único de **Pregão **, gerando relatórios em formato PDF individuais através do motor LaTeX.

A execução foi desenhada para ser simples para o usuário final: basta um duplo clique no script `executar_relatorios.bat` para detectar o ambiente R, abrir a janela de seleção da planilha e abrir a pasta de PDFs ao término da geração.

---

## 🚀 Funcionalidades Principais

- **Execução com 1 Clique (`executar_relatorios.bat`):** Localiza automaticamente a instalação do R no sistema (PATH, Arquivos de Programas 64-bits ou 32-bits) e inicia o pipeline.
- **Gerenciamento Autônomo de Dependências:** Instala pacotes faltantes do CRAN em pasta segura do usuário (`~/R_packages`), contornando limitações de permissão de administrador no Windows.
- **Instalação Automática de LaTeX:** Detecta `pdflatex` no sistema ou baixa e configura o `TinyTeX` de forma transparente caso não exista.
- **Detecção Inteligente de Cabeçalho:** Localiza a linha correta dos títulos no CSV mesmo que o arquivo venha acompanhado de linhas em branco ou textos informativos no início.
- **Mapeamento de UASG & Sanitização:** Identifica a UASG pelo padrão do Pregão (RS, SP, MG, PA, PE, GO) e higieniza caracteres reservados do LaTeX e barras (`/`) em nomes de arquivos.
- **Abertura Automática:** Ao finalizar sem erros, o executável abre diretamente a pasta `Relatorios_PDF` no Windows Explorer.

---

## 💻 Requisitos de Software

| Software | Requisito Mínimo | Descrição |
| :--- | :--- | :--- |
| **Sistema Operacional** | Windows 10 ou 11 | Compatível com o script `executar_relatorios.bat` |
| **R** | Versão `>= 4.1.0` | [Download oficial do R para Windows](https://cran.r-project.org/bin/windows/base/)[cite: 1] |
| **Motor LaTeX** | `TinyTeX` ou `MiKTeX` | Não é necessário pré-instalar: o script instala o `TinyTeX` se não encontrar |

### Pacotes do R Utilizados
O próprio script instala e carrega automaticamente os seguintes pacotes do CRAN:
* `dplyr` e `tidyr`
* `readr` e `stringr`
* `knitr` e `kableExtra`
* `tinytex`
* `janitor`

---

## 📁 Estrutura de Arquivos do Repositório

Certifique-se de manter os seguintes arquivos na mesma pasta[cite: 1]:

```text
gerador-saldo-atas/
│
├── executar_relatorios.bat   # Script batch disparador[cite: 1]
└── gerador.R                 # Script R de processamento e compilação LaTeX
