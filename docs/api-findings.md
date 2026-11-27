# Descobertas da API da Câmara

Resultados da exploração (notebooks). Base: `https://dadosabertos.camara.leg.br/api/v2`, sem chave de acesso.
Endpoints usados: `/deputados` e `/deputados/{id}/despesas`.

## Deputados
- Sem filtro, a lista traz 513 deputados (só quem está no cargo no momento da requisição).
- Com `idLegislatura=57`, traz mais de 850, incluindo suplentes que saíram do cargo e ainda têm despesas. **Decisão: usar a lista idLegislatura=57**
- Campos: `id`, `uri`, `nome`, `siglaPartido`, `uriPartido`, `siglaUf`, `idLegislatura`, `urlFoto`, `email`.

## Despesas
- O registro **não traz o id do deputado**. A extração adiciona `idDeputado` a cada linha.
- A UF não vem nas despesas; vem da lista de deputados (join).
- Campos: `ano`, `mes`, `tipoDespesa`, `codDocumento`, `tipoDocumento`, `codTipoDocumento`, `dataDocumento`, `numDocumento`, `valorDocumento`, `urlDocumento`, `nomeFornecedor`, `cnpjCpfFornecedor`, `valorLiquido`, `valorGlosa`, `numRessarcimento`, `codLote`, `parcela`.

## Paginação e erros
- Máximo de 100 itens por página. A última página pode ter menos; depois dela, `dados` vem vazio.
- `links` sempre traz `self`, `first` e `last`; `next` e `previous` só quando existem. **Regra: continuar enquanto existir `next`.**
- Lista vazia (`dados: []`) = sem dados, resultado normal.
- Recurso inexistente retorna 404 com `status` e `title` ("Recurso não encontrado"). É erro.
- A API já respondeu 502 e 504 (instabilidade). O código usa retry com espera e tentativas limitadas.
- Exemplo de volume (2025): um deputado com 554 registros (6 páginas), outro com 777 (8 páginas).

## Regras de dados
- **Valor da despesa = `valorDocumento − valorGlosa`.** Conferido com o Portal da Transparência (total de um deputado bateu). O `valorLiquido` não é usado: o sinal é inconsistente.
- **Créditos:** `valorDocumento` negativo abate o gasto. Visto só em `PASSAGEM AÉREA - SIGEPA`, sem glosa. Conferido com o portal em um deputado/mês.
- **Tempo:** usar `ano` + `mes`. `dataDocumento` pode ser nula e pode cair em mês diferente de `mes`.
- **Tipos:** `cnpjCpfFornecedor` deve ficar como texto (preserva zeros à esquerda). `codDocumento` vem como texto.
- **Nulos observados** (Todos os registros de 10 deputados, 2025): `dataDocumento` e `urlDocumento`. Os demais campos vieram completos.
- **`tipoDespesa`:** maiúsculas, com e sem ponto final no fim. Normalizar na limpeza.
- **Fornecedor:** pode ser CPF (11 dígitos), que é dado pessoal. Nunca exibir completo.
- `codDocumento` pode ser "0", então não serve como chave única.

## Pendente de verificação
- Fornecedores com identificador de preenchimento (ex.: `00000000000006`, "RAMAL", visto em amostra de 2023). Checar em todos os dados, na limpeza.
- Registros duplicados: 1 registro duplicado em 10 deputados testados; outros usuários já relataram duplicatas na API tambem. Tratar duplicadas em registros.
- Créditos em outros tipos de despesa além de passagem aérea.
- Definição de "deputado em exercício no mês" (fase 3).