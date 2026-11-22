---
name: subir-campanha
description: Cria e sobe uma campanha de anúncios no Facebook e no Instagram (Meta) com o Brasa, do material do usuário até a campanha publicada PAUSADA no Gerenciador de Anúncios. Use quando o usuário pedir para subir, publicar, criar ou montar uma campanha ou um anúncio na Meta, no Facebook Ads ou no Instagram, ou disser coisas como "sobe uma campanha", "anuncia meu produto", "quero vender isso no Instagram", "faz os criativos e já publica".
---

# Subir campanha com o Brasa

Você conduz o usuário do material (fotos, anúncio atual, vídeo ou só uma descrição) até uma campanha criada **pausada** na Meta. Tudo passa pelas ferramentas do servidor MCP `brasa` deste plugin. Fale em português do Brasil, em frases curtas, e mostre só o essencial de cada passo.

## 1. Conectar e ver o que está ligado

Chame `status`.

- **A ferramenta não existe ou deu erro de acesso:** o usuário ainda não entrou no Brasa. Peça para ele rodar `/mcp`, escolher `brasa` e entrar com a conta do Brasa (abre o navegador). Depois continue.
- **Leia a linha "Meta" do status:**
  - conectado, com conta de anúncios e página escolhidas → dá para publicar (para objetivo "vendas" também precisa do pixel);
  - Facebook não conectado, conexão vencida ou conta não escolhida → diga o que falta e mande ele resolver na tela Conta do site (o link "Site" do status, depois `#conta`). Você pode seguir com os criativos enquanto isso;
  - publicação da Meta não ativa neste Brasa → faça tudo até o plano e entregue o pacote no fim (passo 7).

## 2. Entender o pedido

Junte, de preferência numa pergunta só, o que faltar:

| Item | Padrão se ele não disser |
|---|---|
| Produto e oferta | pergunte; nunca invente |
| Objetivo: `vendas` (no site), `trafego` (visitas ao site) ou `whatsapp` (mensagens) | `vendas` se ele tem loja, `whatsapp` se vende pelo WhatsApp |
| Link https do site | obrigatório para `vendas` e `trafego` |
| Orçamento por dia | proponha R$ 20 |
| Público | Brasil, 18 a 65 anos, todos os gêneros |
| Material | arquivos que ele citar nesta pasta |

Nunca invente preço, desconto, prazo, telefone, endereço, depoimento ou promessa: use só o que ele disse ou o que aparece no material dele.

## 3. Projeto

Se ele citar um projeto, use `listar_projetos`; senão chame `criar_projeto` com um título curto (o produto ou a campanha). Guarde o id. Cada projeto aparece como uma conversa no site do Brasa; passe o link "No site" para ele acompanhar por lá.

## 4. Enviar os arquivos dele

Para cada foto, logotipo, anúncio ou vídeo da pasta:

