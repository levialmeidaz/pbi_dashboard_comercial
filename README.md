# 📊 Dashboard Comercial — Power BI

## 🧩 Visão Geral
Este projeto tem como objetivo o desenvolvimento de um **dashboard comercial interativo** em Power BI, conectado a uma base de dados em Excel. O foco é permitir o acompanhamento de **vendas, metas e desempenho por vendedor, cliente e produto**, com atualização facilitada e modelagem 100% construída em **Power Query**.

---

## 🗂️ Estrutura do Repositório

```
PBI_DASHBOARD_COMERCIAL/
│
├── assets/                      # Ícones SVG e tema visual do relatório
│   ├── home.svg
│   ├── pg1.svg
│   ├── pg2.svg
│   ├── pg3.svg
│   └── Tema.json
│
├── basedados/                   # Fontes de dados utilizadas
│   ├── 1.2.03-1464 VERSAO MES A MES (13).xlsx
│   └── metasvendedores.xlsx
│
├── dashboard_comercial_dec.Report/      # Definição do relatório (.pbir)
│
├── dashboard_comercial_dec.SemanticModel/  # Modelo semântico (.pbism)
│
├── pbi_dashboard_comercial.pbip  # Projeto principal do Power BI
│
└── .gitignore / .gitattributes   # Configurações do versionamento
```

---

## 🔗 Fontes de Dados

- **Arquivo Excel 1:** `1.2.03-1464 VERSAO MES A MES (13).xlsx`  
  Contém as tabelas **itens** e **pedidos**, utilizadas como base para a modelagem.  

- **Arquivo Excel 2:** `metasvendedores.xlsx`  
  Contém metas mensais por vendedor.  

As conexões são realizadas via **Power Query**, com **transformações e limpeza** feitas diretamente na interface do Power BI.

---

## 🧮 Modelagem de Dados

A modelagem segue o padrão **Star Schema** (modelo estrela):

### 🤳 Tabelas Fato
| Tabela | Descrição |
|--------|------------|
| `f_vendas` | Fato principal de vendas, originada da junção entre itens e pedidos |
| `f_metas` | Fato de metas comerciais mensais |

### 🤺 Tabelas Dimensão
| Tabela | Origem | Descrição |
|--------|--------|------------|
| `d_vendedor` | Derivada de `f_vendas` | Identificação dos vendedores |
| `d_cliente` | Derivada de `f_vendas` | Cadastro de clientes |
| `d_produto` | Derivada de `f_vendas` | Catálogo de produtos |
| `d_tipovenda` | Derivada de `f_vendas` | Tipos de venda (varejo, atacado etc.) |
| `d_tipopedido` | Derivada de `f_vendas` | Tipos de pedidos (normal, devolução etc.) |

> Todas as dimensões foram criadas **a partir da tabela fato** para garantir consistência e integridade entre as relações.

---

## 🤓 Transformações Principais (Power Query)

1. **Importação dos arquivos Excel**  
   - Definição de intervalos nomeados.  
   - Conversão de tipos de dados.  

2. **Tratamento de valores ausentes**  
   - Aplicação do recurso **“Preencher para baixo”** em colunas com lacunas.  
   - Substituição de nulos por valores padrão em colunas categóricas.  

3. **Criação das dimensões**  
   - Remoção de duplicatas (`Remover Duplicatas`).  
   - Seleção de colunas relevantes para chaves e descrições.  

4. **Criação de colunas calculadas e medidas DAX**  
   - Cálculo de **total de vendas**, **cumprimento de metas**, **ticket médio**, **crescimento mensal**.  

---

## 📊 Indicadores e Visuais

O dashboard apresenta:
- Total de vendas por mês e por vendedor.  
- Comparativo entre **vendas e metas**.  
- Ranking de **produtos mais vendidos**.  
- Análise de **clientes ativos e novos**.  
- Distribuição de **tipos de venda e pedidos**.  

---

