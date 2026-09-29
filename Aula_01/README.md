# Aula_quarta_Sistemas_Automatizados
Atividades da aula de quarta-feira 

#Aula 1 — Fundamentos dos Sistemas Automatizados


Prática da Semana 1: fronteira, entradas, processamento, saídas, perturbações, falhas e comportamento seguro, modelados em diagrams.net.

*Primeiros passos com a ferramenta*
Semáforo como exemplo:

Fronteira: controlador do semáforo
Entrada: tempo de fase
Processamento: decidir fase
Saída: acionar luz (vermelho/amarelo/verde)
Perturbação: botão de pedestre
Falha: lâmpada queimada
Comportamento seguro: pisca-amarelo


*Exercício obrigatório — Porta automática*

Modelo de controle de acesso a um edifício. 

Objetivo: controlar a abertura e o fechamento da porta de acesso ao edifício, permitindo passagem quando autorizada e interrompendo o movimento diante de obstáculos.
Fronteira: sistema de acesso à porta (sensores, lógica da porta, motor)
Entradas: presença detectada, barreira interrompida, sinal do sensor, comando prioritário
Processamento: lógica da porta decide abrir/fechar/parar
Saídas: abrir, fechar, parar
Realimentação: estado da porta e detecção de obstáculo retornam do motor à lógica
Perturbação: barreira interrompida (originada nos sensores)
Falha: sinal do sensor inválido (originada nos sensores)
Comportamento seguro: parar e reabrir


*Exercícios adicionais*
1. Reservatório — controle de nível
Fronteira: controle de nível do reservatório (sensor → controlador → bomba → planta)
Entrada: nível medido (sensor lê o nível da planta e retorna ao controlador)
Processamento: comparar com setpoint, liga/desliga com dois limites (histerese — liga abaixo do mínimo, desliga acima do máximo)
Saída: liga/desliga a bomba
Perturbação: consumo (saída de água do reservatório)
Falha: sensor travado / nível alto-alto
Comportamento seguro: bloquear bomba e soar alarme

2. Iluminação automática — estacionamento
Fronteira: iluminação do estacionamento
Entradas: luminosidade, presença, horário
Processamento: lógica booleana — escuro E (presença OU horário)
Saída: acionar luz
Modo manual: override direto sobre a saída, independente da lógica automática
Falha: sensor travado
Comportamento seguro: manter último estado

Casos de teste:
Cenário	Luminosidade	Presença	Horário	Saída esperada
Noite com movimento	Escuro	Sim	Dentro	Acender
Noite sem movimento, fora do horário	Escuro	Não	Fora	Apagar
Dia com movimento	Claro	Sim	—	Apagar
Falha do sensor	Inválido	—	—	Manter último estado


3. Proposta própria — Catraca de ônibus
Objetivo: controlar a passagem de passageiros mediante validação do pagamento (cartão/QR), liberando a catraca quando o crédito é confirmado.
Usuário: passageiros e motorista/cobrador
Grau de automação: semi-automático (validação e liberação automáticas, com override manual do motorista)
Fronteira: sistema de acesso à catraca (leitor → lógica da catraca → mecanismo)
Entrada: validação do cartão/QR
Processamento: decidir liberar
Saída: travar/destravar
Realimentação (prática recomendada, não exigida pelo enunciado): confirmação de giro retorna da catraca à lógica
Perturbação: fila / passagem dupla
Falha: leitor travado
Comportamento seguro: motorista libera manualmente
Melhoria proposta: conectar um sensor de emergência ao alarme de incêndio do ônibus, forçando a liberação total da catraca automaticamente em situação crítica, sem depender da ação do motorista.

