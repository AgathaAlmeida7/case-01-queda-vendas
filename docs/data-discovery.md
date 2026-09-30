# Data Discovery — CASE 01: Queda de Vendas

## 1. Objetivo da etapa

A etapa de Data Discovery tem como objetivo compreender a estrutura, o significado, a granularidade, os relacionamentos, a integridade e as limitações das tabelas disponíveis antes do início da análise exploratória.

O princípio metodológico adotado neste projeto é:

> **Os dados determinam a história; a história não determina os dados.**

O cenário inicial do projeto considera uma possível deterioração no desempenho de vendas. Entretanto, a hipótese de uma queda específica de 15% não será imposta aos dados.

A magnitude, o período, os segmentos e os fatores associados à deterioração serão determinados a partir das evidências observadas no conjunto de dados.

---

# 2. Pergunta de negócio

A pergunta de negócio utilizada como orientação para a análise é:

> **Quais fatores estão associados à variação negativa no desempenho de vendas da operação analisada e quais segmentos concentram essa deterioração?**

A análise deverá, inicialmente, estabelecer se existe de fato uma deterioração relevante no desempenho de vendas e, posteriormente, investigar onde ela ocorre e quais fatores estão associados a esse comportamento.

---

# 3. Dataset utilizado

O projeto utiliza o:

**Olist Brazilian E-Commerce Public Dataset**

O conjunto contém informações relacionadas a pedidos, clientes, itens vendidos, pagamentos, avaliações, produtos e vendedores.

Os arquivos disponíveis no projeto são:

* `olist_customers_dataset.csv`
* `olist_geolocation_dataset.csv`
* `olist_order_items_dataset.csv`
* `olist_order_payments_dataset.csv`
* `olist_order_reviews_dataset.csv`
* `olist_orders_dataset.csv`
* `olist_products_dataset.csv`
* `olist_sellers_dataset.csv`
* `product_category_name_translation.csv`

Os arquivos originais estão armazenados em:

```text
data/raw/
```

Essa pasta está incluída no `.gitignore`, portanto os dados brutos não serão versionados no repositório.

---

# 4. Inventário das tabelas

| Tabela               |    Linhas | Colunas | Papel preliminar        |
| -------------------- | --------: | ------: | ----------------------- |
| orders               |    99.441 |       8 | Central                 |
| customers            |    99.441 |       5 | Central                 |
| order_items          |   112.650 |       7 | Central                 |
| payments             |   103.886 |       5 | Complementar            |
| reviews              |    99.224 |       7 | Complementar            |
| products             |    32.951 |       9 | Complementar            |
| sellers              |     3.095 |       4 | Complementar            |
| category translation |        71 |       2 | Auxiliar                |
| geolocation          | 1.000.163 |       5 | Potencialmente auxiliar |

A classificação poderá ser revista durante a análise caso alguma dimensão demonstre relevância adicional para a pergunta de negócio.

---

# 5. Tabela `orders`

Arquivo:

```text
olist_orders_dataset.csv
```

Dimensão:

```text
99.441 linhas × 8 colunas
```

A tabela possui uma linha por `order_id`.

O campo `order_id` apresentou:

* 99.441 valores únicos;
* nenhum valor nulo.

Portanto:

> **Granularidade: 1 linha = 1 pedido.**

## Principais relacionamentos

`order_id` relaciona-se com:

* `order_items`
* `order_payments`
* `order_reviews`

`customer_id` relaciona-se com:

* `customers`

## Campos temporais

A tabela contém datas relacionadas ao ciclo do pedido, incluindo:

* criação;
* aprovação;
* envio ao transportador;
* entrega ao cliente;
* previsão de entrega.

Esses campos poderão ser utilizados posteriormente para análise temporal e operacional.

## Valores ausentes

Foram identificados valores ausentes em diferentes datas e diferentes status de pedidos.

Por exemplo:

* `order_approved_at` apresenta ausências em pedidos cancelados, entregues e criados;
* `order_delivered_carrier_date` apresenta ausências em diferentes status;
* `order_delivered_customer_date` também apresenta ausências em diferentes status.

A ausência de uma data não será automaticamente tratada como erro.

A interpretação deverá considerar o status do pedido e o contexto da variável.

