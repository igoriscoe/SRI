⚙️ Detalhamento Técnico do Pipeline (Notebooks)

🧩 **1. 1_1_Segmentacao_Limpeza**
Objetivo: Processar o texto bruto, mitigando ruídos estruturais e segmentando o corpus em unidades sentenciais padronizadas.

Dados de Entrada: data/documentos.csv contendo os campos ["id", "documento"].
https://huggingface.co/datasets/celsowm/imdb-reviews-pt-br
Dataset IMDB Reviews

Processamento Computacional:

Aplicação da biblioteca ftfy para decodificação e correção de entidades HTML e caracteres corrompidos.

Substituição sistemática de quebras de linha (\n) por espaços para evitar quebras semânticas.

Normalização de pontuações redundantes (ex: mapeamento de ??? ou !!! para caracteres únicos).

Remoção de tags e metadados com sintaxe HTML/XML.

Filtro de Extensão (Restrição BERT): Truncamento e descarte de textos que excedam o limite técnico de 512 tokens, garantindo compatibilidade com arquiteturas baseadas em Transformers.

Segmentação do texto limpo em sentenças estruturadas.

Artefato de Saída: data/dataset.zip (contendo o arquivo dataset.csv com o esquema ["id", "sentencas", "documento"]).

🏷️ **2. 1_2_GerarPOS**
Objetivo: Extrair a estrutura gramatical e realizar a etiquetagem morfossintática (Part-of-Speech Tagging) de cada token.

Dados de Entrada: data/dataset.zip.

Processamento Computacional:

Descompactação e leitura do pipeline anterior.

Processamento das sentenças através do modelo de linguagem da biblioteca spaCy configurado para o idioma português.

Mapeamento morfológico e classificação sintática (Substantivos, Adjetivos, Pronomes, etc.).

Processo de Lematização: Redução das palavras às suas formas primitivas/infinitivas, ideal para padronização em sistemas de busca.

Extração isolada de estruturas verbais e predicados.

Artefato de Saída: data/datasetpos.zip (contendo datasetpos.csv no formato ["id", "pos_documento"]).

🏢 **3. 1_3_NER_SPacY**
Objetivo: Detectar, extrair e categorizar núcleos de informação semântica de alta relevância (Entidades Nomeadas).

Dados de Entrada: data/dataset.zip e data/datasetpos.zip.

Processamento Computacional:

Cruzamento de dados textuais com as anotações geradas no estágio de PoS-Tagging.

Submissão das sentenças ao algoritmo de inferência de entidades (Named Entity Recognition) do spaCy.

Identificação e classificação de termos em categorias-alvo:

PER (Pessoas/Entidades Individuais)

ORG (Organizações/Instituições/Empresas)

LOC (Localizações/Pontos Geográficos)

Extração precisa dos índices de início e fim (spans) de cada entidade detectada no texto.

Artefato de Saída: data/datasetner.zip (contendo datasetnes.csv no formato ["id", "ner_documento"]).

📊 **4. 2_1_AnaliseDados**
Objetivo: Agregar o conhecimento gerado em todas as etapas, computar distribuições estatísticas e prover diagnósticos analíticos sobre o corpus.

Dados de Entrada: Integração analítica dos arquivos dataset.zip, datasetpos.zip e datasetner.zip.

Processamento Computacional:

Consolidação e junção de dados (merge/join) via biblioteca Pandas utilizando o identificador único (id).

Cálculo de métricas descritivas centrais: tabelas contendo médias, desvios padrão, valores mínimos, quartis e máximos de frequência para cada classe gramatical por documento.

Agrupamentos analíticos para mensuração volumétrica dos tipos de entidades nomeadas descobertas.

Geração de gráficos estatísticos avançados (distribuições de frequência, histogramas e curvas de densidade de tokens) utilizando as bibliotecas de visualização Matplotlib e Seaborn.

Avaliação de métricas de densidade textual, como proporção de substantivos por verbos e extensões absolutas de sentenças.

Artefato de Saída: Exibição de matrizes dinâmicas de dados e plots estatísticos embutidos para validação do pipeline de SRI.
