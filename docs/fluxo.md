# Fluxo de automação

## Contrato entre etapas

Cada vídeo é uma pasta `saida/<id>/` com:

- `item.json`: dados da ferramenta (vem da fila)
- `roteiro.json`: cenas, falas e textos na tela
- `voz.mp3` e `voz.json`: áudio e timestamps por palavra
- `tela-*.webm`: gravações do Playwright
- `video.mp4`: render final
- `status.json`: estado (`rascunho` → `aprovado` → `publicado`) com URL de publicação

## Etapas

1. **Fila** (`fila/fila.json`): campos `id`, `ferramenta`, `repo`, `licenca`, `preco_software_pago`, `limite_real`, `publico_alvo`, `status`. Antes de entrar, checa duplicidade e licença.
2. **Roteiro**: Claude preenche o template de 60s. O gancho e a seção de limites passam por revisão humana.
3. **Voz**: gera áudio por cena e salva timestamps para sincronizar legendas.
4. **Captura**: um script Playwright por ferramenta executa instalação e demo em viewport 1080x1920, com cursor destacado.
5. **Montagem**: Remotion recebe `roteiro.json` + `voz.json` + vídeos de tela e renderiza as 6 cenas.
6. **Aprovação**: vídeo fica em `saida/` até alguém marcar `aprovado`.
7. **Publicação**: manual no início. TikTok via API exige app aprovado; YouTube Shorts e Reels via APIs oficiais.
8. **Registro**: atualiza `status.json` com URL pública e, depois, métricas (retenção nos 3 primeiros segundos, % assistido, views).

## Orquestração

- GitHub Actions (cron 8h Brasília): pega o próximo item da fila e gera até o rascunho.
- Segredos (ElevenLabs etc.) só em GitHub Secrets, nunca no repo.
- Branches antes da main.

## Roadmap

- **P0**: template Remotion de 6 cenas + script de captura de exemplo + 1 vídeo ponta a ponta.
- **P1**: voz automatizada com timestamps e legendas sincronizadas.
- **P2**: GitHub Actions diário gerando rascunhos.
- **P3**: publicação automatizada (Shorts/Reels) e painel de métricas.
