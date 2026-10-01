# Handoff: Sport Center — Centro de Treinamento (site institucional)

## Overview
Site institucional de 3 telas da Sport Center — Centro de Treinamento, academia de musculação e fisiculturismo com duas unidades em São José da Lapa — MG. O objetivo é levar o visitante ao WhatsApp da unidade escolhida.
Diferencial que pode ser comunicado: **aberta 24 horas, todos os dias, a única academia 24 horas de São José da Lapa.**

Leia também `CONTEXTO-PROJETO.md`: tem o briefing completo, as proibições do cliente, a copy aprovada e as pendências. **As regras da seção 3 desse documento (proibições) valem como requisito.**

## About the Design Files
Os arquivos em `prototipo/` são **referências de design feitas em HTML**: protótipos que mostram a aparência e o comportamento pretendidos. **Não são código de produção.** A tarefa é recriá-los na stack definida:

- **Next.js (App Router) + TypeScript + Tailwind.**
- Sem banco de dados e sem login. Conteúdo em arquivos locais (`src/content/*.ts`) ou CMS headless.

Para abrir os protótipos: `npx serve prototipo` → `http://localhost:3000/Sport%20Center%20v4.dc.html` (**versão final aprovada: v4**). Os arquivos `.dc.html` usam `support.js` (runtime do protótipo, que injeta React). A lógica de cada tela está na classe `Component`, no `<script data-dc-script>`.

## Versão final aprovada (v4) — diferenças em relação às especificações abaixo
O cliente pediu **replicação idêntica à v4 para hospedar**. Quando esta lista e o restante do documento divergirem, **vale a v4**:

1. **Avatar 3D como fundo do hero** (≥1024px e sem reduced-motion). Arquivo de referência: `prototipo/avatar-3d-bg.html`.
   - Ocupa o hero inteiro (`position:absolute; inset:0`), com o personagem deslocado para a direita (`camera.setViewOffset(w, h, -w*0.14, 0, w, h)`).
   - Máscara de degradê: `linear-gradient(90deg, rgba(0,0,0,.28) 0%, rgba(0,0,0,.55) 32%, #000 52%)` intersectada com `linear-gradient(180deg, transparent 0%, #000 14%, #000 82%, transparent 100%)`. Canvas com opacidade .92.
   - O conteúdo do hero fica por cima com `pointer-events:none`; só a linha de CTAs tem `pointer-events:auto`. Assim dá para arrastar o avatar e girar (OrbitControls, sem zoom e sem pan).
   - **Controles na página, não no canvas:** botão "3D" (círculo de 60px, borda `2px #E8C428`, glow dourado), nome do exercício (Archivo 800, 20px, caixa alta) e contador "NN/10 · ARRASTE PARA GIRAR". Posição: `left:64%; bottom:clamp(18px,3vh,34px)`. O botão fica desabilitado durante o teletransporte.
   - H1 com o avatar ligado: `clamp(50px,7.2vw,116px)`, `max-width:60%`.
   - **Personagem:** pele `#9a6f52`, regata `#15181b`, short e articulações `#0e1012`, tênis `#1b1f22`, faixas `#24292d`, cabelo `#0b0a09`. Corpo robusto: ombros e peito largos, trapézio, dorsais, oblíquos, bíceps, tríceps, antebraço, coxa e panturrilha. **Estilizado, não fotorrealista** (regra 5).
   - **Ambiente de academia** em volta: gaiola com barra e anilhas, rack de halteres em dois níveis, banco reto, 3 kettlebells, anilhas encostadas, parede escura, luzes neon lineares no teto, logo SC grande ao fundo (scale 3.4, z -4.4) e 260 partículas de poeira (verde e dourado, additive).
   - **Raio do teletransporte mais discreto** que no original: core ×1.7, traço 0.008, 2 ramificações, flash de intensidade 12, 140–160 faíscas e feixe com opacidade .45.
   - Fala do avatar: "Aberto 24 horas!". Fog `0x000000, 6, 19`, pixel ratio ≤1.25, sombras de 1024px.
   - Comunicação com a página (protótipo via `postMessage`; em produção, props e callbacks de um componente React): `{sc:'avatar', run}` pausa ou retoma o loop, `{cmd:'next'}` troca o exercício e `{cmd:'hello'}` pede o estado. O avatar responde com `{sc:'avatar-ex', name, count, total, busy}`.
