
# REQUISITOS #

- Verificar as fontes de dados ou arquivo.db; ok
- Escolhar qual das opções utilizar (Arquivos .csv ou conexão no banco via odbc); 
- Realizar a exploração de dados na base;
- Entender o desafio em questão;
- Desenvolver o relatório com as devidas análises que respondam as perguntas da diretoria;
- No final construir uma documentação de 10 páginas sobre o projeto em questão.

## DESENVOLVIMENTO

Para estruturar o relatório de forma profissional no Power BI (padrão 16:9), cada página precisa funcionar como uma etapa da tomada de decisão da diretoria, dividida em um padrão visual consistente:

Faixa Superior (Cabeçalho & Filtros): Contexto temporal e filtros globais.
Primeira Faixa (Cards de KPI): Números consolidados de alto impacto com comparações (YoY, vs Meta).
Área Central (Análise Diagnóstica / Gráficos Principais): O "porquê" dos números (tendência, composição e
correlação).

Área Inferior (Detalhamento para Ação / Matrizes & Tabelas): Visão acionável para drill-down e plano de
intervenção.

Abaixo está o detalhamento estruturado de cada uma das 6 páginas: ──────

📄 Página 1: Visão Geral Executiva (Executive Overview)
#### 1. O que responde à Diretoria?

│ "Qual é a real saúde financeira da FarmaVida? Estamos crescendo com lucro sustentável ou apenas inflando │ faturamento enquanto a margem de contribuição é corroída?"

#### 2. Métricas Principais

• Faturamento Bruto ($): SUM(Vendas[Valor_Bruto]) 🆗
• Descontos Concedidos ($ e %): SUM(Vendas[Valor_Desconto]) e [Descontos] / [Fat Bruto] 🆗
• Devoluções ($): SUM(Devolucoes[Valor_Devolvido]) 🆗 
• Faturamento Líquido ($): [Fat Bruto] - [Descontos] - [Devolucoes] 🆗
• CMV ($ e %): Custo das Mercadorias Vendidas 🆗
• Margem de Contribuição (R$ e %): [Fat Líquido] - [CMV] - [Despesas Variáveis / Comissões] 
• Ticket Médio ($): [Fat Líquido] / DISTINCTCOUNT(Vendas[Id_Pedido]) 🆗
• Variações YoY (%): Comparação contra o mesmo período do ano anterior. 🆗

#### 3. Organização Espacial (Wireframe de Layout)

[Logo FarmaVida] 
title: VISÃO GERAL EXECUTIVA [Ano] [Mês] [Canal] [Região] 

> CARDS:
    [Card 1] Fat. Bruto 
    [Card 2] Fat. Líquido 
    [Card 3] Margem R$ 
    [Card 4] MC %
    [Card 5] Devoluções │ │ R$ XX M (+12% YoY) │ R$ XX M (+8% YoY) │ R$ XX M (-3% YoY) │ XX% (Alerta) │ R$ XX k (X% Fat) │ 

> GRÁFICOS:
[Gráfico 1: DRE em Cascata / Waterfall]
[Gráfico 2: Evolução Temporal] Faturamento Bruto ➔ (-) Descontos ➔ │ Linha 1: Faturamento Líquido 
(-) Devoluções ➔ (-) CMV ➔ (-) Comissões ➔ │ Linha 2: Margem de Contribuição % 
Margem de Contribuição Final │ (Eixo X: Meses do Ano)
[Gráfico 3: Mix por Canal - Barras 100%]
[Gráfico 4: Top 5 Categorias]
Participação Fat. Líquido vs. Margem % 
Ranking de categorias com maior
(Loja Física vs. E-commerce)
contribuição de margem líquida

📄 Página 2: Rentabilidade por Canal & Unidades de Negócio
#### 1. O que responde à Diretoria?

│ "O canal digital (e-commerce) é financeiramente sustentável frente às lojas físicas? Quais unidades/CDs dão lucro │ real e quais queimam caixa?"

#### 2. Métricas Principais

• Margem de Contribuição por Canal (R$ e %): Demonstrar o peso do frete/plataforma no e-commerce vs. custo operacional das lojas. • Custo Logístico/Frete por Canal ($): Despesa variável associada às entregas do e-commerce. • Faturamento por Loja / Região: SUM(Vendas[Valor_Liquido]) agrupado por Lojas[Nome_Loja]. • Crescimento YoY por Unidade (%): Taxa de expansão individual de cada unidade física e CD.

#### 3. Organização Espacial (Wireframe de Layout)

