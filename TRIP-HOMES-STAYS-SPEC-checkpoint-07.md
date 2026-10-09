# TRIP HOMES STAYS LISTING AUTOMATION SPEC — Checkpoint 07

Sessão: 08/10/2026 · Imóvel de validação: PP01J · AL37 (RASCUNHO) · Área: Conteúdo · **Checkpoint 07 = checkpoint 06 + intake do bloco operacional "CASA 02 (Otávio)" + foto de capa + empacotamento do agente/plugin. Nenhum campo do Stays foi alterado neste checkpoint** (a aba estava em Config. Preços das Diárias, fora do escopo, por ação do usuário).
Exemplos marcados como (fictício) não são dados reais.

---

## [CHECKPOINT]

| Item | Resultado |
|---|---|
| Área | Conteúdo → Conteúdo descritivo (PT) + Amenities do anúncio + Cômodos (10 cômodos com fotos, 4 suítes com camas) |
| Campos SALVO-VERIFICADO | **Título PT/EN/ES** ("Casa Entremarés …") · **Localização completa** (País, Estado, Sigla, CEP, Cidade, Bairro, Rua, **Número 197**) · **8 atrações** em "Arredores e atrações" com distância do Google Maps · **7 textos × 3 idiomas (PT, EN, ES)**, conferidos caractere a caractere depois de recarregar · 7 textos PT, versão colada (comparados caractere a caractere após recarregar; substituíram a versão do PDF por correção do usuário) · 35 amenities do anúncio (34 + Estacionamento Gratuito) · Garagem Gratuita = Sim, 2 vagas · **10 cômodos, 73 fotos**: Área externa (13), cozinha (2), Detalhes de Luxo (10), Suíte 1 (10), Suíte 2 (10), Suíte 3 (9), Suíte 4 (6), Sala de estar (5), sala de jantar (5), Piscina (3). Tipo, nome, compartilhado, camas, quantidade de fotos e tags conferidos **no servidor** (resposta `room.getRoom`) depois de recarregar |
| Pendências | 29 (seção D: 25 anteriores + 25 a 28 novas), das quais 15 são **alertas de incompatibilidade para o usuário decidir** (seção D2, 1–15) |
| Regras | VALIDADAS: APR-001 a APR-025, mais P7, P8 e I1 da tabela B · PROVISÓRIAS: P1–P4, P6, P9 e P10 · INCERTAS: P5, I2, I3 e I4 |
| Ações humanas | 1 (login, já feita) |
| Objetos criados (neste checkpoint: nenhum no Stays; criados fora do Stays: plugin `trip-homes-stays`, PROMPT e MODELO de pedido) | 10 cômodos reais (nenhum de teste): Área externa (criado pelo usuário; nome completado por mim), cozinha, Detalhes de Luxo, Suíte 1–4, Sala de estar, sala de jantar, Piscina |
| Valores sobrescritos | 7 textos PT: a versão do PDF (gravada por mim nesta sessão) foi substituída pela versão colada, a pedido do usuário. Antes desta sessão, todos os campos estavam vazios. |

---

## A. SPEC STAYS VALIDADA (regras permanentes)

### A1. Acesso e carregamento
- **[APR-001] Login.** Sem sessão ativa, a tela de login aparece ("Welcome / Sign in to your account to continue", campos "Login" e "Senha", botões "Entrar" e "via Google"), mas o **título da aba continua "Anúncios"**. Para saber se há sessão, o agente procura o botão "Entrar" na página, não o título. Login sempre por HUMANO. · VALIDADO · D
- **[APR-010] Carregamento.** Depois de carregar a página pelo endereço, a tela leva de 10 a 15 s para montar todos os módulos. Às vezes aparece "Olá! Parece que houve problemas para carregar todos os módulos da página. Por favor atualize a página!". Quando isso acontecer: recarregar, no máximo 2 vezes. A tela de Conteúdo descritivo está pronta quando existem **19 editores** e as abas PT/EN/ES do Título aparecem. · VALIDADO · A · SCRIPT

### A2. Navegação (mapa de menus)
- **[APR-002]** Para chegar ao anúncio: escolher em "Ir para outro anúncio" (formato "ID | CÓDIGO - Nome") ou abrir `/i/apartment/{ID}` direto. O ID é o código Stays de 5 caracteres que aparece antes do código Trip Homes (ex. fictício: `XX00X - AL99 - …`). · VALIDADO · A · SCRIPT
- Abas do anúncio: **Conteúdo** · Financeiro · Distribuição · Auxiliares · Calendário.
- Seções de Conteúdo e seus endereços diretos:

| Menu | URL | Breadcrumb |
|---|---|---|
| Tipo | `/i/apartment/{ID}/type` | Tipo |
| Localização | `/location` | Localização |
| Amenities do endereço | `/amenities-location` | Amenities do endereço |
| Cômodos | `/rooms` | Cômodos |
| Amenities do anúncio | `/amenities` | **Amenities da unidade** (o nome no breadcrumb é diferente do menu) |
| Conteúdo descritivo | `/setup` | Conteúdo descritivo |
| Regras da acomodação | `/house_rules` | Regras da acomodação |

- O ⚠ amarelo no menu indica seção incompleta. O % no cartão (8% → 17% → 25%) só subiu depois de salvamentos e **não serve para conferir se um campo foi salvo**.

### A3. Salvamento
- **[APR-004]** Cada cartão tem seu próprio botão **"Salvar"**, que fica oculto até alguma alteração e aparece no cabeçalho do cartão. Ao clicar: aparece o aviso "Salvo com sucesso." e o botão volta a ficar oculto. A persistência só está confirmada depois de **recarregar a página e reler o campo**. · VALIDADO
- No cartão "Descrição", um único Salvar grava os 7 textos de uma vez.
- Em Amenities do anúncio, o Salvar do cartão "Amenities básicas" grava todas as caixas de seleção. Os cartões "Área" e "Garagem Gratuita" também mostraram Salvar depois das alterações: **não clicar neles sem dado**, porque gravaria o valor padrão ("Não").

### A4. Ativação e status
- **[APR-003]** Em várias telas aparece o aviso "Seu anúncio ainda não está ativado… **Ativar**". O agente **nunca** clica em Ativar e pode fechar o aviso no "×". O status fica no seletor do cartão ("RASCUNHO ▾"). Mudar o status exige confirmação humana. · VALIDADO · D

### A5. Tipo (`/type`)
- **Tipo de propriedade (endereço)**: seletor com 36 opções (ex.: Casa, Villa, Apartamento, Condomínio, Pousada…).
- **Tipo de anúncio**: seletor com 17 opções (ex.: Villa/Casa, Apartamento, Suíte, Estúdio…).
- **Subtipo**: botões Imóvel inteiro / Quarto privativo / Quarto compartilhado.
- **Categoria**: botões Aluguel por temporada / Compra e venda / Locação residencial.
- **Número de registro**: texto livre.
- **Prioridade comercial**: 5 estrelas.
- **Anotações internas**: texto livre, só para a equipe.
- O rótulo do título em Conteúdo descritivo usa o Tipo de anúncio (ex.: "Villa/Casa Título do anúncio").
- · VALIDADO

### A6. Localização (`/location`)
- Botões **Novo endereço** / **Vincular a existente**. "Vincular" usa o seletor "Endereço com várias unidades": o anúncio herda endereço e localização do endereço vinculado (é o caso de condomínios ou villas com várias unidades).
- Campos:

| Campo | Controle | Obrigatório | Limite |
|---|---|---|---|
| País | seletor com busca (227 opções, formato "Brasil (BR)") | **SIM** | — |
| Estado | texto | ? | — |
| Sigla do estado | texto (placeholder "XX") | ? | **3** |
| CEP | texto | ? | — |
| Cidade | texto | ? | — |
| Bairro | texto | ? | — |
| Rua | texto | ? | — |
| Número | texto | ? | — |
| Complemento | texto | ? | — |
| Mostrar o número do prédio aos usuários? | Global / Individual (Individual abre Sim/Não) | — | — |

- O mapa Google fica à direita e é **geocodificado pelo endereço**. Sem endereço, aparece o erro "Não é possível determinar as coordenadas. Por favor corrija os endereços ou posicione o marcador do mapa manualmente".
- **Fotos relacionadas ao endereço**: área de arrastar ou clicar, ligada a um campo de arquivo (`images`), então dá para enviar arquivos sem arrastar. É para arredores e áreas sociais do endereço.
- **Arredores e atrações**: botão "+ Item". A bolinha à esquerda destaca o item no site.
- · VALIDADO (exceto quais campos além de País são obrigatórios: INCERTO)

### A7. Amenities (`/amenities-location` e `/amenities`)
- **Filtro por canal de venda**: Todos · Itens Selecionados · bookingcom · airbnb · hvmi · decolar · googlevr. A lista muda conforme o canal. Para marcar, usar **Todos**.
- Tamanho das listas: **Endereço** tem 99 itens em 10 categorias; **Anúncio** tem 315 itens em 13 categorias.
- Os itens são caixas de seleção dentro de categorias recolhíveis, com contador "marcados / total". Há também "Busca Rápida".
- Cartões Sim/Não com sub-opções já pré-marcadas. No endereço: Check-in/checkout expressos, Estacionamento (No local · Indisponível · Público · Grátis), Internet a Cabo e Internet Wi-Fi (Em todo o estabelecimento · Grátis), Recepção 24 horas. No anúncio: **Área** (m², só o número) e **Garagem Gratuita**.
- **Regra:** valor pré-marcado não é dado. Ao marcar Sim, cada sub-opção tem que vir da fonte.
- O **endereço** é o nível do local, compartilhado (rótulos "(Compartilhada)", "uso comum"). O **anúncio** é o nível da unidade.
- Texto da interface: "Garagem Gratuita — esta opção não vale pra Booking. Para oferecer garagem no canal, marque estacionamento nas suas amenities do local."
- Texto da interface: "Selecione entre 5 a 10 amenities no mínimo, caso seu anúncio tenha."
- · VALIDADO

### A8. Cômodos (`/rooms`) e capacidade
- **[APR-005]** A capacidade **não é digitada**: ela vem das camas cadastradas em Cômodos. Texto da tela: "complete sua configuração de cômodos e camas para gerar o número máximo de pessoas". Em Regras, o campo "Adultos" fica **bloqueado**. · VALIDADO
- **Vídeo**: só o **ID do YouTube**, não o link inteiro.
- **HTML personalizado**: widgets ou iframes para o site.

