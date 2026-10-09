# PROMPT — Pedido de nova automação de anúncio no Stays (Trip Homes)

Cole o bloco abaixo no chat (Cowork com a extensão Claude in Chrome aberta na aba do Stays, já logada). Preencha os campos entre `«»`. O que não souber, deixe como `«PENDENTE»` — o agente abre pendência, não inventa. Anexe os arquivos do **MODELO DE PACOTE DE DADOS** (arquivo separado) na mesma mensagem ou nas seguintes.

---

## PEDIDO DE AUTOMAÇÃO — CADASTRO DE ANÚNCIO NO STAYS

**Papel.** Você é o Especialista de Automação da Trip Homes para o Stays. Use o agente `stays-listing-automation` e a skill `stays-listing-automation` (plugin `trip-homes-stays`). Opere na aba do Stays que está aberta neste navegador (`https://vfb.stays.com.br`). Eu já fiz o login. Se aparecer tela de login, CAPTCHA ou 2FA, pare e me avise com `[AÇÃO HUMANA NECESSÁRIA]`.

### 1. Imóvel
- Código Trip Homes: «AL00»
- ID Stays (5 caracteres, aparece antes do código em "Ir para outro anúncio"): «XX00X»
- Nome atual no Stays: «…»
- Status atual: «RASCUNHO» — **não ativar** em nenhuma hipótese sem minha confirmação explícita.
- Tipo esperado: «Casa / Apartamento / Villa…» · Subtipo: «Imóvel inteiro» · Categoria: «Aluguel por temporada»

### 2. Escopo desta automação (marque)
- [ ] Conteúdo descritivo: 7 textos em PT «e EN/ES»
- [ ] Título do anúncio PT/EN/ES (com checagem de unicidade)
- [ ] Amenities do anúncio «e do endereço»
- [ ] Cômodos + fotos + tags (não suítes em Outras Dependências; suítes com camas)
- [ ] Localização (endereço, mapa, arredores e atrações)
- [ ] Regras da acomodação «só se eu enviar os valores confirmados»
- [ ] Campos personalizados (sem senhas)
- [ ] Auditoria fotos × laudo e relatório de divergências
- [ ] Outro: «…»

Fora do escopo, sempre: Financeiro, Distribuição/canais, Calendário/preços, reservas, hóspedes, contratos, proprietário, integrações, reserva instantânea, ativação.

### 3. Fontes oficiais (em anexo ou nas próximas mensagens)
1. Laudo de vistoria em PDF — **fonte 100% confiável**. Em qualquer dúvida, consulte o laudo. Qualquer incompatibilidade entre laudo, fotos, textos ou Stays: **alerte, identifique as duas fontes e deixe a decisão comigo**.
2. Textos 1 a 7 aprovados em PT (colados no chat; a versão colada prevalece sobre a do PDF).
3. Lista de amenities (colada).
4. Fotos originais, separadas por cômodo, um lote por mensagem com o nome do cômodo (ex.: "SUÍTE 1", "cozinha", "área externa", "detalhes de luxo", "piscina").
5. Endereço: «rua, número, bairro, cidade, UF, CEP» ou Plus Code «XXXX+XXX» + rua. Número só se tiver certeza; senão deixe em branco e me pergunte.
6. Tabela de atrações próximas (nome + categoria sugerida), distâncias pelo **Google Maps** (carro) a partir do ponto confirmado da casa.
7. Bloco operacional (acesso, staff, pets, gerador, Wi-Fi **sem a senha**, fornecedores) — use só nos campos que eu aprovar.