│ FILTROS: [Canal: Todos/Físico/Online] [Estado/UF] [Centro de Distribuição / Loja] │ │ [Bloco KPI 1: Lojas Físicas] │ [Bloco KPI 2: E-commerce] │ │ Fat: R$ XX M | MC: R$ XX M (MC%: 42%) │ Fat: R$ XX M | MC: R$ XX M (MC%: 18%) │ │ [Visual 1: Mapa Geográfico de Rentabilidade]│ [Visual 2: Dispersão (Scatter Plot)]│ │ • Tamanho da Bolha = Faturamento Líquido │ • Eixo X: Crescimento YoY (%) │ │ • Cor da Bolha = Margem de Contribuição % │ • Eixo Y: Margem de Contribuição % │ │ (Permite identificar cidades deficitárias) │ • Bolhas = Lojas (Quadrantes) │ │ [Visual 3: Matriz Hierárquica DRE de Unidades de Negócio] │ │ Região > Canal > Unidade/Loja │ Fat. Bruto │ Devoluções │ CMV │ Frete │ MC R$ │ MC % │ │ (Com Data Bars na coluna de Margem para destacar anomalias instantaneamente) │ ──────

📄 Página 3: Política Comercial, Descontos & Elasticidade
#### 1. O que responde à Diretoria?

│ "Conceder descontos maiores gerou ganho de volume compensatório ou apenas destruiu rentabilidade? O time respeitou │ os tetos da política (15% loja, 20% e-commerce, 12% PJ)?"

#### 2. Métricas Principais

• Desconto Total Concedido ($): SUM(Vendas[Valor_Desconto]) • % Médio de Desconto Praticado: Média ponderada do desconto. • Transações Fora da Política (% e R$): Volume e valor transacionado acima do teto estipulado. • Margem Deixada na Mesa ($): Simulação de ganho caso o teto fosse respeitado. • Elasticidade Volume vs. Desconto: Relação entre % de desconto aplicado e unidades vendidas.

#### 3. Organização Espacial (Wireframe de Layout)

│ FILTROS: [Canal] [Categoria] [Tipo de Cliente: PF / PJ] [Faixa de Desconto] │ │ [Card 1] Desconto │ [Card 2] % Médio │ [Card 3] Transações │ [Card 4] Margem │ │ Total: R$ XX k │ Desconto: XX% │ Fora Política: XX% │ Queimada: R$ XX k │ │ [Visual 1: Histograma por Faixa de Desconto] │ [Visual 2: Dispersão Desconto x MC]│ │ Barras: Volume Vendido por faixa │ Eixo X: % Desconto Aplicado │ │ (0%, 1-5%, 6-10%, 11-15%, >15%) │ Eixo Y: Margem de Contribuição % │ │ Linha: Margem de Contribuição % │ Mostra se desconto alto traz lucro real │ │ [Visual 3: Aderência à Política por Canal] │ [Visual 4: Simulador What-If Teto] │ │ Barras empilhadas (% Dentro x % Fora) │ Parâmetro deslizante (Slider): │ │ Linha de meta com teto permitido │ "Se o teto fosse 10%, ganho de MC:" │ ──────

📄 Página 4: Gestão da Força de Vendas & Metas
#### 1. O que responde à Diretoria?

│ "Quem são os vendedores que realmente geram caixa vs. quem bate meta 'queimando margem' com desconto máximo? As │ metas de vendas foram calibradas com justiça?"

#### 2. Métricas Principais

• Meta de Vendas ($): SUM(Metas[Valor_Meta]) • Realizado ($): SUM(Vendas[Valor_Liquido]) • % Atingimento de Meta: [Realizado] / [Meta] • Margem Gerada pelo Vendedor (R$ e %): Qualidade da receita entregue pelo colaborador. • Desconto Médio por Vendedor (%): Aferir se o vendedor depende de desconto para fechar vendas. • Comissão Paga ($): Cálculo vinculado à tabela de comissões.

#### 3. Organização Espacial (Wireframe de Layout)

│ FILTROS: [Ano/Mês] [Loja / Canal] [Supervisor / Equipe] │ │ [Card 1] Meta Total │ [Card 2] Realizado │ [Card 3] Ating. % │ [Card 4] Comissão │ │ R$ XX M │ R$ XX M │ XX% (Geral) │ Total: R$ XX k │ │ [Visual 1: Ranking Realizado vs. Meta] │ [Visual 2: Matriz 4 Quadrantes Vendedor]│ │ Gráfico de Barras Bullet por Vendedor │ Eixo X: % Atingimento da Meta │ │ Linha indicadora = 100% da Meta │ Eixo Y: Margem de Contribuição % │ │ Cores: Verde (Atingiu) / Vermelho (Não) │ Q1: Alta MC + Bateu Meta (Estrelas) │ │ │ Q2: Baixa MC + Bateu Meta (Alerta!) │ │ [Visual 3: Tabela de Desempenho Individual] │ │ Vendedor │ Meta │ Realizado │ % Ating. │ Desc. Médio │ MC Gerada (R$) │ MC % │ Com. │ ──────

