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
Parte dos pacientes diagnosticados com Lúpus Eritematoso Sistêmico (SLE) apresentam lesões na substância branca do cérebro que se manifestam através da hiperintensidade do sinal na região da lesão (WMH) em imagens de ressonância magnética ponderadas em T2 ou FLAIR (?). Diante da incerteza sobre a etiologia dessas lesões, busca-se identificar sua etiologia mais provável - isquêmica como no AVC ou desmielinizante como na EM. Para tal, um classificador SVM foi treinado em imagens de ressonância (FLAIR) do cérebro para diferenciar lesões isquêmicas (pacientes com AVC) e desmielinizantes (pacientes com EM). O melhor modelo obtido foi então aplicado às imagens do cérebro de pacientes com SLE para classificar as lesões e responder às seguintes questões:

* As lesões na substância branca do cérebro de pacientes com SLE se assemelham mais a lesões isquêmicas ou desmielinizantes?
* Quais as características das lesões que possivelmente levaram o classificador a tomar tal decisão?

### Ferramentas
<!-- Listagem das ferramentas utilizadas (na forma de itens). -->

Todos os processamentos deste projeto foram realizados em Python (versão) através de Jupyter Notebooks com o Gooble Colab. Os notebooks podem ser encontrados no diretório /notebooks. (?)

### Preparo e uso dos dados
<!-- Descreva o pipeline de pré-processamento dos dados: normalização (se houver); outros processamentos; uso das máscaras (se houver); extração de atributos (se houver); seleção de atributos (se houver). -->

As imagens de ressonância (FLAIR) dos pacientes (AVC ou EM) foram pré-processadas de diferentes formas para avaliar seu impacto na classificação. Realizamos testes com as imagens sem normalização (N0) ou normalizadas por Mínimo e Máximo (N1), considerando a imagem completa (M0) ou apenas a região de interesse (lesões) definida pelas máscaras (M1). Após alguns testes realizados, obtivemos melhores resultados com as imagens normalizadas e a aplicação das máscaras.

Avaliamos atributos baseados em histograma, GLCM e RLM. Para o primeiro conjunto de atributos, apenas os pixels da imagem correspondentes à região de interesse foram considerados no cálculo do histograma. Já para os atributos de GLCM e RLM, foi necessário definir um bounding box, ou seja, cortar as imagens em retângulos que comportam toda a sua região de interesse, atribuindo intensidade zero aos pixels fora da região.

Os X atributos considerados (X de histograma, X de GLCM e X de RLM) passaram por um processo de selelção empírica, aplicando diferentes conjuntos de atributos ao classificador e verificando aqueles que obtiveram os melhores resultados. Os atributos de RLM não demonstraram bom desempenho e não foram utilizados pelo classificador final.





foram normalizadas por Mínimo e Máximo 

O pipeline fiinal de pré-processamento das imagens de ressonância (FLAIR) dos pacientes (AVC, EM ou SLE) consistiu em:

* Normalizar as imagens por Mínimo e Máximo. (EXPLICAR)
* Aplicar as máscaras da lesão 



## Metodologia
<!-- Descreva o classificador escolhido e o pipeline de treinamento: split dos dados de treinamento; escolha de parâmetros do classificador; validação cruzada; métricas de avaliação; resultados do treinamento do classificador usando tabelas e gráficos.
Justificar as escolhas. Esta parte do relatório pode ser copiada da Atividade 11, caso o grupo opte por usar o SVM já treinado. -->

FALAR DO SVM

O conjunto de dados de treinamento disponibilizado é composto por 50 pacientes de AVC e 51 pacientes de EM. As imagens de 10 pacientes de cada classe (escolhidos aleatoriamente, cerca de 20% do conjunto) foram separadas em um conjunto de validação, enquanto as demais imagens foram utilizadas para uma validação cruzada dos modelos considerando diferentes pipelines de pré-processamento (com ou sem normalização, imagem inteira ou região de interesse, diferentes conjuntos de atributos) e parâmetros do classificador. Utilizamos uma validação cruzada com cinco folds e os modelos que apresentaram melhor desempenho foram aplicados ao conjunto de validação.

Utilizamos como métricas de avaliação do modelo sua acurácia e o recall de cada classe (AVC ou EM). No caso da validação cruzada, os melhores modelos foram definidos de acordo com as médias das métricas das cinco etapas de treino/validação.



a ser aplicado aos melhores modelos obtidos nos diferentes testes realizados.


O conjunto de dados de treinamento disponibilizado foi subdividido em um conjunto de validação

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
