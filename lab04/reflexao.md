# Bloco de Reflexão
Aluna: Paloma Dias de Carvalho

*01. Sintese:* Qual dos 7 tipos de transparencia voce considera mais dificil de implementar corretamente em um sistema real? Justifique com um argumento tecnico baseado nos exercicios realizados.

Entre os sete tipos de transparência estudados, considero que a transparência de relocação é a mais difícil de implementar corretamente em um sistema real. Diferente da migração simples, em que o serviço pode ser movido entre requisições, na relocação o recurso muda enquanto ainda está sendo utilizado pelo cliente. No exercício analisado em relocacao_websocket.py, isso exigiu uma máquina de estados explícita (CONNECTED, MIGRATING, RECONNECTING) e um buffer de mensagens (_message_buffer) para preservar as operações em andamento. Em sistemas reais, garantir continuidade da comunicação, evitar perda ou duplicação de mensagens e manter a ordem correta das operações é extremamente complexo. Por isso, transparência de relocação normalmente exige mecanismos avançados como buffering, retransmissão e controle de estado distribuído.

*02. Trade-offs:* Descreva um cenario concreto de um sistema que voce conhece (app, site, jogo) em que esconder completamente a distribuicao levaria a um sistema menos resiliente para o usuario final.

Um exemplo concreto em que esconder completamente a distribuição pode ser prejudicial é em aplicações de streaming ou jogos online. Se um cliente fizer uma chamada a um servidor remoto achando que é uma operação local, ele pode não lidar corretamente com problemas de latência ou indisponibilidade. O exercício da Tarefa 7 (anti_pattern.py) ilustra bem esse risco: a função get_user() parece uma chamada local simples, mas na verdade depende de rede e pode falhar ou demorar centenas de milissegundos. Se o sistema esconder totalmente esses aspectos, o usuário pode enfrentar travamentos ou erros inesperados. Nesse caso, expor explicitamente que a operação é remota, como no exemplo com async/await, permite que o sistema trate melhor timeouts, retries e mensagens de erro.

*03. Conexao com Labs anteriores:* Como o conceito de async/await explorado no Lab 02 se conecta com a decisao de quebrar a transparencia conscientemente, vista na Tarefa 7?

O conceito de async/await, explorado no Lab 02, se conecta diretamente com a ideia de quebrar a transparência de forma consciente. Quando usamos async def e await, estamos deixando claro que aquela operação pode envolver I/O assíncrono, latência de rede ou espera por recursos externos. Isso evita que o desenvolvedor trate uma chamada remota como se fosse uma função local comum. Na Tarefa 7, o exemplo fetch_user_remote() usa async/await para indicar explicitamente que a função pode suspender o fluxo do programa e retornar None em caso de falha. Essa abordagem força o chamador a lidar com possíveis erros e latência, tornando o sistema mais robusto.

*04. GIL e multiprocessing:* Explique com suas palavras por que a Tarefa 6 usa multiprocessing em vez de threading. O que e o GIL e por que ele interfere na demonstracao de race conditions em Python?

Na Tarefa 6 foi utilizado multiprocessing em vez de threading para demonstrar concorrência real. Isso acontece por causa do GIL (Global Interpreter Lock) do CPython, que impede que múltiplas threads executem bytecode Python simultaneamente dentro do mesmo processo. Na prática, isso significa que duas threads não executam código Python em paralelo real, o que pode mascarar ou dificultar a reprodução de race conditions. Com multiprocessing, cada processo tem seu próprio espaço de memória e seu próprio GIL, permitindo que os processos executem verdadeiramente em paralelo. No exercício sem_concorrencia.py, isso permitiu observar claramente a condição de corrida no saldo armazenado no Redis, algo que seria menos previsível com threads.

*05. Desafio tecnico:* Descreva uma dificuldade tecnica encontrada durante o laboratorio (incluindo o provisionamento do Redis Cloud), o processo de diagnostico e a solucao. Se nao houve dificuldade, descreva o exercicio mais interessante e explique por que.

Uma das dificuldades que encontrei durante o laboratório foi configurar corretamente a conexão com o Redis Cloud. Inicialmente o código não conseguia se conectar porque a porta e os parâmetros de conexão estavam incorretos. Ao analisar o erro no terminal (TimeoutError e posteriormente problemas de SSL), percebi que precisava ajustar o arquivo .env com o host, porta e senha corretos fornecidos pelo Redis Cloud. Depois disso, também foi necessário instalar algumas bibliotecas no ambiente virtual, como redis, requests e python-dotenv, para que os scripts funcionassem corretamente. Após corrigir essas configurações e instalar as dependências, consegui executar os exercícios que utilizavam o Redis, como os de migração e concorrência distribuída.

## Print das Execuções

1.1 Analise o problema

<img width="780" height="47" alt="1 1_Analise_o_problema" src="https://github.com/user-attachments/assets/558e3fad-f9c9-409a-9340-523aaa5904db" />

1.2 Aplique a transparência

<img width="862" height="65" alt="1 2_Aplique_a_transparência" src="https://github.com/user-attachments/assets/0d101165-e8a5-4315-85c1-d13cb47261de" />

<img width="1265" height="107" alt="1 2_Aplique_a_transparência_" src="https://github.com/user-attachments/assets/0cf9370c-adc6-4380-a224-e44fe2bc3461" />

2.1 Analise o problema

<img width="862" height="51" alt="2 1_Analise_o_problema" src="https://github.com/user-attachments/assets/8c5e984e-7fb9-46ae-89e4-12abf964f0cd" />

2.2 Aplique a transparência

<img width="1463" height="107" alt="2 2_Aplique_a_transparência" src="https://github.com/user-attachments/assets/3722d3e4-eeef-4e23-afa0-9aec8244c5d8" />

3.1 Analise o anti padrão

<img width="811" height="88" alt="3 1_Analise_o_anti_padrao" src="https://github.com/user-attachments/assets/954a4edd-887a-4004-9383-a15ff7a5b8bf" />

3.2 Instância A salva a sessão e encerra

<img width="810" height="87" alt="3 2_Instancia_A_salva_a_sessao_e_encerra" src="https://github.com/user-attachments/assets/066d5b7b-239e-4241-b156-1d9b8adbac30" />

5. Transparência de replicação

<img width="946" height="310" alt="5 Transparencia_de_replicacao" src="https://github.com/user-attachments/assets/f4ffce1b-be62-4b66-bedc-949b0da2f465" />

6.1 Sem concorrência

<img width="885" height="178" alt="6 1_Sem_concorrencia" src="https://github.com/user-attachments/assets/1ddc5c9b-e334-476c-9aca-99338e9ead64" />

6.2 Com concorrência

<img width="873" height="160" alt="6 2_Com_concorrencia" src="https://github.com/user-attachments/assets/385d8a95-9c70-4e5c-b985-e438db07f079" />