#### [APR-013] Criar um cômodo · VALIDADO · B · SCRIPT
1. Clicar em "+ Quarto / Conjunto de Fotos" (`#btn-add-room`). Isso abre um **formulário vazio**: tipo, nome e fotos em branco, e "Este cômodo é compartilhado?" = Não. O cômodo **ainda não existe**; ele só é criado depois de clicar em Salvar.
2. "Qual é o tipo do cômodo?" (`room-type`, 15 opções): Banheiro · 1/2 Banheiro · Quádruplo · Suíte · Triplo · Com 2 camas de solteiro · Duplo · Individual · Estúdio · Sala/Área Comum · Outras Dependências.
3. Ao escolher "Outras Dependências", aparece "Como se chama este espaço personalizado?" (`room-name`), uma lista com busca.
   - A lista é **instável e cheia de repetições**: tinha 2.028, 1.885, 2.219 e 4.667 opções em carregamentos diferentes, com nomes criados por usuários e textos repetidos com IDs diferentes.
   - **Regra:** escolher pelo **texto exato**, nunca pelo ID. Se houver textos idênticos, usar o primeiro.
4. Contador de quantidade (`room-count`, padrão 1).
5. "Este cômodo é compartilhado?" Sim/Não (`shared-space`).
6. Clicar em **Salvar** no cartão "Qual é o tipo do cômodo?". **O nome só é gravado com esse Salvar.** Escolher na lista não grava sozinho: a escolha se perdeu ao recarregar.
7. **Como conferir:** recarregar e ver o cartão na lista com o **nome** (não "Outras Dependências") e a quantidade de fotos. Logo depois de salvar, o cartão pode mostrar "Outras Dependências" por um instante; o que vale é o estado depois de recarregar.
8. Para abrir um cômodo que já existe, clicar no nome dele na lista (`#room-list a.h5`).
- **Nunca** clicar no cartão vermelho "Remover … / Apagar" ("As fotos e o inventário serão perdidos").

#### [APR-014] Regras Trip Homes para cômodos (confirmadas pelo usuário) · VALIDADO
- **Todo cômodo que não é suíte** é criado como **Outras Dependências** (espaço personalizado).
- O nome do espaço é o da lista **mais parecido** com o cômodo que a fonte ou o usuário indicar. Exemplos reais: "Área externa", "cozinha", "Detalhes de Luxo".
- **Compartilhado = Sim** para áreas internas usadas por todo o grupo hospedado ("espaço dentro da casa, compartilhado entre a família"). Isso não quer dizer que a casa seja dividida com estranhos.
- **Foco da foto decide o cômodo:**
  - foto que mostra um **detalhe específico** da casa (luminária, almofada, peça decorativa, conchas, entalhes) vai para Outras Dependências → "Detalhes de Luxo" (ou "detalhes");
  - foto de **ambiente** vai para o cômodo daquele ambiente.
  - Executor: VISÃO.
- **Qualidade da foto:** sempre enviar o arquivo original. As fotos já foram tratadas pelo setor de mídia. Não redimensionar, não recomprimir e não descartar por tamanho ou orientação (vertical é aceita), mesmo com o aviso "Full HD acima de 1920×1080".

#### [APR-015] Fotos do cômodo · VALIDADO · A · SCRIPT
- O cartão "Fotos do cômodo" só aparece depois que o cômodo existe.
- O envio aceita arrastar, clicar ou usar o **campo de arquivo** `input[type=file][name=images]`, que fica oculto: `multiple`, aceita `image/jpeg, image/png, image/webp, image/avif`.
- Para enviar por automação: tirar a classe `hidden` do campo, enviar os arquivos para ele e depois devolver a classe.
- O envio é **automático**: as fotos aparecem e a contagem no cartão sobe sem clicar em Salvar. Esperar cerca de 15 s a cada 5 fotos.
- Lotes acima de 10 MB por envio devem ser divididos. Cada novo lote é acrescentado no fim da lista.
- Cada linha de foto tem:
  - alça de arrastar, que define a **ordem**;
  - "**Tag das Imagens**" (`image-tag-type`), seleção **múltipla** com busca, 149 tags;
  - uma caixa de seleção.
- As tags só são gravadas com o **Salvar do cartão Fotos**. Como conferir: recarregar e ler as tags de cada foto.
- **A ordem das fotos depois do envio NÃO é garantida** (VALIDADO: na Suíte 3 a ordem mudou em relação à ordem de envio). Antes de marcar tags diferentes por foto, conferir visualmente a ordem no Stays (APR-019).

#### [APR-016] Tag das fotos: a tag mais parecida com o cômodo (regra do usuário) · VALIDADO
| Cômodo | Tag usada |
|---|---|
| Área externa | "Área de estar" (o usuário confirmou como equivalente: área de *estar* externa) |
| cozinha | "Cozinha ou kitnet" |
| Detalhes de Luxo | "Detalhe decorativo" |
| Suítes (quarto) | "Quarto" |
| Suítes (banheiro da suíte) | "Banheiro" |
| Sala de estar | "Sala de Estar" |
| sala de jantar | "Sala de Jantar" |
| Piscina | "Piscina" |
#### [APR-017] Criar uma suíte (tipo "Suíte") · VALIDADO · B · SCRIPT
1. Clicar em `#btn-add-room` → formulário vazio → "Qual é o tipo do cômodo?" = **Suíte**.
2. Campos que aparecem para Suíte:
   - "Este cômodo é compartilhado?" Sim/Não (`shared-space`);
   - "O quarto possui fechadura?" Sim/Não (`door-lock`; "Selecione [Sim] se deseja exibir informações sobre a fechadura");
   - aviso "**As camas determinam a capacidade máxima do anúncio.**";
   - tipo de cama (`bed-type`, 11 opções): Cama (s) de Casal · de Solteiro · de Solteiro (Twin) · em Beliche (1 pessoa) · em Beliche (2 pessoas) · King · **Queen** · Colchão (ões) / Futón Casal · Colchão (ões) / Futón indiv. · Sofá-cama (s) · Sofá-cama (s) Casal;
   - quantidade (`bed-count`, padrão 1) e botão "+ Camas" (`.btn-add-bed`) para outra linha de cama.
   - **Suíte não tem campo de nome** (o cartão aparece só como "Suíte") e **não tem campo de banheiro**.
3. **Armadilha:** escolher só o tipo de cama **não libera o Salvar**. É preciso disparar `input/keyup/change` no campo de quantidade (`bed-count`); aí o Salvar aparece.
4. Clicar em Salvar no cartão do cômodo. O cartão aparece na lista como "Suíte / 1 / 2 / 0" (1 cama, 2 pessoas, 0 fotos).
5. Cada cama Queen vale **2 pessoas** (`_i_persons: 2`); códigos por canal: Airbnb `queen_bed`, Booking 86. Com 4 suítes queen, o campo **Adultos** em Regras passou para **8** (bloqueado, calculado).

#### [APR-018] Verificar cômodos pelo servidor, não pelo cartão · VALIDADO · SCRIPT
- **Armadilha:** a lista de nomes de espaço personalizado (`room-name`) é carregada **parcialmente e de forma diferente a cada carregamento** (118, 238, 441, 1.885, 2.219, 4.667 opções). Quando o ID do nome salvo não está na parte carregada, o cartão mostra "**Outras Dependências**" em vez do nome, **mas o nome continua salvo**.
- **Como conferir de verdade:** ao abrir um cômodo, a página faz a chamada `room.getRoom` (JSON-RPC). A resposta traz `_idtype`, `_idname`, `_b_sharedSpace`, `_i_order`, `beds[]` (com `_i_count` e o tipo) e `images[]` (com `tags`). A verificação final usa essa resposta, não o rótulo do cartão.
- **Nunca** salvar o cartão do cômodo quando o nome aparecer vazio ou "Não selecionado": isso pode apagar o nome. As tags podem ser salvas, porque o Salvar das fotos é um formulário separado.

#### [APR-019] Conferir a ordem das fotos antes de marcar tags · VALIDADO · VISÃO + SCRIPT
- Depois do envio, ler as miniaturas das linhas de foto (`a[style*=background-image]` → `/image/{id}`), montar uma faixa numerada e tirar uma captura de tela.
- Classificar cada posição (por exemplo, quarto ou banheiro) **olhando a faixa**, não pela ordem de envio.
- Remover a faixa auxiliar depois.

#### [APR-020] Regra de decisão do usuário: o laudo PDF é a fonte confiável · VALIDADO
- Em qualquer dúvida, consultar o **laudo de vistoria em PDF** (100% confiável, segundo o usuário).
- Qualquer **incompatibilidade** entre laudo, fotos, textos ou Stays: **alertar, identificar e deixar a decisão para o usuário**. O agente não corrige sozinho.

#### [APR-021] Textos em EN e ES (abas de idioma) · VALIDADO · B · SCRIPT
- Cada campo de texto tem abas **pt / en / es** (`a[href$="_pt_BR"]`, `_en_US`, `_es_ES`). Cada idioma tem seu próprio `textarea[name=pt_BR|en_US|es_ES]`.
- **O editor de EN/ES só é criado na 1ª vez que a aba é aberta** (summernote preguiçoso). Antes disso, o textarea fica oculto e vazio, e o número de editores na página muda.
- **Armadilha:** o 1º conjunto `textarea[name=en_US]` da página é o campo oculto de descrição completa (limite 65.535). Os 7 campos visíveis são os índices 1 a 7. A 1ª aba "en" da página é a do **Título**, não a da Descrição. Sempre pegar a aba **dentro do bloco do campo** (`textarea.closest('.form-group').parentElement.parentElement`).
- **Procedimento:**
  1. Clicar na aba do idioma dentro do bloco do campo.
  2. Esperar cerca de 1 s.
  3. Conferir que o editor está vazio.
  4. Gravar com `jQuery(textarea).summernote('code', '<p>…</p><p>…</p>')`, depois disparar `summernote.change`, e `input`/`keyup` no editor. O contador "caracteres restantes" atualiza e o Salvar aparece.
  5. Um único **Salvar** no cartão Descrição grava os 3 idiomas dos 7 campos.
  6. Recarregar e comparar o texto de cada `textarea` (`</p><p>` vira um parágrafo novo) com a fonte, caractere a caractere.
- Ao clicar nas abas de idioma, o Salvar do cartão **Título** também aparece. Não clicar nele (nada mudou no título).
- Fazer no máximo 3 campos por chamada de script, para não estourar o tempo limite.
- **Regra de conteúdo:** a tradução é **fiel à versão PT aprovada**: mesmos parágrafos, sem acrescentar nem tirar informação. Se o PT tiver uma incompatibilidade em aberto (D2-6, "duas geladeiras"), ela é traduzida como está e continua em aberto nos 3 idiomas.

