
# Aula 6 — Partida com selo 

Programa Ladder executável no Rungs Studio implementando um circuito de partida com retenção (seal-in), com permissivos de parada, emergência e sobrecarga.
Link do projeto: https://studio.rungs.dev/vlOHXDEo

## Tags

| Nome | Tipo | Uso | Função |
|---|---|---|---|
| `START` | BOOL | Input | Botão de partida — inicia o motor |
| `STOP_OK` | BOOL | Input | Condição de parada saudável (1 = sem comando de parada ativo) |
| `EMG_OK` | BOOL | Input | Condição de emergência saudável (1 = sem emergência acionada) |
| `OL_OK` | BOOL | Input | Condição de sobrecarga saudável (1 = sem sobrecarga detectada) |
| `MOTOR` | BOOL | Output | Bobina de saída — energiza o motor |

## Lógica (rung 0)

```
STOP_OK ─ EMG_OK ─ OL_OK ─┬─ START ──┬─ (MOTOR)
                           └─ MOTOR ──┘
```

**Permissivos em série:** `STOP_OK`, `EMG_OK` e `OL_OK` precisam estar todos em 1 (condição saudável) para que exista qualquer possibilidade de energizar o motor. Se qualquer um cair para 0, a bobina perde energia imediatamente, independente do estado de `START`.

**Branch de retenção (selo):** `START` e `MOTOR` em paralelo. O botão `START` inicia o ciclo, mas uma vez que a bobina `MOTOR` energiza, o próprio contato de `MOTOR` mantém o caminho fechado — permitindo soltar o `START` sem desligar o motor. O motor só desliga quando a cadeia de permissivos é rompida.

## Decisões

- As entradas de proteção (`STOP_OK`, `EMG_OK`, `OL_OK`) foram modeladas como "saudável = 1" em vez de "botão de parada pressionado = 1", para que a falta de sinal (fio rompido, sensor sem energia) resulte em parada seguraa por padrão — comportamento fail-safe.
- A falha de qualquer permissivo remove a retenção automaticamente, pois interrompe o próprio caminho que sustenta a bobina — não é necessária lógica adicional para "lembrar" da falha.
- Uma vez interrompida a retenção, o motor não religa sozinho quando a condição de falha é liberada: é necessária uma nova ação de `START`, evitando reinício automático não supervisionado.


## Casos de teste

| ID | Condicao_inicial | Acao | Esperado | Obtido | Situacao | 
|---|---|---|---|---|---|
| T01 | "tudo saudavel (STOP_OK=1 EMG_OK=1 OL_OK=1) M=0", | START=1 | M=1 | M=1 | Aprovado | 
| T02 | M=1 | START=0 | M=1 | M=1 | Aprovado | 
| T03 | M=1 | STOP_OK=0 | M=0 | M=0 | Aprovado | 
| T04 | M=1 | EMG_OK=0 | M=0 | M=0 | Aprovado | 
| T05 | "M=0 EMG_OK=0" | START=1 | M=0 | M=0 | Aprovado | 