2. **Vídeo da academia em segundo plano na tela 02** (Academia), atrás da equipe, dos planos e das fotos da estrutura.
   - Camada `absolute inset:0; z-index:0`. Vídeo com `object-fit:cover; object-position:50% 55%; opacity:.34; filter: contrast(1.2) saturate(.9) blur(1.5px)`.
   - Máscara vertical `transparent 0% → #000 18% → #000 80% → transparent 100%`.
   - Escurecimento por cima: radial `ellipse 80% 70% at 30% 45%, rgba(0,0,0,.55) → rgba(0,0,0,.15) 70%` + linear lateral `.35 → 0 40% → .35`.
   - A seção fica com `background:#000` e conteúdo em `z-index:1`.
   - Com o avatar ligado, **o vídeo de fundo global do hero não é renderizado**. No mobile (<1024px) e com reduced-motion, volta o comportamento da v3: vídeo em tela cheia de fundo e nada de 3D.
3. **Trabalhe conosco** fica só como uma linha discreta no fim da tela 03 (o site tem exatamente 3 telas).
4. O selo "PLACEHOLDER · VÍDEO VERTICAL" foi removido.

## Hospedagem
- Gerar um site estático: `next build` com `output: 'export'` funciona em Vercel, Netlify, Cloudflare Pages ou numa hospedagem comum. A exceção é a rota `/api/assistente`, que precisa de serverless (Vercel/Netlify Functions). Sem ela, o chat da assistente fica desligado e só o botão "Continuar no WhatsApp" aparece.
- Servir `.mp4` com `Accept-Ranges` e cache longo, e as imagens em AVIF/WebP via `next/image`.
- Domínio, HTTPS e redirecionamento www → apex.
- Definir `NEXT_PUBLIC_WHATSAPP_NUMBER` antes do deploy. Sem ela, os botões de contato ficam quebrados.

## Fidelity
**Alta fidelidade.** Cores, tipografia, espaçamentos, copy e interações são finais (a copy ainda está marcada como provisória até o cliente aprovar). Recriar pixel a pixel.

Exceção: tudo que tem a tarja magenta `PLACEHOLDER` / `PENDENTE-CLIENTE` é conteúdo provisório (ver Assets).

---

## Arquitetura obrigatória
- **Orçamento de JS: ≤ 170 KB gzip por página.** O build mede e falha se passar (ex.: `size-limit` ou script com `next build` + gzip das chunks da rota `/`).
- O avatar 3D (three.js, cerca de 150 KB) **fica fora do bundle inicial**: carregar com `dynamic(() => import(...), { ssr:false })` só quando o hero estiver visível e a abertura tiver terminado, em ≥1200px.
- Animação com CSS e um motor mínimo de scroll (IntersectionObserver escrevendo variáveis CSS / data-attrs). **Sem GSAP, Framer Motion ou similares.**
- **Contato centralizado** em `src/lib/contact/` como ÚNICO ponto de contato do site:
  ```ts
  type Unidade = 'jardim' | 'dompedro';
  type ContactIntent = { origem: 'site'; secao: string; plano?: 'Mensal'|'Trimestral'|'Anual'|null; unidade: Unidade; assunto?: 'trabalhe' };
  export function contactHref(i: ContactIntent): string;      // wa.me/<NEXT_PUBLIC_WHATSAPP_NUMBER>?text=...  — funciona sem JS (href real)
  export function sendContactIntent(i: ContactIntent): void;  // dataLayer.push({event:'contact_intent', ...i}) SÓ se consentimento === 'granted'
  ```
  - **Proibido** URL de WhatsApp fixa em qualquer componente.
  - `unidade` é obrigatório.
  - Os textos das mensagens são exatamente:
    - Padrão: `Olá! Vim pelo site da Sport Center e quero saber sobre {o plano X | os planos}. Unidade: {Jardim Encantado | Dom Pedro I}.`
    - Trabalhe conosco: `Olá! Vim pelo site da Sport Center e tenho interesse em trabalhar com vocês. Unidade de preferência: {nome}.`