Foi identificada também a existência de pedidos com status `delivered` sem `order_delivered_customer_date`.

Essa situação será tratada como uma limitação/inconsistência a ser considerada em análises operacionais, sem assumir automaticamente que o registro esteja incorreto.

---

# 6. Tabela `customers`

Arquivo:

```text
olist_customers_dataset.csv
```

Dimensão:

```text
99.441 linhas × 5 colunas
```

`customer_id` apresentou:

* 99.441 valores únicos;
* nenhum valor nulo.

Existe correspondência integral entre os `customer_id` presentes em `orders` e os cadastrados em `customers`.

## `customer_id` × `customer_unique_id`

A tabela possui também:

```text
customer_unique_id
```

Foram encontrados:

* 96.096 `customer_unique_id` únicos;
* 2.997 `customer_unique_id` associados a mais de um `customer_id`;
* máximo de 17 `customer_id` associados a um mesmo `customer_unique_id`.

Isso é metodologicamente importante.

Para análises de:

* recorrência;
* frequência de compra;
* retenção;
* comportamento do cliente;
* quantidade de pedidos por cliente;

deve-se utilizar:

```text
customer_unique_id
```

e não simplesmente `customer_id`.

---

# 7. Tabela `order_items`

Arquivo:

```text
olist_order_items_dataset.csv
```

Dimensão:

```text
112.650 linhas × 7 colunas
```

A tabela representa os itens associados aos pedidos.

`order_id` possui:

```text
98.666 pedidos distintos
```

A combinação:

```text
(order_id, order_item_id)
```

não apresentou duplicidades.

Portanto, essa combinação representa a chave adequada para a granularidade dos itens.

## Pedidos sem itens

Foram identificados:

```text
775 pedidos em orders sem registro correspondente em order_items
```

A distribuição desses pedidos por status mostrou concentração principalmente em:

* `unavailable`: 603
* `canceled`: 164

Também foram encontrados poucos casos em outros status.

Essa ausência não será tratada automaticamente como erro.

Para análises de receita de produtos, o conjunto de `order_items` será utilizado como fonte principal.

## Receita

O campo:

```text
price
```

representa o valor do produto no item.

Assim, para a análise principal de receita de produtos:

```text
Receita de produtos = SUM(price)
```

O campo:

```text
freight_value
```

será tratado separadamente como valor de frete.

---

# 8. Tabela `order_payments`

Arquivo:

```text
olist_order_payments_dataset.csv
```

Dimensão:

```text
103.886 linhas × 5 colunas
```

`order_id` possui 99.440 valores únicos.

Foram identificados:

```text
2.961 pedidos com mais de um registro de pagamento
```

Portanto:

> `order_payments` não possui granularidade de uma linha por pedido.

O campo:

```text
payment_sequential
```

representa a sequência dos pagamentos e não deve ser confundido com o número de parcelas.

## Tipos de pagamento

Foram encontrados:

* credit_card
* boleto
* voucher
* debit_card
* not_defined

Existem 3 registros classificados como `not_defined`, todos com `payment_value = 0`.

A estrutura será preservada sem assumir automaticamente que esses registros representam erro ou ausência de pagamento.

## Comparação com `order_items`

Foi realizada a comparação entre:

```text
price + freight_value
```

e:

```text
payment_value
```

agregado por pedido.

Foram analisados:

```text
98.665 pedidos
```

Resultados:

* 98.365 pedidos apresentaram diferença de até R$ 0,01;
* 300 apresentaram divergência superior a R$ 0,01.

Entre os 300 casos divergentes:

* média da diferença: aproximadamente R$ 9,57;
* mediana: aproximadamente R$ 5,43;
* mínimo: aproximadamente -R$ 51,62;
* máximo: aproximadamente R$ 182,81.

As divergências aparecem em diferentes períodos e não estão restritas a um único intervalo temporal.

## Decisão metodológica

A fonte principal para receita de produtos será:

```text
order_items.price
```

`payment_value` será utilizado como variável complementar para análises relacionadas a pagamentos.

Não será utilizada uma regra simplista de substituição ou ajuste dos valores divergentes sem investigação específica.

---

# 9. Tabela `order_reviews`

Arquivo:

```text
olist_order_reviews_dataset.csv
```

Dimensão:

```text
99.224 linhas × 7 colunas
```

A tabela contém:

* identificação da avaliação;
* pedido;
* nota;
* título;
* mensagem;
* data de criação;
* data de resposta.

## Granularidade

Foram identificados:

```text
98.673 pedidos distintos com avaliações
```

Distribuição de avaliações por pedido:

* 98.126 pedidos com 1 avaliação;
* 543 pedidos com 2 avaliações;
* 4 pedidos com 3 avaliações.

Portanto:

```text
547 pedidos possuem múltiplas avaliações.
```

## `review_id`

Foram observados:

```text
99.224 linhas
98.410 review_id únicos
```

Existem valores de `review_id` repetidos.

A combinação:

```text
(order_id, review_id)
```

não apresentou duplicidades.

Portanto:

> `review_id` não deve ser tratado isoladamente como identificador globalmente único.

## Múltiplas avaliações

Nos 547 pedidos com múltiplas avaliações:

* 155 possuem uma única data distinta;
* 390 possuem duas datas distintas;
* 2 possuem três datas distintas.

Assim:

```text
392 de 547 pedidos
```

possuem avaliações registradas em datas diferentes.

Quanto às notas:

* 345 pedidos mantêm a mesma nota;
* 202 apresentam notas diferentes.

Foi observada a seguinte classificação conjunta:

| Situação                            | Quantidade |
| ----------------------------------- | ---------: |
| Datas diferentes + mesma nota       |        220 |
| Datas diferentes + notas diferentes |        172 |
| Mesma data + mesma nota             |        125 |
| Mesma data + notas diferentes       |         30 |

## Texto das avaliações

Foi criada uma representação combinada de título e mensagem para comparação do conteúdo textual.

Entre os pedidos com múltiplas avaliações:

* 313 possuem um único texto distinto;
* 232 possuem dois textos distintos;
* 2 possuem três textos distintos.

Portanto:

```text
234 de 547 pedidos
```

possuem avaliações com textos diferentes.

## Decisão metodológica

As múltiplas avaliações não serão consideradas duplicidades automaticamente.

Não será utilizado:

```python
drop_duplicates("review_id")
```

nem:

```python
drop_duplicates("order_id")
```

como regra geral.

Também não será utilizado arbitrariamente o primeiro ou o último registro de cada pedido.

Caso uma análise futura exija uma linha por pedido, deverá ser criada uma agregação explícita e documentada de acordo com a pergunta de negócio.

A tabela de reviews será tratada como uma dimensão complementar de satisfação e experiência do cliente.

---

# 10. Tabela `products`

Arquivo:

```text
olist_products_dataset.csv
```

Dimensão:

```text
32.951 linhas × 9 colunas
```

`product_id` apresentou:

* 32.951 valores únicos;
* nenhum valor nulo;
* nenhuma duplicidade completa de linha.

Todos os produtos presentes em `order_items` possuem correspondência no cadastro de produtos.

Também não foram encontrados produtos cadastrados sem presença em `order_items`.

## Categorias

Foram identificadas:

```text
73 categorias não nulas
```

Existem:

```text
610 produtos sem categoria
```

Todos esses produtos aparecem em `order_items`.

Esses produtos representam:

```text
1.603 linhas de venda
```

ou aproximadamente:

```text
1,42% das linhas de order_items
```

Esse percentual se refere a linhas de venda, não à receita.

## Dados físicos

Foram identificados:

* 2 produtos com peso ausente;
* 2 produtos com comprimento ausente;
* 2 produtos com altura ausente;
* 2 produtos com largura ausente.

Também foram encontrados 4 produtos com peso igual a zero.

Foram identificados 6 produtos com alguma inconsistência ou ausência nos atributos físicos.

Esses registros possuem vendas e representam aproximadamente:

```text
R$ 3.446,50
```

em receita de produtos.

Não será feita correção artificial desses valores nesta etapa.

Eles serão tratados como uma limitação de qualidade dos dados.

---

# 11. Tabela `product_category_name_translation`

Arquivo:

```text
product_category_name_translation.csv
```

Dimensão:

```text
71 linhas × 2 colunas
```

A tabela apresenta traduções entre nomes de categorias em português e inglês.

