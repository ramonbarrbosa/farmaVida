# REQUISITOS DO PROJETO - FARMAVIDA

- [x] Verificar as fontes de dados (`farmavida.db` e arquivos `.csv`);
- [x] Selecionar a melhor opção de ingestão (Importação otimizada no Power BI);
- [x] Realizar a exploração e validação de schema das tabelas;
- [x] Estruturar modelo estrela (Star Schema) com tabela dCalendario;
- [x] Desenvolver relatório executivo consolidado em **4 páginas** para otimização de tempo e entrega de alto impacto;
- [ ] Construir a documentação executiva e técnica de 10 páginas sobre o projeto.

---

## 1. ARQUITETURA DO DASHBOARD (PADRÃO 16:9 - CONSOLIDADO EM 4 PÁGINAS)

Cada página segue rigorosamente o padrão executivo **1 - 4 - 2 - 1**:
- **Faixa Superior:** Título, Contexto e Filtros Globais (Período, Região, Canal).
- **Faixa de KPIs (4 Cards):** Métricas-chave essenciais para tomada de decisão instantânea.
- **Área Central (2 Visuais de Diagnóstico):** Tendência temporal e composição de fatores.
- **Área Inferior (1 Tabela/Matriz de Ação):** Visão acionável com formatação condicional para priorização operacional.

---

### 📄 PÁGINA 1: VISÃO EXECUTIVA & RENTABILIDADE DE CANAIS
**Pergunta de Negócio:**
> *"Qual é a real saúde financeira da FarmaVida? Estamos crescendo com lucro sustentável? O canal digital (e-commerce) é rentável frente às lojas físicas?"*

#### 1. Filtros Globais
* `Ano / Mês` (dCalendario)
* `Canal` (Lojas[canal])
* `Região / UF` (Lojas[uf])

#### 2. Cards de KPI (4 Cards)
1. **Faturamento Líquido:** `[Faturamento Líquido]` com indicador YoY `% [Faturamento Líquido YoY]`
2. **Margem de Contribuição (R$):** `[Margem Contribuicao]`
3. **Margem de Contribuição (%):** `[% Margem Contribuicao]` com status/meta
4. **Devoluções:** `[Valor Devoluções]` e `[% Devoluções]`

#### 3. Visuais Principais (Área Central)
* **Visual 1 (Evolução Temporal):** Gráfico de Linha e Colunas Combinadas
  * *Eixo X:* Mês/Ano (`dCalendario[MesAno]`)
  * *Colunas:* `[Faturamento Líquido]`
  * *Linha:* `[% Margem Contribuicao]`
* **Visual 2 (Comparativo de Canais):** Gráfico de Barras Clusterizadas
  * *Eixo Y:* Canal (`Lojas[canal]`)
  * *Valores:* `[Faturamento Líquido]` e `[Margem Contribuicao]`

#### 4. Detalhamento para Ação (Área Inferior)
* **Matriz Hierárquica de Rentabilidade:**
  * *Linhas:* `Lojas[canal]` > `Lojas[uf]` > `Lojas[nome_loja]`
  * *Colunas:* `[Faturamento Bruto]`, `[Valor Descontos]`, `[Valor Devoluções]`, `[Faturamento Líquido]`, `[CMV]`, `[Margem Contribuicao]`, `[% Margem Contribuicao]` (com barras de dados na margem)

---

### 📄 PÁGINA 2: GESTÃO COMERCIAL, METAS & DESCONTOS
**Pergunta de Negócio:**
> *"Quem são os vendedores que realmente trazem margem vs. quem bate meta queimando margem com desconto? A política de descontos está sendo respeitada?"*

#### 1. Filtros
* `Ano / Mês`
* `Canal / Loja`
* `Tipo de Cliente` (Clientes[tipo_pessoa]: PF / PJ)

#### 2. Cards de KPI (4 Cards)
1. **Meta de Vendas:** `[Total Meta]`
2. **Realizado vs Meta:** `[Faturamento Líquido]` e `[% Atingimento Meta]`
3. **Desconto Total Concedido:** `[Valor Descontos]` e `[% Desconto Médio]`
4. **Furos de Política:** `[% Vendas Fora da Política]` (Alerta se desconto > teto da política comercial)