- **Env vars:**
  - `NEXT_PUBLIC_WHATSAPP_NUMBER`: só dígitos, com DDI e DDD. **Pendente do cliente.**
  - `ANTHROPIC_API_KEY`: só no servidor, para a assistente.
- **Assistente de IA:**
  - Rota `app/api/assistente/route.ts`, no servidor, com rate limit por IP.
  - O prompt de sistema está na classe do protótipo (`SYSTEM` em `Sport Center v3.dc.html` e `SYSTEM()` em `Simulacao WhatsApp.dc.html`). Copiar literalmente: ele restringe a IA aos fatos aprovados.
- **SEO local:**
  - Dois `ExerciseGym` (LocalBusiness) em JSON-LD, um por unidade, 24/7. O JSON pronto está no `<helmet>` do protótipo.
  - `<html lang="pt-BR">`, title e description do protótipo.
- **Sem JS:** todo o conteúdo e todos os links de contato funcionam. A abertura não aparece e o vídeo não carrega.

---

## Telas / Views

Página única `/` com 3 seções. A largura máxima do conteúdo é 1320–1440px. Padding lateral: `clamp(20px,5vw,80px)` à esquerda; à direita, `max(clamp(20px,5vw,80px),124px)` em ≥720px, para reservar espaço ao indicador de páginas.

### 0. Abertura (preloader)
- Overlay fixo `#000`, z-index 200, por cima do site, que **já está renderizado no DOM** (nunca bloqueia o LCP).
- **Duração: 5,5 s.** Roda 1 vez por sessão (`sessionStorage.sc_intro_seen`).
- Pode ser pulada com clique em qualquer lugar, Esc ou o botão "PULAR ›" (canto superior direito, 44px, borda `rgba(133,141,151,.5)`, mono 12px).
- Símbolo `simbolo-sc.jpg` em `min(620px,92vw)`, proporção 740/520, com máscara radial `ellipse 50% 50%` (preto 70% → transparente) que esconde o texto em inglês da arte.
- Raio isolado por `clip-path: polygon(50.3% 29.8%, 58.6% 29.8%, 53.6% 50%, 57.7% 50%, 41.9% 88.8%, 46.9% 56.7%, 42.8% 56.7%)`.
- Linha do tempo, em % da duração:

| % | Evento |
|---|---|
| 0–48 | O raio aparece apagado (`brightness(.22)`) e uma cópia acesa (`brightness(1.6)` + drop-shadow dourado) "enche" de baixo para cima (`clip-path: inset(100% 0 0 0 → 0)`). A barra de 280×2px (gradiente `#A87810→#FFE24A`) faz `scaleX` 0→1, com "CARREGANDO" em mono 12px e tracking .24em em `#858D97` |
| 44–50 | O raio treme (translate ±2px) |
| 50–52 | Explosão: flash radial branco→dourado→verde (80% da largura, scale .3→1.2→2.2), tela branca a .75, anel verde 2px com scale .15→5 |
| 52–64 | O símbolo completo acende com tremulação neon (opacidade 1/.35/1/.6/1) e scale 1.18→1 |
| 60–72 | "SPORT CENTER": Archivo 900, `clamp(40px,7vw,84px)`, stretch 66%, `#F6EEE2`, text-shadow verde; letter-spacing .3em → -.01em |
| 66–78 | "CENTRO DE TREINAMENTO": mono 13px, tracking .3em, `#E8C428` |
| 86–100 | Fade-out do overlay |

