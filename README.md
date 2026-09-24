

````markdown
# Aula 4 — Comunicação de Alto Nível: RPC

Nesta aula de Sistemas Distribuídos, estudamos o conceito de RPC (Remote Procedure Call), ou Chamada de Procedimento Remoto.

A ideia principal foi entender como um programa pode solicitar que uma função seja executada em outro processo, como se fosse uma chamada de função normal. Na prática, conseguimos visualizar melhor a diferença entre uma comunicação feita diretamente com sockets e uma comunicação utilizando RPC.

Durante a aula foram feitas uma atividade prática, dois desafios e um desafio extra utilizando um tablet como cliente.

## Principais pontos da aula

Durante a parte teórica, vimos alguns conceitos que já tinham relação com as aulas anteriores, principalmente a comunicação por sockets.

Nos sockets, o programador precisa trabalhar mais diretamente com a comunicação, cuidando de mensagens, conexões, endereços, portas e respostas.

Com RPC, parte dessa comunicação fica mais simples para quem está programando. O cliente pode chamar uma função que está sendo executada em outro processo, enquanto o servidor recebe a solicitação, executa a operação e devolve o resultado.

Um ponto importante que ficou claro durante a aula é que RPC não elimina os problemas da rede. Mesmo parecendo uma chamada de função normal, ainda existe uma comunicação entre cliente e servidor. Por isso, o servidor precisa estar disponível e a rede também pode apresentar falhas.

## Atividade Prática — Mini Calculadora Distribuída

Na atividade prática foi criada uma pequena calculadora distribuída utilizando Python e XML-RPC.

Foram utilizados dois arquivos:

- `servidor_rpc.py`
- `cliente_rpc.py`

No servidor foram criadas as funções:

- Soma
- Subtração
- Multiplicação

Depois essas funções foram registradas no servidor para que pudessem ser chamadas remotamente.

O servidor foi configurado para funcionar na porta `8000`.

No cliente foi utilizado o `ServerProxy` para acessar o servidor RPC. Assim, em vez de trabalhar diretamente com `send()` e `recv()`, o cliente pôde chamar as funções do servidor.

### Testes realizados

Foram utilizados os seguintes testes:

```text
10 + 5 = 15
10 - 5 = 5
10 x 5 = 50
````

Os resultados foram retornados corretamente pelo servidor.

Essa primeira atividade ajudou a entender na prática que o cálculo não precisava ser feito diretamente pelo cliente. O cliente fazia a chamada e o servidor era responsável por executar a operação e devolver o resultado.

## Desafio 1 — Adicionando a divisão

No primeiro desafio, foi adicionada uma nova operação ao servidor: a divisão.

Também foi necessário tratar o caso de divisão por zero para evitar um erro durante a execução.

Foram testadas as seguintes situações:

```text
10 / 2 = 5.0
10 / 0 = Erro: divisão por zero
```

O teste mostrou que o servidor conseguiu executar a nova operação e também tratar corretamente uma situação que poderia causar um problema.

## Desafio 2 — Cliente Interativo

No segundo desafio, o cliente deixou de utilizar somente valores definidos diretamente no código.

Foi criado um menu para que o usuário pudesse:

1. Informar o primeiro valor;
2. Informar o segundo valor;
3. Escolher a operação;
4. Receber o resultado.

As opções disponíveis ficaram:

```text
1 - Somar
2 - Subtrair
3 - Multiplicar
4 - Dividir
```

Durante o teste, foram informados os valores `7` e `9` e escolhida a opção de multiplicação.

O resultado foi:

```text
7 x 9 = 63
```

O mais importante nessa parte foi perceber que o menu e a entrada dos valores estavam no cliente, mas o cálculo continuava sendo realizado pelo servidor através da chamada RPC.

## Teste de indisponibilidade

Também foi realizado o teste em que o servidor era encerrado e o cliente tentava fazer uma chamada novamente.

Com isso, foi possível perceber na prática que uma chamada RPC depende do servidor estar disponível.

Mesmo que no código a chamada pareça simples, como:

```python
servidor.soma(a, b)
```

existe uma comunicação acontecendo entre processos.

Se o servidor não estiver funcionando, o cliente não consegue realizar a operação.

Esse teste ajudou a entender melhor a relação entre RPC, disponibilidade e falhas de comunicação.

## Desafio Extra — Cliente utilizando um tablet

No desafio extra, a proposta era fazer a comunicação entre dispositivos diferentes.

Em vez de utilizar outro computador como cliente, utilizei meu tablet para acessar o servidor RPC que estava sendo executado no computador.

Primeiro, o servidor foi alterado para utilizar:

```python
("0.0.0.0", 8000)
```

Dessa forma, ele poderia aceitar conexões pela rede local.

Depois foi utilizado o endereço IPv4 do computador no cliente do tablet.

O cliente foi configurado para acessar o servidor através do endereço:

```text
http://192.168.1.12:8000/
```

### Teste realizado no tablet

No tablet foram informados:

```text
Primeiro valor: 6
Segundo valor: 8
Operação: Divisão
```

O resultado apresentado foi:

```text
6 / 8 = 0.75
```

Ao mesmo tempo, no computador que estava executando o servidor foi possível observar a requisição recebida pelo cliente.

O servidor registrou a requisição com código HTTP `200`, confirmando que a comunicação entre o tablet e o computador funcionou corretamente.

Esse foi um dos testes que mais ajudou a visualizar o conceito de sistema distribuído, porque o cliente e o servidor estavam em dispositivos diferentes.

## O que consegui entender com a aula

A principal coisa que consegui entender foi a diferença entre uma comunicação mais direta utilizando sockets e uma comunicação utilizando RPC.

Nos sockets, precisamos trabalhar mais diretamente com a comunicação. Já no RPC, conseguimos fazer chamadas para operações que estão sendo executadas em outro processo de uma maneira mais parecida com uma função normal.

A atividade da calculadora ajudou a entender essa ideia de forma simples. Depois, com os desafios, foi possível adicionar novas operações, deixar o cliente interativo e testar uma comunicação real pela rede.

O desafio com o tablet também ajudou a entender que o servidor pode estar em um dispositivo e o cliente em outro, desde que exista comunicação entre eles.

Ao mesmo tempo, ficou claro que RPC não faz a rede deixar de existir. O servidor precisa estar funcionando, o endereço e a porta precisam estar corretos e problemas de comunicação ainda podem acontecer.

## Arquivos desta aula

Neste repositório estão os materiais que produzi durante a Aula 4:

* `Atividade_Pratica_Aula_4_RPC.docx` — passo a passo da atividade prática da Mini Calculadora Distribuída.
* `Desafios_Aula_4_RPC.docx` — Desafio 1, Desafio 2 e Desafio Extra, incluindo o teste com o tablet.
* `Perguntas_Finais_Aula_4_Sistemas_Distribuidos.docx` — respostas das perguntas finais da aula.

## Tecnologias utilizadas

* Python
* XML-RPC
* `xmlrpc.server`
* `xmlrpc.client`
* Cliente e servidor
* Comunicação em rede

## Conclusão

A Aula 4 foi uma continuação do que já vinha sendo estudado sobre comunicação entre processos.

Com a prática, foi possível sair de uma explicação mais teórica e realmente testar uma comunicação RPC funcionando, primeiro no próprio computador e depois entre dispositivos diferentes.

No final, ficou mais fácil entender que o RPC simplifica a forma de solicitar uma operação remota, mas ainda depende da comunicação de rede e da disponibilidade do servidor.

---

**Disciplina:** Sistemas Distribuídos
**Aula:** 04 — Comunicação de Alto Nível: RPC
**Linguagem:** Python
**Tema:** Remote Procedure Call (RPC)

```
```