## 🤊 Medidas DAX

Todas as medidas foram centralizadas na tabela **Medidas**, organizadas por categoria.

### **0. Padrão**
| Medida | Descrição |
|--------|------------|
| Clientes Positivados | Conta os clientes com pelo menos uma venda no período. |
| Descontos | Soma total de valores de desconto concedidos. |
| Faturamento | Soma total de vendas faturadas. |
| Pedidos Faturados | Quantidade de pedidos com status faturado. |
| Produtos Vendidos | Quantidade total de produtos vendidos. |
| Ticket Medio Cliente | Média de faturamento por cliente ativo. |
| Vendas Transmitidas | Soma de pedidos transmitidos (vendas enviadas para faturamento). |

### **1. Percentuais**
| Medida | Descrição |
|--------|------------|
| % Descontos | Percentual de descontos sobre o faturamento total. |
| % Meta | Percentual de cumprimento da meta de faturamento ou vendas. |
| %Meta_Texto | Versão formatada da %Meta para exibição textual em visuais (ex: “95% da meta”). |

### **2. Metas**
| Medida | Descrição |
|--------|------------|
| Meta | Valor total da meta vigente para o período. |
| Meta Clientes Positivados | Meta de número de clientes com venda registrada. |
| Meta Faturamento | Meta de faturamento mensal definida na planilha metasvendedores.xlsx. |

### **3. Temporais**
| Medida | Descrição |
|--------|------------|
| Melhor Mês Bruto | Mês com maior faturamento bruto. |
| Melhor Mês Líquido | Mês com maior faturamento líquido (após descontos). |
| Melhor Mês Pedidos | Mês com maior número de pedidos. |
| Meta QTD | Meta acumulada no trimestre atual (Quarter-To-Date). |
| Meta YTD | Meta acumulada no ano até a data (Year-To-Date). |

### **4. Dinâmicas**
| Medida | Descrição |
|--------|------------|
| ID Selecionado | Captura o identificador selecionado em visuais interativos. |
| Título Data | Gera dinamicamente o título do relatório com base na data filtrada (ex: “Vendas – Março/2025”). |

### 🔧 Boas Práticas nas Medidas
- Nomeação padronizada e agrupamento por função analítica.  
- Formatação personalizada (R$, %, inteiros).  
- Centralização em tabela única para fácil manutenção.  
- Documentação via descrições internas no Power BI.  

---

## ⚙️ Boas Práticas Gerais
- Modelagem em estrela para performance e clareza analítica.  
- Separar camadas (`basedados`, `SemanticModel`, `Report`).  
- Uso de medidas DAX centralizadas.  
- Versionamento Git para histórico e colaboração.  
- Tema JSON padronizado e ícones SVG para consistência visual.  

---

## 🚧 Desafios e Soluções

| Desafio | Solução |
|----------|------------|
| Lacunas de dados em colunas de pedidos | Utilização do recurso **Preencher para baixo** no Power Query |
| Conexão e atualização de múltiplos arquivos Excel | Parâmetros dinâmicos e caminhos relativos |
| Criação de dimensões derivadas | Extração via **Remover Duplicatas** e **Selecionar Colunas** |
| Performance e limpeza de modelo | Eliminação de colunas irrelevantes e cache incremental |

---

## 📚 Ferramentas Utilizadas
- Power BI Desktop (compatível com .pbip)  
- Power Query  
- Excel (Office 365)  
- GitHub e VS Code para versionamento  

---

## 🚀 Próximos Passos
- Automatizar atualização via Power BI Service.  
- Conectar futura base SQL Server para dados históricos.  
- Criar página de *insights automáticos* com DAX e IA Visuals.  

---

## 👤 Autor
**Nome:** Levi Almeida 
**Função:** Analista de Dados  
**Contato:** levi.dacruz23@gmail.com  |  (85) 9 8645-8805

---

**Licença:** Uso interno e educativo. Não distribuído comercialmente.