- `prefers-reduced-motion`: mostra o estado final estático e fecha em 2,6 s.
- **Pedido em aberto:** versão 3D do símbolo (ver `CONTEXTO-PROJETO.md` §10.1), com esta versão em CSS como fallback.

### Fundo global
- `<video>` fixo, em tela cheia, `object-fit:cover`, **opacidade .4**, `filter: contrast(1.15) saturate(.95)`.
- Sempre `muted` e `playsInline`, com `preload="none"`, poster `frame-1.jpg` e `src` atribuído só em `requestIdleCallback`. Não carrega com reduced-motion.
- Camada de gradientes por cima: `linear-gradient(180deg, rgba(0,0,0,.55) 0%, rgba(0,0,0,.1) 35%, rgba(0,0,0,.45) 70%, rgba(0,0,0,.85) 100%)` + `radial-gradient(ellipse 90% 80% at 50% 45%, transparent 40%, rgba(0,0,0,.7) 100%)`.
- Granulação: `body::after` fixo com SVG `feTurbulence` (baseFrequency .85, 2 oitavas), opacidade .08, `pointer-events:none`.
- O vídeo é vertical (720×1280). Deixar a troca pelo 16:9 fácil: `src` vindo do conteúdo, com `<source media>` opcional.

### Header fixo
- Altura de 72px, `rgba(0,0,0,.6)` + `backdrop-filter: blur(12px)`, borda inferior `1px rgba(30,230,170,.14)`.
- **Logo** (`logo-sc.png`, 40px de altura): faixa de brilho (gradiente 105° dourado→branco→verde mascarado pelo próprio PNG) que percorre em loop de 4,5 s. No hover ou foco, passa uma vez em 0,7 s.
- **Unidades:** duas pílulas lado a lado (44px de altura, borda `1.5px #E8C428`, radius 999px, Inter 700 `clamp(12px,1.1vw,14px)`).
  - Ativa: fundo `#E8C428` com texto `#000`. Inativa: fundo `rgba(0,0,0,.5)` com texto `#E8C428`. Usar `aria-pressed`.
  - Textos: "Jardim Encantado" e "Dom Pedro I". Abaixo de 640px: "Jardim" e "Dom Pedro".
  - O rótulo "UNIDADE" (mono 12px `#858D97`) aparece a partir de 460px.
  - A escolha fica em `localStorage.sc_unidade` e alimenta todos os `contactHref`.
- **Raio no header:** linha de 220×2px (gradiente transparente→`#1EE6AA`→`#FFE24A`→branco, glow) que corre pela borda inferior em loop de 5 s `cubic-bezier(.6,0,.4,1)`.

### 01 — Hero (`#inicio`, min-height 100svh, conteúdo alinhado embaixo)
- **Selo 24H:** caixa com borda `1.5px #E8C428`.
  - À esquerda, bloco com gradiente 135° `#FFE24A → #E8C428 45% → #A87810` e texto "24H" (Archivo 900, 40px, stretch 66%) + ícone de raio preto.
  - À direita: "TODOS OS DIAS" (Archivo 800, 17px) e "AGORA hh:mm · ABERTA" (mono 12px `#858D97`, com relógio ao vivo atualizado a cada 20 s).
- **H1** em 3 linhas: Archivo 900, stretch 64%, caixa alta, tracking -.025em, line-height .88, `#F6EEE2`, `clamp(50px,11.5vw,144px)`.
  - Com o avatar visível (≥1200px), o tamanho cai para `clamp(50px,7.4vw,120px)` e a largura máxima fica em `calc(100% - clamp(260px,25vw,380px) - 48px)`.
  - A 3ª linha fica em `#1EE6AA` com `text-shadow: 0 0 24px rgba(30,230,170,.55), 0 0 80px rgba(30,230,170,.28)`.
  - Padrão: **SEU HORÁRIO. / SEU TREINO. / 24 HORAS.** Alternativas aprovadas: "TREINE / A QUALQUER HORA. / 24H ABERTO." e "ABERTO / 24 HORAS. / TODOS OS DIAS."
