# Projeto 4 – Classificação de lesões de substância branca no Lúpus

<!-- O objetivo geral do projeto é, a partir de uma classificador treinado em imagens de ressonância do cérebro para diferenciar lesões isquêmicas e desmielinizantes, identificar qual a etiologia mais provável das lesões presentes em pacientes de Lúpus Eritematoso Sistêmico (LES).

A equipe pode usar qualquer tipo de classificador para a tarefa, desde o SVM já treinado e entregue na Atividade 11, como outro classificador baseado ou não em DL. Os dados de teste não devem ser incorporados no treinamento do classificador.

O conjunto de dados de lesões de pacientes de LES foram compartilhados pelo Google Drive (link nas instruções do P4 no Classroom).

Para o processamento dos dados e treinamento do classificador, sugere-se usar notebooks (e.g., Jupyter). -->

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [Ciência e Visualização de Dados em Saúde](https://ds4h.org/), oferecida no primeiro semestre de 2022, na Unicamp.

| Nome | RA | Especialização |
| --- | --- | --- |
| Caio Pinheiro Santana | 218653 | Elétrica |
| Bruno Rangel Balbino dos Santos | 218450 | Elétrica |

## Introdução
<!-- Apresentação de forma resumida do problema (contexto) e a pergunta que se quer responder. -->
Parte dos pacientes diagnosticados com Lúpus Eritematoso Sistêmico (SLE) apresentam lesões na substância branca do cérebro que se manifestam através da hiperintensidade do sinal na região da lesão em imagens de ressonância magnética FLAIR ou ponderadas em T2[^1]. Diante da incerteza sobre a etiologia dessas lesões, busca-se identificar sua etiologia mais provável - isquêmica como no Acidente Vascular Cerebral (AVC) ou desmielinizante como na Esclerose Múltipla (EM). Para tal, um classificador SVM foi treinado em imagens de ressonância (FLAIR) do cérebro para diferenciar lesões isquêmicas (pacientes com AVC) e desmielinizantes (pacientes com EM). O melhor modelo obtido foi então aplicado às imagens do cérebro de pacientes com SLE para classificar as lesões e responder às seguintes questões:

* As lesões na substância branca do cérebro de pacientes com SLE se assemelham mais a lesões isquêmicas ou desmielinizantes?
* Quais as características das lesões que possivelmente levaram o classificador a tomar tal decisão?

### Ferramentas
<!-- Listagem das ferramentas utilizadas (na forma de itens). -->

Todos os processamentos deste projeto foram realizados em Python (versão) através de Jupyter Notebooks com o Gooble Colab. Os notebooks podem ser encontrados no diretório /notebooks. (?)

### Preparo e uso dos dados
<!-- Descreva o pipeline de pré-processamento dos dados: normalização (se houver); outros processamentos; uso das máscaras (se houver); extração de atributos (se houver); seleção de atributos (se houver). -->

As imagens de ressonância dos pacientes (AVC ou EM) foram pré-processadas de diferentes formas para avaliar seu impacto na classificação. Realizamos testes com as imagens sem normalização - já que em atividades anteriores não notamos muito impacto na extração de atributos - ou normalizadas por Mínimo e Máximo - escolhida para testes devido à sua simplicidade e ao bom comportamento do intervalo de valores obtidos. Consideramos também as imagens completas ou apenas as regiões de interesse (lesões) definidas pelas máscaras.

Ao analisar os dados, notamos algumas máscaras incorretas - compreendendo mais da metade do cérebro do paciente, as vezes considerando o crânio ou até mesmo o fundo da imagem. Percebemos também que a grande maioria das máscaras possui poucos pixels de região de interesse, mas existem outliers com número muito elevado. Assim, após realizar alguns testes (considerando diferentes thresholds e visualizando as máscaras e imagens correspondentes), acabamos definindo um limite para as regiões de interesse de 30 mil pixels nas imagens de AVC e de 6 mil pixels nas imagens de EM, de modo a excluir as máscaras que apresentam erros gritantes.

Note que não possuímos propriedade suficiente para definir se uma máscara está ou não incorreta, esse processo deveria ser realizado com auxílio de um especialista. No entanto, como nos deparamos com problemas bem sérios em algumas máscaras, optamos por desconsiderar ao menos algumas delas.

Avaliamos atributos baseados em histograma, Matriz de Co-Ocorrência (GLCM) e Matriz de Comprimento de Corrida (RLM). Através de análises anteriores, percebemos que os histogramas das imagens pareciam se assemelhar dentro de cada classe, mas apresentar comportamentos diferentes entre as classes. Dessa forma, focamos inicialmente nos atributos de histograma. Optamos por investigar também se outros atributos de textura (GLCM e RLM) ajudariam na classificação.

Vale destacar que, ao aplicar as máscaras, a extração dos atributos variou um pouco. Para os de histograma, apenas os pixels da imagem correspondentes à região de interesse foram considerados. Já para os atributos de GLCM e RLM, foi necessário definir um bounding box, ou seja, cortar as imagens em retângulos que comportam toda a sua região de interesse, atribuindo intensidade zero aos pixels fora da região. A ideia era extrair atributos com base apenas nas lesões, já que estamos mais interessados em suas características. No entanto, isso não foi possível para os atributos de GLCM e RLM, de modo que a abordagem do bounding box pareceu a mais próxima possível.


Vale discutir depois essa questão do bounding box!!!!!


Os 25 atributos considerados (14 de histograma, 6 de GLCM e 5 de RLM) passaram por um processo de seleção empírica, aplicando diferentes conjuntos de atributos ao classificador e selecionando aqueles que obtiveram os melhores resultados.

## Metodologia
<!-- Descreva o classificador escolhido e o pipeline de treinamento: split dos dados de treinamento; escolha de parâmetros do classificador; validação cruzada; métricas de avaliação; resultados do treinamento do classificador usando tabelas e gráficos.
Justificar as escolhas. Esta parte do relatório pode ser copiada da Atividade 11, caso o grupo opte por usar o SVM já treinado. -->

FALAR DO SVM

A partir do conjunto de dados disponibilizado, consideramos apenas as imagens para as quais existe uma máscara correspondente (ou seja, apenas as imagens que certamente apresentam alguma lesão). Dessa forma, o conjunto de treinamento foi composto por 50 pacientes de AVC (581 imagens) e 51 pacientes de EM (630 imagens). Note que, nas análises utilizando as máscaras, o número de pacientes se manteve o mesmo, mas o número de imagens passou a ser 538 de AVC e 611 de EM devido às máscaras excluídas.

As imagens de 10 pacientes de cada classe (escolhidos aleatoriamente, cerca de 20% do conjunto) foram separadas em um conjunto de validação. As demais imagens foram utilizadas para uma validação cruzada dos modelos, considerando diferentes pipelines de pré-processamento <!--(com ou sem normalização, imagem inteira ou região de interesse, diferentes conjuntos de atributos) -->e parâmetros do classificador.

Utilizamos uma validação cruzada com cinco folds. Para cada escolha de normalização e uso de máscara, selecionamos os conjuntos de atributos com melhor desempenho, então avaliamos o kernel do SVM (RBF, linear, polinomial ou sigmoid) e diferentes valores para os parâmetros C e gamma (que fazem o que?). Os modelos que apresentaram melhor desempenho foram treinados em todo o conjunto de validação cruzada e aplicados ao conjunto de validação.

As métricas de avaliação do modelo foram sua acurácia e o recall de cada classe (AVC ou EM) (pq? a acurácia para ter uma visão geral do desempenho do classificador e o recall (tipo sensibilidade e especificidade) para observar o desempenho em cada classe mais de perto e seu balanceamento). No caso da validação cruzada, os melhores modelos foram definidos de acordo com as médias das métricas das cinco etapas de treino/validação.

Os atributos de RLM não demonstraram bom desempenho e não foram utilizados pelo classificador final. Já os atributos de histograma realmente se destacaram, principalmente os percentis, a moda e a média. Em especial, o percentil 90 (p90) sozinho obteve mais de 91% de acurácia na validação cruzada não considerando as máscaras e de 93% ao considerá-las - em ambos os casos sem normalização. Além disso, ele estava entre os atributos utilizados por todos os melhores classificadores que obtivemos. Os atributos de GLCM ajudaram pontualmente no desempenho da classificação, mas aliados aos de histograma. Vale ressaltar que o fato de não termos conseguido obter os atributos de GLCM e RLM considerando apenas a região de interesse pode ter afetado significativamente seu comportamento.

Nos primeiros resultados da validação cruzada, o kernel RBF mostrou melhor desempenho. Então, ele foi fixado para os testes seguintes. Os parâmetros C e gamma variaram para cada pipeline de pré-processamento avaliada. Por fim, obtivemos melhores resultados com as imagens normalizadas por Mínimo e Máximo e considerando as máscaras. A tabela abaixo apresenta os resultados obtidos pelos modelos aplicados ao conjunto de validação.

TABELA 1 - Características dos melhores modelos e seus resultados de validação.
| Normalização |           Máscaras          |   Atributos de histograma   | Atributos de GLCM |   C  | gamma | Recall AVC | Recall EM | Acurácia |
|:------------:|:---------------------------:|:---------------------------:|:-----------------:|:----:|:-----:|:----------:|:---------:|:--------:|
|      Não     |             Não             |       p90, média e p75      |         -         |   1  | scale |    98.2%   |   78.0%   |   87.3%  |
|      Não     |             Não             | percentis (50, 75, 90 e 99) |         -         | 1000 | scale |    99.1%   |   80.3%   |   89.0%  |
|      Não     |             Sim             |      p90, moda e média      |         -         |  10  | 0.001 |    98.1%   |   83.2%   |   90.3%  |
|      Não     | Sim (histograma) Não (GLCM) |      p90, moda e média      |      contrast     |  100 | 0.001 |    96.3%   |   86.6%   |   91.2%  |
|      Sim     |             Sim             |       p90, moda e p25       |    homogeneity    |  100 | scale |    99.1%   |   87.4%   |   92.9%  |

Dessa forma, o melhor classificador encontrado possuía as seguintes características:

* Imagens normalizadas por Máximo e Mínimo;
* Atributos de histograma considerando a máscara: moda e percentis 90 e 25;
* Atributos de GLCM considerando o bounding box: homogeneity;
* SVM com kernel rbf (default) e C = 100.

Tal classificador foi, então, treinado com todos os dados de treinamento (validação cruzada + validação) para predizer a classe das imagens de teste (225 imagens, uma de cada sujeito). Os resultados obtidos foram: 96.2% de recall para os pacientes de AVC, 99.3% para os pacientes de EM e 98.2% de acurácia. A tabela abaixo apresenta a matriz de confusão correspondente:

TABELA 2 - Matriz de confusão dos resultados no conjunto de teste
![Matriz de confusão - teste](assets/confusion_test.jpg)

## Resultados Obtidos e Discussão
<!-- Esta seção deve apresentar o resultado de predição das lesões de LES usando o classificador treinado. Também deve tentar explicar quais os atributos relevantes usados na classificação obtida: apresente os resultados de forma quantitativa e qualitativa; tenha em mente que quem irá ler o relatório é uma equipe multidisciplinar. Descreva questões técnicas, mas também a intuição por trás delas. -->


## Conclusão
<!-- Destacar as principais conclusões obtidas no desenvolvimento do projeto.
Destacar os principais desafios enfrentados.
Principais lições aprendidas.
Trabalhos Futuros: o que poderia ser melhorado se houvesse mais tempo? -->


## Referências Bibliográficas
<!-- Lista de artigos, links e referências bibliográficas (se houver).
Fiquem à vontade para escolher o padrão de referenciamento preferido pelo grupo. -->

[^1]: POSTAL, M. et al. Magnetic resonance imaging in neuropsychiatric systemic lupus erythematosus: current state of the art and novel approaches. Lupus, v. 26, n. 5, p. 517-521, 2017.
