# virais

Pipeline de automação para vídeos curtos de tecnologia do **ArtiSys / @artisysdev**.
Formato sem rosto: tela real + texto animado + voz em off.

Quadro inicial: **"Substitui software pago"** (60s).

## Fluxo

```
fila → roteiro → voz → captura de tela → montagem → aprovação → publicação → registro
```

| Etapa | Pasta | Ferramenta |
|---|---|---|
| 1. Fila de ideias | `fila/` | JSON/SQLite, checagem de duplicidade e licença |
| 2. Roteiro | `roteiros/` | Claude + template de 60s (gancho revisado por humano) |
| 3. Voz | `voz/` | ElevenLabs (ou voz do Hugging Face) + timestamps por palavra |
| 4. Captura de tela | `captura/` | Playwright, 1080x1920, dados fictícios |
| 5. Montagem | `video/` | Remotion, template de 6 cenas alimentado por JSON |
| 6. Aprovação | `saida/` | Humano revisa antes de publicar |
| 7. Publicação | `publicar/` | Manual no início; depois YouTube Shorts e Reels via API |
| 8. Registro | `registro/` | Só marca "publicado" com URL ou print; guarda métricas |

Detalhes em [docs/fluxo.md](docs/fluxo.md). Roteiro-modelo em [roteiros/template-60s.md](roteiros/template-60s.md).

## Regras editoriais

- CONSULTAR ANTES → PUBLICAR/CONFIRMAR → REGISTRAR DEPOIS.
- Verificar licença, instalação própria, limites e custos reais antes de recomendar.
- Priorizar ferramentas inéditas no catálogo e úteis para empresas reais.
- Automatizar a montagem, não o gancho nem o roteiro.

## Orquestração

GitHub Actions com cron diário às 8h (Brasília) para puxar o próximo item da fila e gerar o rascunho. Render do Remotion em CPU.

## Status

Estrutura inicial. Próximo passo: template Remotion de 6 cenas e script Playwright de captura.
