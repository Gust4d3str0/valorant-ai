# Valorant AI - Detecção de Agentes com YOLOv8

Este projeto foi desenvolvido como prática de visão computacional utilizando o modelo YOLOv8.  
O objetivo é detectar agentes/personagens do jogo Valorant em imagens e vídeos, utilizando um modelo customizado treinado com um dataset no formato YOLO.

O projeto possui duas partes principais:

1. Treinamento de um modelo YOLOv8 customizado.
2. Inferência com o modelo já treinado em imagens e vídeos.

---

## Estrutura do projeto

```text
valorant-ai/
├── notebooks/
│   ├── 09-valorant-treinamento-customizado-yolo.ipynb
│   └── 12-valorant-inferencia-modelo-treinado-entrega.ipynb
│
├── models/
│   └── modelo_valorant.pt
│
├── inputs/
│   ├── teste.mp4
│   └── imagem_teste.jpg
│
├── outputs/
│
├── dataset_valorant/
│
├── runs/
│
├── pyproject.toml
├── poetry.lock
├── requirements.txt
├── .gitignore
└── README.md
````

---

## Explicação das pastas e arquivos

### `notebooks/`

Contém os notebooks usados no projeto.

* `09-valorant-treinamento-customizado-yolo.ipynb`: notebook usado para treinar o modelo YOLOv8 com o dataset customizado.
* `12-valorant-inferencia-modelo-treinado-entrega.ipynb`: notebook preparado para carregar o modelo já treinado e realizar testes com imagem e vídeo.

---

### `models/`

Contém o modelo treinado usado na inferência.

O arquivo principal é:

```text
models/modelo_valorant.pt
```

Esse arquivo é uma cópia renomeada do `best.pt`, que é o melhor checkpoint salvo automaticamente pelo YOLO durante o treinamento.

---

### `inputs/`

Pasta usada para colocar arquivos de teste.

Exemplos:

```text
inputs/teste.mp4
inputs/imagem_teste.jpg
```

O notebook de inferência procura os arquivos nessa pasta para executar os testes.

---

### `outputs/`

Pasta onde o YOLO salva os resultados da inferência.

Ao rodar o notebook, são gerados arquivos com as detecções feitas pelo modelo, como imagens ou vídeos com as caixas desenhadas.

---

### `dataset_valorant/`

Pasta onde o dataset foi utilizado localmente durante o treinamento.

Essa pasta **não foi enviada para o GitHub**, pois o dataset possui muitas imagens e arquivos de anotação, deixando o repositório muito pesado.

O dataset pode ser baixado novamente pela fonte indicada abaixo.

---

### `runs/`

Pasta gerada automaticamente pelo YOLO durante o treinamento e a inferência.

Ela contém resultados como:

* gráficos de treinamento;
* imagens de validação;
* pesos treinados;
* arquivos `best.pt` e `last.pt`;
* saídas de testes.

Essa pasta também não é enviada ao GitHub, pois pode ficar muito pesada.

---

## Dataset utilizado

O dataset utilizado foi obtido no Roboflow Universe:

* Link do dataset: [https://universe.roboflow.com/fps/valorant-gygiq](https://universe.roboflow.com/fps/valorant-gygiq)
* Project ID: `valorant-gygiq-ohaba`

O dataset foi baixado no formato compatível com YOLOv8.

Por conta da grande quantidade de imagens e arquivos de anotação, o dataset completo não foi incluído neste repositório.
Para reproduzir o treinamento, é necessário baixar o dataset diretamente pelo Roboflow e colocá-lo na pasta:

```text
dataset_valorant/
```

A estrutura esperada do dataset é semelhante a:

```text
dataset_valorant/
├── train/
│   ├── images/
│   └── labels/
│
├── valid/
│   ├── images/
│   └── labels/
│
├── test/
│   ├── images/
│   └── labels/
│
└── data.yaml
```

---

## Modelo utilizado

O modelo base utilizado foi:

```text
yolov8n.pt
```

Esse modelo foi usado como ponto de partida para o treinamento customizado.

Após o treinamento, o YOLO gerou automaticamente o arquivo:

```text
best.pt
```

Esse arquivo representa o melhor modelo obtido durante a validação.
Para facilitar o uso no notebook de inferência, ele foi copiado e renomeado para:

```text
models/modelo_valorant.pt
```

---

## Configurações do treinamento

As principais configurações usadas no treinamento foram:

```text
Modelo base: yolov8n.pt
Número de épocas: 30
Tamanho das imagens: 416
Batch size: 2
```

### O que significa o número de épocas?

O número de épocas representa quantas vezes o modelo passa por todo o conjunto de treinamento.

Neste projeto, foram utilizadas **30 épocas**.
Isso significa que o modelo analisou o conjunto de treino 30 vezes, ajustando seus pesos a cada rodada para melhorar a detecção dos agentes/personagens.

---

## Como instalar o projeto

### Opção 1 - Usando Poetry

Este projeto possui os arquivos `pyproject.toml` e `poetry.lock`.

Para instalar as dependências com Poetry:

```bash
poetry install
```

Depois, registre o kernel do Jupyter:

```bash
poetry run python -m ipykernel install --user --name valorant-ai --display-name "valorant-ai (poetry)"
```

Em seguida, abra o notebook e selecione o kernel:

```text
valorant-ai (poetry)
```

---

### Opção 2 - Usando pip

Também é possível instalar as dependências com:

```bash
pip install -r requirements.txt
```

---

## Como rodar a inferência

Para testar o modelo já treinado, abra o notebook:

```text
notebooks/12-valorant-inferencia-modelo-treinado-entrega.ipynb
```

Esse notebook realiza as seguintes etapas:

1. Localiza a pasta principal do projeto.
2. Carrega o modelo treinado em `models/modelo_valorant.pt`.
3. Mostra as classes aprendidas pelo modelo.
4. Testa o modelo em uma imagem.
5. Testa o modelo em um vídeo.
6. Salva os resultados na pasta `outputs/`.

---

## Teste com imagem

Para testar com imagem, coloque uma imagem na pasta `inputs/`.

Exemplo:

```text
inputs/imagem_teste.jpg
```

Depois execute a célula de teste com imagem no notebook.

---

## Teste com vídeo

Para testar com vídeo, coloque um vídeo na pasta `inputs/`.

O nome padrão usado no notebook é:

```text
inputs/teste.mp4
```

Se quiser usar outro nome, altere a variável:

```python
VIDEO_NAME = "teste.mp4"
```

O vídeo processado será salvo na pasta:

```text
outputs/video_teste/
```

Caso o YOLO salve o vídeo em formato `.avi`, o notebook tenta converter automaticamente para `.mp4`, facilitando a visualização dentro do próprio Jupyter Notebook ou VS Code.

---

## Observação sobre arquivos grandes

Alguns arquivos não foram enviados ao GitHub para evitar que o repositório fique muito pesado.

Entre eles:

```text
dataset_valorant/
runs/
clips/
outputs/
arquivos .mp4 grandes
arquivos .zip
```

O arquivo necessário para rodar a inferência é:

```text
models/modelo_valorant.pt
```

Com esse arquivo, não é necessário baixar o dataset para testar o modelo já treinado.

O dataset só é necessário caso o objetivo seja treinar o modelo novamente.

---

## Arquivos importantes para avaliação

Para executar o projeto com o modelo já treinado, os arquivos mais importantes são:

```text
notebooks/12-valorant-inferencia-modelo-treinado-entrega.ipynb
models/modelo_valorant.pt
pyproject.toml
poetry.lock
requirements.txt
README.md
```

---

## Resumo do projeto

Este projeto demonstra o uso de YOLOv8 para treinar e executar um detector customizado de agentes/personagens do Valorant.

O dataset foi utilizado apenas na etapa de treinamento e não foi incluído no repositório por causa do tamanho.
Para facilitar a avaliação, o modelo treinado foi disponibilizado na pasta `models/`, permitindo executar a inferência diretamente em imagens e vídeos sem precisar treinar novamente.

```
```
Substitua seu `README.md` inteiro por este:

````markdown
# Valorant AI - Detecção de Agentes com YOLOv8

Este projeto foi desenvolvido como prática de visão computacional utilizando o modelo YOLOv8.  
O objetivo é detectar agentes/personagens do jogo Valorant em imagens e vídeos, utilizando um modelo customizado treinado com um dataset no formato YOLO.

O projeto possui duas partes principais:

1. Treinamento de um modelo YOLOv8 customizado.
2. Inferência com o modelo já treinado em imagens e vídeos.

---

## Estrutura do projeto

```text
valorant-ai/
├── notebooks/
│   ├── 09-valorant-treinamento-customizado-yolo.ipynb
│   └── 12-valorant-inferencia-modelo-treinado-entrega.ipynb
│
├── models/
│   └── modelo_valorant.pt
│
├── inputs/
│   ├── teste.mp4
│   └── imagem_teste.jpg
│
├── outputs/
│
├── dataset_valorant/
│
├── runs/
│
├── pyproject.toml
├── poetry.lock
├── requirements.txt
├── .gitignore
└── README.md
````

