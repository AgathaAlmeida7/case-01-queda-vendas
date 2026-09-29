# Data Discovery

## 1. Objetivo da descoberta dos dados

A etapa de Data Discovery tem como objetivo compreender e documentar a estrutura, o significado, a granularidade e os relacionamentos das tabelas disponíveis no conjunto de dados utilizado no CASE 01 — Queda de Vendas.

Esta etapa antecede o profiling detalhado e a análise exploratória. Seu propósito é estabelecer uma visão estruturada dos dados, identificar quais informações podem contribuir para responder ao problema de negócio e registrar os principais cuidados necessários para evitar interpretações ou cálculos incorretos.

A descoberta dos dados será orientada pelo problema de negócio definido para o projeto, evitando a análise indiscriminada de todas as tabelas e variáveis disponíveis.

### Objetivos específicos

* compreender o papel de cada tabela no contexto do negócio;
* identificar a unidade de observação de cada tabela;
* identificar possíveis chaves primárias e estrangeiras;
* compreender os relacionamentos entre as tabelas;
* identificar quais tabelas e atributos são relevantes para o diagnóstico de desempenho de vendas;
* distinguir dados essenciais, complementares e auxiliares;
* registrar riscos relacionados à granularidade, duplicidade e relacionamentos entre tabelas;
* estabelecer uma base metodológica para as etapas posteriores de profiling, análise exploratória e diagnóstico.

---

## 2. Problema de negócio

O CASE 01 parte de um cenário empresarial simulado no qual a gestão comercial identificou uma deterioração no desempenho de vendas e solicita uma investigação para compreender o comportamento observado.

O cenário inicial é apresentado como uma situação de referência — "faturamento caiu 15%" — e não como uma conclusão previamente estabelecida a partir dos dados.

A análise deverá verificar o comportamento efetivamente observado no conjunto de dados e, caso a magnitude ou a natureza da variação seja diferente do cenário inicial, adaptar a investigação às evidências encontradas.

### Pergunta central

> Quais fatores estão associados à variação negativa no desempenho de vendas da operação analisada e quais segmentos concentram essa deterioração?

A análise deverá distinguir fatos observados, padrões identificados, hipóteses investigativas e conclusões sustentadas pelos dados, evitando atribuir causalidade sem evidência suficiente.

---

## 3. Perguntas analíticas

A partir da pergunta central do projeto, foram definidas perguntas analíticas que orientarão a descoberta e as etapas posteriores da investigação.

### Desempenho comercial

1. Como o desempenho de vendas evoluiu ao longo do tempo?
2. O comportamento observado está relacionado à quantidade de pedidos?
3. O ticket médio apresentou variação relevante ao longo do período?
4. A quantidade de itens vendidos acompanhou o comportamento dos pedidos?

### Produtos e categorias

5. Quais categorias ou produtos concentram as maiores variações no desempenho?
6. Existem mudanças relevantes no mix de produtos ao longo do período?

### Clientes e geografia

7. Existem diferenças relevantes no comportamento das vendas entre estados ou regiões?
8. Existem segmentos de clientes associados à variação observada?

### Vendedores

9. Existem diferenças relevantes no desempenho entre vendedores?
10. A variação observada está concentrada em determinados vendedores ou grupos de vendedores?

### Operação e experiência

11. Existem indicadores operacionais que apresentam comportamento associado à variação das vendas?
12. Existem padrões relacionados a entrega, frete, pagamentos ou avaliações que mereçam investigação?

As perguntas acima representam hipóteses de investigação e não pressupõem que os respectivos fatores expliquem ou causem a variação observada.

---

## 4. Necessidades de dados

Para responder às perguntas analíticas definidas, serão necessárias diferentes categorias de informação. A relação abaixo representa a necessidade inicial de dados e será validada durante as etapas seguintes da Data Discovery.

| Necessidade analítica              | Informações necessárias                 | Tabelas potencialmente relevantes               |
| ---------------------------------- | --------------------------------------- | ----------------------------------------------- |
| Evolução temporal das vendas       | Data do pedido e valor dos itens        | `orders`, `order_items`                         |
| Quantidade de pedidos              | Identificador do pedido e data          | `orders`                                        |
| Receita de vendas                  | Valor dos itens vendidos                | `order_items`                                   |
| Ticket médio                       | Receita e quantidade de pedidos         | `orders`, `order_items`                         |
| Quantidade de itens vendidos       | Itens associados aos pedidos            | `order_items`                                   |
| Produtos vendidos                  | Identificador e atributos do produto    | `order_items`, `products`                       |
| Categorias de produtos             | Categoria do produto                    | `products`, `product_category_name_translation` |
| Perfil e localização dos clientes  | Identificador, cidade e estado          | `customers`                                     |
| Desempenho por vendedor            | Identificador e localização do vendedor | `order_items`, `sellers`                        |
| Formas e valores de pagamento      | Tipo, parcelas e valor do pagamento     | `order_payments`                                |
| Avaliação da experiência           | Nota e informações de avaliação         | `order_reviews`                                 |
| Informações de entrega             | Datas de compra, estimativa e entrega   | `orders`                                        |
| Informações geográficas adicionais | Coordenadas e localização por CEP       | `geolocation`                                   |