- **Diferencial:** raio dourado + "A única academia 24 horas de São José da Lapa." (Archivo 800, `clamp(18px,1.8vw,24px)`, caixa alta, branco).
- **CTAs** (56px de altura, radius 2px, Archivo 800, 17px, stretch 85%):
  - "QUERO COMEÇAR →": fundo `#E8C428` com texto `#000`; hover `#F4D75A` + glow dourado. Vai para `contactHref({secao:'hero'})`.
  - "VER PLANOS": borda `2px #E8C428`, texto dourado; hover com fundo `rgba(232,196,40,.12)`. Âncora `#academia`.
- **Botão de som** (à direita da linha dos CTAs): pílula de 48px, borda `rgba(30,230,170,.7)`, ícone de alto-falante + "SOM DESLIGADO"/"SOM LIGADO" (mono 12px), com `aria-pressed`.
- **"ROLE ↓":** centralizado embaixo, mono 12px, tracking .2em, `#858D97`.
- **Avatar 3D** (≥1200px; ver `prototipo/avatar-3d.html`):
  - Posição absoluta à direita, `bottom: clamp(140px,19vh,200px)`, largura `clamp(260px,25vw,380px)`, proporção 5/6.
  - **Sem moldura.** Máscara radial `ellipse 50% 52%` (preto 55% → .6 em 75% → transparente 100%) para fundir com a página; iframe com opacidade .92.
  - Loop pausado quando o hero sai da tela (protótipo: `postMessage({sc:'avatar', run:boolean})`). Em produção, virar componente React com `renderer.setAnimationLoop(null)`.
  - Fala do avatar: "Aberto 24 horas!" (não usar "Mais uma série!").

### 02 — Academia (`#academia`, uma tela só, 100svh)
- Fundo: `linear-gradient(180deg, rgba(0,0,0,.55), rgba(0,0,0,.82) 30%, rgba(0,0,0,.82))`.
- **Grid:** em ≥1024px, `minmax(0,1.1fr) minmax(0,.9fr)`, gap `clamp(24px,4vw,64px)`; abaixo, 1 coluna.
- **Esquerda:** foto `equipe.jpg` com `max-height:58vh`, `object-fit:contain`.
  - Máscara: linear vertical (topo .3 → preto 9% → preto 82% → transparente) intersectada com uma radial `ellipse 70% 92%`.
  - Halo atrás: radial verde `rgba(30,230,170,.6) → .2 em 45% → 0 em 72%`, `blur(46px)`.
- **Direita, de cima para baixo** (gap `clamp(16px,2.2vh,24px)`):
  - Rótulo "02 — A ACADEMIA" (mono 12px, tracking .14em, `#858D97`).
  - H2 "QUEM FAZ ACONTECER" (Archivo 900, stretch 66%, `clamp(42px,4.6vw,76px)`, line-height .9).
  - Parágrafo: "Musculação e fisiculturismo, aberta 24 horas, em duas unidades em São José da Lapa." (Inter 16–18px, line-height 1.55, máx. 44ch).
  - Lista com check verde (svg 18px, traço 3): Acesso 24 horas · Livre trânsito nas duas unidades · Estrutura completa · Suporte profissional. Grid `auto-fit minmax(210px,1fr)`.