---

## Explicação das pastas e arquivos

### `notebooks/`

Contém os notebooks usados no projeto.

* `09-valorant-treinamento-customizado-yolo.ipynb`: notebook usado para treinar o modelo YOLOv8 com o dataset customizado.
* `12-valorant-inferencia-modelo-treinado-entrega.ipynb`: notebook preparado para carregar o modelo já treinado e realizar testes com imagem e vídeo.

---

### `models/`

Contém o modelo treinado usado na inferência.

O arquivo principal é:

```text
models/modelo_valorant.pt
```

Esse arquivo é uma cópia renomeada do `best.pt`, que é o melhor checkpoint salvo automaticamente pelo YOLO durante o treinamento.

---

### `inputs/`

Pasta usada para colocar arquivos de teste.

Exemplos:

```text
inputs/teste.mp4
inputs/imagem_teste.jpg
```

O notebook de inferência procura os arquivos nessa pasta para executar os testes.

---

### `outputs/`

Pasta onde o YOLO salva os resultados da inferência.

Ao rodar o notebook, são gerados arquivos com as detecções feitas pelo modelo, como imagens ou vídeos com as caixas desenhadas.

---

### `dataset_valorant/`

Pasta onde o dataset foi utilizado localmente durante o treinamento.

Essa pasta **não foi enviada para o GitHub**, pois o dataset possui muitas imagens e arquivos de anotação, deixando o repositório muito pesado.