A classificação definitiva das tabelas como essenciais, complementares ou auxiliares será realizada somente após a validação de sua granularidade, chaves, relacionamentos e utilidade para as perguntas do projeto.

---

# 5. Descoberta das tabelas

## 5.1 `olist_orders_dataset`

### Papel da tabela

A tabela `orders` representa o nível de **pedido** da operação analisada.

Cada registro corresponde a um pedido identificado por `order_id`.

### Estrutura observada

* Linhas: 99.441
* Colunas: 8
* `order_id`: 99.441 valores distintos
* `order_id`: nenhum valor nulo
* `customer_id`: 99.441 valores distintos
* `customer_id`: nenhum valor nulo

### Granularidade

A granularidade observada é:

> **1 linha = 1 pedido**

O `order_id` apresenta unicidade na tabela.

Essa característica é fundamental para métricas como:

* quantidade de pedidos;
* evolução temporal dos pedidos;
* distribuição por status;
* tempo entre etapas do pedido;
* ticket médio, quando combinado corretamente com o valor dos itens.

### Principais atributos

| Atributo                        | Papel analítico                                          |
| ------------------------------- | -------------------------------------------------------- |
| `order_id`                      | Identificador do pedido                                  |
| `customer_id`                   | Identificador do registro de cliente associado ao pedido |
| `order_status`                  | Situação do pedido                                       |
| `order_purchase_timestamp`      | Data/hora da realização do pedido                        |
| `order_approved_at`             | Data/hora da aprovação                                   |
| `order_delivered_carrier_date`  | Data/hora de entrega ao transportador                    |
| `order_delivered_customer_date` | Data/hora da entrega ao cliente                          |
| `order_estimated_delivery_date` | Data estimada para entrega                               |

### Relacionamentos identificados

`order_id` funciona como chave de relacionamento com tabelas relacionadas ao pedido, principalmente:

* `order_items`;
* `order_payments`;
* `order_reviews`.

`customer_id` relaciona `orders` com `customers`.

### Observação importante sobre `customer_id`

Embora `customer_id` seja utilizado como identificador do cliente associado ao pedido, a análise mostrou que cada `customer_id` aparece uma única vez em `orders`.

Portanto, neste dataset:

> `customer_id` não deve ser utilizado isoladamente para identificar recorrência de clientes.

Para análises de comportamento recorrente, deverá ser utilizado `customer_unique_id`, presente na tabela `customers`.

### Decisão analítica

A tabela `orders` será uma das tabelas centrais do CASE 01, principalmente para:

* contagem de pedidos;
* análise temporal;
* status dos pedidos;
* relacionamento com clientes;
* análise de prazos e entrega;
* construção do modelo analítico do projeto.

---

# 5.2 `olist_customers_dataset`

### Papel da tabela

A tabela `customers` contém informações cadastrais e geográficas dos clientes associados aos pedidos.

### Estrutura observada

* Linhas: 99.441
* Colunas: 5
* `customer_id`: 99.441 valores distintos
* `customer_id`: nenhum valor nulo
* `customer_unique_id`: 96.096 valores distintos
* `customer_unique_id`: nenhum valor nulo

### Granularidade

A tabela apresenta:

> **1 linha = 1 registro de cliente associado a um `customer_id`**

Foi verificada correspondência completa entre os `customer_id` presentes em `orders` e `customers`:

* `customer_id` de `orders` sem correspondência em `customers`: 0
* `customer_id` de `customers` sem correspondência em `orders`: 0

### `customer_id` × `customer_unique_id`

Foi identificada uma distinção importante para a análise de clientes.

`customer_id` identifica o registro cadastral associado ao pedido, enquanto `customer_unique_id` permite identificar o cliente em nível de negócio.

Foram identificados:

* 96.096 `customer_unique_id` distintos;
* 2.997 `customer_unique_id` associados a mais de um `customer_id`;
* máximo de 17 `customer_id` associados a um único `customer_unique_id`.

