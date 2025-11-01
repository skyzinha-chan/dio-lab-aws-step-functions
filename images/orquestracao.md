### Exemplo Prático: Workflow de Processamento de Documentos

Como parte da minha experiência prática com a AWS, desenvolvi um workflow com o Step Functions para automatizar o processamento de documentos. A imagem a seguir é o gráfico de execução dessa máquina de estado:

![Workflow de Processamento de Documentos](images/step.png)

Este fluxo demonstra um caso de uso real e robusto:

1.  **Start:** O workflow é iniciado (provavelmente por um evento, como um arquivo chegando no S3).
2.  **ProcessarTextract:** A primeira tarefa (`Task`) é invocada, provavelmente uma função Lambda que usa o serviço **AWS Textract** para extrair texto e dados do documento.
3.  **ProcessarNLP:** Se o Textract for bem-sucedido, o texto extraído é passado para a próxima tarefa, que realiza o Processamento de Linguagem Natural (NLP), talvez usando o **AWS Comprehend**.
4.  **MoverNoS3:** Após o processamento do NLP, uma tarefa final move os resultados (ex: um JSON enriquecido) para um bucket S3 de destino.
5.  **End:** O fluxo é concluído com sucesso.

O mais importante é o **Tratamento de Erros** nativo do Step Functions:
* Se a etapa `ProcessarTextract` falhar, o fluxo é desviado para `FalhaNoProcessamento`.
* Se a etapa `ProcessarNLP` falhar, o fluxo é desviado para `FalhaNoProcessamentoNLP`.
* Se a etapa `MoverNoS3` falhar, o fluxo é desviado para `FalhaNaMovimentacao`.

Todos os caminhos, de sucesso ou falha, convergem para o `End`, garantindo que o workflow sempre termine de forma controlada e monitorada. Este exemplo prático encapsula perfeitamente os conceitos de orquestração, resiliência e separação de responsabilidades.