#### [APR-022] Título do anúncio (cartão Título) · VALIDADO · B · SCRIPT
- Campos: `input[name=avatar]` = Nome interno (máx. 64) e `input[name=pt_BR|en_US|es_ES]` = Título por idioma, com abas pt/en/es **dentro do cartão** (`.panel` do input pt_BR). EN/ES ficam ocultos até a aba ser clicada, mas aceitam valor por script.
- Gravar: clicar na aba do idioma, `input.value = …`, disparar `input/keyup/change`; voltar para pt; **um Salvar** grava nome interno + 3 títulos. Conferir recarregando e comparando os 3 valores.
- **O Salvar do cartão grava tudo que estiver alterado no cartão, inclusive edições de outra pessoa não salvas** (foi o que aconteceu com o Nome interno nesta sessão). Antes de salvar, ler o valor atual de todos os campos do cartão e registrar o que estava diferente.
- O Nome interno **não** muda sozinho quando o título muda; são campos independentes.
- Regras de conteúdo (usuário): sem pontuação (regra da interface), **menos de 50 caracteres** (limite do Airbnb; a confirmar), e o nome próprio da casa **não pode aparecer no Google** como nome de outra hospedagem. Nas traduções, o nome próprio fica em português; só o conector muda (em / in / en).
- **Procedimento de unicidade:** (1) ler os 79 títulos do portfólio no seletor "Ir para outro anúncio" e checar palavra-chave; (2) buscar no Google o nome entre aspas, sem localidade ("Casa X"); (3) buscar com a cidade; (4) só aprovar se nenhum resultado trouxer o nome exato como hospedagem. Nesta sessão: "Casa Maré Alta", "Casa Arrecife", "Casa Maré Mansa" e "Casa Maré Cheia" reprovaram; "Casa Entremarés" passou.

#### [APR-023] Localização: preencher endereço e marcador · VALIDADO (marcador: PROVISÓRIO) · B
- "Novo endereço" já vem marcado. País via `jQuery('#adrcountry').selectpicker('val','BR')` + `change`; os demais são `#adrstate #adrstatecode #adrzip #adrcity #adrregion #adrstr #adrnum #adradd` (texto, disparar `input/keyup/change/blur`). O Salvar do cartão Endereço aparece após qualquer campo mudar; salvar, recarregar e comparar os 8 campos + País.
- **Geocodificação:** depois de salvar, o Stays posiciona o marcador pelo endereço e grava as coordenadas no HTML da página (procurar `-9.xxxxxx,-35.xxxxxx` em `main.innerHTML`). Sem número, o ponto cai no meio da rua: na AL37 ficou a **~107 m** do ponto real.
- **Marcador:** arrastar o pino (`.GMAMP-maps-pin-view`) move o pino e atualiza a coordenada na página, **mas não aparece Salvar e a posição não persiste** nem salvando o cartão Endereço (testado 2×; ao recarregar volta ao ponto geocodificado). Posicionar o marcador é **AÇÃO HUMANA** até se descobrir como o Stays grava isso. Arrastar duas vezes seguidas também falha (o 2º arrasto pode abrir um ponto de interesse do Google e mover o mapa).
- **Plus Code → coordenadas:** decodificar com Open Location Code (célula de 1° recuperada pela cidade) e **confirmar no Google Maps** (`/maps/search/lat,lon` devolve o Plus Code e o bairro). Google não mostra número de casa nessa rua; número só com fonte oficial.
- CEP: cidade usa a faixa 57180-000 a 57199-999; 57180-000 é o CEP geral (confirmado pelo Google Maps para o Plus Code). Ruas sem CEP próprio ficam com o geral.

#### [APR-024] Arredores e atrações (Localização) · VALIDADO · B · SCRIPT
- Botão "+ Item" = `#add-neighbourhoods`. Cada linha tem: **bolinha** (`input[type=radio][name=neighbourhoods…]`, "destacar no site"), **categoria** (`select[name=name]`, bootstrap-select: Café/Bar · Feira · Lago · Mar/Oceano · Montanha · Praia · Restaurante · Rio · Supermercados/mercearias · Teleférico), **nome** (texto, placeholder "nome"), **distância** (texto, placeholder "d", aceita decimal com ponto: 1.5) e **unidade** (`select[name=unit]`: ft · km · m · milhas). Não há campo de tempo.
- **A bolinha de destaque é obrigatória**: sem uma linha marcada, o Salvar mostra "Selecione uma destas opções" e não grava. Só uma linha pode ser destaque.
- Gravar por script: clicar em "+ Item", pegar a última linha, `selectpicker('val')` + `change` na categoria e na unidade, `value` + `input/keyup/change` em nome e distância, marcar a bolinha de uma linha, Salvar do cartão, recarregar e reler as linhas.
- Linhas adicionadas sem salvar somem ao recarregar (seguro para testar).
- **Limite do catálogo:** não há categoria para centro histórico, bairro, mirante ou falésia. Regra adotada: falésias e mirante → "Montanha" (relevo), lagoas → "Lago", rio → "Rio"; o que não encaixa (centro histórico, bairro gastronômico) **fica pendente para o usuário**, porque a categoria vai para os canais.
- **Fonte das distâncias:** valores do Google Maps (rota de carro mais curta, a partir do ponto confirmado da casa), decisão do usuário. Procedimento: `places_search` para obter o ponto de cada atração → `https://www.google.com/maps/dir/LAT,LON/LAT2,LON2/data=!4m2!4m1!3e0` → ler "N min / X km / via …" do painel. Pontos de água (lagoa, praia) podem cair no ponto errado da feição: conferir onde o Google colocou o pino.

#### [APR-025] Número e marcador: salvar o endereço re-geocodifica e **sobrescreve** o marcador manual · VALIDADO
- Ao salvar o cartão Endereço com o número preenchido, o Stays recalculou o marcador (de -9.831577,-35.884534, posição colocada à mão pelo usuário, para -9.8315818,-35.8843864, geocodificado para o nº 197, ~18 m do Plus Code). Ou seja: **qualquer salvamento do cartão Endereço apaga o ajuste manual do pino**. Ordem obrigatória: 1) todos os campos de endereço, 2) salvar, 3) só então ajustar o pino à mão (humano), 4) não salvar o cartão Endereço de novo.
- O Google rotula o mesmo ponto como "R. Escritor Félix Lima Júnior, **189**" (interpolação), enquanto o número confirmado é **197**: a numeração do Google não serve como fonte de número.

Outras tags úteis do catálogo: Sala de Estar · Sala de Jantar · Quarto · Foto de todo o quarto · Banheiro · Piscina · Vista da piscina · Churrasqueira · Jardim · Varanda / Terraço · Pátio · Fachada · Praia · Vista do Mar · Área para café / chá · Lounge ou bar · TV e multimídia.

### A9. Conteúdo descritivo (`/setup`)
- Cartão **Título**:
  - Nome interno: máx. 64 caracteres, já vem preenchido com "CÓDIGO - Nome".
  - Título do anúncio: abas PT/EN/ES. A interface pede "não incluir caracteres especiais, como pontuação ou símbolos".
- Cartão **Descrição**: 7 campos, todos com abas PT/EN/ES. Eles correspondem **1:1** às 7 seções de texto do laudo Trip Homes:

| Stays | Laudo Trip Homes | Limite |
|---|---|---|
| Descrição resumida | 1. Descrição resumida | **500** |
| Observações gerais | 2. Notas gerais | 49.150 |
| Sobre o espaço | 3. Sobre o espaço | 49.150 |
| Sobre o acesso ao espaço | 4. Sobre o acesso ao espaço | 49.150 |
| Sobre interação com anfitrião | 5. Interação com o anfitrião | 49.150 |
| Descrição do bairro | 6. Descrição do bairro | 49.150 |
| Informações sobre locomoção | 7. Informações sobre locomoção | 49.150 |

- O contador "N caracteres restantes" aparece abaixo de cada editor.
- O editor é **rich text (Summernote)**: um campo de texto oculto mais uma área editável visível. Enter cria um parágrafo novo. Quando a aba PT recebe texto, o ⚠ some.
- Existe um editor oculto extra, o 1º da página, que após salvar recebe a junção automática de Descrição resumida + Observações gerais. Nunca editar esse editor.
- **Proibido:** os botões "Reescrever" e o selo "AI", porque substituem ou criam texto.
- Cartão **Campos personalizados**: Guia do Hóspede, Rede wi-fi, **Senha wi-fi**, Acesso acomodação, **Senha porta**, Guia do hóspede, Estacionamento, Instrução check-in, Instrução check-out, Equipe interna, Contato portaria. Os campos de senha são sempre preenchidos por HUMANO.
- · VALIDADO

### A10. Regras da acomodação (`/house_rules`)
- **Adultos**: bloqueado (vem das camas).
- **Idade mínima**: placeholder 18; um campo vazio não é 18.
- **Crianças (2–12)** e **Bebês (0–2)**: Sim/Não + quantidade + texto PT/EN/ES.
- **Berços**: Sim/Não.
- **Fumar**: Sim/Não.
- **Pets**: Sim / Não / Mediante Solicitação, com Grátis / Possibilidade de Cobrança.
- **Eventos**: Sim/Não.
- **Regras de silêncio**: Sim/Não.
- **Regras adicionais**: vai para o site e para o Airbnb; o Airbnb aceita só um idioma, o padrão da Stays.
- O anúncio pode já ter valores salvos sem fonte (Nível 4).
- · VALIDADO

### A11. Procedimentos de execução validados
- **[APR-011] Digitar texto no editor.**
  1. Rolar até o campo.
  2. Pôr o foco no editor certo e **conferir o foco antes de digitar**: o elemento ativo precisa ser o editor esperado (o índice do editor na ordem da página bate com a tabela A9, contando o oculto como 0).
  3. Digitar parágrafo por parágrafo, com Enter entre eles.
  4. Conferir o contador de caracteres.
  5. Clicar em "Salvar" do cartão.
  6. Recarregar a página.
  7. Comparar o texto salvo com a fonte, caractere a caractere.
  - · VALIDADO · B · SCRIPT
- **[APR-012] Marcar amenities.**
  1. Localizar cada caixa pelo **rótulo exato**; o rótulo tem que existir **uma única vez**.
  2. Marcar só as que estão desmarcadas.
  3. Conferir os contadores das categorias.
  4. Clicar em "Salvar" do cartão "Amenities básicas".
  5. Recarregar a página.
  6. Comparar a lista de marcadas com a lista esperada: nenhuma faltando, nenhuma a mais.
  - Não é preciso abrir as categorias recolhidas.
  - · VALIDADO · A · SCRIPT

