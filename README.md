# Integração Asaas + n8n + BotConversa

![Fluxo Automático com Asaas e n8n](./PAGAMENTO_AUTOMATICO_ASAAS%201%20.png)

Este projeto consiste em uma série de fluxos criados no **n8n** que integram a **API do Asaas** e o **BotConversa (BrisaBot)** para automatizar processos de cobrança, confirmação de pagamento e envio de lembretes. Essa automação foi desenvolvida para o setor financeiro, reduzindo significativamente o tempo e o custo operacional (cerca de 70% de redução) ao eliminar a necessidade de envio manual de mensagens.

## Objetivos do Projeto

- **Automatizar cobranças:** Envio automático de lembretes após o vencimento da fatura (ex.: 3, 7, 10 dias), incluindo aviso de cancelamento no 10º dia.
- **Confirmar pagamentos:** Recebimento de webhooks do Asaas e envio de notificações de pagamento confirmado com link do comprovante.
- **Menu Financeiro:** Disponibilizar links de pagamento e comprovantes sob demanda, permitindo ao cliente solicitar essas informações diretamente via BotConversa.
- **Reduzir carga manual:** Otimizar o processo de cobrança e acompanhamento de pagamentos, aumentando a eficiência do setor financeiro.

## Como Funciona

### 1. Lembretes de Pagamento

![Exemplo de Lembrete - 3 Dias Após o Vencimento](./3%20DIAS%20APÓS%20O%20VENCIMENTO%20.png)

- **Exemplo: 3 dias após o vencimento**
  - Um **Schedule Trigger** é configurado para rodar diariamente às 8h.
  - O fluxo consulta a API do Asaas para identificar faturas com status `OVERDUE`.
  - É calculado se já se passaram 3 dias (ou outro prazo configurado) a partir da data de vencimento.
  - Se a condição for atendida, o BotConversa envia uma mensagem automática para o cliente com o link de pagamento.
- A mesma lógica é aplicada para outros prazos (7 dias, 10 dias, etc.), onde no 10º dia, o fluxo também envia um aviso de cancelamento da assinatura caso o pagamento não seja efetuado em até 2 dias.

### 2. Confirmação de Pagamento

![Confirmação de Pagamento no Asaas](./ASAAS%20confirmacao%20de%20pagamento.png)

- O Asaas dispara webhooks (ex.: `PAYMENT_CONFIRMED` ou `PAYMENT_RECEIVED`) que são recebidos pelo n8n.
- O fluxo extrai os dados do cliente (nome, telefone, link do comprovante) e envia:
  - Uma mensagem de confirmação para o cliente com o link do comprovante.
  - Uma notificação para a equipe financeira.

### 3. Menu Financeiro

![Menu Financeiro no Asaas](./MENU%20FINANCEIRO%20ASAAS.png)

- **Solicitar link de pagamento:**  
  Recebe via webhook o CPF/CNPJ do cliente, consulta faturas com status `PENDING` na API do Asaas e retorna o link de pagamento ao cliente.

- **Solicitar comprovante de pagamento:**  
  De forma similar, o fluxo busca faturas com status `RECEIVED` ou `CONFIRMED` e envia o link do comprovante ao solicitante.

## Tecnologias Utilizadas

- **n8n:** Plataforma de automação utilizada para orquestrar os fluxos.
- **API do Asaas:** Responsável por fornecer dados de cobrança (status, dueDate, invoiceUrl, etc.).
- **BotConversa (BrisaBot):** Plataforma de envio de mensagens via WhatsApp para notificar clientes e a equipe interna.
- **JavaScript (nós Code):** Utilizado para formatação de datas, validação e formatação de números de telefone.

## Benefícios e Resultados

- **Automação Completa:** Envio automático de lembretes, confirmações e links, sem intervenção manual.
- **Redução de Custos:** Aproximadamente 70% menos trabalho manual no setor financeiro.
- **Eficiência:** Contato rápido e organizado com os clientes, garantindo que todos recebam as informações no prazo correto.
- **Escalabilidade:** A lógica pode ser facilmente adaptada para novos prazos de vencimento ou diferentes cenários de cobrança.

---

**Autor:** [João Gustavo L Lima]  
**Contato:** [joaogusta260@gmail.com & 5571999123129]