### Implicação analítica

Para análises como:

* recorrência;
* frequência de compras;
* retenção;
* comportamento de clientes;
* segmentação por cliente;

deverá ser utilizado:

> `customer_unique_id`

e não simplesmente `customer_id`.

### Informações geográficas

A tabela contém:

* 27 estados distintos;
* 4.119 cidades distintas;
* 14.994 CEPs distintos.

Essas informações permitem análises de vendas por localização geográfica.

### Decisão analítica

A tabela `customers` será utilizada principalmente para:

* segmentação geográfica;
* identificação de clientes;
* análise de recorrência;
* relacionamento entre pedidos e clientes.

A variável `customer_unique_id` será considerada a referência para análises de comportamento recorrente.

---

# 5.3 `olist_order_items_dataset`

### Papel da tabela

A tabela `order_items` representa os itens comercializados dentro dos pedidos.

É uma das principais tabelas para o cálculo das métricas de vendas do CASE 01.

### Estrutura observada

* Linhas: 112.650
* Colunas: 7
* `order_id` distintos: 98.666
* `order_id`: nenhum valor nulo
* `order_item_id`: 21 valores distintos
* `order_item_id`: nenhum valor nulo

### Granularidade

A granularidade observada é:

> **1 linha = 1 item/linha de item associado a um pedido**

Dessa forma:

* número de linhas ≠ número de pedidos;
* um pedido pode possuir vários itens;
* `order_id` não é uma chave única nesta tabela.

Foi verificada a unicidade da combinação:

> `order_id` + `order_item_id`

Não foram identificadas duplicidades nessa combinação.

### Principais atributos

| Atributo              | Papel analítico                       |
| --------------------- | ------------------------------------- |
| `order_id`            | Identificador do pedido               |
| `order_item_id`       | Sequência do item dentro do pedido    |
| `product_id`          | Identificador do produto              |
| `seller_id`           | Identificador do vendedor             |
| `shipping_limit_date` | Limite de envio informado para o item |
| `price`               | Valor do item                         |
| `freight_value`       | Valor do frete associado ao item      |

### Relacionamento com `orders`

Foram identificados 98.666 pedidos presentes em `order_items`.

Existem 775 pedidos presentes em `orders` que não possuem registros em `order_items`.

Esses pedidos apresentam os seguintes status:

| Status        | Pedidos sem itens |
| ------------- | ----------------: |
| `unavailable` |               603 |
| `canceled`    |               164 |
| `created`     |                 5 |
| `invoiced`    |                 2 |
| `shipped`     |                 1 |
| **Total**     |           **775** |

A concentração desses pedidos nos status `unavailable` e `canceled` foi registrada como uma observação de descoberta.

Essa associação não será interpretada como causalidade.

### Relacionamento com produtos

Todos os `product_id` presentes em `order_items` possuem correspondência em `products`.

Também não foram identificados produtos cadastrados em `products` sem ocorrência em `order_items`.

Relacionamento conceitual:

> `products` 1:N `order_items`

### Relacionamento com vendedores

Todos os `seller_id` presentes em `order_items` possuem correspondência em `sellers`.

Também não foram identificados vendedores cadastrados em `sellers` sem ocorrência em `order_items`.

Relacionamento conceitual:

> `sellers` 1:N `order_items`

### Implicações para métricas

Para o CASE 01:

**Pedidos:**

> `COUNT(DISTINCT order_id)`

**Itens vendidos:**

> `COUNT(*)`

**Valor dos produtos:**

> `SUM(price)`

**Frete:**

> `SUM(freight_value)`

Portanto, não será utilizado simplesmente o número de linhas de `order_items` como quantidade de pedidos.

### Decisão analítica

`order_items` será uma das principais tabelas do projeto e será utilizada como fonte primária para o valor dos produtos comercializados.

A variável `price` será utilizada como base da métrica de receita de produtos, mantendo `freight_value` como componente separado.

---

# 5.4 `olist_order_payments_dataset`

### Papel da tabela

A tabela `order_payments` registra informações relacionadas aos pagamentos dos pedidos.

Ela é complementar à análise comercial e permite investigar características como:

* modalidade de pagamento;
* número de parcelas;
* sequência dos registros de pagamento;
* valor registrado nos pagamentos.

### Estrutura observada

* Linhas: 103.886
* Colunas: 5
* `order_id` distintos: 99.440
* `order_id`: nenhum valor nulo

Como o número de linhas é superior ao número de pedidos distintos, `order_id` não representa uma chave única nessa tabela.