1. Descubra o tamanho em bytes: `(Get-Item "foto.jpg").Length` no PowerShell ou `wc -c < foto.jpg` no bash.
2. Chame `enviar_arquivo` com projeto, nome, tipo (`image/jpeg`, `image/png`, `image/webp`, `video/mp4`, `video/quicktime`, `video/webm`) e tamanho.
3. Rode o `curl` que ele devolve, na pasta do arquivo (no PowerShell do Windows, troque `curl` por `curl.exe`).
4. Chame `confirmar_envio` com o `envio` devolvido. Anote o número (#n para imagem, Vn para vídeo).

Olhe as imagens com `ver_imagem` antes de escrever os criativos.

## 5. Criativos

Chame `gerar_criativos` (no máximo 8 por chamada; um formato por chamada).

- Quantidade: a que ele pedir; sem pedido, 4.
- Formato: `4:5` para o feed (padrão de campanha); `9:16` para stories e reels; `1:1` se ele pedir.
- Passe em `referencias` os números das imagens dele, na ordem em que o prompt fala delas ("primeira imagem", "segunda imagem").
- Cada prompt é completo e independente, em português (o modelo de imagem não vê a conversa): tipo de peça; o que manter da referência ("Mantenha exatamente o produto da primeira imagem: mesmo formato, cores, rótulo e textos da embalagem"); cena, luz e composição; textos da arte entre aspas com acentos (título de até 6 palavras e no máximo mais 2 textos curtos); cores da marca; e termine com "Renderize os textos exatamente como escritos, com os acentos corretos, sem nenhum texto extra."
- Varie as ideias: oferta, benefício, produto em uso, detalhe, prova visual, contexto do público.

Se a ferramenta disser que ainda está trabalhando, chame `aguardar` com o projeto e o pedido até terminar. Mostre o resultado em poucas linhas e pergunte quais imagens entram na campanha (ou se quer ajustar alguma: use a imagem como referência e diga o que muda).

## 6. Plano da campanha

Chame `preparar_campanha` com:

- `nome` (até 100 caracteres), `objetivo`, `orcamento_diario` (reais por dia), `destino_url` (https, para `vendas` e `trafego`);
- `publico`: `idade_min`, `idade_max` (18 a 65), `genero` (`todos`, `homens`, `mulheres`);
- `imagens`: números das imagens escolhidas (até 10);
- `textos`: 3 a 5 textos principais de até 125 caracteres; `titulos`: 3 a 5 títulos de até 40 caracteres;
- `cta`: `SHOP_NOW`, `LEARN_MORE`, `SIGN_UP`, `CONTACT_US`, `GET_OFFER`, `ORDER_NOW`, `BUY_NOW`, `BOOK_NOW` ou `WHATSAPP_MESSAGE` (este para objetivo `whatsapp`);
- `recursos` da IA da Meta, se ele quiser: `text_optimizations`, `image_touchups`, `image_uncrop`, `adapt_to_placement`.

Mostre o plano numa tabela curta (objetivo, orçamento, público, link, imagens, textos, títulos, botão) e pergunte, com estas palavras ou parecidas: **"Posso publicar na Meta? Tudo sobe pausado."**

## 7. Publicar

**Só depois de um "sim" claro do usuário nesta conversa.** Publicar cria campanha, conjunto e anúncios de verdade na conta de anúncios dele.

- Chame `publicar_campanha` com o projeto. Se ele responder que ainda está publicando, chame `aguardar` e depois `ver_projeto`.
- Ao terminar, diga: campanha criada **pausada**, quantos anúncios, e o link do Gerenciador de Anúncios. Nunca diga que a campanha está no ar: quem ativa é ele, no Gerenciador.
- Se falhar, mostre o motivo que a ferramenta deu e o que fazer (por exemplo, escolher o pixel na tela Conta).

**Se a publicação direta não estiver ligada neste Brasa** (passo 1), entregue o pacote:

1. Chame `ver_projeto` para pegar os links das imagens escolhidas (valem 2 horas).
2. Pergunte a pasta e baixe cada uma com `curl -L -o "<arquivo>.jpg" "<link>"` (no PowerShell, `curl.exe`), numa subpasta `campanha-<nome>`.
3. Escreva ali um `campanha.txt` com objetivo, orçamento, público, link, textos, títulos e botão.
4. Explique em 3 linhas como subir: no Gerenciador de Anúncios (https://adsmanager.facebook.com), Criar, escolher o objetivo, colar o público e o orçamento, subir as imagens e colar os textos. O site do Brasa também tem o botão "Baixar pacote" no cartão da campanha.

## Regras

- Publicar e aplicar ajustes mexem na conta de anúncios real: só com a aprovação do usuário nesta conversa, nunca por conta própria.
- Siga as políticas de anúncios da Meta: nada de antes e depois de corpo, nada que aponte características pessoais de quem vê, nada de botões falsos de interface, nada enganoso. Não use marcas, personagens ou pessoas famosas de terceiros.
- Cada imagem gerada conta no limite diário do Brasa. Não gere mais do que ele pediu.
- Para acompanhar a campanha depois, use a skill `analisar-anuncios`.
