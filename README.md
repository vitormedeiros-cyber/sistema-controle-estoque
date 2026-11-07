# InStok — Sistema de Controle de Estoque

Projeto acadêmico em Python, Flask, Jinja2, HTML/CSS/JavaScript e SQLite.
Arquitetura: routes → services → repositories → database/connection.

## Estado atual

Implementados nesta etapa: login com hash de senha, sessão, logout por POST,
proteção das páginas internas, gestão de usuários restrita a administradores,
CSRF e testes com banco temporário. Status e perfil são revalidados em cada
requisição autenticada. A sessão dura até oito horas sem renovação; o uso da
aplicação renova esse prazo. A sessão não contém senhas ou hashes.

O repository de usuários e parte do service já existem, mas os CRUDs pela
interface, inventário, movimentações, alertas, dashboard e relatórios ainda são
páginas provisórias. Os links de navegação não indicam que esses módulos estejam
concluídos. Login, menu lateral e temas usam o visual desenvolvido pela equipe,
adaptado aos templates Flask. O login é por usuário cadastrado, sem seleção de
perfil ou credenciais demonstrativas.

## Frontend integrado

Referência: [frontend da equipe, commit f999815](https://github.com/Mikelutti/sistema-controle-estoque-instok/commit/f999815540e44fd1ca8e051efb99d355382860e1),
arquivos `InStock Controle de Estoque frontend/index.html`, `styles.css` e
`script.js`. A adaptação preserva a paleta azul-marinho, composição do login,
menu lateral e temas. A fonte Manrope vem do Google Fonts com fallback Arial
quando o serviço não está disponível.

O JavaScript controla tema, visibilidade da senha e menu móvel. A preferência
de tema é o único dado salvo em localStorage. Login, logout e navegação usam
formulários e URLs reais; autenticação e permissões permanecem no servidor.
Não foram importados produtos, movimentos ou indicadores demonstrativos.

Verificação manual: entrar com uma conta existente, alternar tema, navegar
entre páginas e sair. Em tela estreita, abrir/fechar o menu e usar Escape.
O menu de usuários só aparece para administradores. As operações de estoque
e demais CRUDs ainda serão implementados.

## Executar no Windows / PowerShell

Na raiz do projeto (ambiente validado: Python 3.14.7):

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
$env:SECRET_KEY = (& .\.venv\Scripts\python.exe -c "import secrets; print(secrets.token_hex(32))")
$env:FLASK_DEBUG = "false"
.\.venv\Scripts\python.exe run.py
```

Acesse http://127.0.0.1:5000/. A raiz redireciona para o login ou dashboard.
Use o administrador já cadastrado no banco local. Não existe senha padrão.
A chave acima vale para o processo/sessão do terminal; alterá-la invalida cookies
anteriores. Para manter sessões entre reinícios, configure uma chave persistente
no ambiente de execução, fora do Git. A aplicação recusa chave ausente, curta
(menos de 32 caracteres fora de testes) ou igual ao valor do exemplo.

`.env.example` documenta as variáveis, mas **run.py não carrega `.env`
automaticamente**. Defina-as no ambiente. `DATABASE_PATH` aceita caminho absoluto
ou relativo à raiz do projeto; o padrão é `instance/controle_estoque.db`.
`FLASK_DEBUG=true` é apenas para desenvolvimento. Em hospedagem HTTPS, configure
`SESSION_COOKIE_SECURE=true`; o servidor de desenvolvimento Flask não é uma
configuração de produção.

## Inicialização de uma instalação nova

Os comandos abaixo **gravam no banco configurado**. Não são necessários para
reutilizar o banco local já inicializado:

```powershell
.\.venv\Scripts\python.exe inicializar_banco.py
.\.venv\Scripts\python.exe criar_admin.py
```

Execute a inicialização a partir da raiz. O primeiro comando cria as tabelas;
o segundo solicita os dados do administrador e gera o hash da senha. O schema
usa `IF NOT EXISTS`, que não migra tabelas antigas. Não remova o banco para
atualizar o projeto. Não há migração aplicada nesta etapa.

## Testes

Sem dependências adicionais: usa `unittest` da biblioteca padrão.

```powershell
.\.venv\Scripts\python.exe -B -m unittest discover -s tests -v
.\.venv\Scripts\python.exe -B testar_banco.py
```

Cada teste de integração cria SQLite em diretório temporário e o remove ao
terminar. `create_app` aceita configuração explícita; em `TESTING`, exige
`DATABASE_PATH` isolado e bloqueia o banco local. Não use o banco real em testes.
O script `testar_banco.py` executa somente os testes de constraints temporários
e retorna código diferente de zero se houver falha.

Há cobertura real em `test_auth.py`, `test_config.py`, `test_database.py` e
`test_script.py`. Os cinco arquivos antigos dos outros módulos ainda contêm
placeholders `assert True`, que não são contabilizados pelo unittest e não
representam cobertura funcional.

## Segurança e pendências

- Visitantes acessam login e arquivos estáticos; telas internas exigem sessão.
- O módulo de usuários exige ADMINISTRADOR. Demais permissões de escrita serão
  definidas ao implementar cada CRUD. O perfil vem do banco, nunca do formulário.
- Formulários POST devem conter `csrf_token`; APIs futuras podem enviar
  `X-CSRF-Token`. A validação também cobre PUT/PATCH/DELETE.
- Logout é POST com CSRF. Não reutilize links GET para encerrar sessões.
- A auditoria encontrou uma conta irregular produzida pelo script antigo.
  Ela não foi alterada; hashes inválidos não autenticam. O saneamento do banco
  depende de autorização separada.
- Limitação de tentativas de login e configuração de hospedagem ficam para a
  revisão de segurança antes de exposição pública.
- Nunca versione `.env`, banco local, ambiente virtual, senhas ou chaves.

Os planos e o progresso estão em `docs/plano-autenticacao.md` e
`docs/plano-integracao-frontend.md`.
