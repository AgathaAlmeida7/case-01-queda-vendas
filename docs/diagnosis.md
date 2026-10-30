# CASE 01 — Diagnóstico da Queda de Receita

## 1. Resumo executivo

Este estudo investiga a redução da receita de vendas do marketplace entre maio e junho de 2018, buscando identificar quais dimensões comerciais mais contribuíram para a variação observada.

A receita, calculada pela soma dos preços dos itens vendidos, passou de **R$ 996.517,68 em maio para R$ 865.124,31 em junho**, representando uma redução de **R$ 131.393,37 (13,19%)**.

A decomposição matemática da variação indicou que a redução do volume de pedidos respondeu por **76,69% da queda**, enquanto a diminuição da receita média por pedido representou os **23,31% restantes**.

As análises complementares identificaram maior frequência de reduções de receita entre os produtos, mais interrupções do que entradas de atividade comercial e diferenças relevantes entre categorias e vendedores.

Entre os dez vendedores selecionados pelas maiores reduções líquidas de receita, a perda conjunta foi de **R$ 94.991,99**, equivalente a **72,30% da redução líquida total do marketplace**.

Os resultados permitem caracterizar a queda e identificar segmentos prioritários para investigações posteriores. Entretanto, os dados disponíveis não permitem determinar suas causas operacionais.

## 2. Objetivo da investigação

O objetivo deste estudo é investigar a redução da receita do marketplace entre maio e junho de 2018, identificando:

- A magnitude da variação da receita e dos principais indicadores comerciais;
- A participação do volume de pedidos e da receita média por pedido na variação observada;
- O comportamento da receita dos produtos entre os dois meses;
- As mudanças na atividade comercial dos produtos;
- As categorias e os vendedores com maiores perdas;
- As limitações das evidências disponíveis e os pontos que exigem investigações adicionais.

A análise possui caráter diagnóstico e exploratório. Seu propósito é identificar padrões e direcionar novas investigações, sem atribuir causalidade a fatores que não foram diretamente observados.

## 3. Principais indicadores

A comparação entre maio e junho de 2018 apresentou os seguintes resultados:

| Indicador | Maio/2018 | Junho/2018 | Variação |
|---|---:|---:|---:|
| Receita | R$ 996.517,68 | R$ 865.124,31 | -13,19% |
| Pedidos com itens | 6.853 | 6.160 | -10,11% |
| Itens vendidos | 7.925 | 7.078 | -10,69% |
| Receita média por pedido | R$ 145,41 | R$ 140,44 | -3,42% |
| Vendedores ativos | 1.115 | 1.175 | +5,38% |
| Produtos vendidos | 4.052 | 4.025 | -0,67% |

**Interpretação:**

A queda da receita ocorreu simultaneamente à redução do número de pedidos com itens e da quantidade de itens vendidos. A receita média por pedido também diminuiu, embora em menor proporção.

O número de vendedores ativos aumentou, enquanto a quantidade de produtos vendidos apresentou uma pequena redução. Portanto, a diminuição da receita não foi acompanhada por uma redução generalizada da quantidade de vendedores ativos.

A combinação desses indicadores sugere que a investigação deve considerar não apenas a quantidade de vendedores e produtos disponíveis, mas também o volume efetivo de transações e a distribuição das vendas entre os participantes do marketplace.

## 4. Decomposição da variação da receita

Para compreender melhor a redução da receita, foi realizada uma decomposição matemática considerando dois componentes:

- **Efeito volume:** variação associada à mudança na quantidade de pedidos com itens.
- **Efeito receita média por pedido:** variação associada à mudança no valor médio de receita por pedido.

Os resultados foram:

| Componente | Efeito financeiro | Participação na queda |
|---|---:|---:|
| Volume de pedidos | -R$ 100.771,45 | 76,69% |
| Receita média por pedido | -R$ 30.621,92 | 23,31% |
| **Variação total** | **-R$ 131.393,37** | **100,00%** |

**Interpretação:**

A redução do volume de pedidos foi o principal componente matemático da queda de receita, representando aproximadamente 76,69% da variação negativa.

A diminuição da receita média por pedido respondeu pelos 23,31% restantes.