#### 3. Visuais Principais (Área Central)
* **Visual 1 (Atingimento por Vendedor):** Gráfico de Barras Horizontais
  * *Eixo Y:* `Vendedores[nome_vendedor]` (Top 10 ou Geral)
  * *Eixo X:* `[Faturamento Líquido]` com Linha de Referência `[Total Meta]`
* **Visual 2 (Elasticidade e Volume por Faixa de Desconto):** Gráfico de Colunas
  * *Eixo X:* Faixa de Desconto (`0%`, `1-5%`, `6-10%`, `11-15%`, `>15%`)
  * *Valores:* `[Quantidade Vendida]` e `[% Margem Contribuicao]`

#### 4. Detalhamento para Ação (Área Inferior)
* **Tabela de Desempenho e Qualidade de Vendas:**
  * *Colunas:* `Vendedor`, `Loja`, `Meta`, `Realizado`, `% Atingimento`, `% Desconto Médio`, `Margem Gerada R$`, `% Margem Contribuicao` *(Formatação condicional semáforo)*

---

### 📄 PÁGINA 3: INTELIGÊNCIA DE CLIENTES & MIX DE PRODUTOS
**Pergunta de Negócio:**
> *"Temos dependência de poucos clientes corporativos (Curva Pareto)? Quais categorias e tipos de medicamentos sustentam o resultado (Marca Própria vs Genéricos)?"*

#### 1. Filtros
* `Tipo de Cliente` (PF / PJ)
* `Categoria de Produto` (`Produtos[categoria]`)
* `Tipo Medicamento` (`Produtos[tipo_medicamento]`)

#### 2. Cards de KPI (4 Cards)
1. **Clientes Ativos:** `[Qtd Clientes Ativos]`
2. **Ticket Médio:** `[Ticket Médio]`
3. **Concentração Top 10 Clientes:** `[% Fat Top 10 Clientes]`
4. **Mix Marca Própria:** `[% Faturamento Marca Própria]` e `[% Margem Marca Própria]`

#### 3. Visuais Principais (Área Central)
* **Visual 1 (Curva de Pareto de Clientes):** Gráfico de Linha e Colunas
  * *Colunas:* Clientes ordenados por faturamento decrescente
  * *Linha:* `% Acumulado de Faturamento` (Linha de corte nos 80%)
* **Visual 2 (Rentabilidade do Portfólio):** Gráfico de Barras Clusterizadas
  * *Eixo Y:* `Produtos[categoria]` ou `Produtos[tipo_medicamento]`
  * *Eixo X:* `[% Margem Contribuicao]` e `[Faturamento Líquido]`

#### 4. Detalhamento para Ação (Área Inferior)
* **Tabela de Clientes Críticos / Produtos em Risco:**
  * *Linhas:* Clientes PJ com maior faturamento e histórico de descontos, ou produtos com alta venda e margem deprimida.

---

### 📄 PÁGINA 4: DETALHAMENTO TRANSACIONAL & AUDITORIA
**Pergunta de Negócio:**
> *"Como auditar um pedido específico, filial ou operação para investigação pontual ou conferência executiva na ponta do lápis?"*

#### 1. Filtros Avançados
* `Período Completo`, `Canal`, `Loja`, `Vendedor`, `Cliente`, `Tipo Medicamento`

#### 2. Cards Resumo de Seleção (3 Cards)
1. **Qtd Pedidos Selecionados:** `[Total Pedidos]`
2. **Receita Selecionada:** `[Faturamento Líquido]`
3. **Margem Média Selecionada:** `[% Margem Contribuicao]`

#### 3. Visual Principal (Full-Width)
* **Tabela Analítica para Drill-Through / Exportação:**
  * *Campos:* `ID Pedido` | `Data` | `Loja` | `Canal` | `Vendedor` | `Cliente` | `Produto` | `Qtd` | `Preço Unit.` | `Desc. (%)` | `Fat. Líquido` | `Margem R$` | `Margem %`