Foram identificadas 2 categorias presentes em `products` sem tradução:

```text
pc_gamer
portateis_cozinha_e_preparadores_de_alimentos
```

Essas categorias possuem:

* 13 produtos;
* 24 linhas de venda;
* R$ 5.514,48 em receita de produtos.

Portanto, essas categorias não devem ser excluídas da análise apenas por ausência de tradução.

Os nomes originais em português serão preservados.

A tradução será considerada uma dimensão auxiliar para apresentação, quando aplicável.

---

# 12. Tabela `sellers`

Arquivo:

```text
olist_sellers_dataset.csv
```

Dimensão:

```text
3.095 linhas × 4 colunas
```

`seller_id` apresentou:

* 3.095 valores únicos;
* nenhum valor nulo;
* nenhuma duplicidade completa.

Todos os sellers presentes em `order_items` possuem cadastro correspondente.

Também não foram identificados sellers cadastrados sem vendas.

## Localização

Os sellers estão distribuídos em:

```text
23 UFs
```

A localização do seller deve ser distinguida da localização do cliente.

Portanto:

```text
seller_state != customer_state
```

representam dimensões geográficas diferentes e não devem ser utilizadas como se fossem a mesma variável.

## Distribuição das vendas

Foram calculadas as vendas por seller.

Para `linhas_venda`:

* média: 36,40;
* mediana: 8;
* máximo: 2.033.

Para `receita_produtos`:

* média: R$ 4.391,48;
* mediana: R$ 821,48;
* máximo: R$ 229.472,63.

A diferença entre média e mediana demonstra uma distribuição assimétrica, com presença de sellers com volumes de venda muito superiores aos valores centrais.

Essa observação é descritiva e não implica, por si só, concentração causal ou desempenho superior.

---

# 13. Tabela `geolocation`

Arquivo:

```text
olist_geolocation_dataset.csv
```

Dimensão:

```text
1.000.163 linhas × 5 colunas
```

A tabela possui informações geográficas associadas a prefixos de CEP.

Neste momento, ela não é considerada essencial para responder à pergunta central do projeto.

Poderá ser utilizada posteriormente caso uma análise geográfica mais detalhada seja necessária.

---

# 14. Modelo lógico identificado

A estrutura principal pode ser representada conceitualmente da seguinte maneira:

```text
CUSTOMERS
    │
    │ customer_id
    ▼
 ORDERS
    │
    ├──────────────► PAYMENTS
    │
    ├──────────────► REVIEWS
    │
    └──────────────► ORDER_ITEMS
                         │
                         ├────────► PRODUCTS
                         │
                         └────────► SELLERS
```

A tabela de tradução de categorias complementa:

```text
PRODUCTS
    │
    ▼
CATEGORY TRANSLATION
```

A tabela de geolocalização permanece como dimensão potencialmente auxiliar.

---

# 15. Tabelas centrais para a análise

Para o problema de negócio definido, as principais tabelas são:

### 1. `orders`

Utilização:

* pedidos;
* status;
* datas;
* ciclo do pedido;
* análise temporal.

### 2. `order_items`

Utilização:

* receita;
* itens vendidos;
* produtos;
* sellers;
* preço;
* frete.

### 3. `customers`

Utilização:

* identificação do cliente;
* recorrência;
* localização;
* comportamento de compra.

Essas três tabelas formam o núcleo principal da análise.

---

# 16. Tabelas complementares

### `payments`

Utilização:

* forma de pagamento;
* parcelamento;
* validação/complementação de valores.

### `reviews`

Utilização:

* satisfação;
* experiência;
* avaliação do pedido.

### `products`

Utilização:

* categoria;
* características do produto;
* dimensões físicas.

### `sellers`

Utilização:

* vendedor;
* localização;
* concentração/distribuição das vendas.

---

# 17. Tabelas auxiliares

### `product_category_name_translation`

Utilização:

* tradução das categorias;
* apresentação dos resultados.

### `geolocation`

Utilização potencial:

* análises geográficas mais detalhadas.

---

# 18. Definição preliminar dos principais KPIs

Os principais indicadores que poderão ser utilizados na análise são:

### Receita de produtos

```text
SUM(order_items.price)
```

