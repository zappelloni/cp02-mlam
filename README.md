# Regressão Linear com PIB e Índice ABCR

Investigação da relação entre a atividade econômica brasileira (PIB) e o fluxo de veículos nas rodovias pedagiadas (Índice ABCR), com construção de um modelo de regressão linear em Python.

## Pergunta

Existe relação entre o índice de volume do PIB e o Índice ABCR de fluxo total de veículos no Brasil? Um modelo de regressão linear simples, treinado com o histórico, consegue prever o Índice ABCR a partir do PIB?

## Fontes dos dados

| Fonte | Indicador | Onde foi obtido |
|---|---|---|
| IBGE – SIDRA, Tabela 1620 | Índice de volume trimestral do PIB a preços de mercado (Base: média 1995 = 100), série sem ajuste sazonal, Brasil | sidra.ibge.gov.br/tabela/1620 — via API pública |
| ABCR / Tendências Consultoria | Índice ABCR — fluxo total de veículos (leves + pesados), série original, Brasil, base 1999 = 100 | melhoresrodovias.org.br/indice-abcr_2 → aba Brasil → Índice ABCR - Total → "Ver histórico" (planilha .xlsx, aba "(C) Original") |

Período: 20 anos completos, 2006 a 2025.

## Estrutura do repositório
dados/
pib_indice_trimestral.csv # PIB_indice por trimestre (2006-2025), fonte: SIDRA 1620
abcr_indice_mensal.csv # ABCR_indice por mês (2006-2025), fonte: planilha ABCR, aba "(C) Original"
dados_anuais.csv # tabela agregada por ano: Ano, PIB_indice, ABCR_indice (gerada pelo notebook)
regressao_pib_abcr.ipynb # notebook com todo o código e os resultados
README.md

## Metodologia
1. **Coleta**: índice trimestral do PIB (SIDRA 1620) e índice mensal do ABCR (planilha histórica, série original/sem ajuste sazonal), ambos para o Brasil, 2006-2025.
2. **Agregação**: para cada ano, `PIB_indice` é a média dos 4 valores trimestrais e `ABCR_indice` é a média dos 12 valores mensais — sempre em número-índice, nunca em variação percentual.
3. **Análise**: gráfico de dispersão PIB x ABCR e correlação de Pearson entre as duas séries anuais.
4. **Modelo**: `sklearn.linear_model.LinearRegression`, treinado com `PIB_indice` como entrada (X) e `ABCR_indice` como alvo (y).
   - Treino: 2006–2021 (16 primeiros anos).
   - Teste: 2022–2025 (4 últimos anos).
   - Ordem cronológica preservada, sem embaralhar os dados.
5. **Avaliação**: MAE, MSE e R² no conjunto de teste, com tabela comparando valores observados e previstos.
## Resultados (resumo)
- Correlação de Pearson entre PIB_indice e ABCR_indice: **≈ 0,96** (forte e positiva).
- No conjunto de teste (2022-2025): **MAE ≈ 4,9**, **MSE ≈ 25,9**, **R² ≈ 0,49**.
- O modelo acompanha a tendência geral, mas tende a superestimar levemente o Índice ABCR nos últimos anos — o fluxo de veículos cresceu um pouco menos do que a relação histórica entre PIB e ABCR sugeriria.
Detalhes, gráficos e a discussão completa (incluindo dificuldades encontradas e soluções adotadas) estão no notebook `regressao_pib_abcr.ipynb`.
## Conclusão
A correlação é alta, mas **não implica causalidade**: PIB e Índice ABCR provavelmente respondem a fatores econômicos comuns (nível de atividade, consumo, produção), e não necessariamente um causa o outro diretamente. Com apenas 20 observações anuais, os resultados devem ser lidos como exploratórios, especialmente por incluir eventos atípicos como a crise de 2008-2009 e a pandemia de 2020.
## Como reproduzir
```bash
pip install pandas numpy matplotlib scikit-learn jupyter
jupyter notebook regressao_pib_abcr.ipynb
