tinyml-shrimp-pam
Detecção embarcada, em tempo real, da atividade alimentar de Litopenaeus vannamei
por monitoramento acústico passivo (PAM), usando TinyML em microcontrolador de baixo custo.
Status: em desenvolvimento — Trabalho de Conclusão de Curso, Bacharelado em Ciência
da Computação, Departamento de Computação, Universidade Federal Rural de Pernambuco (UFRPE).
---
Motivação
A ração é o principal custo operacional da carcinicultura marinha, e o manejo alimentar
ainda se apoia majoritariamente em bandejas e inspeção visual — método subjetivo, de
informação tardia e baixa acurácia.
Camarões peneídeos emitem cliques audíveis quando as mandíbulas colidem durante a
trituração do alimento, e a última década consolidou o PAM como forma não invasiva de
estimar a atividade alimentar a partir desses cliques. As soluções descritas na
literatura, porém, rodam em computador de propósito geral ou em software proprietário de
bioacústica, o que pressupõe transmissão contínua de áudio bruto — incompatível com nós
sensores autônomos em viveiro.
Este projeto investiga o que acontece quando a detecção é deslocada para a borda.
Pergunta de pesquisa
É viável executar a detecção dos cliques alimentares em tempo real, integralmente sobre um
microcontrolador de baixo custo? E, sendo viável, qual é o compromisso quantitativo entre
desempenho de detecção e recursos computacionais e energéticos consumidos?
Escopo
O que está no escopo:
Geração de sinal sintético parametrizado pelos descritores acústicos publicados para a espécie
Três abordagens de detecção com complexidades distintas
Execução embarcada com estímulo injetado digitalmente
Medição de acurácia, latência, memória e consumo
O que não está:
Coleta com animais vivos — não há experimentação animal neste trabalho
Cadeia de aquisição analógica (transdutor, pré-amplificação, conversão A/D)
Atuação sobre alimentador, comunicação sem fio, fusão com sensores de água
Estimativa de biomassa ou de consumo absoluto de ração
O estímulo é injetado digitalmente a partir da memória Flash, o que isola a camada de
processamento e torna as medições integralmente repetíveis. A construção da cadeia
analógica é o desdobramento imediato previsto.
Abordagens comparadas
	Descrição	Complexidade
A	Filtro passa-faixa → envelope → limiar adaptativo	Baixa
B	FFT / energia por sub-banda → classificador raso	Média
C	CNN 1D quantizada em INT8 (TFLite Micro + ESP-NN)	Alta
Cada abordagem é avaliada em três configurações de janelamento (512, 1024 e 2048 amostras
a 192 kHz), totalizando nove implementações embarcadas.
Plataforma
Módulo ESP32-S3-WROOM-1-N16R8 — Xtensa LX7 de dois núcleos a 240 MHz, 512 KB de SRAM,
16 MB de Flash, 8 MB de PSRAM.
A escolha se apoia na maturidade do `esp-tflite-micro`, port oficial do TensorFlow Lite
Micro mantido pela Espressif, e no ESP-NN, que fornece kernels em linguagem de montagem
explorando as instruções vetoriais do S3.
Estrutura do repositório
```
datagen/     gerador de sinal sintético, parâmetros e sementes versionados
detectors/   implementações offline das abordagens A, B e C
firmware/    projeto ESP-IDF: injetor digital, buffer, janelamento, instrumentação
eval/        protocolo de medição, métricas e gráficos
docs/        descritores acústicos da literatura, decisões de projeto, resultados
```
Reprodução
> Instruções detalhadas serão adicionadas conforme cada módulo for concluído.
Requisitos previstos: Python 3.11+ (NumPy, SciPy, scikit-learn, TensorFlow) e ESP-IDF 5.x.
Licenças
Código: Apache License 2.0 — ver `LICENSE`
Conjunto de dados sintético, figuras e documentação: Creative Commons Atribuição 4.0 Internacional (CC BY 4.0)
A licença Apache 2.0 foi escolhida por incluir concessão explícita de patente e por
coincidir com a licença do ESP-IDF e do TensorFlow Lite Micro, eliminando questões de
compatibilidade.
Citação
```bibtex
@misc{oliveira2026tinymlshrimppam,
  author = {Oliveira, Carlos Swamy Ferraz de},
  title  = {tinyml-shrimp-pam: detecção embarcada da atividade alimentar de
            Litopenaeus vannamei via TinyML},
  year   = {2026},
  url    = {https://github.com/RecifeStargazer/tinyml-shrimp-pam}
}
```
Aviso
Os dados utilizados são sintéticos, gerados a partir de descritores acústicos
publicados na literatura. Nenhum resultado deste repositório foi validado contra
gravações reais de L. vannamei. Os números aqui reportados caracterizam o custo
computacional das abordagens sob estímulo controlado, e não seu desempenho em viveiro.
