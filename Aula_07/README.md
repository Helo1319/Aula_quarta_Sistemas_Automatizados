# Semana 7 — Atraso de partida com TON


Link do projeto: [https://studio.rungs.dev/vlOHXDEo](https://studio.rungs.dev/w8RAt35o)

## Tags

| Nome | Tipo | Uso | Função |
|---|---|---|---|
| `START` | BOOL | Input | Pedido de partida (deve ser mantido durante a temporização) |
| `STOP_OK` | BOOL | Input | Condição de parada saudável (1 = sem comando de parada) |
| `EMG_OK` | BOOL | Input | Condição de emergência saudável (1 = sem emergência) |
| `MOTOR_DELAY` | BOOL | Output | Saída comandada após o atraso |
| `T1` | TIMER | Local | Temporizador TON com `PRE` = 3000 |

**Correspondência com a nomenclatura dos slides:**

| Slide | Rungs Studio |
|---|---|
| `PT` | `T1.PRE` (3000 = 3 s) |
| `ET` | `T1.ACC` |
| `Q` | `T1.DN` |

## Lógica

```
Rung 0:  STOP_OK ─ EMG_OK ─ START ─────────[ TON  T1  PRE=3000 ]
Rung 1:  T1.DN ─────────────────────────────( MOTOR_DELAY )
```

- **Rung 0:** os três contatos em série alimentam o TON. Enquanto todos estiverem em 1, `T1.ACC` aumenta. Se qualquer um cair, o timer zera.
- **Rung 1:** quando `T1.ACC` chega a 3000, `T1.DN` vira 1 e a bobina `MOTOR_DELAY` liga.

## Decisões de projeto

- **Sem selo:** o `START` precisa permanecer ligado pelos 3 s. Se for solto antes, a temporização zera (T01). Se for solto depois, a saída desliga junto, porque `T1.DN` cai.
- **O rung 3 do slide não foi desenhado separadamente.** "NOT EMG_OK ou NOT STOP_OK remove continuidade" já é consequência dos contatos em série no rung 0: ao perderem a continuidade, o TON zera `ACC` e `DN`, e a saída volta ao estado seguro.
- **Entradas modeladas como "saudável = 1"**, como na Semana 6: a perda do sinal (fio rompido, falta de energia) é tratada como falha, e a saída desliga.
- **Sem reinício automático:** liberar uma falha não religa a saída (T05). O timer só volta a contar com o `START` ligado.


## Testes executados

| ID | Resultado |
|---|---|
| T01 | Aprovado: `START` por ~2 s, `T1.ACC` chegou a 1700 e voltou a 0 ao soltar; saída sempre em 0 |
| T02 | Aprovado: `T1.ACC` = 3000, `T1.DN` = 1, `MOTOR_DELAY` = 1 |
| T03 | Aprovado: `STOP_OK` = 0 durante a contagem zerou `T1.ACC` e a saída, com `START` ainda em 1 |
| T04 | Aprovado: `EMG_OK` = 0 com a saída ligada levou a saída e `T1.ACC` a 0, com `START` ainda em 1 |
| T05 | Aprovado: falha liberada sem `START`, saída em 0 e `T1.ACC` em 0 após ~6 s |

O preset de 3000 corresponde a 3 s reais (a saída ligou cerca de 3 s após o `START`).

## Casos de teste

| ID  | Acao | Esperado | Obtido | Situacao |
| --- | --- | --- | --- | --- |
| T01 | START por 2 s (STOP_OK=1 EMG_OK=1) | Q=0 e MOTOR_DELAY=0 | START por ~2 s: ACC chegou a 1700 com DN=0 e saida=0; ao soltar ACC=0 DN=0 saida=0 | Aprovado |
| T02 | START por 3 s | Q=1 e MOTOR_DELAY=1 | ACC=3000 DN=1 MOTOR_DELAY=1  | Aprovado |
| T03 | STOP_OK=0 durante ET (START mantido) | ET zera e saida=0   | ACC=0 DN=0 MOTOR_DELAY=0 com START=1   | Aprovado |
| T04 | EMG_OK=0 com saida ligada (START mantido) | saida=0 | MOTOR_DELAY=0 ACC=0 DN=0 com START=1 | Aprovado |
| T05 | Liberar falha sem START (EMG_OK 0 para 1 com START=0) | saida permanece 0   | apos ~6 s MOTOR_DELAY=0 ACC=0 EN=0 | Aprovado |