---

## B. REGRAS PROVISÓRIAS / INCERTAS

| # | Regra | Status | Como confirmar |
|---|---|---|---|
| P1 | Para casa independente, as amenities vão só no **anúncio**; o **endereço** fica vazio até haver dados (garagem gratuita, Wi-Fi). | PROVISÓRIO (decisão do usuário "por ora") | Regra Trip Homes definitiva |
| P2 | O Estacionamento para o Booking precisa ser marcado em Amenities do endereço, com sub-opções vindas da fonte. Na AL37, a gratuidade foi confirmada, mas o endereço continua vazio (regra P1); não confirmei se é preciso reservar. | PROVISÓRIO (texto da interface; não testado) | Decisão Trip Homes sobre o endereço |
| P8 | Cartão "Garagem Gratuita" (anúncio): ao marcar Sim, aparece a quantidade de vagas (`garagecount`). Salvar com o botão do próprio cartão. | VALIDADO | — |
| P9 | ~~Ordem das fotos = ordem de envio~~ → **falso** (APR-019). A 1ª foto do 1º cômodo vira a capa? Ordem atual dos cômodos: Área externa, cozinha, Detalhes de Luxo, Suítes 1–4, Sala de estar, sala de jantar, Piscina. | INCERTO | Regra Trip Homes de capa e ordem |
| P10 | Suítes ficam com compartilhado = **Não** (quarto privativo do hóspede que ocupa); a regra "compartilhado = Sim" vale para as áreas comuns da casa. | PROVISÓRIO (decisão minha, a confirmar) | Usuário |
| I4 | Cada foto tem um nome interno `_t_msname` igual ao **tipo** do cômodo ("Outras Dependências"), não ao nome personalizado. Não sei se isso vira legenda nos canais. A tela diz "adicione legenda nas imagens", mas não encontrei o campo de legenda. | INCERTO | Abrir uma foto com o usuário acompanhando |
| P3 | EN e ES ficam vazios sem tradução aprovada. | PROVISÓRIO | Regra Trip Homes |
| P4 | "Vincular a existente" serve para unidades do mesmo condomínio ou endereço. | PROVISÓRIO | Observar um anúncio vinculado |
| P5 | O limite do Título do anúncio vem dos canais, não do campo (o campo não tem limite). | INCERTO | Testar ou consultar a documentação Stays |
| P6 | **Contexto Trip Homes (não é regra do Stays):** os imóveis "geralmente" têm vaga para pelo menos 2 automóveis. O agente **não preenche** vagas nem estacionamento por essa expectativa: exige a quantidade na fonte do imóvel. Se a fonte não informar vagas, abrir PENDÊNCIA. | PROVISÓRIO | — |
| P7 | Para **substituir** um texto: pôr o foco no editor, selecionar todo o conteúdo do editor, apagar, conferir que ficou vazio (0 caracteres), digitar, salvar, recarregar e comparar. Depois de apagar, a Descrição resumida ficou como texto simples, sem parágrafo; o conteúdo salvo ficou idêntico. | VALIDADO | — |
| I1 | ~~O que "+ Quarto / Conjunto de Fotos" faz~~ → resolvido: ver APR-013 (abre um formulário vazio; o cômodo só é criado com Salvar). | VALIDADO | — |
| I2 | Quais campos de endereço, além de País, são obrigatórios. | INCERTO | Tentar salvar só com País (com o endereço real) |
| I3 | Mapeamento de rótulos ambíguos: "Ducha", "Itens Básicos de Cozinha", "Copos de Vinho", "WC para Convidados", "Sistema de segurança" (câmeras), "Ar condicionado controlado individualmente no quarto", "Área de estar com sofá/cadeira". | INCERTO | Definição Trip Homes de cada rótulo |

---

## C. FICHA DO IMÓVEL — AL37 (usado neste cadastro)

| Campo | Valor | Fonte |
|---|---|---|
| Stays | PP01J · AL37 · Casa Maré Alta em Barra de São Miguel · RASCUNHO | N1 (usuário confirmou que é a casa do laudo) |
| Município/UF | Barra de São Miguel / AL | N2 laudo 07/10/2026 |
| Tipo | Casa · Villa/Casa · Imóvel inteiro · Aluguel por temporada (já estava no Stays; bate com o laudo, não alterado) | N2 + N4 |
| Suítes | 4, cada uma com 1 cama queen, ar-condicionado, TV 32", frigobar e banheiro privativo; a master tem área de estar e closet (ver alertas D2 sobre a master) | N2 |
| Banheiros | 5 (4 das suítes + 1 social) | N2 |
| Garagem | interna, 2 vagas | N2 |
| Água quente | a gás | N2 |
| Áreas restritas | guarda-roupa do corredor, armários da bancada de apoio, closet da master | N2 |
| Câmeras | só na área externa | N2 |
| Textos EN/ES 1–7 | tradução fiel da versão PT colada, feita pelo agente a pedido do usuário. ES em **espanhol latino-americano neutro** (estadía, autos, parlantes, mesada, freezer, refrigeradores) | N1 (pedido do usuário) |
| Título do anúncio | **Casa Entremarés em Barra de São Miguel** / in / en (38 caracteres; escolhido pelo agente a pedido do usuário, após checagem de unicidade). Valor anterior gravado: "Casa Maré Alta em Barra de São Miguel"; no momento do salvamento o campo estava como "Casa em Barra de São Miguel" (edição do usuário não salva) | N1 (delegação) |
| Nome interno | "AL37 - Casa Maré Mansa em Barra de São Miguel" — **alterado pelo usuário na aba** (antes: "AL37 - Casa Maré Alta …") e gravado junto com o Salvar do título. Não alinhado com o título; decisão pendente | N4/N1 |
| Endereço | Brasil (BR) · Alagoas · AL · CEP 57180-000 · Barra de São Miguel · Porto de Vacas · Rua Escritor Félix Lima Júnior, **197** (confirmado pelo usuário) · Plus Code 5498+954 = -9.8316125, -35.8845469. Marcador do Stays agora: **-9.8315818, -35.8843864** (geocodificado com o nº 197, ~18 m do Plus Code; substituiu o ajuste manual do usuário) | N1 + Google Maps |
| Arredores e atrações (8, distâncias do Google Maps de carro) | ★ Praia da Barra de São Miguel 1.5 km (destaque no site, escolha do agente) · Praia Niquim 1 km · Lagoa do Roteiro 9.9 km · Falésias da Barra 8.3 km (Montanha) · Praia do Gunga 13.3 km · Falésias do Gunga 16.3 km (Montanha) · Mirante do Gunga 12 km (Montanha) · Praia do Francês 10.2 km | N1 (lista do usuário) + Google Maps |
| Distâncias medidas (não gravadas) | Rio Niquim: sem ponto no Google (Chácara Niquim 5.0 km) · Centro Histórico de Marechal Deodoro 17.8 km · Lagoa Manguaba 46.8 km (extremo norte; margem em Marechal ~18 km) · Massagueira 14.1 km · Aeroporto Zumbi dos Palmares 56.0 km / 1 h 04 · Centro de Maceió 29.0 km / 35 min | Google Maps |
| Textos PT 1–7 | **versão colada no chat** (correção do usuário; substituiu a versão do PDF); marcas "Laudo-Vistoria-Barra-Sao-Miguel…" removidas | N1 |
| Estacionamento (portfólio) | "geralmente todos os imóveis têm vaga para pelo menos 2 automóveis" → expectativa do portfólio, **não** dado de cada imóvel; na AL37, as 2 vagas internas estão confirmadas pelo laudo | N1 (contexto) |
| Estacionamento | **gratuito** (usuário) · garagem interna, 2 vagas (laudo) → amenity "Estacionamento Gratuito" + Garagem Gratuita = Sim, 2 | N1 + N2 |
| Cômodos criados (ordem no Stays) | 1 Área externa (Outras Dep., compart. Sim, 13 fotos, Área de estar) · 2 cozinha (2, Cozinha ou kitnet) · 3 Detalhes de Luxo (10, Detalhe decorativo) · 4 Suíte 1 (Suíte, compart. Não, 1 Queen, 10 fotos: 7 Quarto + 3 Banheiro) · 5 Suíte 2 (idem, 10: 7+3) · 6 Suíte 3 (idem, 9: 7+2) · 7 Suíte 4 / master (idem, 6: Quarto) · 8 Sala de estar (Outras Dep., Sim, 5, Sala de Estar) · 9 sala de jantar (5, Sala de Jantar; inclui a foto `89dfa04f`) · 10 Piscina (3, Piscina) | N1 (fotos e lotes do usuário) + N2 (camas) |
| Capacidade no Stays | **8 adultos**, calculado pelas 4 camas Queen (bloqueado). A capacidade autorizada continua **pendente** no laudo | derivado, não autorizado |
| Bloco operacional "CASA 02 (Otávio)" (recebido após o cp-06, **ainda não gravado**) | Acesso: rua pública, chave manual e controle remoto · Staff: Trip Homes, sem staff inclusa · Caseiro/jardineiro/piscineiro: Júnior (contato guardado fora deste relatório; vai só para o campo "Equipe interna" se aprovado) · Pets: sob demanda · Sem gerador · Wi-Fi: existe, rede "Avelino" (**senha recebida no chat: não registrada, não digitada — campo "Senha wi-fi" = HUMANO**) · Restaurantes: Engenho da Barra, Vila Niquim, Ceceu (frutos do mar) · Farmácias: RM, Verde · Fornecedores: mercearia (água/gás), Supermercado Unicompras (+ padaria), açougue/peixaria/bebidas em frente, Ponto Verde Pescados | N1 (documento interno Trip Homes) |
| Foto de capa (pedido do usuário) | Foto da sala de estar enviada 4× (1280×853, idênticas: md5 61ccd284…) = **mesma cena** da foto já carregada `c9585d86` (2048×1365, 4ª da Sala de estar; diferença média 2,1/255). Pela regra "sempre a qualidade original", a capa deve usar `c9585d86`, não o arquivo menor. **Mecanismo de capa no Stays ainda não mapeado (P9)** | N1 + VISÃO |
| Amenities do anúncio (35) | Estacionamento Gratuito · Piscina Privada · Jardim ou Quintal · Varanda · Mobília de Exterior · Área de Refeições Exterior · Água Quente · Toalhas · Vaso sanitário · Churrasqueira · Fogão · Forno · Microondas · Geladeira · Freezer · Pia · Cozinha Completa · Louças e Talheres · Mesa de Jantar · Assentos na Sala de Jantar · Cafeteira · Liquidificador · Mini-frigorífico · TV · Smart TV · Sistema de Som · Rede de Descanso · Ar Condicionado · Interfone · Roupas de cama · Guarda-roupa ou Armário · Máquina de Lavar Roupa · Sofá · Grades de Janela · Lixeiras | N2 (inventário) + N1 (lista do usuário) |

