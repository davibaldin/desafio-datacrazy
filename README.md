# Desafio Datacrazy

Parabéns! Se você chegou aqui, queremos te conhecer.

Temos alguns desafio que selecionamos pra testar os seus conhecimento. Você pode escolher quantos desafios estiver pronto para executar.

Bora?

## Desafio n.1 :: Front de mensageria

### Objetivo

Crie uma aplicação web utilizando React que permita:

1. ⁠Solicitar e obter permissão de áudio do navegador;
1. ⁠Capturar o áudio do usuário em tempo real;
1. ⁠Enviar o áudio para a API de transcrição da OpenAI (Whisper) e exibir o texto transcrito;
1. ⁠Enviar a transcrição para a API de completions da OpenAI (ChatGPT) e exibir a resposta gerada em tela.

### Requisitos Técnicos

1. ⁠Utilizar React (com ou sem framework como Next.js).
1. ⁠Organização de código em componentes reutilizáveis.
1. ⁠Utilizar boas práticas de estrutura de pastas e separação de responsabilidades.
1. ⁠Interação com as APIs da OpenAI (transcrição e completions).

### O que será avaliado

1. ⁠Clareza e organização do código;
1. ⁠Capacidade de encarar e resolver desafios técnicos, como lidar com permissões do navegador, conversão de áudio e chamadas assíncronas;
1. ⁠Boas práticas com React (componentização, estado, hooks, etc);
1. Se você usar IA para gerar seu código e o seu projeto, esteja pronto(a) para explicar tudo ao vivo. Queremos que você use IA para escalar a sua produtividade e não somente trabalhando por você.

Boa Sorte!


## Desafio n.2 :: Backend com Prisma pode ser reforçado?

### Objetivo

1. Crie uma class que exponha métodos DAO que implementam (create/delete/update/getById/findByName) de uma entidade Pessoa (abstraia as propriedades dessa classe, como nome, idade, cpf, endereço, email, telefone, etc.)
1. Nesta mesma interface construa mais dois métodos findByEmail() e findByTelefone() que ao invés de usar a API nativa de consulta do Prisma, utiliza um `SELECT ... FROM ... WHERE` nativo do RDBMS escolhido.
1. Nestes mesmos métodos guarde o resultado da consulta em algum cache (redis? - Talvez não, nos surpreenda se você encontrar algum cache melhor se existir), assim se o método for chamado novamente para um mesmo endereço de e-mail ele pega o resultado que está em cache.
1. Dica: você pode fazer um hash SHA256 do raw SQL pra usar como `key` desse cache.
1. Preveja quanto tempo seria seguro manter em cache.
1. Como forçar deletar do cache se o registro for atualizado no Prisma?




### Requisitos Técnicos

1. Usa o Prisma
1. Conectar a um banco de dados usando o prisma como o ConnectionFactory.
1. Usar SQL nativo.
1. Usar uma camada de cache no código.
1. Sofisticar o código com controle de cache como Evict e TTL.

### O que será avaliado

1. ⁠Clareza e organização do código;
1. ⁠Capacidade de encarar e resolver desafios técnicos, como lidar RDBMS e mecanismos de cache;
1. ⁠Boas práticas de cache e de performance;
1. Se você usar IA para gerar seu código e o seu projeto, esteja pronto(a) para explicar tudo ao vivo. Queremos que você use IA para escalar a sua produtividade e não somente trabalhando por você.

Boa sorte.