📄 Página 5: Inteligência de Clientes, Concentração & Retenção
#### 1. O que responde à Diretoria?

│ "Temos risco de dependência excessiva em poucos clientes corporativos (PJ)? Os clientes novos voltam a comprar ou │ há churn acelerado?"

#### 2. Métricas Principais

• Clientes Ativos: Clientes com compra no período selecionado. • Concentração Pareto (80/20): % do faturamento gerado pelo top 10% ou 20% de clientes. • Segmentação RFV: Recência (dias desde a última compra), Frequência (qtd pedidos) e Valor (faturamento total acumulado). • Taxa de Retenção de Coortes (%): Clientes que compraram no mês M₀ e continuam ativos em M₁, M₂, M₃ …. • Base Inativa (>60/90 dias): Clientes dormentes para ação de reativação.

#### 3. Organização Espacial (Wireframe de Layout)

│ FILTROS: [Tipo de Cliente: PF / PJ] [Segmento RFV] [Canal de Compra] │ │ [Card 1] Clientes │ [Card 2] Ticket │ [Card 3] Pareto │ [Card 4] Clientes │ │ Ativos: XX.XXX │ Médio: R$ XXX │ Top 10 Clientes:X%│ Inativos: X.XXX │ │ [Visual 1: Curva de Pareto ABC de Clientes] │ [Visual 2: Segmentação RFV (Treemap)]│ │ Colunas: Receita acumulada por cliente │ Blocos: Campeões, Fiéis, Potenciais, │ │ Linha: % Acumulado (revela dependência) │ Em Risco, Inativos │ │ [Visual 3: Matriz de Coortes (Cohort Retenção)] │ [Visual 4: Tabela de Ação CRM] │ │ Mês Cadastro │ M0 │ M1 │ M2 │ M3 │ M4 │ Top Clientes Em Risco/Inativos │ │ Com gradiente de calor (% clientes que retornam)│ Cliente | Últ. Compra | Histórico│ ──────

📄 Página 6: Mix de Produtos & Oportunidades de Portfólio
#### 1. O que responde à Diretoria?

│ "Quais categorias e tipos de medicamentos sustentam a rentabilidade (Marca Própria e Genéricos vs. Referência)? │ Quais produtos devem ser promovidos juntos (Cross-sell)?"

#### 2. Métricas Principais

• Margem por Tipo de Medicamento: Comparativo Genérico vs. Referência vs. Similar. • Penetração de Marca Própria (%): Representatividade da marca própria FarmaVida no volume e na margem total. • Matriz BCG de Produtos: Classificação em Estrelas (Alto volume / Alta margem), Vacas Leiteiras (Alto volume / Baixa margem), etc. • Regras de Associação / Cesta de Compras: Produtos com maior co-ocorrência no mesmo pedido.

#### 3. Organização Espacial (Wireframe de Layout)

│ FILTROS: [Categoria] [Tipo: Genérico/Ref/Similar] [Marca Própria: Sim/Não] │ │ [Card 1] Faturam. │ [Card 2] Margem │ [Card 3] Mix % │ [Card 4] Mix % │ │ Portfólio: R$ XX M │ Portfólio: XX% │ Marca Própria: X% │ Genéricos: X% │ │ [Visual 1: Margem Marca Própria x Terceiro] │ [Visual 2: Matriz BCG de Portfólio]│ │ Barras clusterizadas comparando: │ Eixo X: Volume de Vendas │ │ • Genéricos vs. Referência │ Eixo Y: Margem de Contribuição % │ │ • Marca Própria vs. Terceiros │ Bolhas: Produtos/Subcategorias │ │ [Visual 3: Sazonalidade das Categorias] │ [Visual 4: Matriz de Cross-Sell] │ │ Gráficos múltiplos (Small Multiples) │ Tabela/Heatmap de Co-ocorrência │ │ Evolução mês a mês por categoria │ "Quem comprou Produto A comprou B" │ ──────

💡 Recomendações de Interatividade no Power BI
Drill-Through Estratégico:
• Configurar botão de drill-through da Página 1 (Mix por Canal) ou Página 2 direto para a Página 4 (Vendedores daquela loja).

Tooltips Personalizados (Dica de Ferramenta em Página):
• Ao passar o mouse sobre uma loja no mapa (Pág. 2) ou sobre um vendedor (Pág. 4), abrir um mini-card com o histórico dos últimos 3 meses e a taxa de devolução.

Formatação Condicional Semáforo:
• Margem de contribuição < 20% em vermelho suave, 20%–35% em amarelo, > 35% em verde em todas as matrizes.

