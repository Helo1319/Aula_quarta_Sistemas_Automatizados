# TP05 - Hipóteses e critérios de segurança

# Atividade 23 - Identificar elementos

- As receptividades foram definidas como condições testáveis para representar
  a mudança entre as etapas.
- A condição de falha considerada é o não atingimento do nível dentro do
  tempo limite.

# Atividade 24 - Semáforo

- E0 representa o sinal vermelho.
- E1 representa o sinal verde.
- E2 representa o sinal amarelo.
- Os tempos considerados são 8 s, 10 s e 3 s, respectivamente.
- O ciclo é fechado e retorna à etapa inicial.

# Atividade 25 - Porta automática

- A emergência possui prioridade sobre o funcionamento normal.
- Na emergência, o motor é desligado e o sistema permanece em estado seguro.
- O retorno à operação ocorre somente após RESET_SEGURO.
- Os sensores FC_ABERTA e FC_FECHADA são utilizados para confirmar as
  posições da porta.

# Atividade 26 - Esteira separadora

- As classificações TIPO_A e TIPO_B são mutuamente exclusivas.
- Uma leitura inconsistente do sensor não deve resultar na seleção de A ou B.
- Em caso de falha do sensor, a esteira é parada e a falha é sinalizada.
- O retorno do atuador deve ser confirmado antes da liberação da peça e do
  início de um novo ciclo.


# Casos de teste
Atividade,Caso,Situação,Resultado Esperado
24,Normal,E0 após 8 s,E1 ativa
24,Normal,E1 após 10 s,E2 ativa
24,Normal,E2 após 3 s,E0 ativa
24,Limite,E0 com 7,9 s,Permanecer E0
24,Limite,E0 com 8,0 s,Passar para E1
24,Limite,E1 com 10,0 s,Passar para E2
24,Limite,E2 com 3,0 s,Retornar para E0
24,Falha,Tempo ainda não atingido,Transição não dispara
