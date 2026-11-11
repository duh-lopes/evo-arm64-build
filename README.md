# Patches da Evolution: canal por convite e mídia em canal

`midia-canal.patch`: foto com legenda em canal (@newsletter). Sobe a foto sem criptografia pelo endereço de canal e manda o handle (media_id) e o mediatype, como a whatsmeow.

Adiciona `POST /chat/fetchChannelByInvite/{instância}` com corpo `{"invite":"<link ou código>"}`.
Só leitura: usa `newsletterMetadata('invite', código)` do Baileys e devolve `{id (…@newsletter), name, description, subscribers}`.
Código inválido = 400; canal não achado = 404. A Edge Function deve limitar o uso (cache por código, `limite_whatsapp_conexao`).

Base: EvolutionAPI/evolution-api tag `2.4.0-rc2` (commit 5624bda…). Patch: `canal-por-convite.patch` (3 arquivos, ~46 linhas, testado com `git apply` e `tsc --noEmit`).

## Publicar
Actions → "Imagem Evolution (patch canais)" → Run workflow (demora 20–40 min, emulação arm64).
O último passo mostra `ghcr.io/…/evo-arm64:2.4.0-rc2-canais2@sha256:…`: fixar esse digest no `docker-compose.yml`.
A imagem nasce privada: a VPS precisa de `docker login ghcr.io` com um token só de leitura (read:packages) — feito pelo fundador.

## Nova versão da Evolution
Mudar `EVO_TAG`/`EVO_SHA` no workflow, rodar `git apply --check`; se falhar, refazer as 3 inserções.
