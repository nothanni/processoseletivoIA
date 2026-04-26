Nome: Paula Hânnia Rocha Santana

1️- Resumo da Arquitetura do Modelo

O modelo desenvolvido é uma Rede Neural Convolucional (CNN) simples para classificação de dígitos manuscritos do dataset MNIST.

A arquitetura é composta por:

* Duas camadas convolucionais (Conv2D) com ativação ReLU, responsáveis por extrair características das imagens
* Camadas de MaxPooling para redução de dimensionalidade
* Uma camada Flatten para transformar os dados em vetor
* Duas camadas densas (Dense), sendo a última com ativação Softmax para classificação em 10 classes (dígitos de 0 a 9)

O modelo foi projetado para ser leve e eficiente, adequado para execução em ambientes com restrição de recursos.

2- Bibliotecas Utilizadas

* TensorFlow (>= 2.12)
* NumPy

O TensorFlow foi utilizado para construção, treinamento e conversão do modelo, enquanto o NumPy é utilizado internamente para manipulação de dados.

3️- Técnica de Otimização do Modelo

Foi utilizada a técnica de **Dynamic Range Quantization** através do TensorFlow Lite.

Essa técnica reduz o tamanho do modelo e melhora a eficiência computacional ao converter os pesos para um formato mais leve, mantendo um bom nível de precisão.

A conversão foi realizada utilizando o TFLiteConverter com a opção de otimização padrão (`tf.lite.Optimize.DEFAULT`).

4️- Resultados Obtidos

O modelo atingiu uma acurácia de aproximadamente **99%** no conjunto de teste.

Esse resultado indica que o modelo foi capaz de aprender de forma eficaz a identificar os dígitos manuscritos, mesmo utilizando uma arquitetura simples e com poucas épocas de treinamento.

5️- Comentários Adicionais

Durante o desenvolvimento, uma das principais dificuldades foi a familiarização com conceitos de Machine Learning e com o uso da biblioteca TensorFlow.
Apesar disso, foi possível compreender o fluxo completo de criação de um modelo de IA, incluindo treinamento, avaliação e otimização para Edge AI.
O projeto demonstrou na prática como modelos podem ser adaptados para execução em dispositivos com recursos limitados, mantendo um bom desempenho.
Como aprendizado, destaca-se a compreensão do pipeline de Machine Learning e da importância da otimização de modelos para aplicações reais.