### Registros [CAMPO]
Valores atuais (versão colada, gravada depois da correção):
- [CAMPO] Descrição resumida (PT) | /setup › Descrição | editor rich text | — | 500 | 432 caracteres | N1 | **SALVO-VERIFICADO**
- [CAMPO] Observações gerais (PT) | /setup › Descrição | editor rich text | — | 49.150 | 917 caracteres, 5 parágrafos | N1 | **SALVO-VERIFICADO**
- [CAMPO] Sobre o espaço (PT) | idem | 1.713 caracteres, 10 parágrafos | N1 | **SALVO-VERIFICADO**
- [CAMPO] Sobre o acesso ao espaço (PT) | idem | 435 caracteres, 4 parágrafos | N1 | **SALVO-VERIFICADO**
- [CAMPO] Sobre interação com anfitrião (PT) | idem | 393 caracteres, 3 parágrafos | N1 | **SALVO-VERIFICADO**
- [CAMPO] Descrição do bairro (PT) | idem | 536 caracteres, 3 parágrafos | N1 | **SALVO-VERIFICADO**
- [CAMPO] Informações sobre locomoção (PT) | idem | 285 caracteres, 2 parágrafos | N1 | **SALVO-VERIFICADO**

Valores anteriores, substituídos (versão do PDF): 439 / 1.027 / 770 / 480 / 371 / 510 / 214 caracteres.

- [CORREÇÃO] Textos 1–7 | Valor anterior: versão do PDF (já gravada e verificada) | Valor oficial atual: versão colada no chat | Fonte: usuário | Outros campos afetados: nenhum no Stays. As amenities já tinham sido derivadas do inventário e da lista colada.
- [CAMPO] Amenities do anúncio | /amenities | caixas de seleção | — | — | 35 itens (34 + Estacionamento Gratuito) | N2/N1 | **SALVO-VERIFICADO**
- [CAMPO] Garagem Gratuita | /amenities | Sim/Não + quantidade | — | — | Sim, 2 | N1 + N2 | **SALVO-VERIFICADO**
- [CAMPO] Cômodo Área externa | /rooms | tipo, nome, compartilhado, fotos, tags | — | — | Outras Dependências · "Área externa" · Sim · 13 fotos · Área de estar | N1 | **SALVO-VERIFICADO**
- [CAMPO] Cômodo cozinha | /rooms | idem | — | — | Outras Dependências · "cozinha" · Sim · 2 fotos · Cozinha ou kitnet | N1 | **SALVO-VERIFICADO**
- [CAMPO] Cômodo Detalhes de Luxo | /rooms | idem | — | — | Outras Dependências · "Detalhes de Luxo" · Sim · 10 fotos · Detalhe decorativo | N1 | **SALVO-VERIFICADO**
- [CAMPO] Suítes 1–4 | /rooms | tipo, compartilhado, cama, fotos, tags | — | — | Suíte · Não · 1× Cama (s) Queen cada · 10/10/9/6 fotos · Quarto/Banheiro | N1 + N2 | **SALVO-VERIFICADO** (servidor)
- [CAMPO] Sala de estar / sala de jantar / Piscina | /rooms | idem | — | — | Outras Dependências · Sim · 5/5/3 fotos · Sala de Estar / Sala de Jantar / Piscina | N1 | **SALVO-VERIFICADO** (servidor; o cartão às vezes mostra "Outras Dependências", ver APR-018)

### Divergências registradas
- [DIVERGÊNCIA] Número da casa | Bloco "CASA 02 (Otávio)" (N1, doc. interno) = **189** · Google (N3, interpolação) = 189 | Confirmação do usuário no chat (N1, mais recente) = **197** (gravado) | O número vai para canais, hóspedes e geocodificação (pino já recalculado para o 197) | Decisão: usuário (manter 197 ou corrigir para 189 e re-salvar o endereço, o que re-geocodifica).
- [DIVERGÊNCIA] Bairro | Bloco "CASA 02 (Otávio)" = **Barramar** | Stays (gravado) e Google Maps = **Porto de Vacas** | Aparece no endereço público e na busca por bairro | Decisão: usuário.
- [DIVERGÊNCIA] Pets | Regras do Stays (N4, alteradas durante a sessão) = "Mediante Solicitação + Grátis" | Bloco "CASA 02" = "sob demanda" (compatível) · Laudo = "definir se a casa aceita pet" | Agora há fonte para "Mediante Solicitação"; "Grátis" continua sem fonte | Decisão: usuário confirma Grátis/Possibilidade de cobrança.
- [DIVERGÊNCIA] Wi-Fi | Pendência 9 dizia "existência não confirmada" | Bloco "CASA 02" confirma rede "Avelino" | Permite marcar a amenity "Wi-fi" (anúncio) e preencher "Rede wi-fi" (campo personalizado); velocidade continua pendente; senha é HUMANO | Decisão: usuário autoriza marcar/preencher.
- [DIVERGÊNCIA] Textos 1–7 | F2 colado (N1) | F1 PDF (N2) | O usuário escolheu primeiro o PDF e depois **corrigiu para a versão colada** | Gravada a versão colada; vale a mais recente.
- [DIVERGÊNCIA] Código | Laudo: "casa nova, ainda sem código" | Stays: AL37/PP01J | Usuário confirmou que é a mesma casa.
- [DIVERGÊNCIA] Freezer | Área externa: horizontal (laudo diz que "o registro anterior dizia vertical") | Cozinha: freezer vertical Brastemp | Provavelmente são 2 freezers diferentes; não afeta a amenity "Freezer".

---

## D. PENDÊNCIAS E AÇÕES HUMANAS

| # | Campo | Informação encontrada | Motivo | O que falta / quem confirma |
|---|---|---|---|---|
| 1 | ~~Endereço~~ → **completo** (nº 197 confirmado). Marcador a ~18 m do Plus Code; se quiser exatidão, ajustar o pino à mão **sem** salvar o cartão Endereço depois (APR-025) | — | — | Usuário (opcional) |
| 23 | Atrações sem categoria no Stays | Centro Histórico de Marechal Deodoro (17.8 km), Massagueira (14.1 km), Rio Niquim (sem ponto no Google) e Lagoa Manguaba (qual ponto: extremo norte 46.8 km ou margem em Marechal ~18 km?) | catálogo do Stays não tem "centro histórico"/"bairro"; rio e lagoa sem ponto confiável | Usuário decide categoria/ponto; então gravo |
| 24 | Destaque no site | ★ Praia da Barra de São Miguel | escolha do agente (campo obrigatório) | Usuário confirma ou troca |
| 25 | Bloco operacional → campos personalizados | acesso (chave manual + controle remoto), equipe interna (caseiro Júnior), estacionamento, sem gerador, sem staff inclusa | textos dos campos "Acesso acomodação", "Equipe interna", "Estacionamento", "Guia do Hóspede" não foram aprovados; "Senha wi-fi" é HUMANO | Usuário aprova o texto de cada campo (ou manda não preencher) |
| 26 | Restaurantes/fornecedores → "Arredores e atrações"? | Engenho da Barra, Vila Niquim, Ceceu (Restaurante); Unicompras, mercearia (Supermercados/mercearias); farmácias (sem categoria) | catálogo tem Restaurante e Supermercados/mercearias; farmácia não tem; distâncias teriam de ser medidas | Usuário decide se entram (e quais) ou se ficam só no Guia do Hóspede |
| 27 | Foto de capa | foto enviada = `c9585d86` (já na Sala de estar, 4ª posição) | regra de capa do Stays não mapeada (P9: 1ª foto do 1º cômodo? ordem dos cômodos?); hoje o 1º cômodo é Área externa | Mapear com a aba livre (leitura); depois reordenar com confirmação |
| 28 | Aba do Stays | em "Config. Preços das Diárias" (`/sellprice/timeline`) por ação do usuário | fora do escopo do agente; nada foi tocado | Usuário volta a aba para a área Conteúdo quando quiser continuar |
| 2 | **Fotos que ainda faltam** (auditoria completa contra os 12 ambientes do laudo) | 73 fotos enviadas | sem foto: **banheiro social**, **banheiro da Suíte 4**, **corredor**, **lavanderia**, **garagem**; fora do laudo: fachada/entrada, fotos do endereço (arredores/praia), vídeo. Itens do laudo não vistos em foto: freezer da varanda, TV 32″ da varanda, caixas de som, frigobar da master. Nenhuma foto com pessoas ✔ | Usuário |
| 22 | Fotos DET05, DET06, DET07 (lounge e rede) | estão em "Detalhes de Luxo" | são fotos de ambiente (regra do foco) | Usuário decide se vão para "Área externa" |
| 3 | ~~Suítes/camas~~ → **feito** (4 suítes, 1 Queen cada) | — | — | — |
| 18 | ~~Foto `89dfa04f`~~ → **feito** (está na sala de jantar) | — | — | — |
| 20 | **Banheiro social como cômodo** | o laudo diz 5 banheiros; o Stays conta 4 (1 por Suíte) | o Stays tem os tipos "Banheiro" e "1/2 Banheiro". A regra do usuário manda todo cômodo não suíte para Outras Dependências | Usuário: criar cômodos do tipo Banheiro (contagem de banheiros nos canais) ou manter a regra? |
| 21 | Fechadura das suítes | — | "O quarto possui fechadura?" ficou no padrão Não (não exibir) | Usuário/proprietário |
| 19 | Capa e ordem das fotos | — | ordem e capa não conferidas | Regra Trip Homes de capa e ordem |
| 4 | **Capacidade autorizada** | o laudo diz "pendente — 8 hóspedes nas camas?"; o Stays agora calcula **8 adultos** pelas camas | palavra de alerta: capacidade autorizada não confirmada | Proprietário. Se a capacidade autorizada for diferente, é preciso mudar as camas (o campo Adultos é bloqueado) |
| 5 | Título × Nome interno | título = "Casa Entremarés…"; nome interno = "AL37 - Casa Maré Mansa…" (mudado pelo usuário) | divergem | Usuário: (A) alinhar nome interno ao título ou (B) trocar o título para "Maré Mansa" (nome já usado por pousada/praia em AL) |
| 6 | ~~Textos EN/ES~~ → **feito** (7 campos × 2 idiomas, verificados) | — | — | Usuário pode revisar a variante do espanhol |
| 7 | Voltagem | transformador com etiqueta 220V | "pendente — 110V ou 220V" | Proprietário |
| 8 | Pets | caminha de pet na cozinha | "definir se a casa aceita pet" | Proprietário (Regras; hoje o Stays mostra "Não" sem fonte) |
| 9 | Wi-Fi | roteador visto na sala | existência e velocidade não confirmadas | Proprietário (amenity Wi-fi e campos personalizados de rede) |
| 10 | Aquecimento da piscina | — | não informado | Proprietário |
| 11 | ~~Garagem gratuita~~ → **resolvido no anúncio** (Estacionamento Gratuito + Garagem Gratuita = Sim, 2). Falta só o Estacionamento das **amenities do endereço** (Booking): se é preciso reservar e se o endereço deve ser preenchido | — | regra P1 (endereço vazio por ora) | Trip Homes |
| 12 | Área (m²) | — | não informada | Proprietário |
| 13 | Segurança (extintor, detector de fumaça, kit de primeiros socorros) | — | não citados no laudo | Vistoria |
| 14 | Distâncias e aeroporto | ~~conferir~~ → medidas no Google Maps: aeroporto **56.0 km / 1 h 04** (laudo ≈45 km/50 min), centro de Maceió **29.0 km / 35 min** (laudo ≈35 km), Praia do Francês **10.2 km** (laudo ≈20 km) | o texto 7 (locomoção) não cita distâncias; se quiser incluí-las, é texto novo a aprovar | Usuário |
| 15 | Regras atuais do Stays | Nível 4, **mudaram durante a sessão sem ação do agente** (ver D2-8) | sem fonte | Trip Homes/proprietário |
| 16 | Amenities de confiança MÉDIA (7) | ver B · I3 | rótulo ambíguo | Trip Homes |
| 17 | Bebidas sobre os frigobares | — | não se sabe se são do proprietário ou do hóspede | Proprietário |