- **Planos:** `role=radiogroup`, 3 botões em grid 3×1fr com gap 10px, altura mínima 84px, radius 4px.
  - Cada botão tem o rótulo "PLANO" (mono 12px) e o nome (Archivo 900, `clamp(24px,2.2vw,32px)`).
  - Mensal e Trimestral: borda `1px rgba(30,230,170,.45)`; selecionado: `2px #1EE6AA` com fundo `rgba(30,230,170,.10)`.
  - **Anual:** rótulo "DESTAQUE" e nome em `#E8C428`, borda `1.5px rgba(232,196,40,.7)` (selecionado: `2px #E8C428` com fundo `rgba(232,196,40,.12)`) e `box-shadow 0 0 32px rgba(232,196,40,.22)`. É o padrão selecionado.
  - **SEM VALORES.**
- **CTA "Clique aqui e confira":** dourado, 56px. Vai para `contactHref({secao:'academia', plano})`. Ao lado: "PLANO {X} · VALORES PELO WHATSAPP" (mono 12px `#858D97`).
- **Tira "ESTRUTURA":** 5 figuras em `grid-auto-flow:column`, `minmax(150px,1fr)`, altura `clamp(110px,16vh,170px)`, radius 4px, scroll-snap no mobile.
  - Imagens: `grayscale(.55) contrast(1.3) brightness(.85)`, com borda interna `inset 0 0 0 1px rgba(30,230,170,.4)` + `inset 0 0 20px rgba(30,230,170,.14)`.

### 03 — Contato (`#contato`)
- Fundo `rgba(0,0,0,.86)`, borda superior verde .14.
- Letreiro "24 HORAS" centralizado ao fundo: Archivo 900, `clamp(160px,30vw,520px)`, cor `rgba(30,230,170,.06)`, `-webkit-text-stroke: 1.5px rgba(30,230,170,.16)`, `aria-hidden`.
- Rótulo "03 — UNIDADES E CONTATO".
- H2 "ABERTA AGORA. ABERTA SEMPRE." (`clamp(46px,6.4vw,104px)`, máx. 14ch).
- Texto: "A única academia 24 horas de São José da Lapa. Duas unidades."
- **Cards de unidade** (grid `auto-fit minmax(340px,1fr)`): fundo `rgba(7,9,10,.9)`, radius 4px, padding `clamp(24px,2.6vw,36px)`.
  - Card da unidade selecionada: borda `#1EE6AA`, glow `0 0 40px rgba(30,230,170,.18)` e selo "SUA UNIDADE" (fundo `#1EE6AA`, texto preto). Card não selecionado: borda `rgba(133,141,151,.35)`.
  - Conteúdo: "UNIDADE 01/02", nome (Archivo 900, `clamp(34px,3.4vw,48px)`) e `<address>` em 17px.
  - Botões (48px): "COMO CHEGAR" (contorno, Google Maps) e "FALAR COM ESTA UNIDADE" (dourado, `contactHref` com a unidade do card).
  - **Unidade 01, Jardim Encantado:** Av. Transamazônica, 1151 / Jardim Encantado, São José da Lapa — MG / 33350-000.
  - **Unidade 02, Dom Pedro I:** Av. João Alves da Costa, 497 / Dom Pedro I, São José da Lapa — MG / 33350-000.
- **Ícones grandes** (círculos de 80px):
  - WhatsApp: fundo `#1EE6AA` com ícone preto e glow, texto "WHATSAPP".
  - Instagram: borda `2px #1EE6AA`, texto "INSTAGRAM" / "@SPORTCENTER.CT", link para `instagram.com/sportcenter.ct`.
- **Trabalhe conosco (discreto)** (`#trabalhe`): linha com borda superior `rgba(133,141,151,.22)`, 15px `#858D97`. Texto: "Quer trabalhar com a gente?" + link dourado "Envie seu interesse pelo WhatsApp →" (`assunto:'trabalhe'`).

### Rodapé
- Logo (34px de altura) + "© 2026 Sport Center — Centro de Treinamento · São José da Lapa — MG" (14px `#858D97`).
- Botões mono de 44px: "TRABALHE CONOSCO" (âncora) e "PREFERÊNCIAS DE COOKIES".
- O botão "REVER ABERTURA" existe só no protótipo. Remover.

