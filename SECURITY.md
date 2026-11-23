# Segurança

| Risco | Proteção | Onde |
|---|---|---|
| Acesso sem login | **Todo serviço** exige JWT (`Authorization: Bearer ...`), exceto `/saude` e o login. Não depende do gateway. | `app/auth.py` de cada serviço |
| Token falsificado | HS256 com segredo compartilhado, emissor, validade, `sub` e `perfil` obrigatórios; `alg: none` e perfis inventados são recusados | idem |
| Segredo no código | Só por `.env` ou volume; menos de 32 caracteres = o serviço não sobe | `app/config.py` |
| Token vazando entre serviços | O token só é repassado para serviços internos (Usuários e Inventário), nunca registrado em log | `chamados/app/clientes.py` |
| Senhas vazadas | scrypt com sal; a senha nunca é devolvida | usuarios |
| Adivinhar senhas | 5 erros = bloqueio temporário; mesma mensagem para e-mail inexistente | usuarios |
| Ver ou mexer no que não é seu | Permissões por perfil; solicitante e autor vêm do token; 404 para o que não é seu; lista de usuários sem e-mail | todos |
| XSS / clickjacking | Tela escapa HTML, CSP restritiva, `X-Frame-Options` e demais cabeçalhos | gateway |
| Dados enormes | Corpo limitado a 64 KB (gateway e cada serviço) e limites por campo | todos |
| Serviço exposto | Só o gateway publica porta (`127.0.0.1`); rede interna sem internet; serviços sem documentação Swagger | `docker-compose.yml` |
| Contêiner | Usuário sem privilégios, sistema de arquivos somente leitura, `cap_drop: ALL`, `no-new-privileges` | Dockerfiles e Compose |
| Dependências | Versões fixas, `pip-audit` por serviço no CI, Dependabot | `.github/` |
| Erros que vazam detalhes | Erros de banco respondem mensagem genérica; erros de rede entre serviços não mostram endereços | todos |

## Antes de usar em produção

- Defina `JWT_SEGREDO` no `.env` (ou use um gerenciador de segredos) e rode com `DADOS_EXEMPLO=0`.
- Coloque o gateway atrás de HTTPS.
- Troque o SQLite por PostgreSQL (um por serviço) com backup, e proteja o Redis com senha.
- Considere tokens curtos com refresh e/ou revogação (hoje uma conta desativada vale até o token vencer).
- Limite de tentativas de login em Redis, para funcionar com várias instâncias.
