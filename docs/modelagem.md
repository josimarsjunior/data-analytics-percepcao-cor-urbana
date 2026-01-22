# Case de Análise de Dados – Experiência do Usuário e Percepção da Cor em Espaços Urbanos (Power BI)

## Visão Geral
Este projeto utiliza um **modelo dimensional estruturado no Power BI**, construído a partir de dados reais de pesquisa, com foco em **análise de percepção afetiva**, **segmentação de usuários** e **comparação entre cenários cromáticos**.

A modelagem foi pensada para garantir:
- Clareza analítica  
- Boa performance  
- Facilidade de criação de métricas DAX  
- Aderência às boas práticas de BI aplicadas a dados de survey  

---

## Estratégia de Modelagem
Adotou-se um **modelo em estrela estendida**, no qual existe:

- Uma **tabela fato central**, que registra o evento analítico principal  
- Múltiplas **tabelas dimensão**, responsáveis por fornecer contexto às análises  

Esse tipo de modelagem é especialmente adequado para:
- Dados de questionários  
- Pesquisas comportamentais  
- Análises perceptivas e subjetivas  

---

## Tabela Fato

### Medição_do_Afeto
Tabela central do modelo, responsável por armazenar as **respostas afetivas dos usuários** frente aos diferentes estímulos cromáticos.

#### Origem
Planilha 07 – *Medição do Afeto*

#### Campos
- `Cod_Usuario` – Identificador do respondente  
- `Cod_Cor` – Identificador da cor apresentada  
- `Cod_Adjetivo` – Identificador do adjetivo afetivo  
- `Grau` – Grau da resposta afetiva  

#### Papel Analítico
Esta tabela representa o **nível mais granular da análise**, concentrando os registros que permitem:
- Cálculo de afeto positivo e negativo  
- Comparações entre cores  
- Análises segmentadas por perfil e comportamento  
- Consolidação de indicadores afetivos  

---

## Tabelas Dimensão

### Dim_Cores
Dimensão responsável por caracterizar os **cenários cromáticos avaliados**.

#### Origem
Planilha 01 – *Cores*

#### Campos
- `Cod_Cor` – Identificador da cor  
- `Nome_Cor` – Nome da cor  
- `Temperatura` – Classificação cromática (quente/fria)  

#### Uso Analítico
Utilizada para:
- Comparações entre cores  
- Agrupamentos cromáticos  
- Análises visuais e afetivas por cenário  

---

### Dim_Perguntas_Afeto
Dimensão que organiza os **adjetivos utilizados na mensuração afetiva**.

#### Origem
Planilha 02 – *PerguntasAfeto*

#### Campos
- `Cod_Adjetivo` – Identificador do adjetivo  
- `Adjetivos` – Descrição do adjetivo  
- `Tipo` – Classificação do afeto (Positivo ou Negativo)  

#### Uso Analítico
Fundamental para:
- Separar Afeto Positivo (AP) e Afeto Negativo (AN)  
- Criar métricas agregadas  
- Garantir coerência conceitual da análise emocional  

---

### Dim_Resposta_Afeto
Dimensão de apoio que padroniza a **escala de resposta afetiva**.

#### Origem
Planilha 03 – *RespostaAfeto*

#### Campos
- `Grau` – Valor numérico da escala  
- `Resposta` – Descrição da resposta  

#### Uso Analítico
Permite:
- Padronização das escalas  
- Melhor leitura dos resultados  
- Evitar valores ambíguos na tabela fato  

---

### Dim_Usuarios
Dimensão que representa os **participantes da pesquisa**.

#### Origem
Planilha 04 – *Usuários*

#### Campos
- `Cod_Usuario` – Identificador do usuário  
- `Data_Entrevista` – Data da coleta   
- `Teste_Ishihara` – Validação da percepção cromática  

#### Uso Analítico
Utilizada para:
- Controle de qualidade da amostra  
- Segmentações por condição visual  
- Rastreabilidade das respostas  

