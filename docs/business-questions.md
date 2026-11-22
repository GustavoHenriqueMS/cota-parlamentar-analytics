# Business Questions

## Público-alvo
Cidadão ou jornalista que quer entender como a cota parlamentar é usada.

## Escopo
- Dentro: despesas da cota parlamentar.
- Fora (por enquanto): votações, proposições, presença.

## Definições globais
- **Valor de uma despesa:** valor líquido (`valorLiquido`).
- **Granularidade dos dados:** uma linha = um documento de despesa de um deputado.
- **Valores nominais:** sem correção pela inflação.
- **Deputado em exercício no mês:** a definir na fase 3.
- **Período de dados do projeto:** a definir na fase 4.

## Perguntas

### Q1 — Tipos de despesa mais caros
- **Pergunta:** Quais tipos de despesa custam mais por ano?
- **Tipo:** ranking
- **Métrica:** valor total
- **Dimensão:** tipo de despesa
- **Período:** anual
- **Visual sugerido:** barras horizontais, com seletor de ano
- **Definições e ressalvas:** valor conforme definições globais.
- **Campos da API:** a preencher na fase 2
- **Status:** a fazer

### Q2 — Média de gastos por deputado em cada estado
- **Pergunta:** Qual o valor médio gasto por deputado em cada estado, por mês?
- **Tipo:** comparação
- **Métrica:** valor médio por deputado (total gasto no mês ÷ número de deputados)
- **Dimensão:** estado (UF)
- **Período:** mensal
- **Visual sugerido:** a definir
- **Definições e ressalvas:** quem entra no denominador (deputados em exercício no mês, mesmo sem despesa?) a definir na fase 3. Estados com poucos deputados têm média mais instável.
- **Campos da API:** a preencher na fase 2
- **Status:** a fazer

### Q3 — Evolução do gasto total
- **Pergunta:** O gasto total mensal da Câmara está subindo ou caindo ao longo dos anos? Há meses que se destacam?
- **Tipo:** evolução
- **Métrica:** valor total
- **Dimensão:** tempo (mês)
- **Período:** mensal, ao longo de vários anos
- **Visual sugerido:** gráfico de linha
- **Definições e ressalvas:** valores nominais, então comparar anos distantes engana. Recessos e eleições podem explicar meses atípicos. Critério de "se destacam" a definir.
- **Campos da API:** a preencher na fase 2
- **Status:** a fazer

### Q4 — Concentração de fornecedores
- **Pergunta:** Quantos fornecedores concentram 50% de todo o dinheiro gasto?
- **Tipo:** concentração
- **Métrica:** valor total por fornecedor e percentual acumulado
- **Dimensão:** fornecedor
- **Período:** anual
- **Visual sugerido:** barras dos maiores fornecedores com linha de percentual acumulado
- **Definições e ressalvas:** identificar fornecedor por CNPJ/CPF, não pelo nome. Nunca exibir CPF completo. Verificar na fase 2 se há despesas sem fornecedor identificado.
- **Campos da API:** a preencher na fase 2
- **Status:** a fazer

### Q5 — Gasto acima da média do estado
- **Pergunta:** Quais deputados gastaram bem acima da média do próprio estado?
- **Tipo:** valor atípico
- **Métrica:** gasto mensal médio do deputado comparado à média do estado
- **Dimensão:** deputado (comparado ao seu estado)
- **Período:** anual 
- **Visual sugerido:** a definir
- **Definições e ressalvas:** critério de "bem acima" a definir após ver a distribuição real. Considerar só deputados com tempo mínimo de mandato no período. Depende da base de cálculo da Q2.
- **Campos da API:** a preencher na fase 2
- **Status:** a fazer