### D2. ALERTAS DE INCOMPATIBILIDADE — decisão do usuário (APR-020)
O agente **não alterou nada** por causa destes alertas.

| # | Onde | Laudo PDF (fonte confiável) | O que foi encontrado | Por que importa |
|---|---|---|---|---|
| 1 | Suíte 4 (master): TV | "TV 32″" | Nas 6 fotos a TV da master é **visivelmente maior** que as TVs das Suítes 1–3 (que batem com 32″). A foto não serve para medir o tamanho (Nível 3) | Inventário e caução; o laudo pode estar desatualizado |
| 2 | Suíte 4 (master): sofá | "Sofá 4 lugares" | O sofá das fotos parece de 2 módulos (3 lugares?). Confiança BAIXA | Inventário |
| 3 | Suíte 4 (master): frigobar | "Frigobar 1" | O frigobar **não aparece** em nenhuma das 6 fotos (não quer dizer que não exista) | Os textos dizem que todas as suítes têm frigobar |
| 4 | Suíte 4 (master): objetos pessoais | closet "TRANCADO (área do proprietário)" | Nas fotos aparecem perfumes no rack da TV, chapéus no cabideiro e uma caixa de som portátil | Pode ser objeto do proprietário à vista no anúncio |
| 5 | Suíte 1: enxoval | "Colcha matelassê branca, 2 porta-travesseiros…" (sem manta) | Nas fotos há uma **manta verde** sobre a cama | Inventário de enxoval |
| 6 | Texto "Sobre o espaço" (versão colada) | Cozinha: **1 geladeira** Brastemp duplex + **1 freezer vertical** | O texto diz "**duas geladeiras/refrigeradores**" | O texto do anúncio pode não bater com o laudo |
| 7 | Capacidade | "pendente — 8 hóspedes nas camas?" | O Stays passou a mostrar **Adultos = 8** (calculado pelas camas) | Capacidade autorizada não confirmada |
| 8 | Regras da acomodação (mudaram sem ação do agente) | Pets: "Definir se a casa aceita pet" | Antes: idade vazia, crianças 1, berços Não, pets Não. **Agora**: idade 18, **crianças 10**, berços Sim (1), **pets "Mediante Solicitação" + Grátis** | Crianças 10 é maior que a capacidade de 8. A regra de pets ainda não foi definida pelo laudo. Foi o usuário quem mudou? |
| 9 | Banheiros | 5 banheiros (4 privativos + social) | **[REVISÃO]** O resumo do cartão do anúncio mostra **4 banheiros**: o Stays conta 1 banheiro por Suíte. Falta só o **banheiro social** (não há cômodo para ele) | A contagem nos canais deve sair 4 em vez de 5 (ver D-20) |

| 10 | Cartão Título (Conteúdo descritivo) | — | Enquanto o agente trabalhava, o **Nome interno** foi alterado na aba para "Casa Maré Mansa" e o título PT ficou "Casa em Barra de São Miguel" sem salvar; o Salvar do agente gravou os dois. "Maré Mansa" aparece no Google (pousada na região de Maceió, praia em Barra de Santo Antônio) | Decidir A/B (D-5) |

| 11 | Tabela de atrações do usuário × Google Maps | — | Divergências grandes: Lagoa do Roteiro (tabela 3–5 km; Google 9.9–12.6 km), Praia do Gunga (6–8 km; 13.3–16 km por estrada), Falésias da Barra (4–6; 8.3–11), Falésias do Gunga (7–9; 16.3–19), Massagueira (24–27; 14.1), Lagoa Manguaba (21–24; depende do ponto). Praia Niquim, Francês e Marechal conferem | Gravados os valores do Google, por decisão do usuário |
| 12 | Número da casa | — | Usuário confirmou **197**; o Google interpola **189** para o mesmo ponto | 197 gravado (fonte: usuário) |

| 13 | Endereço: número | — (bloco interno "CASA 02": 189) | Stays: **197** (confirmação do usuário) · Google: 189 | Duas fontes internas divergem; o pino depende do número |
| 14 | Endereço: bairro | — (bloco interno "CASA 02": Barramar) | Stays e Google: **Porto de Vacas** | Endereço público e busca por bairro |
| 15 | Foto de capa | — | Arquivo enviado (1280×853) é cópia menor de `c9585d86` (2048×1365) já carregada | Regra "qualidade original": usar a já carregada; não reenviar a menor |

**Ações humanas:** login (feito). **Senha do Wi-Fi recebida no chat: não foi registrada em nenhum arquivo nem digitada; preencher "Senha wi-fi" é ação humana.** **Ação humana feita:** o usuário arrastou o pino para -9.831577,-35.884534; depois o salvamento do nº 197 re-geocodificou para -9.8315818,-35.8843864 (~18 m). Se quiser o ponto exato, repetir o arrasto **depois** de qualquer salvamento do cartão Endereço. Ativar o anúncio fica para o fim, com confirmação.

---

## E. CONTRATO DE DADOS DE ENTRADA (área Conteúdo)

| Campo no Stays | Obrigatório? | Tipo / valores permitidos | Fonte necessária | Executor |
|---|---|---|---|---|
| Tipo de propriedade (endereço) | sim (já vem preenchido) | 1 de 36 opções | laudo (tipo do imóvel) | LLM → SCRIPT |
| Tipo de anúncio | sim (já vem preenchido) | 1 de 17 opções | laudo | LLM → SCRIPT |
| Subtipo | sim | Imóvel inteiro / Quarto privativo / Quarto compartilhado | laudo (acesso) | LLM |
| Categoria | sim | Aluguel por temporada / Compra e venda / Locação residencial | Trip Homes | SCRIPT |
| Número de registro | não | texto | documento oficial | SCRIPT |
| País | **sim** | "Nome (ISO)" | endereço oficial | SCRIPT |
| Estado / Sigla / CEP / Cidade / Bairro / Rua / Número / Complemento | ? | texto (Sigla ≤ 3) | endereço oficial | SCRIPT |
| Mapa (marcador) | — | coordenadas geocodificadas | endereço; ajuste manual se falhar | VISÃO/HUMANO |
| Fotos do endereço | não | imagens (campo de arquivo) | fotos de arredores e áreas comuns | VISÃO → SCRIPT |
| Arredores e atrações | não | itens | laudo (bairro) | LLM |
| Amenities do endereço | não | 99 caixas de seleção + 5 cartões Sim/Não com sub-opções | laudo + confirmação textual das sub-opções | LLM (mapeamento) → SCRIPT |
| Amenities do anúncio | não (5–10 mínimo recomendado) | 315 caixas de seleção (ver apêndice) + Área (número em m²) + Garagem Gratuita | inventário do laudo (existência); foto basta só para itens visualmente inequívocos | LLM (mapeamento) → SCRIPT |
| Cômodos / camas | sim (gera a capacidade) | cômodos + camas | laudo (camas e tamanhos); foto não define tamanho de cama | LLM → HUMANO valida |
| Cômodo não suíte: tipo | sim | "Outras Dependências" (fixo) | regra Trip Homes | SCRIPT |
| Cômodo não suíte: nome | sim | texto exato da lista `room-name` (instável e com repetições) | nome do ambiente dado pela fonte ou pelo usuário → texto da lista mais parecido | LLM (escolhe o nome) → SCRIPT |
| Cômodo: compartilhado | sim | Sim / Não | regra Trip Homes: áreas usadas pelo grupo = Sim | SCRIPT |
| Fotos do cômodo | não | jpeg/png/webp/avif, várias por envio, arquivos originais | fotos aprovadas pela mídia, separadas por cômodo; a classificação detalhe vs. ambiente é feita por VISÃO | VISÃO → SCRIPT |
| Tag das Imagens | não | 1+ das 149 tags | tag mais parecida com o cômodo (APR-016) | LLM → SCRIPT |
| Garagem Gratuita | não | Sim/Não + quantidade | gratuidade (Trip Homes) + vagas (laudo) | SCRIPT |
| Suíte: tipo de cama + quantidade | sim (gera a capacidade) | 1 das 11 opções `bed-type` × quantidade | **laudo PDF** (tamanho da cama; foto não define tamanho) | SCRIPT |
| Suíte: fechadura | não | Sim/Não | proprietário | SCRIPT |
| Classificação quarto × banheiro × detalhe por foto | — | — | foto | VISÃO |
| Vídeo | não | ID do YouTube | link aprovado | SCRIPT |
| Nome interno | sim (já vem preenchido) | texto ≤ 64 | código Trip Homes + nome | SCRIPT |
| Título do anúncio PT/EN/ES | sim (⚠) | texto sem pontuação nem símbolos | texto aprovado | SCRIPT (nunca gerar) |
| 7 campos de Descrição PT | ⚠ quando vazio | rich text; resumida ≤ 500 | seções 1–7 do laudo (sem frases com palavra de alerta) | SCRIPT |
| 7 campos de Descrição EN/ES | ⚠ quando vazio | rich text | tradução aprovada | SCRIPT |
| Campos personalizados (exceto senhas) | não | texto | operação Trip Homes | SCRIPT |
| Senha wi-fi / Senha porta | não | texto | — | **HUMANO** |
| Regras (idade, crianças, bebês, berços, fumar, pets, eventos, silêncio, adicionais) | parcialmente | Sim/Não/quantidades/texto | proprietário / Trip Homes | SCRIPT após confirmação |