---

### Dim_Sociodemografico
Dimensão responsável pela **caracterização social e demográfica** dos usuários.

#### Origem
Planilha 05 – *Sociodemográfico*

#### Campos
- `Cod_Usuario`  
- `FaixaEtaria`  
- `Genero`  
- `Grau_de_Escolaridade`  
- `Idade`  
- `Residente_da_Cidade`  
- `Bairro`  
- `Cidade_Origem`  

#### Uso Analítico
Permite análises:
- Demográficas  
- Comparativas por perfil  
- Cruzamentos entre percepção e contexto social  

---

### Dim_Relacao_Praca
Dimensão que descreve a **relação comportamental do usuário com a praça e seus mobiliários urbanos**.

#### Origem
Planilha 06 – *Relação com a Praça*

#### Campos
- `Cod_Usuario`  
- `Frequencia_Visita`  
- `Uso_Mobiliarios`  
- `Mobiliario_Mais_Usado`  
- `Mobiliario_Atencao`  
- `Motivo_Atencao`  
- `Mudaria_Cor`  
- `Cor_Escolhida`  
- `Justificativa`  

#### Uso Analítico
Essencial para:
- Avaliar engajamento com o espaço  
- Analisar preferências declaradas  
- Contextualizar os resultados afetivos  

---

## Relacionamentos

O modelo utiliza uma **estrutura em estrela estendida**, combinando relacionamentos diretos com a tabela fato e relacionamentos indiretos via dimensão Usuários.

### Relacionamentos Diretos com a Tabela Fato (1:N)
- `Dim_Cores[Cod_Cor]` → `Medição_do_Afeto[Cod_Cor]`  
- `Dim_Perguntas_Afeto[Cod_Adjetivo]` → `Medição_do_Afeto[Cod_Adjetivo]`  
- `Dim_Resposta_Afeto[Grau]` → `Medição_do_Afeto[Grau]`  
- `Dim_Usuarios[Cod_Usuario]` → `Medição_do_Afeto[Cod_Usuario]`  

### Relacionamentos Indiretos (via Dim_Usuarios)
- `Dim_Usuarios[Cod_Usuario]` → `Dim_Sociodemografico[Cod_Usuario]` (1:1)  
- `Dim_Usuarios[Cod_Usuario]` → `Dim_Relacao_Praca[Cod_Usuario]` (1:N)  

### Direcionalidade
- Todos os relacionamentos utilizam **filtro unidirecional**  
- O fluxo de filtro ocorre das **dimensões para a tabela fato**, passando pela dimensão Usuários quando aplicável  
- Essa estratégia evita ambiguidade, dependência circular e problemas de contexto em medidas DAX  

---

## Preparação dos Dados (ETL)
O tratamento dos dados foi realizado no **Power Query**, incluindo:
- Padronização de categorias textuais  
- Ajuste de tipos de dados  
- Tratamento de valores nulos  
- Garantia de unicidade das chaves  
- Organização lógica das tabelas  

Essa etapa assegura que os dados estejam **confiáveis, consistentes e prontos para análise**.

---

## Boas Práticas Aplicadas
- Modelo dimensional bem definido  
- Separação clara entre fatos e dimensões  
- Uso de dimensões de escala  
- Métricas concentradas em DAX  
- Estrutura preparada para expansão  

---

## Valor Analítico do Modelo
A modelagem adotada permite:
- Comparar cenários cromáticos de forma objetiva  
- Analisar respostas afetivas por múltiplos recortes  
- Transformar dados subjetivos em métricas acionáveis  
- Sustentar dashboards orientados à decisão  

---

## Considerações Finais
Este modelo demonstra uma abordagem profissional de **Análise de Dados e Business Intelligence**, conectando dados comportamentais, percepção afetiva e visualização analítica em uma estrutura clara, escalável e orientada a resultados.
