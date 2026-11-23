# Diagrama de Estados

Este documento cobre o item condicional “diagrama de estados, quando uma entidade possuir ciclo de vida”. Os estados de `SALE` e `FINANCIAL_TITLE` foram extraídos dos catálogos dos DERs; o ciclo de `CASH_SESSION` é **proposto**, pois a entidade ainda não existe no modelo-base (R-05 em [[RiscosQuestoesAbertas]]).

## 1. Venda — `SALE`

 Catálogo: `SALE_STATUS` = `PROCESSED`, `CANCELED` (`DER-commercial.md`).

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
stateDiagram-v2
    state "Processada / Concluída (PROCESSED)" as PROCESSED
    state "Cancelada (CANCELED)" as CANCELED

    [*] --> PROCESSED : POST /api/v1/sales (UC11)
    PROCESSED --> CANCELED : POST /api/v1/sales/{id}/cancel (UC12)
    PROCESSED --> [*] : comprovante emitido (UC10)
    CANCELED --> [*] : histórico preservado (RN04)

    note right of CANCELED
        Não exclusão física.
        Motivo/responsável pendentes (R-06).
    end note
```

## 2. Título financeiro — `FINANCIAL_TITLE`

Catálogo: `FINANCIAL_TITLE_STATUS` = `PENDING`, `PARTIAL`, `PAID`, `CANCELED` (`DER-financial.md`).

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
stateDiagram-v2
    state "Pendente (PENDING)" as PENDING
    state "Parcial (PARTIAL)" as PARTIAL
    state "Paga (PAID)" as PAID
    state "Cancelada (CANCELED)" as CANCELED

    [*] --> PENDING : geração a partir da venda
    PENDING --> PARTIAL : baixa parcial (CASH_PROCESS_ITEM)
    PENDING --> PAID : baixa total
    PARTIAL --> PAID : quitação
    PENDING --> CANCELED : cancelamento
    PARTIAL --> CANCELED : cancelamento
    PAID --> [*]
    CANCELED --> [*]

    note right of PARTIAL
        PAYED_VALUE registra o valor pago;
        padronizar para PAID_VALUE (R-17).
    end note
```

## 3. Sessão de caixa — proposta

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
stateDiagram-v2
    state "Aberta (OPEN)" as OPEN
    state "Fechada (CLOSED)" as CLOSED

    [*] --> OPEN : POST /api/v1/cash-sessions/open (UC08)
    OPEN --> OPEN : sangria / reforço (POST /cash-movements)
    OPEN --> OPEN : vendas e estornos vinculados
    OPEN --> CLOSED : POST /close + PATCH /confirm-close
    CLOSED --> [*]

    note right of OPEN
        Entidade CASH_SESSION não existe
        no modelo-base — ver R-05.
    end note
```

## 4. Matriz de transições

| Entidade | Estado inicial | Transições | Estado final | Origem |
|---|---|---|---|---|
| `SALE` | `PROCESSED` | cancelar | `CANCELED` | UC12, RN04 |
| `FINANCIAL_TITLE` | `PENDING` | baixa parcial/total, cancelamento | `PAID` / `CANCELED` | DER-financial, UC11 |
| `CASH_SESSION` (proposta) | `OPEN` | sangria, reforço, venda, estorno, fechamento | `CLOSED` | UC08, RF16, RN06 |

>Este diagrama não altera os `erDiagram` existentes. A inclusão de `CASH_SESSION` depende de decisão formal registrada em [[RiscosQuestoesAbertas]].