---

## F. FLUXO ÓTIMO PARA O PRÓXIMO IMÓVEL (área Conteúdo)

1. **Intake (sem cliques):** separar fontes por nível; marcar frases com palavras de alerta ("conferir", "pendente", "?", "≈"); conferir que o código Stays corresponde à casa (perguntar se o laudo diz "sem código").
2. **Abrir** `/i/apartment/{ID}/setup` e esperar ficar pronto (19 editores; se aparecer o erro de módulos, recarregar).
3. **Ler os valores atuais** antes de escrever (tamanho de cada editor); se algum não estiver vazio, registrar o valor anterior e pedir confirmação.
4. **Textos:** para cada um dos 7 campos, pôr o foco, **conferir o foco**, digitar por parágrafos e conferir o contador. Um único Salvar no cartão Descrição.
5. **Verificar:** recarregar e comparar com a fonte, caractere a caractere.
6. **Amenities:** abrir `/amenities`, montar a lista de rótulos exatos a partir do inventário, confirmar que cada rótulo existe uma única vez, marcar, Salvar em "Amenities básicas", recarregar e comparar.
7. **Cômodos não suíte** (para cada lote de fotos):
   1. Classificar cada foto como ambiente ou detalhe (VISÃO).
   2. Clicar em `#btn-add-room`.
   3. Escolher o tipo "Outras Dependências".
   4. Escolher compartilhado.
   5. Escolher o nome pelo texto exato.
   6. Clicar em Salvar no cartão do cômodo.
   7. Abrir o cômodo, mostrar o campo de arquivo e enviar os originais em lotes de até 10 MB.
   8. Esperar a contagem bater.
   9. Marcar a tag de cada foto.
   10. Clicar em Salvar no cartão Fotos.
   11. Recarregar e conferir nome, compartilhado, quantidade de fotos e tags.
8. **Suítes** (para cada suíte do laudo):
   1. Clicar em `#btn-add-room` e escolher o tipo **Suíte**.
   2. Compartilhado = Não (P10).
   3. Escolher o tipo de cama exato do laudo.
   4. Preencher a quantidade e disparar o evento nela (é isso que libera o Salvar).
   5. Clicar em Salvar.
   6. Enviar as fotos em lotes de até ~5 MB.
   7. Montar a faixa de conferência (APR-019) e classificar quarto ou banheiro.
   8. Marcar as tags e salvar.
9. **Verificar tudo pelo `room.getRoom`** (APR-018), em lotes de 5 cartões. Conferir **Adultos** em Regras (é calculado pelas camas) e comparar com a capacidade autorizada.
10. **Comparar fotos × laudo PDF** e listar as incompatibilidades para o usuário decidir (APR-020).
11. **Localização** (quando houver endereço), depois **Regras** (quando houver confirmação do proprietário).
- **Atalho validado:** com a janela pequena ou mudando de tamanho, operar os seletores pela API da página (`jQuery(select).selectpicker('val', …)` + `change`) e os botões com `.click()`, sem coordenadas. Sempre confirmar o resultado lendo o DOM e recarregando.

### Otimizações e erros observados
- [OTIMIZAÇÃO] Conhecimento empacotado: agente `stays-listing-automation` + skill com referências 01–10 + `helpers.js` (plugin `trip-homes-stays`), mais PROMPT e MODELO de pedido. Próximo imóvel começa do fluxo F sem redescobrir as regras APR. Observação: `helpers.js` foi reconstruído a partir dos procedimentos validados (o código original ficou fora do histórico) e deve ser testado em um objeto real antes de uso em lote.
- [FOTOS] Capa | 4 arquivos idênticos (md5 61ccd284…) 1280×853 | = `c9585d86` (Sala de estar, 2048×1365) | nada enviado ao Stays | classificação: ambiente (sala de estar), candidata a capa.
- [OTIMIZAÇÃO] Antigo: abrir cada categoria de amenities e clicar item por item (cerca de 13 categorias + 34 cliques). Melhor: localizar pelo rótulo e marcar por script. Economia: cerca de 45 cliques.
- [OTIMIZAÇÃO] Ler o catálogo de rótulos da própria página na hora da execução, em vez de fixar a lista abaixo (ela pode mudar).
- [ERRO OBSERVADO] Clique por coordenada ou referência em editor mais abaixo na página caiu no canto da tela; o menu lateral abriu e o texto digitado se perdeu (nada foi gravado). Causa provável: a posição da página pula sozinha. Regra aprendida: **sempre conferir o elemento em foco antes de digitar**. Correção segura: SIM (nada foi salvo).
- [ERRO OBSERVADO] Um segundo clique foi parar no editor da Descrição resumida, já preenchido. O foco foi detectado e nada foi digitado. Mesma regra.
- [ERRO OBSERVADO] A janela do navegador mudou de tamanho no meio da sessão (1471 → 979 → 1160 → 600 px), o que invalida coordenadas antigas. Regra: localizar elementos por rótulo; nunca reutilizar coordenadas de capturas de tela antigas.
- [ERRO OBSERVADO] Digitei na busca de nomes de cômodo sem conferir onde estava o cursor; as teclas foram para a página. Regra (reforçada): **conferir o cursor sempre antes de digitar**. Correção segura: SIM.
- [ERRO OBSERVADO] Um clique planejado para "Não" (compartilhado) caiu na área de envio de fotos porque a página tinha se mexido, e pode ter aberto uma janela de escolher arquivos no computador do usuário. Nada foi enviado. Regra: não clicar por coordenada em telas que mudam de altura (lista de fotos); usar o DOM.
- [ERRO OBSERVADO] O nome do cômodo escolhido na lista não foi gravado (ao recarregar, aparecia "Não selecionado"). Causa: faltou o Salvar do cartão do cômodo. Corrigido e conferido.
- [ERRO OBSERVADO] O usuário e o agente operaram a mesma aba ao mesmo tempo. Regra: se a página mudar sem ação do agente, **parar** e pedir ao usuário que libere a aba.
- [OTIMIZAÇÃO] Tags por script: 12 fotos marcadas de uma vez e um único Salvar, em vez de cerca de 36 cliques (abrir, buscar e escolher, em cada foto).
- [ERRO OBSERVADO] Na 1ª tentativa de criar a Suíte 1, o Salvar estava oculto e o clique não fez nada. Causa: o Salvar só aparece com um evento no campo de quantidade de camas. Correção: disparar `input/keyup/change` em `bed-count` (APR-017). Nada foi duplicado.
- [ERRO OBSERVADO] A verificação com cliques em sequência nos 10 cartões passou do tempo limite da ferramenta. Regra: verificar em lotes de 5 cartões.
- [ERRO OBSERVADO] A captura das respostas `room.getRoom` cortada em 3.000 caracteres quebrou a leitura do JSON. Regra: capturar a resposta inteira (via `jQuery(document).ajaxComplete`).
- [ERRO OBSERVADO] Na 1ª tentativa, o script pegou a 1ª aba "en" da página, que era a do **Título**, e não a da Descrição. Nada foi gravado. Regra: localizar a aba dentro do bloco do campo (APR-021).
- [ERRO OBSERVADO] O registro antigo dizia que o título PT estava vazio. Estava errado: o resumo de campos tratava "valor vazio + espaço" como preenchido e não conferia o valor real. O título PT já existia. Regra: ler o valor real (`.value`), nunca um indicador resumido.
- [ERRO OBSERVADO] Arrastar o marcador do mapa: o 1º arrasto move o pino; o 2º arrasto não move (ou abre um ponto de interesse do Google e move o mapa); e nada persiste após recarregar, mesmo salvando o cartão. Regra: marcador = ação humana (APR-023). Correção segura: SIM (nada foi gravado).
- [ERRO OBSERVADO] Salvar um cartão com edição alheia não salva (Nome interno) gravou a edição junto. Regra: ler todos os campos do cartão antes de salvar e registrar diferenças (APR-022).
- [ERRO OBSERVADO] O Salvar de "Arredores e atrações" não gravou na 1ª tentativa: faltava marcar a bolinha de destaque (obrigatória). Regra: marcar uma linha antes de salvar (APR-024).
- [ERRO OBSERVADO] Chamadas JS pesadas (`querySelectorAll('*')` + `innerText` em cartões grandes) falharam repetidamente na extensão. Regra: consultas pequenas e seletores específicos (`#add-neighbourhoods`, `select[name=name]`).
- [OTIMIZAÇÃO] Gravar EN/ES pela API do editor (`summernote('code')`) em vez de digitar: cerca de 9.500 caracteres em 6 chamadas, sem erro de digitação, conferidos depois de recarregar.
- [OTIMIZAÇÃO] Uma função única para criar cômodos (`btn-add-room` → tipo → compartilhado → nome ou cama → Salvar → mostrar o campo de arquivo) e outra para as tags: 7 cômodos e 48 fotos criados em cerca de 25 chamadas.

---

## APÊNDICE 1 — Catálogo "Amenities do anúncio" (315 itens, filtro Todos, lido em 08/10/2026)