Essa decomposição permite quantificar a participação de cada componente na variação observada. Entretanto, trata-se de uma atribuição matemática, não de uma demonstração de causalidade. A participação calculada também depende da ordem adotada na decomposição, pois o efeito de interação entre os componentes é atribuído ao segundo componente.

## 5. Evidências estatísticas

Foram realizadas análises estatísticas complementares para investigar se os padrões observados entre maio e junho apresentavam evidências de assimetria.

### 5.1. Direção das variações de receita dos produtos

Entre os produtos analisados:

- 1.224 apresentaram redução de receita;
- 1.041 apresentaram aumento de receita;
- 148 permaneceram estáveis.

O teste binomial exato apresentou **p-valor de 0,000130**, indicando evidência estatística de uma frequência maior de reduções do que de aumentos entre os produtos com variação diferente de zero.

**Interpretação:**

O resultado reforça que a queda não se limitou a poucos produtos isolados. Houve uma assimetria estatisticamente significativa na direção das variações.

Entretanto, a frequência de produtos com redução não representa, por si só, a magnitude financeira das perdas. Produtos diferentes podem apresentar impactos muito distintos sobre a receita total.

### 5.2. Comportamento da receita média por produto

Foram identificados **399 produtos com vendas nos dois meses**, permitindo comparar sua receita média por pedido entre maio e junho.

O teste de Wilcoxon apresentou **p-valor de 0,850340**, sem evidência estatística de uma mudança sistemática na receita média por pedido desse grupo.

**Interpretação:**

Não foram encontradas evidências de uma alteração sistemática na receita média por pedido entre os 399 produtos ativos nos dois meses.

Esse resultado não demonstra que os valores permaneceram iguais. Também não deve ser generalizado para todos os produtos, pois a análise considera exclusivamente aqueles com atividade comercial nos dois períodos.

### 5.3. Transições de atividade comercial dos produtos

A comparação da atividade dos produtos identificou as seguintes transições:

| Transição | Quantidade de produtos |
|---|---:|
| Ativos em maio e junho | 399 |
| Ativos em maio e sem vendas em junho | 1.080 |
| Sem vendas em maio e ativos em junho | 934 |
| Sem vendas nos dois meses | 0 |

O teste exato de McNemar apresentou **p-valor de 0,001228**, indicando assimetria estatisticamente significativa entre as interrupções e as entradas de atividade.

**Interpretação:**

Foram identificados mais produtos com vendas em maio e sem vendas em junho do que produtos com o movimento contrário.

Esse resultado indica uma redução líquida na quantidade de produtos com atividade comercial entre os períodos analisados.

É importante destacar que a ausência de vendas em junho não comprova que um produto tenha sido desativado, retirado do catálogo ou ficado indisponível em estoque. O resultado representa exclusivamente a ausência de vendas observadas no período.

## 6. Análise por categorias e produtos

A análise por categorias e produtos buscou identificar onde as perdas de receita estavam mais concentradas.

### 6.1. Categorias com maiores perdas

As dez categorias com maiores perdas brutas acumularam **R$ 135.440,90 em reduções de receita**.

Esse valor é superior à queda líquida do marketplace porque algumas categorias apresentaram crescimento de receita, compensando parcialmente as perdas observadas em outras.

**Interpretação:**

A redução da receita não ocorreu de maneira uniforme entre as categorias. Algumas concentraram perdas expressivas, enquanto outras apresentaram movimentos positivos.

Portanto, a análise das perdas brutas por categoria deve ser diferenciada da contribuição líquida de cada categoria para a variação total do marketplace.

### 6.2. Produtos com maiores reduções de receita

Foi realizada uma investigação complementar sobre os 105 produtos com maiores perdas de receita.

Entre eles, **76 apresentaram receita em maio e nenhuma venda em junho**.

Para esse grupo, os resultados posteriores foram:

| Indicador | Resultado |
|---|---:|
| Receita em maio | R$ 104.243,02 |
| Receita em junho | R$ 0,00 |
| Receita em julho | R$ 7.697,47 |
| Receita em agosto | R$ 4.372,60 |
| Receita acumulada em julho e agosto | R$ 12.070,07 |

A receita acumulada em julho e agosto correspondeu a aproximadamente **11,58% da receita registrada por esses produtos em maio**.

Dos 76 produtos:

- 14 voltaram a apresentar vendas em julho ou agosto;
- 62 não apresentaram vendas nesses dois meses.