---

## 2. DICIONÁRIO CONTEXTUALIZADO DE MEDIDAS DAX (NEGÓCIO & SINTAXE)

---

### BLOCO 1: FATURAMENTO, DESCONTOS E RECEITA LÍQUIDA

#### Medida 1: Faturamento Bruto
* **Contexto de Negócio:** Representa o valor total movimentado "a preço de tabela/etiqueta", antes de qualquer desconto promocional ou devolução. Responde: *"Qual o tamanho do nosso faturamento potencial?"*.
* **Lógica e Sintaxe:** A tabela `vendas` não possui uma coluna com o valor total por linha. Usa-se a função iteradora `SUMX`, que multiplica linha a linha `quantidade * valor_unitario` e depois soma todo o resultado do contexto de filtro.
```dax
Faturamento Bruto = 
SUMX(
    vendas, 
    vendas[quantidade] * vendas[valor_unitario]
)
```

#### Medida 2: Valor Total de Descontos
* **Contexto de Negócio:** Mensura em reais o volume financeiro concedido em reduções de preço. Essencial para verificar se a equipe comercial está corroendo a rentabilidade para bater metas.
* **Lógica e Sintaxe:** A coluna `perc_desconto` contém valores percentuais inteiros (ex: 10 para 10%). A fórmula divide esse campo por 100 para aplicar a alíquota correta sobre o valor bruto de cada item vendido.
```dax
Valor Descontos = 
SUMX(
    vendas, 
    vendas[quantidade] * vendas[valor_unitario] * (vendas[perc_desconto] / 100)
)
```

#### Medida 3: % Médio de Desconto Praticado
* **Contexto de Negócio:** Indicador de governança comercial. Mede o percentual médio de desconto que incide sobre a receita bruta total.
* **Lógica e Sintaxe:** Utiliza `DIVIDE` em vez da barra `/`. A função `DIVIDE(numerador, denominador, 0)` trata divisões por zero com segurança, evitando falhas em períodos sem movimentação.
```dax
% Desconto Médio = 
DIVIDE([Valor Descontos], [Faturamento Bruto], 0)
```

#### Medida 4: Valor Total de Devoluções
* **Contexto de Negócio:** Estorna da receita o impacto de produtos devolvidos por avaria, erro de pedido ou desistência do cliente.
* **Lógica e Sintaxe:** Itera sobre a tabela `devolucoes`. Busca o preço unitário e o desconto original da venda na tabela relacionada `vendas`, garantindo que o valor estornado seja exatamente o valor líquido pago pelo cliente.
```dax
Valor Devoluções = 
SUMX(
    devolucoes,
    VAR _VendaID = devolucoes[id_venda]
    VAR _PrecoUnitario = LOOKUPVALUE(vendas[valor_unitario], vendas[id_venda], _VendaID)
    VAR _PercDesconto = LOOKUPVALUE(vendas[perc_desconto], vendas[id_venda], _VendaID)
    RETURN
    devolucoes[quantidade_devolvida] * _PrecoUnitario * (1 - (_PercDesconto / 100))
)
```

#### Medida 5: Faturamento Líquido (Receita Líquida Real)
* **Contexto de Negócio:** É o dinheiro real que entra no caixa da FarmaVida. É a métrica oficial sobre a qual incidem o CMV, os custos variáveis e a margem de contribuição.
* **Lógica e Sintaxe:** Subtração direta reaproveitando as medidas bases já calculadas (medidas encadeadas mantêm a integridade contábil do relatório).
```dax
Faturamento Líquido = 
[Faturamento Bruto] - [Valor Descontos] - [Valor Devoluções]
```

#### Medida 6: Quantidade Vendida e Pedidos
* **Contexto de Negócio:** Indicadores operacionais de volume físico e número de notas/pedidos emitidos.
```dax
Quantidade Vendida = 
SUM(vendas[quantidade])

Total Pedidos = 
DISTINCTCOUNT(vendas[num_nota_fiscal])
```

