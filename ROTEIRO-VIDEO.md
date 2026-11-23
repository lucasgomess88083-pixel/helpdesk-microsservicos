# Roteiro do vídeo (4 a 6 minutos)

> O vídeo precisa ser gravado por você, com a sua voz. Este roteiro é só um guia: leia os arquivos citados
> antes, para conseguir explicar com suas palavras.

| Tempo | O que mostrar | O que falar |
|---|---|---|
| 0:00–0:40 | O diagrama Mermaid do README | Quais são os serviços, quem fala com quem (HTTP × evento) e por que só o gateway é exposto |
| 0:40–1:10 | `docker compose up --build` e depois `docker compose ps` | Todos os serviços "healthy"; abrir http://localhost:8000 |
| 1:10–2:30 | Tela: entrar como **Ana**, abrir chamado com equipamento; entrar como **Carla**, atribuir, resolver; Ana fecha | Mostrar nas notificações os avisos (vieram do Redis). Mostrar `docker compose logs chamados usuarios inventario` com as chamadas HTTP entre eles |
| 2:30–3:20 | Segurança: `curl localhost:8000/chamados` (401); token adulterado (401); Ana tentando ver chamado do Bruno (404) | Cada serviço confere o JWT sozinho; mostrar `servicos/chamados/app/auth.py` |
| 3:20–4:00 | Bancos separados: `docker volume ls` e `docker compose exec` ou mostrar `modelos.py` de Chamados | Sem chave estrangeira entre serviços; só o número do usuário/ativo |
| 4:00–5:00 | `docker compose stop inventario`; abrir chamado **com** equipamento (erro 503 claro) e **sem** equipamento (funciona); `curl localhost:8000/saude`; depois `docker compose start inventario` | Timeout de 3 s, mensagem clara, degradação parcial. Mostrar `clientes.py` |
| 5:00–5:40 | Tabela "O que mudou" do README | Chamada de função virou HTTP/evento; FK virou id solto; um banco virou vários; limitações (outbox, token) |