### Granularidade

A granularidade observada é:

> **1 linha = 1 registro de pagamento associado a um pedido**

Um mesmo pedido pode possuir múltiplos registros de pagamento.

Foram identificados:

* 2.961 pedidos com mais de um registro de pagamento;
* máximo de 29 registros de pagamento para um único pedido.

### `payment_sequential`

`payment_sequential` representa a sequência dos registros de pagamento dentro do pedido.

Foram identificados valores de 1 a 29.

Essa variável não deve ser confundida com `payment_installments`.

### `payment_installments`

Representa o número de parcelas associado ao registro de pagamento.

Foram identificados dois registros com `payment_installments = 0`, ambos associados a pagamentos com cartão de crédito.

Esses registros foram classificados como casos atípicos para investigação posterior, sem correção ou exclusão nesta etapa.

### Relacionamento com `orders`

Todos os `order_id` presentes em `order_payments` possuem correspondência em `orders`.

Foi identificado um pedido presente em `orders` sem registro correspondente em `order_payments`:

`bfbd0f9bdef84302105ad712db648a6c`

Esse pedido apresenta status `delivered`, possui itens e avaliação, mas não possui registro na tabela de pagamentos.

O caso foi registrado como uma inconsistência entre tabelas e não será interpretado como erro do dataset sem investigação adicional.

### Tipos de pagamento

Foram identificados os seguintes tipos:

| Tipo          | Registros |
| ------------- | --------: |
| `credit_card` |    76.795 |
| `boleto`      |    19.784 |
| `voucher`     |     5.775 |
| `debit_card`  |     1.529 |
| `not_defined` |         3 |

Os três registros classificados como `not_defined` possuem `payment_value = 0`.

Esses registros serão considerados casos atípicos, sem inferir que representam necessariamente pagamentos não realizados.

---

## 5.4.1 Comparação entre pagamentos e itens

Foi realizada uma comparação entre:

> `SUM(payment_value)` por pedido

e

> `SUM(price) + SUM(freight_value)` por pedido.

Foram comparados 98.665 pedidos presentes simultaneamente nas duas estruturas.

### Resultado

* 98.365 pedidos apresentaram diferença de até R$ 0,01;
* 300 pedidos apresentaram diferença superior a R$ 0,01.

Isso representa aproximadamente 99,70% dos pedidos comparáveis com correspondência dentro da tolerância de R$ 0,01.

As divergências foram classificadas por magnitude:

| Faixa de diferença absoluta | Pedidos |
| --------------------------- | ------: |
| Até R$ 0,05                 |  98.405 |
| R$ 0,06 a R$ 10             |     162 |
| R$ 10,01 a R$ 50            |      90 |
| Acima de R$ 50              |       8 |

A faixa "Até R$ 0,05" inclui tanto correspondências exatas quanto pequenas diferenças superiores a R$ 0,01 e inferiores ou iguais a R$ 0,05.

### Comportamento temporal

As divergências foram observadas em diferentes momentos do período analisado, desde outubro de 2016 até períodos posteriores.

Não foi identificada, nesta etapa, evidência de concentração das divergências em um único intervalo temporal.

### Distribuição por tipo de pagamento

Entre os 300 pedidos com divergência:

| Tipo de pagamento | Pedidos com divergência |
| ----------------- | ----------------------: |
| `credit_card`     |                     290 |
| `boleto`          |                      13 |
| `debit_card`      |                       7 |
| `voucher`         |                       7 |

Como os tipos de pagamento possuem volumes totais muito diferentes, a quantidade absoluta de divergências não foi utilizada isoladamente para inferir concentração.

A taxa de divergência observada foi:

| Tipo de pagamento | Total de pedidos | Com divergência |   Taxa |
| ----------------- | ---------------: | --------------: | -----: |
| `debit_card`      |            1.520 |               7 | 0,461% |
| `credit_card`     |           74.883 |             278 | 0,371% |
| `voucher`         |            2.648 |               2 | 0,076% |
| `boleto`          |           19.614 |              13 | 0,066% |

As taxas observadas são baixas em todos os grupos. A diferença numérica entre as modalidades não será interpretada como evidência de causa nesta etapa.

### Decisão metodológica

A análise mostrou que `payment_value` apresenta alta compatibilidade com a soma de produtos e frete na maior parte dos pedidos, porém existem divergências que ainda não possuem explicação determinada.

Por esse motivo:

> `payment_value` não será utilizado automaticamente como fonte primária da receita de produtos do CASE 01.

