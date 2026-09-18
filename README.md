NutrIA — Plataforma de Saúde com Chat de IA

Plataforma web de saúde e nutrição com calculadoras, conteúdo informativo, cadastro de usuários e um assistente de IA restrito ao tema de saúde.

🔗 Demo: https://rthurms-dev.github.io/nutrIA-chat/

Funcionalidades
Calculadoras

Lógica de cálculo escrita em JavaScript, sem biblioteca externa:

IMC — índice de massa corporal com classificação da faixa
Ingestão diária de água — recomendação a partir do peso
Gasto calórico — estimativa de necessidade calórica diária
Chat com IA

Assistente especializado em saúde e nutrição, com duas camadas de controle:

Filtro de escopo no front-end — antes de qualquer requisição, a pergunta passa por uma verificação de palavras-chave (listas de termos permitidos e termos fora do domínio). Pergunta fora de saúde e nutrição é barrada localmente e não consome chamada de API.
System prompt dedicado — define o papel da assistente e reforça os limites do que ela responde.
Autenticação

Cadastro e login de usuários reais com Supabase, incluindo sessão persistente e atualização da navbar conforme o estado do usuário.

Conteúdo

Seções informativas sobre distúrbios alimentares e viroses, além de página de referências.

Arquitetura e segurança

A decisão técnica mais importante do projeto foi não expor a chave da API de IA no código do site.

Navegador  ──►  Cloudflare Worker  ──►  API de IA
(front-end)     (proxy / backend)       (provedor)
                       ▲
                       └── a chave vive aqui, como variável
                           de ambiente, nunca no client

Como o site é estático e hospedado no GitHub Pages, qualquer chave colocada no JavaScript ficaria pública no código-fonte. Por isso o front-end nunca fala com a API de IA diretamente: ele envia a mensagem para um Cloudflare Worker próprio, que guarda a credencial e faz a chamada. Assim a chave nunca trafega nem aparece para o usuário.

Stack

JavaScript (ES6+) · HTML5 · CSS3 · Bootstrap 5 · Supabase (PostgreSQL + Auth) · Cloudflare Workers · GitHub Pages

Como rodar

O site é estático — basta abrir o index.html no navegador ou acessar a demo.

Para rodar com o chat e o login funcionando, é preciso configurar:

um projeto no Supabase (URL e chave anônima) com Row Level Security ativado nas tabelas;
um Cloudflare Worker com a chave da API de IA guardada como variável de ambiente, e a URL do Worker apontada no front-end.
Próximos passos
Separar CSS e JavaScript em arquivos por responsabilidade (calculadoras, chat, autenticação)
Salvar o histórico de conversas do usuário logado no Supabase
Melhorar a responsividade em telas pequenas
Tratar erros de rede do chat com mensagem amigável
