# Contexto do projeto (leia primeiro)

Posts de texto para Instagram (cards tipográficos 4:5) do perfil **@eu.nohan** (Nohan Ventura, selo verificado, foto em `assets/avatar.png` recortada do print do perfil). Citações de mentalidade, estratégia, estoicismo e história (Napoleon Hill, Sun Tzu, Alexandre, Júlio César, Maquiavel, Marco Aurélio, Sêneca, Epicteto, Nietzsche, Dostoiévski), sempre com **autor** no card. Meta: 3 posts por semana (seg, qua, sex, 09:00 Brasília), gerados na nuvem, com notificação (issue) para revisar antes de publicar.

**Este projeto é separado do projeto do @gabrielvenezaa (repositório `Venezacode/post-automatico`). São pessoas e perfis diferentes: nunca misturar nome, @, foto, frases, histórico ou segredos entre os dois.**

Este repositório é público (Instagram/Buffer baixam as imagens por URL). **Nunca coloque chaves, tokens ou dados pessoais aqui.** Segredos ficam em GitHub → Settings → Secrets → Actions.

## Regras
- Português do Brasil, direto. O dono quer "faça por mim" e só cuidar de logins/senhas.
- Visual dos cards: manter os 8 estilos existentes (ver `render.py`/`config.yaml`). Estilos tweet: foto + "Nohan Ventura" + selo + @eu.nohan. Nos outros, @eu.nohan no card.
- **Frases verificáveis:** só citações de autoria estabelecida; preencher `obra` e `confianca` (alta/media/baixa). Frases virais de autoria duvidosa (ex.: Alexandre "não conquistei a metade do mundo…") ficam de fora. A IA não verifica fontes: conferir antes de postar.

## Como funciona
```
domingo 09:00 BRT -> GitHub Actions (.github/workflows/lote.yml)
   main.py lote 3 -> frases de fila.json (escritas pelo Claude em sessão; se acabar, IA via API se houver chave)
                  -> render.py (PNG) -> posts/*.png + historico.json + agenda.csv (commit)
   cria issue "Revisar posts da semana" -> painel docs/index.html (GitHub Pages, pasta /docs da main)
```
Para reabastecer a fila: pedir numa sessão do Claude "escreva mais N frases em fila.json". Imagens de fundo por IA não são usadas (a chave Gemini testada tem limite 0 para imagem no plano grátis).

## Arquivos e comandos
`config.yaml` (perfil, nicho, tom, pilares, estilos, agenda, IA), `writer.py` (IA Gemini/Claude), `render.py`, `main.py`, `fila.json`, `publisher.py` (API do Instagram, não usado), `SETUP.md` + `setup.sh` (chave Google AI Studio → secret `GEMINI_API_KEY`).
```
python main.py demo | preview N | lote N     # PA_DEMO=1 python main.py lote 3 testa sem IA
```
Segredos: `GEMINI_API_KEY` (ou `ANTHROPIC_API_KEY` se `provedor: claude`), opcional `NTFY_TOPIC`.

## Status
- Publicação automática no Instagram não configurada; hoje o agendamento é manual pelo painel/Buffer.
- Pendente: gravar `GEMINI_API_KEY` novo (a chave usada em testes foi colada no chat; apagar e recriar).