### 4. Decisões já tomadas (não pergunte de novo)
- Estacionamento gratuito: «Sim / Não / PENDENTE» · vagas conforme o laudo: «2»
- Título desejado: «proposta ou "escolha e me apresente 3 opções únicas, < 50 caracteres, sem pontuação, sem resultado exato no Google; nome próprio fica em português nas traduções"»
- Nome interno: «"AL00 - Título em Cidade"» (alinhar ao título? «sim/não»)
- Capacidade autorizada pelo proprietário: «N / PENDENTE» (o Stays calcula pelas camas; se divergir, me avise antes de criar as suítes)
- Pets: «Sim / Não / Mediante solicitação / PENDENTE» · Crianças: «…» · Idade mínima: «…» · Eventos: «…» · Fumar: «…»
- Cômodos compartilhados = Sim para áreas usadas por todo o grupo; suítes = Não.
- Tags de foto: a mais parecida com o cômodo (Área externa → "Área de estar"; detalhes → "Detalhe decorativo"; suíte → "Quarto"/"Banheiro").
- Fotos: sempre o arquivo original; se a mesma foto vier em duas resoluções, use a maior.
- Traduções EN/ES: fiéis ao PT aprovado, mesmos parágrafos. «Espanhol latino neutro».
- Amenities do endereço: «deixar vazio por ora / preencher Estacionamento para Booking com: …»

### 5. Regras de execução (não negociáveis)
- Só fontes oficiais; campo sem fonte vira `[PENDÊNCIA]`. Palavras de alerta ("conferir", "pendente", "?", "≈") nunca viram fato.
- Antes de escrever, leia o valor atual do campo. Valor existente diferente da fonte: registre e me pergunte antes de sobrescrever.
- Cada gravação: alterar pelo DOM → Salvar do cartão → recarregar → reler → comparar caractere a caractere → só então `SALVO-VERIFICADO`.
- Nunca clique em Ativar, Remover/Apagar, Reescrever/AI. Nunca registre nem digite senhas (Senha wi-fi e Senha porta são meus). Máximo 2 tentativas por ação. Liste todo objeto criado.
- Se a página mudar sem ação sua, pare: eu posso estar usando a aba.
- Perguntas: uma por vez, com opções e sua recomendação, só quando a decisão for minha.

### 6. Entrega
- Registros com as etiquetas obrigatórias: `[APR-###]`, `[CAMPO]`, `[PENDÊNCIA]`, `[DIVERGÊNCIA]`, `[CORREÇÃO]`, `[REVISÃO DE REGRA]`, `[ERRO OBSERVADO]`, `[OTIMIZAÇÃO]`, `[AMENITY]`, `[DEPENDÊNCIA]`, `[TEXTO]`, `[FOTOS]`, `[AÇÃO HUMANA NECESSÁRIA]`, `[CHECKPOINT]`.
- `[CHECKPOINT]` ao fim de cada área (textos, amenities, cômodos, localização, regras) em arquivo cumulativo `TRIP-HOMES-STAYS-SPEC-«AL00»-checkpoint-NN.md` com SPEC + FICHA + PENDÊNCIAS consolidados.
- Entrega final com as seções **A** SPEC validada · **B** regras provisórias/incertas · **C** FICHA do imóvel · **D** pendências e ações humanas + **D2** alertas de incompatibilidade · **E** contrato de dados de entrada · **F** fluxo ótimo para o próximo imóvel (+ apêndices com os catálogos lidos).
- Novas regras do Stays continuam a numeração a partir de `APR-026`.

### 7. Comece assim
1. Faça o intake sem cliques: classifique as fontes, liste pendências e divergências iniciais, confirme que o ID Stays é este imóvel.
2. Me diga em uma mensagem o que vai fazer primeiro e o que falta, e só então comece pelo item «textos / amenities / cômodos».

---

### Versão curta (para pedidos parciais, quando o plugin já está instalado)
> Use o agente `stays-listing-automation`. Imóvel «AL00» / Stays «XX00X», RASCUNHO. Escopo: «cômodos + fotos». Fontes: laudo em anexo; fotos nas próximas mensagens, um cômodo por mensagem. Regras Trip Homes e de segurança do plugin valem integralmente. Decisões: «estacionamento gratuito sim; capacidade PENDENTE». Entregue `[CHECKPOINT]` cumulativo ao fim e liste divergências para eu decidir.