- **Acessibilidade (12):** Acesso Livre de Degraus em Casa | Acesso sem degraus (áreas do hóspede) | Banheira Adaptada | Caminho até a entrada iluminado à noite | Caminho plano e regular até a entrada | Comodidades para hóspedes com mobilidade reduzida | Ducha Adaptada | Ducha sem Degraus | Porta Ampla em Casa | Porta larga | Unidade Inteira Acessível de Cadeira de Rodas | Vaga de Estacionamento para Deficientes
- **Ao ar livre / Vista (47):** À beira d'água | Acesso ao Lago | Acesso ao Resort | Acesso à Praia | Área de Refeições Exterior | Chuveiro Externo | Cozinha externa | Em frente ao rio | Espreguiçadeiras | Esqui In e Out | Guindaste para Piscina | Jardim ou Quintal | Lareira externa | Mobília de Exterior | Piscina Aquecida Privada | Piscina com Água Salgada | Piscina com Borda Infinita | Piscina com Lado Raso | Piscina com Vista | Piscina de Imersão | Piscina na Cobertura | Piscina não-aquecida Privada | Piscina Privada | Piscina Privada Climatizada | Varanda | Vista para a baía | Vista para a Cidade | Vista para a marina | Vista para a Montanha | Vista para a Piscina | Vista para a Praia | Vista para o campo de golfe | Vista para o canal | Vista para o deserto | Vista para o Jardim | Vista para o Lago | Vista para o Mar | Vista para o Marco | Vista para o Oceano | Vista para o parque | Vista para o porto | Vista para o resort | Vista para o rio | Vista para o vale | Vista para o vinhedo | Vista para Pátio Interno | Vista para Rua Tranquila
- **Banheiro (29):** Água Quente | Banheira | Banheira de Hidromassagem | Banheira de hidromassagem separada | Banheira grande | Banheira ou chuveiro separados | Banheiro Compartilhado | Banheiro em mármore | Banheiro sem chuveiro | Bidê | Cadeira de Ducha | Casa de Banho Adicional | Chuveiro Acessível | Condicionador | Ducha | Escova de Dentes | Gel de banho | Itens Básicos de Banheiro | Jacuzzi | Lavatório mais Baixo | Papel Higiênico | Sabonete de corpo | Secador de Cabelo | Toalhas | Toalhas para Piscina | Touca de Banho | Vaso sanitário | WC para Convidados | Xampu
- **Climatização (3):** Sauna | Ventilador de teto | Ventilador Portátil
- **Cozinha e sala de jantar (40):** Adega | Assentos na Sala de Jantar | Bancada americana | Café | Cafeteira | Chaleira | Chaleira Elétrica | Chocolate/Bolachas | Churrasqueira | Compactador de Lixo | Copos de Vinho | Cozinha americana | Cozinha Compartilhada | Cozinha Completa | Cozinha/Cozinha Compacta | Fogão | Folha de Assar | Forno | Freezer | Frutas | Garrafa de Água | Geladeira | Grill para Churrasco | Itens Básicos de Cozinha | Kitchenette | Lava-louças | Liquidificador | Louças e Talheres | Máquina de gelo | Máquina de Pão | Mesa | Mesa de Jantar | Mesas e cadeiras | Microondas | Mini-frigorífico | Panela de Arroz | Pia | Torradeira | Utensílios para Churrasco | Vinho/Champanhe
- **Entretenimento (64):** Adaptador | Barco | Bicicleta | Bicicleta Infantil | Caiaque | Canais a Cabo | Canais Pay-per-view | Console de Jogos - PS4 | Console de Jogos - Wii U | Console de Jogos - Xbox One | Equipamento de Exercício | Esqui aquático | Filmes | Fliperama | Gaiola de rebatidas (batting cage) | Home Theater | Jet ski | Jogos de Tabuleiro/Quebra-Cabeças | Jogos gigantes | Laser Tag | Lista de canais de filmes disponíveis | Livros | Luz de Leitura | Mesa de air hockey | Mesa de jogos de cartas | Mesa de pebolim (totó) | Mesa de Ping Pong | Mesa de Sinuca | Mini Golfe | Parede de Escalada | Piano | Pista de Boliche | Pista de Hóquei | Pista de Skate | Playground | Prancha de stand up paddle (SUP) | Prancha de surfe | Pranchas de windsurf | Projetor e tela | Quarto Temático | Rádio | Rádio via satélite | Rede de Descanso | Reprodutor de Bluray | Reprodutor de CD | Reprodutor de DVD | Sala de Jogos | Sala de mídia | Seabob (scooter aquático) | Serviço de filmes/vídeos gratuito | Serviço de Streaming (como Netflix) | Shuffleboard | Sistema de Som | Smartphone | Smart TV | Tampões para os Ouvidos | Televisão de alta definição (HD) - 32 polegadas ou mais | Televisão via satélite | Toca-discos | TV | TV de tela larga | Vídeo-game | Videogame | Vídeo sob demanda
- **Estacionamento e instalações (22):** Academia (privativa) | Acessível por Elevador | Acessível por Escadas Apenas | Acomodação Térrea | Apartamento Privado num Edifício de Apartamentos | Aquecimento Central | Ar Condicionado | Ar condicionado controlado individualmente no quarto | Carrinho de golfe | Elevador | Entrada Privativa | Estacionamento Gratuito | Estacionamento na Rua | Estacionamento Pago | Estacionamento Pago no Local | Fogueira | Interfone | Lareira Interna | Local para Barco | Piso com Carpete | Sistema de aquecimento/ar condicionado controlado pelo hóspede | Terraço
- **Família (18):** Ar Condicionado Individual para o Quarto de Hóspedes | Área de estar | Área de estar com sofá/cadeira | Artigos de Praia | Banheira para Bebê | Berço | Berço Portátil | Biblioteca de Livros/DVDs/Música para Crianças | Cadeira Alta para Crianças | Chinelos | Monitor de Bebê | Não permite pets | Prensa para Calças | Purificadores de Ar Disponíveis | Quartos para famílias | Recomendações de Babá | Trocador | Utensílios de Jantar Infantil
- **Internet e escritório (14):** Cadeira fornecida com a mesa | Computador | Espaço pronto para uso de notebook | Internet | Internet (via cabo) | Laptop | Mesa com tomada elétrica | Quarto à prova de som | Telefone com duas linhas | Telefone TDD/Textfone | Tomada elétrica próxima à cama | Tomada Perto da Cama | Wi-fi | Wi-fi Portátil
- **Limpeza e desinfecção (5):** Limpeza Antes do Check-out | Livre de Alergénicos | Lixeiras | Produtos de Limpeza | Tábua de passar
- **Quarto e Lavanderia (27):** Arara para Roupas | Armários no quarto | Blackout nas Cortinas | Cabides | Cama Dobrável | Camas Extra-longas (> 2 metros) | Cobertores Elétricos | Cobertores e travesseiros extras | Ferro de Passar | Guarda-roupa ou Armário | Lavanderia Próxima | Máquina de lavar e secar | Máquina de Lavar Roupa | Múltiplos armários | Pijama | Quarto de Vestir | Roupão | Roupas de cama | Secadora | Sofá | Sofá-Cama | Tipo de roupa de cama de luxo | Toalhas/Lençóis com Sobretaxa | Travesseiro de penas | Travesseiro hipoalergênico | Travesseiro sem penas | Varal para Secar Roupas
- **Segurança doméstica (23):** Barras de Apoio no Chuveiro | Capa para Piscina | Cartões Eletrónicos | Casa de Banho com Barras de Apoio | Chaves Eletrónicas | Cofre | Cofres | Cordão de Emergência na Casa de Banho | Cortina Privativa | Desinfetante para as Mãos | Detector de Fumaça | Detector de Monóxido de Carbono | Escadas com Portão (segurança) | Extintor de incêndio | Grades de Janela | Grades de Lareira | Grades de Segurança para Bebés | Kit de Primeiros Socorros | Mosquiteiro | Proteção nos Cantos de Mesa | Protetores de Tomada | Quarto antialérgico | Sistema de segurança
- **Serviços (11):** Café da Manhã Incluído | Carregador de VE | Cofre grande o suficiente para acomodar um laptop | Depósito de Bagagem Permitido | Estação de ancoragem para iPod | Estadias Prolongadas Permitidas | Fax | Guindaste de Teto | Porteiro | Serviço de Despertar/Relógio Despertador | Telefones celulares

## APÊNDICE 2 — Catálogo "Amenities do endereço" (99 itens)

- **Ao ar livre / Vista (22):** Beira-mar | Campo de golfe | Churrasqueira / Área para piquenique | Instalações para esportes aquáticos (na propriedade) | Jardim | Lojas (na propriedade) | Moveis externos | Não possui Jardim | Não possui Playground | Não possui Terraço | Parquinho infantil | Piscina ao ar livre (Compartilhada) | Piscina aquecida (Compartilhada) | Piscina interna (Compartilhada) | Praia privada | Quadra de tênis | Quintal | Salão / área de TV | Solário | Terraço | Vista para a montanha | Vista para a praia
- **Banheiro (3):** Banho turco / Sauna a vapor | Itens básicos de praia | Jacuzzi (uso comum)
- **Climatização (2):** Ar condicionado | Sauna
- **Cozinha e sala de jantar (5):** Cozinha Compartilhada | Máquina de venda automática (lanches) | Não oferece café da manhã | Não possui restaurante | Restaurante
- **Entretenimento (22):** Academia (área comum) | Aluguel de bicicletas | Bar | Beach club | Biblioteca | Casa noturna / DJ | Cassino | Clube de tênis | Clube infantil | Entretenimento à noite | Equitação | Não possui academia | Pesca | Quadra de badminton | Quadra de basquete | Quadra de bocha | Quadra de petanca | Quadra de raquetebol | Quadra de squash | Quadra de vôlei | Salão de jogos | Trilhas a pé
- **Estacionamento e instalações (11):** Acesso a spa | Capela / Santuário | Check-in presencial com anfitrião | Elevador | Estacionamento com acessibilidade | Estacionamento de rua (gratuito) | Estacionamento de rua (pago) | Estacionamento seguro (pago) | Garagem | Pátio ou Varanda | Serviço de manobrista
- **Internet e escritório (2):** Carregador elétrico veicular | Não possui sala de conferência
- **Quarto e Lavanderia (5):** Lavagem a seco | Lavanderia | Máquina de Lavar (uso comum, paga ou grátis) | Quartos para não fumantes | Secadora (uso comum, pago ou grátis)
- **Sem categoria (2):** Aluguel de equipamento de esqui (no local) | Venda de passe de esqui
- **Serviços (25):** Aluguel de carros | Armários individuais | Babá / Serviços para crianças | Balcão de turismo | Business center | Café da manhã incluso | Caixa eletrônico na propriedade | Cofre | Engraxate | Entrega de compras | Fatura mediante pedido | Loja de presentes / Souvenirs | Não oferece babá | Não oferece Concierge | Não oferece massagem | Não possui Spa | Outra forma de check-in | Permite estadias acima de 28 noites | Permitido deixar malas | Serviço de câmbio | Serviço de concierge | Serviço de quarto | Serviço diário de limpeza | Serviços de massagem | Transfer gratuito (aeroporto)
- **Cartões Sim/Não:** Check-in/checkout expressos · Estacionamento (local: No local / Em local próximo · reserva: Indisponível / Necessário reservar / Não é necessário reservar · área: Público / Particular · custo: Grátis / Custo Adicional) · Internet a Cabo e Internet Wi-Fi (abrangência: Em todo o estabelecimento / Apenas em áreas públicas / Todas as acomodações / Em algumas acomodações / Business center · custo: Grátis / Custo Adicional) · Recepção 24 horas
