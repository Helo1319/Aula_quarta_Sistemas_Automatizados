# Projeto de Rede Industrial — Semana 9

## 1. Cenário
Duas células de produção e uma sala de controle, com 120 m entre elas e EMI elevada perto dos motores.

| Elemento | Definição |
|---|---|
| Célula A | CLP, HMI, duas E/S remotas e três inversores (PROFINET) |
| Célula B | controlador legado Modbus RTU |
| Supervisão | servidor SCADA na sala de controle |
| Distância | 120 m entre células e sala |
| Ambiente | EMI elevada próxima aos motores |
| Disponibilidade | parada de até 15 min aceitável |
| Expansão | quatro novos nós no próximo semestre |

## 2. Entregáveis
| Item | Arquivo / link |
|---|---|
| Diagrama de rede (editável) | `diagrama_rede.drawio` |
| Diagrama exportado | `diagrama_rede.pdf` / `.png` |
| Planilha (Nós, Endereçamento, Matriz, Justificativas, Testes, Riscos) | [link do Google Planilhas] |

## 3. Decisões principais
- **Zonas:** Supervisão (VLAN 10, 10.20.10.0/24), Célula A (VLAN 30, 10.20.30.0/24) e Célula B (VLAN 40, 10.20.40.0/24), roteadas pelo SW-SUP (L3).
- **Célula A:** PROFINET em cobre Cat6A blindado. Foi a alternativa com maior pontuação na matriz (95 pontos contra 70 do Ethernet comum), e o Modbus RTU foi eliminado por incompatibilidade com RIO e inversores.
- **Célula B:** o controlador legado permanece em Modbus RTU (RS-485, endereço 5) e é integrado por um gateway (GW-B) Modbus RTU para Modbus TCP.
- **Backbone:** fibra óptica entre SW-SUP e os switches das células, porque 120 m excede o limite de 100 m do cobre e a fibra é imune a EMI.
- **Endereçamento:** IP estático nos ativos críticos, faixas reservadas por função e a faixa 50-59 de cada zona reservada para expansão.

## 4. Casos de teste planejados
| ID | Teste | Resultado esperado |
|---|---|---|
| T1 | Conectividade entre zonas (ping SCADA para CLP-A e GW-B) | 0% de perda |
| T2 | Comunicação PROFINET e recuperação de falha de RIO | falha detectada e recuperada automaticamente |
| T3 | Leitura do CTRL-B via GW-B | valor lido sem timeout |
| T4 | Falha: rompimento da fibra SW-SUP ↔ SW-A | alarme no SCADA; célula A segue em controle local; recuperação em até 15 min |