### Elementos globais
- **Indicador de páginas** (≥720px): fixo à direita, centralizado na vertical.
  - Links 01/02/03 (mono 13px, 52px de altura): ativo `#1EE6AA`, inativo `#858D97`, com `aria-current`.
  - Trilho de 2px `rgba(133,141,151,.35)`, com preenchimento verde e glow.
  - Um **halter** dourado em SVG (28×14, rotacionado 90°) desliza até a seção ativa (top 30/88/146px) em 0,6 s `cubic-bezier(.16,1,.3,1)`.
- **Transição entre telas:** a cada mudança de seção ativa (IntersectionObserver, rootMargin `-45% 0px -45% 0px`) ou clique no indicador:
  - Overlay fixo de 0,6 s com flash radial dourado, linha verde de varredura de cima para baixo e raio SVG em zigue-zague diagonal.
  - Traço `#FFE24A` de 6px + branco de 2px + verde de 1,5px, animado por `stroke-dashoffset` 1→0, com o lado alternando a cada troca.
  - Não aparece com reduced-motion nem durante a abertura.
- **WhatsApp flutuante:** círculo de 62px `#1EE6AA` no canto inferior direito, com `contactHref({secao:'flutuante'})`.
- **Assistente IA:**
  - Botão: círculo de 62px acima do WhatsApp (bottom +76px), com borda `2px #1EE6AA`, avatar Ela/Ele dentro e selo "IA". Pulsa enquanto a IA digita.
  - Balão depois de 5 s: "Oi! Quer ajuda para escolher seu plano?".
  - Painel de 380×540, fundo `#0B0D0F`, borda verde .55:
    - Cabeçalho "ASSISTENTE SC" / "ASSISTENTE VIRTUAL COM IA · PODE ERRAR".
    - Seletor Ela/Ele.
    - Mensagens: usuário em `#E8C428` com texto preto; IA em `#14171A` com texto branco.
    - Sugestões: "Quais são os planos?", "Onde ficam as unidades?", "Abre de madrugada?", "Como trabalhar aí?".
    - Input e botão "CONTINUAR NO WHATSAPP" (verde, texto preto).
- **Cookies:** banner no canto inferior esquerdo com "ACEITAR" / "RECUSAR". A escolha fica em `localStorage.sc_consent`. Sem consentimento, o analytics não dispara.
- **Skip link** "Pular para o conteúdo" (aparece no foco).

## Simulação do WhatsApp (`prototipo/Simulacao WhatsApp.dc.html`)
Protótipo de vendas que mostra ao cliente como a IA atenderia no WhatsApp. **Não faz parte do site em produção.** Em produção, a IA do WhatsApp será configurada na WhatsApp Business API com o mesmo prompt de sistema. Detalhes em `CONTEXTO-PROJETO.md` §7.

---

## Interações e comportamento — resumo
- Todos os CTAs de contato são `<a href={contactHref(...)} target="_blank" rel="noopener" onClick={() => sendContactIntent(...)}>`.
- Âncoras com `scroll-behavior:smooth` (desligado com reduced-motion).
- **Foco:** `:focus-visible { outline: 3px solid #E8C428; outline-offset: 3px }` em tudo.
- Alvos de toque ≥ 44px.
- **Reduced motion:** desliga todas as animações, a transição de raio, o vídeo e os loops do header e do logo. A abertura mostra o estado final.
- **Breakpoints que precisam ser validados:** 390, 768 e 1440px.
  - <640px: nomes curtos das unidades.
  - <720px: sem o indicador de páginas.
  - <1024px: a seção Academia fica em 1 coluna.
  - <1200px: sem o avatar 3D.

## Estado
- `unidade: 'jardim'|'dompedro'`, com persistência.
- `plano: 'mensal'|'trimestral'|'anual'` (padrão anual).
- `muted: boolean` (padrão true).
- `secaoAtiva: 0|1|2`.
- `introVisivel`.
- `consent: 'granted'|'denied'|null`.
- Assistente: `aberto`, `persona`, `mensagens[]`, `ocupado`, `balaoVisto`.