**Interpretação:**

A análise posterior mostrou que a ausência de vendas em junho persistiu para a maior parte dos produtos selecionados, embora uma parcela tenha voltado a registrar vendas nos meses seguintes.

A presença dos produtos no catálogo não comprova que estavam efetivamente disponíveis para compra, com estoque ou com anúncios ativos. Portanto, não é possível determinar, com os dados disponíveis, por que deixaram de registrar vendas.

## 7. Análise por vendedores

A investigação dos vendedores buscou avaliar como a variação da receita estava distribuída entre os participantes do marketplace.

Foram identificados:

- 1.115 vendedores ativos em maio;
- 1.175 vendedores ativos em junho;
- 803 vendedores com vendas nos dois meses;
- 312 vendedores com vendas somente em maio;
- 372 vendedores com vendas somente em junho.

A comparação da receita por vendedor identificou:

| Movimento | Quantidade de vendedores |
|---|---:|
| Aumento de receita | 718 |
| Redução de receita | 742 |
| Receita estável | 27 |

Considerando os vendedores com redução de receita, as perdas brutas somaram **R$ 430.279,41**. Entre os vendedores com aumento, os ganhos brutos totalizaram **R$ 298.886,04**.

A diferença entre esses movimentos correspondeu à redução líquida de **R$ 131.393,37** do marketplace.

Os ganhos dos vendedores com aumento de receita compensaram aproximadamente **69,47% das perdas brutas**.

### 7.1. Participação dos dez vendedores com maiores perdas

Os dez vendedores selecionados pelas maiores reduções líquidas de receita acumularam uma perda conjunta de **R$ 94.991,99**, equivalente a **72,30% da redução líquida total do marketplace**.

**Interpretação:**

Esse resultado indica uma participação expressiva desses dez vendedores na variação líquida observada.

Entretanto, a seleção foi realizada justamente com base nas maiores reduções de receita. Assim, a participação encontrada deve ser interpretada considerando esse critério de seleção, e não como evidência de concentração equivalente em uma amostra aleatória de vendedores.

### 7.2. Análise das combinações vendedor-produto

Para aprofundar a investigação, foram analisadas as combinações de vendedores e produtos associadas aos dez vendedores selecionados.

Foram identificadas 183 combinações, distribuídas da seguinte maneira:

| Classificação | Quantidade de combinações | Variação de receita |
|---|---:|---:|
| Interrupção | 84 | -R$ 62.005,20 |
| Redução | 37 | -R$ 46.967,96 |
| Estabilidade | 3 | R$ 0,00 |
| Aumento | 13 | +R$ 2.894,30 |
| Entrada em junho | 46 | +R$ 11.086,87 |
| **Total** | **183** | **-R$ 94.991,99** |

As interrupções e reduções representaram perdas de **R$ 108.973,16**, parcialmente compensadas por aumentos e entradas, que somaram **R$ 13.981,17**.

**Interpretação:**

Entre as combinações analisadas, as interrupções de vendas foram o movimento mais frequente e representaram uma parcela expressiva das perdas.

Entretanto, a classificação como interrupção significa apenas que determinada combinação vendedor-produto apresentou vendas em maio e nenhuma venda em junho. Não comprova encerramento do anúncio, indisponibilidade de estoque ou saída definitiva do vendedor.

Além disso, essa análise está restrita às combinações dos dez vendedores selecionados, não representando necessariamente todo o marketplace.

## 8. Diagnóstico integrado

A integração dos resultados permite caracterizar a queda de receita sob diferentes perspectivas.

Primeiramente, o marketplace apresentou uma redução de **R$ 131.393,37 na receita entre maio e junho de 2018**, equivalente a 13,19%.

A decomposição matemática indicou que a redução do volume de pedidos foi o principal componente da variação negativa, respondendo por 76,69% da queda. A redução da receita média por pedido representou os 23,31% restantes.

As análises estatísticas também identificaram maior frequência de produtos com redução de receita e mais interrupções de atividade comercial do que entradas entre os dois meses.

A investigação por categorias, produtos e vendedores demonstrou que as perdas estavam distribuídas de maneira desigual. Algumas categorias concentraram reduções expressivas, 76 produtos entre os 105 selecionados pelas maiores perdas não registraram vendas em junho, e os dez vendedores selecionados pelas maiores reduções líquidas responderam por 72,30% da queda líquida total.

