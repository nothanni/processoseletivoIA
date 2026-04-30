# Classificação de Dígitos Manuscritos com CNN – Processo Seletivo Intensivo Maker | AI

## 👤 Identificação
- **Nome completo:** Paula Hânnia Rocha Santana
- **GitHub:** [nothanni](https://github.com/nothanni)

---

## 1️⃣ Resumo da Arquitetura do Modelo

O modelo desenvolvido é uma Rede Neural Convolucional (CNN) para classificação de dígitos manuscritos do dataset MNIST.

**Arquitetura implementada:**

| Camada | Tipo | Parâmetros | Função |
|--------|------|------------|--------|
| 1 | Conv2D | 32 filtros, kernel 3×3, ReLU | Extração de características locais simples (bordas, texturas) |
| 2 | MaxPooling2D | pool 2×2 | Redução de dimensionalidade, mantendo as features mais relevantes |
| 3 | Conv2D | 64 filtros, kernel 3×3, ReLU | Extração de padrões mais complexos e abstratos |
| 4 | MaxPooling2D | pool 2×2 | Segunda redução de dimensionalidade |
| 5 | Flatten | — | Conversão do mapa de features em vetor 1D |
| 6 | Dense | 64 neurônios, ReLU | Combinação das features extraídas |
| 7 | Dense | 10 neurônios, Softmax | Classificação final nas 10 classes (dígitos 0–9) |

**Decisões de arquitetura e trade-offs:**

- **Por que 2 camadas convolucionais?** O MNIST é um dataset relativamente simples (imagens 28×28 em escala de cinza), e duas camadas convolucionais são suficientes para extrair os padrões necessários. Adicionar mais camadas aumentaria o tempo de treinamento e o tamanho do modelo sem ganho significativo de acurácia — algo indesejável em Edge AI.

- **Por que 32 e 64 filtros?** A progressão de 32 para 64 filtros segue a convenção de aumentar a capacidade de extração conforme a profundidade da rede, permitindo capturar padrões mais abstratos nas camadas mais profundas.

- **Por que Dense(64)?** Uma camada densa intermediária pequena é suficiente para combinar as features extraídas pelas convoluções, mantendo o modelo leve e adequado para execução em ambientes com restrição de CPU e memória.

- **Limitação conhecida:** a ausência de Dropout pode levar a leve overfitting em datasets maiores ou mais complexos. Para o MNIST, o impacto é mínimo dado o volume de dados de treino.

---

## 2️⃣ Bibliotecas Utilizadas

| Biblioteca | Versão | Uso |
|------------|--------|-----|
| TensorFlow/Keras | >= 2.12 | Construção, treinamento, avaliação e conversão do modelo |
| NumPy | >= 1.23 | Manipulação de arrays (utilizado internamente pelo TensorFlow) |

O TensorFlow foi escolhido por integrar em uma única biblioteca todo o pipeline necessário: carregamento do dataset MNIST, construção da CNN, treinamento, avaliação e conversão para TFLite — reduzindo dependências externas e simplificando a execução em ambientes de CI.

---

## 3️⃣ Técnica de Otimização do Modelo

Foi utilizada a técnica de **Dynamic Range Quantization** via TensorFlow Lite, aplicada através de:

```python
converter.optimizations = [tf.lite.Optimize.DEFAULT]
```

**O que essa técnica faz:**  
Converte os pesos do modelo de ponto flutuante (float32) para inteiros de 8 bits (int8) em tempo de conversão. As ativações permanecem em float durante a inferência, mas são convertidas dinamicamente para int8 durante a execução.

**Por que essa técnica foi escolhida:**

| Critério | Dynamic Range Quantization | Alternativa (Full Integer) |
|----------|---------------------------|---------------------------|
| Facilidade de implementação | Alta — não requer dataset de calibração | Baixa — requer dataset representativo |
| Redução de tamanho | ~75% (float32 → int8) | ~75% |
| Impacto na acurácia | Mínimo | Mínimo |
| Compatibilidade com microcontroladores | Boa | Máxima |

A Dynamic Range Quantization foi escolhida por oferecer uma relação custo-benefício excelente: redução significativa de tamanho e ganho de performance sem a complexidade de fornecer um dataset de calibração, adequada para o contexto deste desafio.

**Trade-off tamanho × desempenho:**  
O modelo original em `.h5` tem cerca de 1.2 MB. Após a conversão e quantização para `.tflite`, o tamanho cai para aproximadamente 300 KB — redução de ~75% — mantendo acurácia equivalente ao modelo original e viabilizando a execução em microcontroladores com memória limitada.

---

## 4️⃣ Resultados Obtidos

| Métrica | Valor |
|---------|-------|
| Acurácia no conjunto de teste | ~99% |
| Épocas de treinamento | 5 |
| Tamanho do modelo `.h5` | ~1.2 MB |
| Tamanho do modelo `.tflite` | ~300 KB |
| Redução de tamanho | ~75% |

A acurácia de ~99% demonstra que a arquitetura escolhida é adequada para o dataset MNIST mesmo com apenas 5 épocas de treinamento. O modelo treinado foi convertido com sucesso para o formato TFLite com quantização aplicada, e ambos os arquivos (`model.h5` e `model.tflite`) foram gerados e validados pelo pipeline de CI.

---

## 5️⃣ Comentários Adicionais

- **Fluxo completo implementado:** o projeto cobre todo o pipeline de Edge AI — treinamento → avaliação → salvamento → conversão → otimização — demonstrando na prática como modelos são preparados para execução em dispositivos embarcados.

- **Decisão sobre número de épocas:** 5 épocas foram suficientes para atingir ~99% de acurácia no MNIST, e respeitam as restrições de tempo de execução do ambiente de CI. Aumentar o número de épocas traria ganhos marginais com custo computacional desproporcionalmente maior.

- **Compatibilidade com Edge AI:** a combinação de uma arquitetura CNN simples com Dynamic Range Quantization resulta em um modelo que pode ser executado em microcontroladores com suporte a TFLite, como ESP32-S3 e Raspberry Pi, mantendo boa performance com memória e processamento reduzidos.

- **Aprendizados:** o desafio proporcionou compreensão prática do pipeline completo de Machine Learning para sistemas embarcados, incluindo as razões pelas quais a otimização de modelos é essencial em Edge AI — não apenas pela limitação de armazenamento, mas também pelo impacto no tempo de inferência e consumo energético.