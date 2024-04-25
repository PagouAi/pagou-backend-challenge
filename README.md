<p align="center">
<img src="/media/logo.svg?raw=true" alt="Logo" style="width:50%" />
</p>

# Desafio Back-end - Pagou.ai

Fala Dev! Tudo bem?

Primeiramente, obrigado pelo seu interesse em trabalhar conosco e buscar crescer ao nosso lado! Abaixo você encontrará todas as informações necessárias para iniciar o seu teste.

## Objetivo

Este teste tem como objetivo avaliar e desafiar você. É sugerido realizá-lo completamente sem auxílio de ferramentas de autocomplete ou pair-code, no entanto não é proibido, queremos apenas reconhecer seu esforço e potencial para compreender o desafio, aprender, se adaptar e tomar decisões baseado no que será apresentado.

Neste desafio você deverá construir uma versão simplificada de um Payment Service Provider (PSP), seu conhecimento será testado e você poderá aprender um pouco mais sobre como funcionam as plataformas de pagamentos no Brasil.

## O que necessito saber para fazer?

Esta API deverá seguir o mínimo de práticas RESTful e pode conter listagens, busca, paginação e filtros. Fique à vontade para decidir quais filtros são mais interessantes.

Para concluir o teste com sucesso você deve saber (ou aprender) um pouco sobre as seguintes tecnologias:

- Conceitos de API RESTful
- Modelagem de dados
- NodeJS
- Banco de Dados (por exemplo, MySQL, PgSQL, MongoDB, etc...)
- Git

Vamos ao teste!

## Contexto

Um PSP consiste basicamente em duas funções essenciais:

1. Permitir que nossos clientes processem transações ("cash-in")
2. Transferir recebíveis para os nossos clientes ("cash-out")

As operações consistem basicamente em: permitir que os nossos clientes recebam pagamentos pelos produtos ou serviços prestados e permitir que recebam o valor após determinado tempo, que muda de acordo com a forma de pagamento.

Considerando as informações acima, temos duas entidades que as representam:

- `transactions`: informações da compra, dados do comprador, forma de pagamento, valor, etc
- `payables`: recebíveis que serão pagos ao cliente

## Requisitos

Para esse desafio iremos focar especialmente na primeira função especificada, o "cash-in", neste momento não iremos considerar a possibilidade de vários clientes o que traria uma maior complexidade para este desafio o que não é nosso objetivo, sendo assim, você deve criar um serviço com os seguintes requisitos:

1. O serviço deve processar transações, recebendo as seguintes informações:

   - Valor da transação em centavos. Ex:`2099` representa R$20,99 - Obs: pontos flutuantes e textos não são aceitos.
   - Descrição da transação. Ex: `'Airpods'`
   - Método de pagamento (`pix` ou `credit_card`)
   - Nome do Pagador
   - CPF do Pagador Ex: `12345678900` - Obs: Apenas números são aceitos
   - Número do cartão (Obrigatório somente se metódo for `credit_card`) - Obs: Apenas números são aceitos.
   - Data de validade do cartão (MMAA) (Obrigatório somente se metódo for `credit_card` Obs: Apenas números são aceitos.
   - Código de verificação do cartão (CVV) (Obrigatório somente se metódo for `credit_card`) - Obs: Apenas números são aceitos.

   > Nota: Quando valores não aceitos forem enviados o sistema deve retornar um erro de entidade não processável especificando quão campo contém dados fora do formato aceitável.

### Payload

Este é apenas um exemplo, você pode fazer uma proposta de payload, como preferir:

POST /transaction

```json
{
	"amount": 1000,
	"description": "Cafe",
	"method": "pix",
	"name": "John Doe",
	"cpf": 12345678900,
	"card_number": null,
	"valid": null,
	"cvv": null
}
```

2. O serviço deve retornar uma lista com todas transações já criadas

3. Por se tratar de uma informação sensível, e seguindo os requisitos de compliance do PCI DSS, o serviço só pode armazenar e retornar os 4 últimos dígitos do cartão do pagador.

4. O serviço deve criar os recebíveis do cliente (`payables`), seguindo as seguintes regras:

   - Se a transação for feita com um pix:

     - O payable deve ser criado com status = `paid` (indicando que o cliente já recebeu esse valor)
     - O payable deve ser criado com a data de pagamento (payment_date) = data da criação da transação (D+0).

   - Se a transação for feita com um cartão de crédito:
     - O payable deve ser criado com status = `waiting_funds` (indicando que o cliente vai receber esse dinheiro no futuro)
     - O payable deve ser criado com a data de pagamento (payment_date) = data da criação da transação + 15 dias (D+15).

5. No momento de criação dos payables também deve ser descontado a taxa de processamento (que chamamos de `fee`) do cliente. Ex: se a taxa for 8% e o cliente processar uma transação de R$100,00, ele só receberá R$92,00. Considere as seguintes taxas:

   - 2.99% para transações feitas com pix
   - 8.99% para transações feitas com um cartão de crédito

6. O serviço deve prover um meio de consulta para que o cliente visualize seu saldo com as seguintes informações:
   - Saldo `available` (disponível): tudo que o cliente já recebeu (payables `paid`)
   - Saldo `waiting_funds` (a receber): tudo que o cliente tem a receber (payables `waiting_funds`)

> Nota: neste desafio, você não precisa se preocupar com parcelamento.

## Restrições

1. O serviço **deve ser escrito em Node.js** com um framework de sua preferência, encorajamos o uso de Fastify ou Hono.
2. O serviço **deve armazenar informações em um banco de dados**. Você pode escolher o banco que achar melhor mas, tenha em mente que na Pagou.ai usamos amplamente PostgreSQL.
3. O projeto **deve ter um README.md com todas as instruções** sobre como executar e testar o projeto e os serviços disponibilizados.
4. O projeto **deve conter testes unitários**, sugerimos utilizar Jest.

## O que será avaliado?

- Documentação
- Ser consistente e saber argumentar suas escolhas
- Apresentar boas soluções que domina
- Modelagem de Dados
- Manutenibilidade do Código
- Tratamento de erros
- Cuidados com itens de segurança
- Se for para vaga sênior, foque bastante no desenho de arquitetura
- Código limpo e organizado (nomenclatura, etc)
- Testes bem estruturados (previsão de erros)

## O que será um diferencial?

- Uso de Docker
- Uso de Design Patterns
- Documentação completa (com endpoints, payloads e resultados esperados)
- Propostas de melhoria na arquitetura

## Avaliação

1. Crie um repositório público em seu Github, não cite ou mencione a Pagou.ai em qualquer momento ou arquivo dentro do repositório, ao concluir o desafio compartilhe o link para o repositório do Github com o responsável pelo seu teste, nesse momento seu código será considerado entregue e não serão mais aceitas modificações.
2. Depois que corrigirmos o desafio, te chamamos para conversar com o time, você deverá apresentar o desafio em execução em sua máquina, onde iremos conversar sobre pontos que julgarmos necessários e discutir sobre as decisões que você tomou
3. Achamos que **5 dias** é um tempo ok para fazer o desafio, mas sabemos que nem todo mundo tem o mesmo nível de disponibilidade. Portanto, nos avise se precisar de mais tempo, ok?
4. Boa sorte :)
