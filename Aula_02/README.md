# Casos de teste — aula 2

Todos os casos executados na simulação do Wokwi

| Caso | Potenciômetro | Temperatura | Botão | Esperado | Obtido | Situação |
|---|---|---|---|---|---|---|
| T1 | 0 | 24.0 °C | Liberado | NORMAL; LED apagado | `POT=0 \| TEMP=24.0 C \| ESTADO=NORMAL` | Aprovado |
| T2 | 682 | 24.0 °C | Liberado | ATENÇÃO; LED apagado | `POT=682 \| TEMP=24.0 C \| ESTADO=ATENCAO` | Aprovado |
| T3 | 777 | 24.0 °C | Liberado | ALARME; LED apagado | `POT=777 \| TEMP=24.0 C \| ESTADO=ALARME` | Aprovado |
| T4 | 0 | 36.3 °C | Liberado | ATENÇÃO; LED apagado | `POT=0 \| TEMP=36.3 C \| ESTADO=ATENCAO` | Aprovado |
| T5 | 0 | 45.8 °C | Liberado | ALARME; LED apagado | `POT=0 \| TEMP=45.8 C \| ESTADO=ALARME` | Aprovado |
| T6 | 0 | 25.6 °C | Pressionado | NORMAL; LED aceso | `BTN=PRESSIONADO \| POT=0 \| TEMP=25.6 C \| ESTADO=NORMAL` | Aprovado |
| T7 | 0 | Inválida (fio de dados desconectado) | Liberado | FALHA DE SENSOR | `TEMP=INVALIDA \| ESTADO=FALHA DE SENSOR` | Aprovado |

# Aula 2 — Processos, sensores e transdutores

Protótipo funcional no Wokwi que integra uma entrada digital, uma entrada analógica e um sensor de temperatura simulado, aplicando uma decisão com prioridade entre falha de instrumento, alarme, atenção e normalidade.

**Projeto no Wokwi:** 
https://wokwi.com/projects/476464424198692865

# Objetivo do protótipo

Estação de monitoramento onde um botão aciona um LED de forma independente, enquanto um potenciômetro e um DHT22 alimentam uma classificação automática em NORMAL, ATENÇÃO, ALARME ou FALHA DE SENSOR. O botão representa uma entrada discreta de presença, o potenciômetro uma variável contínua de processo, e o DHT22 uma medição real de temperatura.

# Mapa de pinos

| Variável | Entrada | Pino | Resposta |
|---|---|---|---|
| Presença | Pushbutton | D2 | Aciona o LED (D8) |
| Nível do processo | Potenciômetro | A0 | Classifica NORMAL/ATENÇÃO/ALARME |
| Temperatura | DHT22 | D4 | Classifica o estado; falha gera FALHA DE SENSOR |
| Sinalização | LED + resistor 220 Ω | D8 | Aceso com o botão pressionado |

# Regras de classificação (por prioridade)

| # | Condição | Estado |
|---|---|---|
| 1 | Temperatura inválida (`isnan`) | FALHA DE SENSOR |
| 2 | Temp ≥ 40°C ou pot ≥ 750 | ALARME |
| 3 | Temp ≥ 30°C ou pot ≥ 400 | ATENÇÃO |
| 4 | Nenhuma anterior | NORMAL |



