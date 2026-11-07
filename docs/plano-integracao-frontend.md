# Integração visual do frontend da equipe

Entrega autorizada: integrar login e navegação, testar no navegador e criar
commit local. Em 30/10/2026, o responsável solicitou também atualizar o
repositório GitHub. O banco existente permanece sem alterações.

Referência visual: https://github.com/Mikelutti/sistema-controle-estoque-instok/commit/f999815540e44fd1ca8e051efb99d355382860e1

Arquivos de origem: `InStock Controle de Estoque frontend/index.html`,
`styles.css` e `script.js`. Preservar identidade InStok, azul-marinho,
tipografia Manrope com fallback local, painel do login, menu lateral e temas.

## Abordagem

Adaptar a apresentação aos templates Jinja2 e URLs Flask existentes. As
alternativas de copiar o protótipo inteiro ou manter uma SPA separada repetiriam
autenticação e regras de negócio; não são necessárias para esta arquitetura.
Continuar usando login por usuário, perfil vindo do banco, CSRF e logout POST.
localStorage guarda somente tema. Não importar produtos, movimentações,
datas fictícias, seleção de perfil ou ações sem funcionalidade.

## Passos

1. Verificar estado Git e hash do banco; testar os contratos de navegação,
   permissões, indicação de página ativa e logout.
2. Adaptar `app/templates/base.html`, `auth/login.html` e parcial de mensagens;
   organizar estilos em `global.css`/`login.css`; implementar apresentação em
   `app.js` e inicialização de tema em `theme.js`.
3. Executar suíte, validar login, erros, navegação, tema, senha visível, logout
   e menu móvel no navegador com banco temporário.
4. Revisão independente, documentação e commit explícito dos arquivos da
   autenticação e desta integração (etapa anterior ainda sem commit).
5. Iniciar aplicação local para o usuário testar com sua conta existente.

## Limites e decisões

- Campos de custo e indicadores dependem de escopo/backend futuro.
- CRUD de usuários continua sendo a próxima entrega funcional.
- Demais telas permanecem provisórias e não exibem dados simulados.
- Menu com todos os módulos existentes; Usuários visível somente para admin.
- Mobile: abrir/fechar, overlay, Escape, foco e fechamento ao ampliar a janela.
- Testes e prévia usam somente banco descartável; a execução local normal
  reaproveita o banco existente sem inicialização, migração ou seed.
- O arquivo alheio `ntent .gitignore` ficará fora do commit.

## Progresso

- Estado inicial: autenticação e 27 testes já implementados; hash do banco
  ED24DEA5F81F0E442963B0E226E354B4422CE1DCD55604C737A4E77DA0678A33.
- Login e estrutura de navegação adaptados aos templates Flask, com tema,
  exibição de senha, menu móvel, indicação de página ativa e nomes escapados.
- Suíte completa executada em 30/10/2026: 30 testes aprovados em banco temporário.
  O banco local conserva o hash inicial. `git diff --check` sem erros.
- Login integrado exibido no navegador e controle de exibição de senha
  verificado. A rodada manual completa do menu móvel e dos temas ainda está
  pendente; não confundir implementação desses controles com homologação concluída.
- A revisão independente da autenticação anterior foi concluída. A tentativa
  de revisão independente desta integração foi interrompida por limite de uso;
  a revisão final desta entrega foi feita pelo próprio implementador.
- Commit inclui a autenticação da etapa anterior e a integração visual; o
  arquivo alheio `ntent .gitignore` permanece fora do versionamento.