A métrica principal de receita de produtos será calculada a partir de:

> `SUM(order_items.price)`

O frete será analisado separadamente a partir de:

> `SUM(order_items.freight_value)`

`payment_value` permanecerá disponível como variável relacionada ao comportamento de pagamentos e poderá ser utilizada em análises específicas quando sua interpretação for adequada.

Essa decisão evita transformar uma variável de pagamento em uma métrica de receita sem validação suficiente de sua semântica.

---

# 6. Modelo inicial de relacionamentos

Com base nas descobertas realizadas até o momento, foi estabelecido o seguinte modelo conceitual inicial:

```text
                         customers
                             │
                       customer_id
                             │
                             ▼
                          orders
                             │
             ┌───────────────┼────────────────┐
             │               │                │
          order_id        order_id         order_id
             │               │                │
             ▼               ▼                ▼
       order_items    order_payments    order_reviews
             │
        ┌────┴────┐
        │         │
   product_id  seller_id
        │         │
        ▼         ▼
    products    sellers
```

Para análise de recorrência de clientes:

```text
customer_unique_id
        │
        ├── customer_id
        ├── customer_id
        ├── customer_id
        └── ...
```

Esse relacionamento deverá ser considerado nas análises de comportamento recorrente dos clientes.

---

# 7. Classificação preliminar das tabelas

Com as descobertas realizadas até o momento, as tabelas podem ser classificadas preliminarmente da seguinte forma:

### Tabelas centrais

* `olist_orders_dataset`
* `olist_order_items_dataset`
* `olist_customers_dataset`

Essas tabelas são fundamentais para reconstruir o comportamento comercial, os pedidos, os itens vendidos e o perfil dos clientes.

### Tabelas complementares

* `olist_order_payments_dataset`
* `olist_order_reviews_dataset`
* `olist_products_dataset`
* `olist_sellers_dataset`

Essas tabelas permitem aprofundar o diagnóstico em dimensões financeiras, experiência do cliente, produtos e vendedores.

### Tabela auxiliar

* `product_category_name_translation.csv`

Sua função principal será auxiliar na interpretação e apresentação das categorias de produtos.

### Tabela potencialmente auxiliar

* `olist_geolocation_dataset`

Sua utilização dependerá das necessidades de análise geográfica identificadas nas etapas posteriores.

Essa classificação é preliminar e poderá ser revisada conforme novas descobertas sejam realizadas.

---

# 8. Principais decisões metodológicas registradas

Até o momento, foram estabelecidas as seguintes decisões:

1. O cenário de "queda de 15%" não será tratado como fato previamente confirmado pelos dados.
2. As conclusões deverão ser determinadas pelas evidências encontradas no dataset.
3. `order_id` representa um pedido na tabela `orders`.
4. `order_items` possui granularidade de item, e não de pedido.
5. Pedidos deverão ser contabilizados por `COUNT(DISTINCT order_id)` quando a análise partir de `order_items`.
6. `customer_unique_id` será utilizado para análises de recorrência e comportamento de clientes.
7. `order_items.price` será a fonte primária para a métrica de receita de produtos.
8. `freight_value` será analisado separadamente.
9. `payment_value` não será tratado automaticamente como receita.
10. Divergências entre pagamentos e itens serão preservadas para análise, sem correção ou exclusão durante a Data Discovery.
11. Associações observadas entre variáveis não serão tratadas automaticamente como relações causais.
12. Tabelas serão selecionadas conforme sua capacidade de responder às perguntas de negócio, e não simplesmente porque fazem parte do dataset.

---

# 9. Próximas etapas da Data Discovery

Após a consolidação das tabelas já investigadas, a descoberta continuará priorizando as estruturas diretamente relacionadas ao problema de negócio.

A próxima etapa será investigar:

1. `olist_products_dataset`
2. `product_category_name_translation`
3. `olist_sellers_dataset`
4. `olist_order_reviews_dataset`
5. `olist_geolocation_dataset`

A prioridade será determinada pela relevância para as perguntas analíticas e pelos relacionamentos identificados no modelo.

Após a conclusão da Data Discovery, será realizada a etapa de **Data Profiling**, na qual serão investigados de forma sistemática:

* tipos de dados;
* valores nulos;
* duplicidades;
* integridade das chaves;
* valores inválidos ou atípicos;
* distribuição das variáveis;
* consistência temporal;
* qualidade dos dados;
* regras de tratamento necessárias para a análise.

A análise exploratória e os cálculos estatísticos somente serão iniciados após essa etapa de compreensão e validação estrutural dos dados.
