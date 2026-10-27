# Aplicação de Machine Learning na Identificação de Casos de Superação Acadêmica em Contextos de Vulnerabilidade Social Utilizando Microdados do ENEM 2023


## **João Pedro Lopes Mendes RA: 10400793 / Luis Felipe Cunha RA: 10419514**

Este projeto foi desenvolvido para a disciplina de Inteligência Artificial com o objetivo de treinar um modelo de Machine Learning capaz de prever e analisar os fatores associados à superação acadêmica, utilizando os microdados do ENEM 2023.

## Contexto
Embora a literatura acadêmica documente forte correlação entre renda e rendimento acadêmico no ENEM, existem estudantes que atingem desempenhos muito acima da média esperada para sua faixa socioeconômica (o que a literatura chama de superação acadêmica). Diante disso, o modelo foi treinado para identificar padrões dos alunos que alcançaram o topo (≈10%) das notas dentro de sua respectiva faixa de renda (em salários-mínimos).

<img width="1282" height="590" alt="image" src="https://github.com/user-attachments/assets/8ecc9baa-2392-4b0e-b0d4-60c073403db8" />


O projeto busca identificar fatores socioeducacionais associados ao desempenho acima do esperado para a faixa de renda, contribuindo para estudos e ações voltados à equidade educacional.

## O que o projeto faz
- Analisa os microdados do ENEM 2023;
- Filtra participantes válidos para a análise;
- Constrói um indicador de “superação” com base na comparação entre nota real e nota esperada para a mesma faixa de renda;
- Treina um modelo para prever quais alunos estão acima do esperado;
- Avalia quais fatores têm maior influência no desempenho.

## Tecnologias, bibliotecas e métricas utilizadas:
- Python  
- Pandas
- NumPy
- Scikit-learn
- Catboost
- Matplotlib
- Seaborn
- Scipy
- CatBoostClassifier (modelo principal)  
- StratifiedGroupKFold (validação por município)  
- Early stopping (validação interna)
- Engenharia de features (tratamento de categorias, criação de ANOS_DESDE_CONCLUSAO, etc.)  
- Permutation importance (importância por permutação no teste externo)  
- ROC‑AUC
- PR‑AUC (average precision)
- lift

## Como o experimento foi desenvolvido
O processo foi organizado em 5 etapas:

1. **Preparação dos dados e Engenharia de Features:**  
   Foram considerados apenas participantes com presença nas 4 provas objetivas, renda informada e notas completas. Isso garante que a análise seja comparável e consistente. Na parte de engenharia de features, renda não foi utilizada como feature direta no modelo e município foi removido como variável para evitar data leakage, pois durante o desenvolvimento do experimento o modelo decorava a variável e assim inflava o ROC‑AUC (data leak);

2. **Construção do target:**  
   Para cada faixa de renda, foi calculada a nota esperada e o desvio entre a nota real e a nota esperada. Os estudantes acima de um limiar específico (P90 do resíduo do treino) foram classificados como casos de superação acadêmica. 

3. **Modelagem:**  
   O modelo principal utilizado foi o CatBoostClassifier, que é altamente eficiente para dados com variáveis categóricas e mostrou boa performance no problema.

4. **Validação:**  
   Para evitar resultados artificialmente bons, o projeto utilizou validação externa por município, em que municípios inteiros do conjunto de teste ficaram fora do treino. Isso reduz o risco de vazamento de dados e torna a avaliação mais realista.

5. **Interpretação do modelo:**  
   Foram aplicadas métricas como ROC‑AUC e PR‑AUC, além de importância por permutação para identificar quais variáveis mais influenciam o modelo.

## Principais resultados
- População final analisada: 2.678.264 registros
- Modelo principal: CatBoost
- ROC‑AUC OOF: aproximadamente 0,73
- PR‑AUC OOF: aproximadamente 0,227
- Melhoria média sobre o baseline: aproximadamente +0,061 em ROC‑AUC e +0,059 em PR‑AUC

## Interpretação das features
As variáveis que mais influenciaram o modelo foram:
- TP_FAIXA_ETARIA: O modelo indica que participantes entre 17 e 20 anos tendem a apresentar maior probabilidade de desempenho acima do esperado. Já as faixas etárias mais distantes dessa idade tendem a apresentar probabilidades menores.
- ANOS_DESDE_CONCLUSAO: Complementa o resultado da faixa etária, indicando que o tempo desde a conclusão do ensino médio também pode estar relacionado ao desempenho. 
- SG_UF_PROVA: O estado onde a prova foi aplicada agrega contexto regional, como diferenças na infraestrutura e condições educacionais (esse fator funciona como proxy de contexto local)
- OPCAO_INGLES:: A escolha do inglês pode indicar que o participante teve acesso a mais recursos educacionais, como cursos de idiomas e materiais de estudo.
- TP_COR_RACA: aparece como variável relevante e deve ser interpretada como sinal de desigualdades estruturais refletidas nos dados. Esse resultado indica que diferenças associadas a raça/etnia podem influenciar o desempenho e reforça a importância de análises de equidade e políticas públicas.
TP_DEPENDENCIA_ADM_ESC / TP_ESCOLA: Essas variáveis identificam o tipo de escola em que o participante estudou e sua dependência administrativa. Entre as categorias estão escolas públicas federais, estaduais e municipais, além de escolas privadas. Essas diferenças podem estar relacionadas às condições de ensino, à infraestrutura e ao acesso a recursos educacionais.

## Conclusão

O modelo alcançou ROC-AUC de 0,73, indicando uma capacidade de classificação moderada. Mesmo assim, conseguiu identificar padrões associados ao desempenho acima do esperado para a faixa de renda, mostrando o potencial do modelo para analisar fatores educacionais e desigualdades sociais.