#### Medida 7: Ticket Médio
* **Contexto de Negócio:** Receita média gasta por cada pedido finalizado. Ajuda a identificar se o crescimento da receita é puxado por volume de clientes ou por compras maiores.
```dax
Ticket Médio = 
DIVIDE([Faturamento Líquido], [Total Pedidos], 0)
```

---

### BLOCO 2: CUSTOS, MARGEM DE CONTRIBUIÇÃO E RENTABILIDADE

#### Medida 8: Custo das Mercadorias Vendidas (CMV)
* **Contexto de Negócio:** Aponta quanto a FarmaVida pagou à indústria/fornecedores para adquirir os produtos comercializados no período.
* **Lógica e Sintaxe:** Como o custo na tabela `custo_produto` varia historicamente mês a mês (`ano` e `mes`), a fórmula identifica o ano e o mês da venda e busca o custo unitário correspondente via `LOOKUPVALUE`.
```dax
CMV = 
SUMX(
    vendas,
    VAR _Ano = YEAR(vendas[data_venda])
    VAR _Mes = MONTH(vendas[data_venda])
    VAR _ProdRef = RELATED(produtos[cod_produto])
    VAR _CustoUnitario = 
        LOOKUPVALUE(
            custo_produto[custo_unitario],
            custo_produto[ano], _Ano,
            custo_produto[mes], _Mes,
            custo_produto[cod_produto_ref], _ProdRef
        )
    RETURN
    vendas[quantidade] * COALESCE(_CustoUnitario, 0)
)
```
> 💡 *Dica de Performance:* Caso prefira acelerar o processamento, mescle a coluna de custo unitário diretamente na tabela `vendas` no Power Query utilizando a chave composta `cod_produto + ano + mes`.

#### Medida 9: Margem de Contribuição (R$)
* **Contexto de Negócio:** É a sobra financeira gerada pelas vendas para cobrir as despesas fixas da empresa e gerar lucro líquido. Indica a contribuição efetiva de cada unidade de negócio.
```dax
Margem Contribuicao = 
[Faturamento Líquido] - [CMV]
```

#### Medida 10: % Margem de Contribuição
* **Contexto de Negócio:** É o percentual de rentabilidade de cada produto, categoria ou canal. Revela a eficiência operacional: cada R$ 100 vendidos geram quanto de margem?
```dax
% Margem Contribuicao = 
DIVIDE([Margem Contribuicao], [Faturamento Líquido], 0)
```

#### Medida 11: % Devoluções sobre o Faturamento
* **Contexto de Negócio:** Mede a taxa de perda por devolução. Se estiver acima de 2-3%, acende o alerta de qualidade logística ou insatisfação de clientes.
```dax
% Devoluções = 
DIVIDE([Valor Devoluções], [Faturamento Bruto], 0)
```

---

### BLOCO 3: INTELIGÊNCIA TEMPORAL E CRESCIMENTO (YoY)

#### Medida 12: Faturamento Líquido Ano Anterior (LY)
* **Contexto de Negócio:** Base comparativa para aferir crescimento sustentável ano contra ano (eliminando sazonalidades).
* **Lógica e Sintaxe:** `CALCULATE` aplica a função de Time Intelligence `SAMEPERIODLASTYEAR` sobre a dimensão de calendário `dCalendario[Date]`.
```dax
Faturamento Líquido LY = 
CALCULATE(
    [Faturamento Líquido],
    SAMEPERIODLASTYEAR(dCalendario[Date])
)
```

#### Medida 13: Crescimento Anual (% YoY)
* **Contexto de Negócio:** Taxa percentual de expansão da empresa em relação ao mesmo período do ano anterior.
```dax
% Faturamento Líquido YoY = 
DIVIDE([Faturamento Líquido] - [Faturamento Líquido LY], [Faturamento Líquido LY], 0)
```

---

### BLOCO 4: GESTÃO COMERCIAL, METAS E GOVERNANÇA

#### Medida 14: Meta Total de Vendas
* **Contexto de Negócio:** O orçamento de vendas estipulado pela diretoria para os colaboradores e lojas.
```dax
Total Meta = 
SUM(metas_vendedor[meta_receita_liquida])
```

