# Sport Center — Centro de Treinamento · Contexto completo do projeto

Documento de transferência para outra IA continuar o trabalho. Resume o briefing, as regras validadas, o que já foi construído e o que está pendente. Os pedidos novos (artefato 3D na introdução e ajustes no site) estão no fim, na seção "PRÓXIMA TAREFA".

---

## 1. Negócio

- **Nome:** Sport Center — Centro de Treinamento (CT)
- **Atividade:** musculação e fisiculturismo
- **Diferencial verificável:** **aberta 24 horas, todos os dias. É a ÚNICA academia 24 horas de São José da Lapa — MG.**
- **Unidade 1 — Jardim Encantado:** Av. Transamazônica, 1151 — Jardim Encantado, São José da Lapa — MG, 33350-000
- **Unidade 2 — Dom Pedro I:** Av. João Alves da Costa, 497 — Dom Pedro I, São José da Lapa — MG, 33350-000
- **Instagram:** @sportcenter.ct
- **WhatsApp:** PENDENTE. Precisa ser configurável por variável de ambiente (`NEXT_PUBLIC_WHATSAPP_NUMBER`).
- **Posicionamento:** acolhedor, também para iniciantes. Corpo e energia são o assunto. Tecnologia é só acabamento.

---

## 2. Identidade visual

| Papel | Valor | Uso |
|---|---|---|
| Fundo | `#000000` | preto profundo |
| Verde-neon | `#1EE6AA` | luz, halos, bordas, acentos, texto GRANDE. **Reprova como texto normal (3,57:1)** |
| Dourado | `#E8C428` | raio da logo, botões primários (texto preto sobre dourado 11,59:1), destaque |
| Dourado hover | `#F4D75A` | |
| Texto secundário | `#858D97` | 5,86:1 sobre preto |
| Creme de título | `#F6EEE2` | headlines |
| Superfícies | `#07090A`, `#0B0D0F`, `#101316`, `#14171A` | cards, chat |
| Tarja PLACEHOLDER | `#FF2BD6` (magenta) | conteúdo provisório. Não usar amarelo, porque se confunde com o dourado |

- **Contraste:** mínimo 4,5:1 para texto normal e 3:1 para texto grande e UI. Todo par precisa ser medido.
- **Tipografia:**
  - Archivo: pesos 900/800, `font-stretch` de 62 a 72% (condensada), caixa alta, tracking negativo (-0,02em).
  - Inter para o corpo.
  - JetBrains Mono para rótulos no estilo HUD.
  - Todas com licença aberta.
- **Estética:**
  - Cinematográfica: alto contraste, pretos esmagados, granulação de filme (overlay SVG `feTurbulence` a 8%), luz lateral dura, halo neon nas bordas do metal.
  - HUD leve: molduras finas, fonte mono, rótulos numerados ("01 — A ACADEMIA").
- **Logo:**
  - `assets/logo-sc.png`: monograma SC com o raio dourado.
  - `assets/simbolo-sc.jpg`: símbolo aprovado com explosão verde e faíscas (740×520, texto em inglês recortado por máscara). **Usar exatamente esse símbolo. Não recriar outro.**

---

## 3. Proibições (validadas com o cliente)

1. **Copy banida:** "o ferro é real", "repete", "não pare", "sem desculpas", "a desculpa não", "Fale menos. Treine mais.", qualquer uso de "ferro". É clichê de maromba e afasta o iniciante.
2. **Nada inventado:** sem preços, telefones, nomes de professores, depoimentos, número de alunos ou prêmios. Os valores R$ 179, R$ 229 e R$ 3,99 e os telefones de documentos antigos são FALSOS e devem ser descartados.
3. **Sem comparação não comprovável:** nada de "a maior", "a melhor", "Nº 01". Só "a única 24h de São José da Lapa", que é verificável.
4. **Sem promessa de resultado** ("resultado não tem hora marcada" e similares).
5. **Pessoas geradas por IA não podem posar como equipe ou alunos reais.** A foto `assets/equipe.jpg` é a ÚNICA foto real da equipe, e tem autorização de uso. Os avatares da assistente devem ser ilustração ou 3D estilizado, nunca fotorrealistas.
6. **Sem estética de dashboard,** sem linhas geométricas soltas, sem azul corporativo.
7. **Autoplay com som é proibido.** O vídeo fica sempre mudo por padrão, com botão para ligar o som.
8. **Sites de referência:** servem só para analisar padrões. É proibido copiar CSS, componentes, assets, fontes ou textos.
9. **Idioma:** português do Brasil.

---

## 4. Copy aprovada (toda marcada como COPY PROVISÓRIA até aprovação final)