## Design tokens
```
--preto: #000000        --verde: #1EE6AA (só acento/texto grande — 3,57:1)
--dourado: #E8C428      --dourado-hover: #F4D75A   --dourado-claro: #FFE24A   --dourado-escuro: #A87810
--creme: #F6EEE2 (títulos)   --cinza: #858D97 (texto secundário, 5,86:1)
--sup-1: #07090A  --sup-2: #0B0D0F  --sup-3: #101316  --sup-4: #14171A
--placeholder: #FF2BD6 (tarja magenta; texto #FF7AE6 na versão contorno)
Raios: 2px (botões), 4px (cards), 999px (pílulas), 50% (ícones)
Fontes: Archivo (wdth 62–125, wght 500–900) — títulos 900 stretch 62–72%, botões 800 stretch 85%
        Inter 400/500/600/700 — corpo
        JetBrains Mono 500/700 — rótulos HUD (12px, tracking .12–.2em)
Easing padrão: cubic-bezier(.16,1,.3,1)
```
Contraste mínimo: 4,5:1 para texto normal e 3:1 para texto grande e UI. Texto sobre dourado sempre `#000`.

## Assets (`prototipo/assets/`)
| Arquivo | Uso | Status |
|---|---|---|
| `logo-sc.png` | header, rodapé, máscara do brilho | Provisório: pedir SVG |
| `simbolo-sc.jpg` | abertura (símbolo aprovado, não recriar) | Aprovado |
| `equipe.jpg` | seção 02. Única foto real da equipe, com autorização | Provisório: pedir a original sem a arte "Nº 01", idealmente PNG recortado |
| `equipe-sc.jpg` | variação da foto da equipe | Referência |
| `sportcenter-vertical.mp4` | fundo (720×1280, 16,7 s, 3,33 MB) | Provisório: a versão 16:9 será gravada |
| `frame-1/4/5/6/7.jpg` | poster do vídeo + tira Estrutura | Provisório: trocar por fotos reais dos equipamentos |
| Avatares Ela/Ele | botão da assistente | Pendente: ilustração ou 3D estilizado, **nunca fotorrealista** |

Pendências de dados do cliente: número do WhatsApp, tabela de preços e equipe (só para a IA, **nunca publicar no site**) e aprovação da copy.

## Files
- `prototipo/Sport Center v4.dc.html`: **site completo, versão final aprovada (referência principal)**.
- `prototipo/avatar-3d-bg.html`: avatar 3D em three.js com o ambiente de academia (fundo do hero).
- `prototipo/Simulacao WhatsApp.dc.html`: simulação do atendimento com IA.
- `prototipo/support.js`, `image-slot.js`, `ios-frame.jsx`: runtime e auxiliares do protótipo (não portar).
- `CONTEXTO-PROJETO.md`: briefing completo, proibições, copy e pendências.

## Checklist de aceite
- [ ] Nenhum preço, telefone, nome, depoimento, número de alunos ou prêmio no site
- [ ] Nenhuma copy banida ("ferro", "repete", "não pare", "sem desculpas", "Fale menos. Treine mais.")
- [ ] Única afirmação comparativa: "a única academia 24 horas de São José da Lapa"
- [ ] Nenhum `wa.me` fora de `src/lib/contact/`; `unidade` sempre presente
- [ ] `contact_intent` só depois do consentimento
- [ ] Vídeo mudo por padrão, lazy, nunca LCP
- [ ] Bundle da página ≤ 170 KB gzip (build falha se passar); three.js em chunk separado e sob demanda
- [ ] Reduced motion, foco visível, alvos ≥ 44px, contraste medido
- [ ] Conteúdo e links funcionam sem JS
- [ ] 2× JSON-LD ExerciseGym
- [ ] Testado em 390 / 768 / 1440px