### Frete

```text
SUM(order_items.freight_value)
```

### Pedidos

```text
COUNT(DISTINCT order_id)
```

### Itens vendidos

```text
COUNT(order_item_id)
```

### Clientes

Para análises de clientes:

```text
COUNT(DISTINCT customer_unique_id)
```

### Ticket médio

Conceitualmente:

```text
Receita de produtos / quantidade de pedidos
```

Essas métricas serão calculadas somente após definição das regras de inclusão/exclusão dos pedidos de acordo com a pergunta analítica.

---

# 19. Possível decomposição do desempenho de vendas

Uma das linhas de investigação será decompor a receita em componentes.

Conceitualmente:

```text
Receita
   │
   ├── Quantidade de pedidos
   │
   └── Ticket médio
```

Posteriormente, o ticket poderá ser decomposto em dimensões como:

```text
Itens por pedido
        ×
Preço médio por item
```

Também poderão ser investigadas dimensões como:

* clientes;
* frequência de compra;
* categorias;
* produtos;
* sellers;
* regiões;
* período.

A seleção definitiva será determinada pelos resultados observados nos dados.

---

# 20. Principais riscos metodológicos identificados

## Risco 1 — Forçar a hipótese de queda de 15%

A análise não assumirá que a queda foi exatamente de 15%.

Primeiro será necessário medir o comportamento real da receita e dos demais KPIs.

---

## Risco 2 — Utilizar `payment_value` como receita principal

Como existem divergências entre pagamentos e valores de itens + frete, `payment_value` não será utilizado como fonte única de receita.

A receita principal será baseada em:

```text
order_items.price
```

---

## Risco 3 — Confundir `customer_id` com cliente real

Para recorrência e frequência será utilizado:

```text
customer_unique_id
```

---

## Risco 4 — Considerar múltiplas reviews como duplicatas

As avaliações múltiplas serão preservadas.

Qualquer agregação futura deverá possuir regra analítica explícita.

---

## Risco 5 — Interpretar ausência como erro

Valores ausentes serão interpretados de acordo com:

* variável;
* status;
* granularidade;
* contexto do negócio.

---

## Risco 6 — Inferir causalidade a partir de associação

Uma relação observada entre duas variáveis não será apresentada automaticamente como causa.

As conclusões deverão distinguir:

* fato observado;
* associação;
* hipótese;
* evidência;
* conclusão.

---

# 21. Decisões metodológicas consolidadas

Ao final da Data Discovery, foram estabelecidas as seguintes decisões:

1. `orders` representa pedidos.
2. `order_items` representa itens vendidos.
3. A chave dos itens é a combinação `(order_id, order_item_id)`.
4. A receita principal de produtos será calculada através de `order_items.price`.
5. `freight_value` será analisado separadamente.
6. `payment_value` será complementar.
7. `customer_unique_id` será utilizado para análises de recorrência.
8. `review_id` não será tratado como identificador globalmente único.
9. Reviews múltiplas não serão removidas automaticamente.
10. Produtos sem categoria serão preservados.
11. Categorias sem tradução serão preservadas.
12. Sellers serão analisados separadamente da localização dos clientes.
13. Dados ausentes não serão automaticamente considerados erros.
14. Anomalias serão documentadas antes de qualquer tratamento.
15. Não será imposta uma queda de 15% aos dados.
16. Não serão feitas afirmações causais sem evidência adequada.

---

# 22. Encerramento da etapa de Data Discovery

A estrutura do conjunto de dados foi investigada e documentada de forma suficiente para iniciar a próxima etapa do projeto.

Neste momento, já estão estabelecidos:

* o significado das principais tabelas;
* a granularidade dos dados;
* as principais chaves;
* os relacionamentos;
* as fontes de receita;
* as dimensões relevantes;
* as limitações conhecidas;
* os principais riscos metodológicos.

Portanto, a etapa de Data Discovery pode ser considerada **metodologicamente concluída após a consolidação e versionamento desta documentação**.

A próxima etapa será:

> **Data Profiling orientado pela pergunta de negócio.**

O objetivo será medir a qualidade e o comportamento das variáveis efetivamente relevantes para a análise, preparando o conjunto para a Análise Exploratória de Dados (EDA).