- **Headline do hero** (3 linhas, a última em verde neon com glow). Opções:
  - "SEU HORÁRIO. / SEU TREINO. / **24 HORAS.**" (padrão)
  - "TREINE / A QUALQUER HORA. / **24H ABERTO.**"
  - "ABERTO / 24 HORAS. / **TODOS OS DIAS.**"
- **Diferencial:** "A única academia 24 horas de São José da Lapa."
- **Introdução:** "SPORT CENTER · CENTRO DE TREINAMENTO". A versão anterior usava "SEU FUTURO COMEÇA AQUI."
- **Alternativas aprovadas para seções:** "A GENTE NUNCA FECHA." · "ABERTA AGORA. ABERTA SEMPRE." · "AQUI A HORA É SUA." · "O ÚNICO CT DA REGIÃO QUE NÃO DORME."
- **Botões:** "QUERO COMEÇAR", "VER PLANOS", "Clique aqui e confira"
- **Equipe:** título "QUEM FAZ ACONTECER"
- **Planos:** MENSAL, TRIMESTRAL, ANUAL (o anual em destaque), SEM valores.
  - Benefícios com check verde: Acesso 24 horas · Livre trânsito nas duas unidades · Estrutura completa · Suporte profissional.

---

## 5. Arquivos do projeto

- `Sport Center v4.dc.html`: **versão final aprovada**. Avatar 3D de fundo no hero e vídeo atrás da tela 02. As anteriores foram mantidas para comparação: v2 e `Sport Center.dc.html` (v1).
- `Simulacao WhatsApp.dc.html`: simulação do atendimento com IA no WhatsApp.
- `assets/`:
  - `logo-sc.png`, `simbolo-sc.jpg`
  - `equipe.jpg` (1179×768; a foto contém uma arte "Nº 01" que precisa ser trocada pela original)
  - `sportcenter-vertical.mp4`: 720×1280, 16,7 s, H.264, 3,33 MB. É **vertical**; a versão 16:9 será gravada depois.
  - `frame-1/4/5/6/7.jpg`: frames extraídos do vídeo, usados como fotos provisórias da estrutura.
- `image-slot.js`: espaço de arrastar e soltar para os avatares.
- `ios-frame.jsx`: moldura de celular usada na simulação.
- `uploads/`: material original do cliente (brief, imagens de referência de layout, vídeo, foto).
- `README.md`: como rodar localmente (servidor estático, ex.: `npx serve .`, abrir `http://localhost:3000/Sport%20Center%20v3.dc.html`).

Os protótipos são HTML autocontido: estilos inline, lógica em classe JS e React injetado. **A implementação final prevista é Next.js (App Router) + TypeScript + Tailwind.**

---

## 6. O que está construído na v3 (estado atual)

### 6.1 Introdução / preloader
- Tela preta. O raio do símbolo aparece apagado e vai sendo "carregado" de dourado de baixo para cima, com a barra "CARREGANDO".
- Ao encher, o raio treme e EXPLODE: flash branco e dourado, onda de choque verde. O símbolo `simbolo-sc.jpg` acende com tremulação de letreiro neon.
- Surge "SPORT CENTER" (letras se aproximando) e "CENTRO DE TREINAMENTO" em dourado mono. Depois o site abre com fade.
- **Duração: 5,5 s**, ajustável de 3 a 8 s (Tweak `introSeconds`). O cliente pediu mais lento para dar tempo de ver.
- Pode ser pulada com clique, Esc ou o botão "PULAR ›". Roda 1 vez por sessão (`sessionStorage`), com modo "sempre" ou "desligado" para revisão.
- Com `prefers-reduced-motion`, mostra o estado final aceso e fecha em cerca de 2,6 s.
- Feita só com CSS e keyframes. O conteúdo já está no DOM, então a introdução não atrasa o LCP.

### 6.2 Fundo
- Vídeo em tela cheia, fixo atrás de todo o site, com **opacidade de 40%** (Tweak `videoOpacity`), `object-fit: cover`, gradientes escuros e vinheta.
- Mudo por padrão. O botão de som fica no hero.
- Carrega com lazy load (`requestIdleCallback`) e usa `frame-1.jpg` como poster. Não carrega com reduced-motion.
- Os elementos girando foram removidos a pedido do cliente.

### 6.3 Cabeçalho fixo
- Logo SC à esquerda com um brilho dourado e verde que passa por cima em loop. No hover, o raio passa na hora.
- **As duas unidades lado a lado** (Jardim Encantado | Dom Pedro I), em pílulas; a selecionada fica em dourado preenchido. No mobile os nomes ficam curtos (Jardim | Dom Pedro). A escolha fica salva em `localStorage`.
- Uma luz de raio corre de tempos em tempos pela borda inferior do cabeçalho.

