# Gerador de Relatórios de Pregão e Saldo de Empenho

Automação em **R** e **Windows Batch Script** voltada à leitura, higienização e processamento de planilhas de controle de saldo de empenho (`.csv`), gerando relatórios tabulares em **PDF** compilados nativamente em **LaTeX**.

O fluxo segmenta automaticamente os itens licitados por cada número de pregão, separando itens com propostas regulares daqueles classificados como desertos, fracassados ou sem protocolo numérico SEI.

---

## Funcionalidades Principais

- **Execução com um clique (`.bat`):** Localiza automaticamente a instalação do R (`Rscript.exe`) no ambiente do sistema (PATH, Arquivos de Programas 64-bit e 32-bit) e abre a pasta de saída ao concluir.
- **Detecção dinâmica de cabeçalho:** Identifica a linha exata dos títulos das colunas na planilha bruta por sobreposição de palavras-chave, tolerando metadados ou linhas em branco no início do CSV.
- **Sanitização de caracteres para LaTeX:** Escape e tratamento rigoroso de caracteres especiais (`%`, `_`, `&`, `$`, `#`, `~`, `^`, `{`, `}`, `\`, além do ordinal `º` transformado para `.o`) para prevenir falhas de compilação do TeX.
- **Cálculo automático de aquisição média:** Limpeza de strings numéricas e formatações percentuais (conversão de vírgula para ponto decimal) para cálculo da média global por pregão.
- **Divisão automática de relatórios:**
  - **Relatório Principal (`Relatorio_pregao_[ID].pdf`):** Contém itens com número de protocolo SEI válido (numérico).
  - **Relatório de Desertos/Fracassados (`Relatorio_pregao_[ID]_deserto_fracassado.pdf`):** Contém itens com propostas vazias, textos descritivos ou protocolos não numéricos.
- **Auto-gerenciamento de pacotes:** Instala e carrega pacotes R necessários em diretório isolado do usuário (`%USERPROFILE%\R_packages`), evitando bloqueios por falta de privilégios de administrador.

---

## Estrutura do Repositório

```text
├── run_gerador.bat         # Script inicializador em lote para Windows
├── gerador.R               # Script principal em R (parsing, higienização e LaTeX)
└── README.md               # Documentação do projeto

## Pré-requisitos

* **Sistema Operacional:** Windows 10 ou superior.
* **R:** Versão 4.0 ou superior instalada ([Download CRAN](https://cran.r-project.org/bin/windows/base/)).[cite: 1]
* **Distribuição LaTeX:** TinyTeX (instalado automaticamente pelo script se ausente) ou MiKTeX/TeX Live configurado no sistema.[cite: 2]

### Pacotes R Utilizados

O próprio script se encarrega de verificar e baixar as bibliotecas faltantes na primeira execução:[cite: 2]

* `dplyr`[cite: 2]
* `tidyr`[cite: 2]
* `knitr`[cite: 2]
* `readr`[cite: 2]
* `stringr`[cite: 2]
* `janitor`[cite: 2]
* `tinytex`[cite: 2]
* `kableExtra`[cite: 2]

---

## Formato Esperado do Arquivo de Entrada (`.csv`)

O arquivo CSV deve utilizar o separador ponto e vírgula (`;`) e a codificação `latin1` (ou `Windows-1252`).[cite: 2] A detecção automática busca as seguintes colunas essenciais na planilha:[cite: 2]

| Coluna de Referência | Descrição |
| :--- | :--- |
| `Pregão` | Identificador do processo licitatório (ex: `90013/2025 RS`) |
| `Item` | Número identificador do item da licitação |
| `Solicitante(s)` | Unidade ou responsável solicitante |
| `Quantidade total LFDA-RS` | Quantidade demandada homologada |
| `Saldo atualizado` | Quantidade residual disponível para empenho |
| `Nº Ata` | Identificador da Ata de Registro de Preços |
| `Descrição conforme Termo de Referência...` | Especificação completa do objeto licitado |
| `Proposta (protocolo SEI)` | Protocolo numérico SEI ou status do item |
| `Porcentagem adquirida do item` | Percentual de execução/aquisição do saldo (ex: `100,00%` ou `0%`) |

