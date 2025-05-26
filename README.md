# Eficientiza  
- Alexsandro Macedo — RM557068  
- Leonardo Faria Salazar — RM557484  
- Guilherme Felipe da Silva Souza — RM558282  

## Descrição
Este projeto implementa um sistema de **Visão Computacional** para classificação de motocicletas **logo na entrada** do pátio da empresa Mottu. Utilizando um modelo de **TensorFlow/Keras** baseado em **EfficientNet**, o programa classifica em tempo real o tipo de moto assim que a imagem é capturada.

## Link video
https://youtu.be/cIcSEcjw84c

## Funcionalidades
- **Classificação em Imagem**: carrega uma imagem estática e classifica o tipo de moto (Econômica, Popular, Esportiva).
- **Saída Gráfica**: exibe a imagem com o rótulo predito e porcentagem de confiança.

## Tecnologias e Dependências
- Python 3.8+
- TensorFlow 2.x
- NumPy
- Matplotlib
- [Keras](https://keras.io/)

Instale as dependências com:

```bash
pip install tensorflow numpy matplotlib
```

### Treinamento do Modelo

O modelo foi desenvolvido com a arquitetura **EfficientNetB1** da API `tf.keras.applications`, utilizando pesos pré-treinados da ImageNet. A seguir, os principais pontos do processo de treinamento:

- **Pré-processamento com Data Augmentation** via `ImageDataGenerator`, incluindo:
  - rotação, zoom, deslocamento horizontal e vertical
  - alteração de brilho, inversão horizontal/vertical
  - deslocamento de canais RGB
  - normalização das imagens para o intervalo `[0, 1]`

- **Divisão do Dataset** em duas pastas principais:
  - `train/`: imagens usadas no treinamento
  - `val/`: imagens usadas na validação

- **Estrutura da Rede Neural:**
  - Camada base: `EfficientNetB1` com `include_top=False` e parcialmente congelada (~70%)
  - Camadas adicionais:
    - `GlobalAveragePooling2D`
    - `Dense` com 512 unidades, `Dropout`, `BatchNormalization`, regularização L1/L2
    - `Dense` com 256 unidades, `Dropout` e regularização L2
    - `Dense` final com 3 saídas e ativação `softmax`

- **Configuração de Treinamento:**
  - Otimizador: `Adam` com `learning_rate=1e-4` e `weight_decay`
  - Função de perda: `categorical_crossentropy`
  - Métricas monitoradas: `accuracy`, `precision`, `recall`
  - Estratégias:
    - `EarlyStopping` monitorando `val_loss`, com paciência de 7 épocas
    - `ModelCheckpoint` salvando automaticamente o melhor modelo com base em `val_accuracy`

## Estrutura do Projeto
- challenge2025.ipynb      # Notebook com código de treinamento e predição
- best_model_v2.h5         # Arquivo do modelo salvo
  (Para evitar fazer todo o treinamento é recomnedado fazer o dowload indo em https://github.com/GuiFelSS/challenge2025Iot/blob/main/best_model_v2.h5 e clicando em View raw/Ver bruto)
- README.md                # Documentação deste repositório

## Como Testar

Para testar o modelo com novas imagens de motocicletas, siga os passos abaixo:

1. **Clone o repositório:**
Instale as dependências:
pip install tensorflow numpy matplotlib
Abra o notebook no Jupyter ou Google Colab:

Arquivo: challenge2025.ipynb

Verifique se o modelo pré-treinado está no caminho correto:

O modelo salvo deve estar como best_model_v2.h5 no mesmo diretório do notebook ou referenciado corretamente no código.
Clique em “View raw” (ou “Ver bruto”) e salve o arquivo no seu computador.

Carregue uma imagem para teste:
No notebook, localize a célula que contém a função predict_mottu() e execute com o caminho da sua imagem,
por exemplo:
predict_mottu("caminho/para/sua_imagem.jpg")

Resultado Esperado:

A imagem será exibida com o tipo de moto classificado no título.
Também será mostrada a porcentagem de confiança da predição.