### 6.4 Hero (01)
- Selo "24H · TODOS OS DIAS · AGORA hh:mm · ABERTA", com o relógio ao vivo.
- Headline gigante de 3 linhas, com a última em verde neon.
- Raio dourado + "A única academia 24 horas de São José da Lapa."
- Botões "QUERO COMEÇAR" (dourado preenchido, vai para o WhatsApp) e "VER PLANOS" (contorno dourado).
- Botão de som e o indicador "ROLE ↓".

### 6.5 Academia + Planos + Estrutura (02), numa tela só
- Composição que segue as imagens de referência 2 e 3 do cliente:
  - **Esquerda:** foto da equipe com halo verde difuso atrás e bordas esfumadas.
  - **Direita:** "QUEM FAZ ACONTECER", texto curto, os 4 benefícios com check verde e 3 botões de plano (Mensal / Trimestral / Anual, o anual com borda dourada e glow), seguidos de "Clique aqui e confira", que vai para o WhatsApp com o plano escolhido.
  - **Embaixo:** tira horizontal com 5 fotos da estrutura, em P&B contrastado e borda neon.

### 6.6 Contato (03)
- Letreiro "24 HORAS" gigante ao fundo, em baixa opacidade.
- "ABERTA AGORA. ABERTA SEMPRE." com o diferencial.
- Dois cards de unidade lado a lado, com endereço, "COMO CHEGAR" (Google Maps) e "FALAR COM ESTA UNIDADE". A unidade selecionada ganha a borda verde e o selo "SUA UNIDADE".
- Ícones grandes de WhatsApp e Instagram.

### 6.7 Trabalhe conosco
- Seção antes do rodapé, com o botão "ENVIAR MEU INTERESSE" (WhatsApp com a mensagem de candidatura e a unidade). Também tem link no rodapé.

### 6.8 Elementos globais
- **Transição entre telas:** a cada troca de seção, um raio dourado, branco e verde risca a tela na diagonal, alternando o lado, com flash e uma linha verde de varredura (0,6 s). Não aparece com reduced-motion.
- **Indicador de páginas:** no lado direito, 01 / 02 / 03 clicáveis e um halter dourado que desliza até a seção atual, com trilho verde de progresso. Aparece a partir de 720px; as seções reservam 124px à direita para que ele não cubra conteúdo.
- **Botão flutuante de WhatsApp** (verde) em toda a navegação.
- **Assistente virtual com IA:**
  - Botão redondo acima do WhatsApp, com selo "IA" e um balão depois de 5 s ("Quer ajuda para escolher seu plano?").
  - O chat responde de verdade, tem 4 sugestões rápidas e um seletor de avatar Ela/Ele (espaços para a arte), e termina em "CONTINUAR NO WHATSAPP".
  - Se identifica como "assistente virtual · pode errar".
- **Consentimento de cookies:** o evento `contact_intent` só é disparado depois do aceite.
- Granulação global, foco visível dourado, link "Pular para o conteúdo".

### 6.9 Contato centralizado (regra técnica)
- Um único ponto de contato: `contactHref(intent)` e `sendContactIntent({ origem, secao, plano, unidade, assunto? })`. O campo `unidade` é obrigatório.
- **PROIBIDO deixar URL de WhatsApp fixa nos componentes.** Na implementação final isso vira `src/lib/contact/`.
- Tweak `whatsappDestino`: "simulacao" (padrão agora, abre a simulação com a mensagem pronta) ou "real" (`wa.me/NUMERO?text=...`).

### 6.10 Tweaks disponíveis (v3)
`headline`, `introSeconds`, `introMode`, `avatar` (mulher/homem), `videoOpacity`, `videoSrc`, `showPlaceholders`, `whatsappNumber`, `whatsappDestino`.

### 6.11 SEO / acessibilidade
- Dois `ExerciseGym` / LocalBusiness em JSON-LD (um por unidade, 24/7).
- Respeita `prefers-reduced-motion`.
- Área de toque mínima de 44px.
- Conteúdo legível sem JS.

---

## 7. Simulação do WhatsApp (`Simulacao WhatsApp.dc.html`)

- Celular (moldura iOS escura) com um chat nas cores da marca. É um visual próprio, não uma cópia da interface do WhatsApp.
  - Tem "digitando…", confirmação de lida, respostas rápidas, cartão de localização com link para o mapa, anexo em PDF e o aviso "conversa transferida para a equipe".
- Recebe pela URL o texto, a unidade, o plano e o assunto vindos do site.
- **Modo "Conversa de exemplo":** roda sozinho, com pausar, reiniciar e velocidade 1×/2×.
  - Cenário "Quero treinar": a cliente fictícia Mariana pergunta sobre planos e valores, madrugada, endereço e instrutores, e agenda uma visita.
  - Cenário "Trabalhe conosco": o candidato envia o currículo.