#### Medida 15: % Atingimento da Meta
* **Contexto de Negócio:** Revela o percentual de cumprimento da meta planejada (`>= 100%` indica meta batida).
```dax
% Atingimento Meta = 
DIVIDE([Faturamento Líquido], [Total Meta], 0)
```

#### Medida 16: Volume e % de Vendas Fora da Política Comercial
* **Contexto de Negócio:** A política comercial estipula tetos de desconto (exemplo: 15% para loja física e 20% para e-commerce). Transações acima desse teto ferem a governança e destroem a margem.
* **Lógica e Sintaxe:** `CALCULATE` com `FILTER` isola apenas as transações com desconto acima da política e divide pela quantidade total de transações.
```dax
Qtd Vendas Fora Politica = 
CALCULATE(
    COUNTROWS(vendas),
    vendas[perc_desconto] > 15
)

% Vendas Fora da Política = 
DIVIDE([Qtd Vendas Fora Politica], COUNTROWS(vendas), 0)
```

---

### BLOCO 5: CLIENTES, CONCENTRAÇÃO E MIX DE PRODUTOS

#### Medida 17: Clientes Ativos
* **Contexto de Negócio:** Quantidade de clientes únicos que compraram no período filtrado.
```dax
Qtd Clientes Ativos = 
DISTINCTCOUNT(vendas[cod_cliente])
```

#### Medida 18: Faturamento e Mix de Marca Própria (%)
* **Contexto de Negócio:** Produtos de Marca Própria (marca FarmaVida) oferecem margem superior aos produtos de grandes laboratórios multinacionais. A diretoria precisa ver a penetração dessa linha no faturamento total.
```dax
// Faturamento absoluto da Marca Própria
Faturamento Marca Própria = 
CALCULATE(
    [Faturamento Líquido],
    FILTER(produtos, LOWER(produtos[marca]) = "farmavida")
)

// Representatividade percentual no faturamento
% Faturamento Marca Própria = 
DIVIDE([Faturamento Marca Própria], [Faturamento Líquido], 0)
```

---

## 3. ROTEIRO DE MODELAGEM E EXECUÇÃO NO POWER BI

1. **Tabela de Calendário (dCalendario):**
   * Criar via DAX cobrindo o período dos dados:
     ```dax
     dCalendario = 
     ADDCOLUMNS(
         CALENDAR(DATE(2023, 1, 1), DATE(2024, 12, 31)),
         "Ano", YEAR([Date]),
         "MesNum", MONTH([Date]),
         "MesNome", FORMAT([Date], "mmm", "pt-BR"),
         "MesAno", FORMAT([Date], "mmm/yyyy", "pt-BR"),
         "AnoMesInt", YEAR([Date]) * 100 + MONTH([Date])
     )
     ```
   * Classificar a coluna `MesNome` pela coluna `MesNum`.
   * Marcar como Tabela de Data oficial.

2. **Relacionamentos no Modelo Estrela (Star Schema):**
   * `dCalendario[Date]` (1) ➔ `vendas[data_venda]` (N)
   * `lojas[cod_loja]` (1) ➔ `vendas[cod_loja]` (N)
   * `vendedores[cod_vendedor]` (1) ➔ `vendas[cod_vendedor]` (N)
   * `clientes[cod_cliente]` (1) ➔ `vendas[cod_cliente]` (N)
   * `produtos[cod_produto]` (1) ➔ `vendas[cod_produto]` (N)
   * `vendas[id_venda]` (1) ➔ `devolucoes[id_venda]` (N)
   * `dCalendario` e `vendedores` relacionados à tabela `metas_vendedor`.

3. **Garantia de Entrega Ágil (Padrão de Layout):**
   * Monte a estrutura da **Página 1** (Cabeçalho com logo, 4 Cards KPI, 2 visuais centrais e 1 matriz inferior).
   * Utilize a paleta institucional (Azul institucional `#005088`, Verde rentabilidade `#2E7D32`, Vermelho alerta `#C62828`, Cinza neutro `#F4F5F7`).
   * Duplique a página para as Páginas 2, 3 e 4, reaproveitando alinhamentos, fontes e formatações.