Em conjunto, as evidências indicam que a queda de receita esteve associada principalmente à redução do volume de pedidos, acompanhada por uma diminuição da receita média por pedido e por uma distribuição heterogênea das perdas entre produtos, categorias e vendedores.

Esses resultados caracterizam os movimentos observados, mas não permitem determinar quais fatores operacionais ou comerciais os provocaram.

## 9. Limitações da análise

A interpretação dos resultados deve considerar as seguintes limitações:

- **Janela temporal:** a comparação principal considera maio e junho de 2018, dois meses consecutivos. Esse recorte não permite identificar tendências de longo prazo, sazonalidade ou padrões recorrentes.
- **Ausência de evidência causal:** as análises identificam associações, diferenças e padrões, mas não demonstram que determinado fator provocou a queda.
- **Decomposição matemática:** a participação dos efeitos de volume e receita média depende da metodologia e da ordem da decomposição.
- **Magnitude financeira:** a quantidade de produtos com redução não representa necessariamente a participação desses produtos na perda total de receita.
- **Ausência de vendas:** não comprova indisponibilidade de estoque, desativação de anúncios ou descontinuação de produtos.
- **Catálogo de produtos:** a presença de um produto no catálogo não comprova que ele estava efetivamente disponível para compra.
- **Seleção de vendedores e produtos:** algumas análises aprofundadas foram realizadas sobre grupos selecionados pelas maiores perdas, o que restringe a generalização dos resultados.
- **Dimensões sobrepostas:** categorias, produtos e vendedores representam perspectivas diferentes e podem envolver as mesmas transações. Portanto, suas perdas não devem ser somadas como se fossem impactos independentes.

### Hipóteses ainda não comprovadas

Com os dados disponíveis, não é possível confirmar se a queda de receita foi provocada por:

- Indisponibilidade de estoque;
- Desativação ou alteração de anúncios;
- Redução da demanda dos consumidores;
- Mudanças na visibilidade dos produtos;
- Alterações de preços ou condições comerciais;
- Encerramento da atividade comercial de vendedores;
- Descontinuação de produtos.

Esses fatores devem ser tratados como hipóteses para investigações posteriores, e não como conclusões deste estudo.

## 10. Direcionamentos para investigações futuras

Para aprofundar o diagnóstico e investigar possíveis causas da redução de receita, recomenda-se obter dados complementares e desenvolver análises específicas.

| Frente de investigação | Perguntas principais | Dados necessários |
|---|---|---|
| Estoque e disponibilidade | Os produtos estavam disponíveis para compra? Houve ruptura de estoque? | Histórico de estoque e disponibilidade |
| Atividade dos vendedores | Os vendedores mantiveram seus anúncios e operações ativos? | Histórico de anúncios e atividade dos vendedores |
| Demanda e conversão | Houve redução de visitas, interesse ou conversão? | Tráfego, visitas, visualizações e conversões |
| Condições comerciais | Ocorreram alterações de preço, publicidade ou exposição dos produtos? | Histórico de preços, campanhas e posicionamento |
| Evolução temporal | A queda foi pontual ou parte de uma tendência? | Série histórica de vendas de meses anteriores e posteriores |

A obtenção dessas informações permitirá testar as hipóteses levantadas e avaliar com maior precisão os fatores associados à variação da receita.

## 11. Conclusão

A investigação identificou uma redução de **13,19% na receita do marketplace entre maio e junho de 2018**, correspondente a R$ 131.393,37.

A decomposição matemática demonstrou que a redução do volume de pedidos foi o principal componente da queda, enquanto a receita média por pedido também apresentou diminuição.

As análises complementares revelaram maior frequência de reduções de receita entre os produtos, mais interrupções do que entradas de atividade comercial e uma distribuição desigual das perdas entre categorias e vendedores.

Os resultados oferecem uma visão estruturada do comportamento da receita e identificam segmentos que merecem investigação adicional. Contudo, os dados analisados não permitem afirmar que a queda tenha sido causada por problemas de estoque, demanda, publicidade, desativação de anúncios ou encerramento de vendedores.

**O diagnóstico estabelece uma base analítica para investigações posteriores, permitindo direcionar a coleta de informações complementares e apoiar decisões futuras com maior fundamentação.**