O dataset pode ser baixado novamente pela fonte indicada abaixo.

---

### `runs/`

Pasta gerada automaticamente pelo YOLO durante o treinamento e a inferência.

Ela contém resultados como:

* gráficos de treinamento;
* imagens de validação;
* pesos treinados;
* arquivos `best.pt` e `last.pt`;
* saídas de testes.

Essa pasta também não é enviada ao GitHub, pois pode ficar muito pesada.

---

## Dataset utilizado

O dataset utilizado foi obtido no Roboflow Universe:

* Link do dataset: [https://universe.roboflow.com/fps/valorant-gygiq](https://universe.roboflow.com/fps/valorant-gygiq)
* Project ID: `valorant-gygiq-ohaba`

O dataset foi baixado no formato compatível com YOLOv8.

Por conta da grande quantidade de imagens e arquivos de anotação, o dataset completo não foi incluído neste repositório.
Para reproduzir o treinamento, é necessário baixar o dataset diretamente pelo Roboflow e colocá-lo na pasta:

```text
dataset_valorant/
```

A estrutura esperada do dataset é semelhante a:

```text
dataset_valorant/
├── train/
│   ├── images/
│   └── labels/
│
├── valid/
│   ├── images/
│   └── labels/
│
├── test/
│   ├── images/
│   └── labels/
│
└── data.yaml
```

---

## Modelo utilizado

O modelo base utilizado foi:

```text
yolov8n.pt
```

Esse modelo foi usado como ponto de partida para o treinamento customizado.

Após o treinamento, o YOLO gerou automaticamente o arquivo:

```text
best.pt
```

Esse arquivo representa o melhor modelo obtido durante a validação.
Para facilitar o uso no notebook de inferência, ele foi copiado e renomeado para:

```text
models/modelo_valorant.pt
```

---

## Configurações do treinamento

As principais configurações usadas no treinamento foram:

```text
Modelo base: yolov8n.pt
Número de épocas: 30
Tamanho das imagens: 416
Batch size: 2
```

### O que significa o número de épocas?

O número de épocas representa quantas vezes o modelo passa por todo o conjunto de treinamento.

Neste projeto, foram utilizadas **30 épocas**.
Isso significa que o modelo analisou o conjunto de treino 30 vezes, ajustando seus pesos a cada rodada para melhorar a detecção dos agentes/personagens.

---

## Como instalar o projeto

### Opção 1 - Usando Poetry

Este projeto possui os arquivos `pyproject.toml` e `poetry.lock`.

Para instalar as dependências com Poetry:

```bash
poetry install
```

Depois, registre o kernel do Jupyter:

```bash
poetry run python -m ipykernel install --user --name valorant-ai --display-name "valorant-ai (poetry)"
```

Em seguida, abra o notebook e selecione o kernel:

```text
valorant-ai (poetry)
```

---

### Opção 2 - Usando pip

Também é possível instalar as dependências com:

```bash
pip install -r requirements.txt
```

---

## Como rodar a inferência

Para testar o modelo já treinado, abra o notebook:

```text
notebooks/12-valorant-inferencia-modelo-treinado-entrega.ipynb
```

Esse notebook realiza as seguintes etapas:

1. Localiza a pasta principal do projeto.
2. Carrega o modelo treinado em `models/modelo_valorant.pt`.
3. Mostra as classes aprendidas pelo modelo.
4. Testa o modelo em uma imagem.
5. Testa o modelo em um vídeo.
6. Salva os resultados na pasta `outputs/`.

---

## Teste com imagem

Para testar com imagem, coloque uma imagem na pasta `inputs/`.

Exemplo:

```text
inputs/imagem_teste.jpg
```

Depois execute a célula de teste com imagem no notebook.

---

## Teste com vídeo

Para testar com vídeo, coloque um vídeo na pasta `inputs/`.

O nome padrão usado no notebook é:

```text
inputs/teste.mp4
```

Se quiser usar outro nome, altere a variável:

```python
VIDEO_NAME = "teste.mp4"
```

O vídeo processado será salvo na pasta:

```text
outputs/video_teste/
```

Caso o YOLO salve o vídeo em formato `.avi`, o notebook tenta converter automaticamente para `.mp4`, facilitando a visualização dentro do próprio Jupyter Notebook ou VS Code.

---

## Observação sobre arquivos grandes

Alguns arquivos não foram enviados ao GitHub para evitar que o repositório fique muito pesado.

Entre eles:

```text
dataset_valorant/
runs/
clips/
outputs/
arquivos .mp4 grandes
arquivos .zip
```

O arquivo necessário para rodar a inferência é:

```text
models/modelo_valorant.pt
```

Com esse arquivo, não é necessário baixar o dataset para testar o modelo já treinado.

O dataset só é necessário caso o objetivo seja treinar o modelo novamente.

---

## Arquivos importantes para avaliação

Para executar o projeto com o modelo já treinado, os arquivos mais importantes são:

```text
notebooks/12-valorant-inferencia-modelo-treinado-entrega.ipynb
models/modelo_valorant.pt
pyproject.toml
poetry.lock
requirements.txt
README.md
```

---

## Resumo do projeto

Este projeto demonstra o uso de YOLOv8 para treinar e executar um detector customizado de agentes/personagens do Valorant.

O dataset foi utilizado apenas na etapa de treinamento e não foi incluído no repositório por causa do tamanho.
Para facilitar a avaliação, o modelo treinado foi disponibilizado na pasta `models/`, permitindo executar a inferência diretamente em imagens e vídeos sem precisar treinar novamente.