- **Modo "Converse você":** a IA responde de verdade.
- **Regras da IA:**
  - Usa só os fatos da seção 1 e 4.
  - Quando perguntarem sobre preço, instrutores ou qualquer dado desconhecido, escreve `[VALOR DO PLANO · PENDENTE-CLIENTE]`, `[NOMES E HORÁRIOS DA EQUIPE · PENDENTE-CLIENTE]` ou `[INFORMAÇÃO · PENDENTE-CLIENTE]`, exibidos como tarja magenta.
  - Não promete resultado, não monta treino nem dieta e não finge ser humana.
  - Sem emojis e sem markdown.

---

## 8. Pendências do cliente

- Número do WhatsApp.
- Tabela de preços e lista da equipe, para alimentar a IA.
- Vídeo horizontal 16:9. No desktop, o vídeo vertical fica esticado e cortado; há uma tarja de placeholder.
- Foto original da equipe sem a arte "Nº 01", idealmente um PNG recortado com fundo transparente, para o halo ficar atrás das pessoas.
- Fotos reais dos equipamentos. Hoje são frames do vídeo.
- Logo em vetor (SVG).
- Arte dos avatares Ela/Ele: ilustração ou 3D estilizado, nunca foto realista.
- Aprovação final da copy.

---

## 9. Arquitetura técnica da versão final

- Next.js (App Router) + TypeScript + Tailwind. Sem banco de dados, sem login. Conteúdo em arquivos locais ou CMS headless.
- Animação com CSS e um motor mínimo de scroll que escreve variáveis CSS. **Não usar biblioteca pesada de animação.**
- **Orçamento de JS: no máximo 170 KB gzip por página.** O build deve medir e falhar se ultrapassar.
- Mídia com lazy load, nunca como LCP, com fallback em imagem estática no mobile, com reduced-motion e sem JS.
- A assistente de IA precisa de uma rota de API no servidor, com a chave protegida. O protótipo usa um recurso próprio do ambiente de prototipação.
- Analytics: evento `contact_intent` só depois do consentimento.

---

## 10. PRÓXIMA TAREFA (pedido novo)

### 10.1 Artefato 3D na introdução
Objetivo: criar uma versão 3D do símbolo SC com o raio dourado para a introdução.

- Base visual: `assets/simbolo-sc.jpg` e `assets/logo-sc.png`. O monograma SC em metal escuro com bordas acesas em verde-neon `#1EE6AA`, e o raio em dourado metálico `#E8C428` com emissão.
- Roteiro que já foi aprovado e deve ser mantido:
  1. Tela preta.
  2. O raio se carrega de energia (loading).
  3. O raio atinge o monograma: explosão, faíscas, fumaça verde, onda de choque.
  4. O SC acende em neon.
  5. Aparece "SPORT CENTER · CENTRO DE TREINAMENTO".
  6. O site abre.
- Ideias para o 3D: rotação lenta do SC com luz lateral dura e reflexos no metal, câmera se aproximando, partículas de faísca.
- Restrições:
  - Ser pulável e rodar 1 vez por sessão.
  - Com `prefers-reduced-motion`, mostrar direto o estado final aceso.
  - Não atrasar o LCP: carregar o 3D de forma assíncrona, com o conteúdo do site já no DOM.
  - Respeitar o orçamento de 170 KB gzip: preferir um GLB leve, comprimido com Draco ou meshopt, ou uma cena three.js mínima carregada só na introdução, com fallback para a versão CSS atual.
  - Opcional: exportar o modelo em GLB/OBJ para reaproveitar em redes sociais e vídeo.
- Não inventar outro símbolo. O 3D deve ser fiel ao monograma aprovado.

### 10.2 Ajustes no site
[preencher aqui a lista do que você quer mudar]

---

## 11. Checklist para qualquer mudança

- [ ] Nenhum preço, telefone, nome ou depoimento inventado
- [ ] Nenhuma copy banida (seção 3)
- [ ] Só a afirmação "única 24h de São José da Lapa", nenhuma de superioridade
- [ ] Verde só como acento ou texto grande; botões em dourado com texto preto
- [ ] Contraste medido (4,5:1 / 3:1)
- [ ] Vídeo mudo por padrão
- [ ] Respeita reduced-motion; foco de teclado visível; toque ≥ 44px
- [ ] Conteúdo provisório com tarja magenta PLACEHOLDER
- [ ] WhatsApp só via `contactHref`, sempre com `unidade`
- [ ] Responsivo em 390, 768 e 1440px
- [ ] Texto em português
