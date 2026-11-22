---
name: analisar-anuncios
description: Lê o desempenho das campanhas de Facebook e Instagram (Meta) pelo Brasa e diz o que escalar, pausar ou trocar, com os números; aplica ajustes só com aprovação. Use quando o usuário perguntar como estão os anúncios ou as campanhas, quanto gastou, qual o custo por resultado ou ROAS, ou pedir para escalar, pausar ou mudar orçamento.
---

# Analisar anúncios com o Brasa

Fale em português do Brasil, com números e frases curtas. Use as ferramentas do servidor MCP `brasa`.

1. Chame `status`. Se a Meta não estiver conectada ou a conta não estiver escolhida, diga o que falta (tela Conta do site do Brasa) e pare. Se a leitura da Meta não estiver ligada neste Brasa, ofereça analisar um print ou os números do Gerenciador de Anúncios que ele colar aqui.
2. Chame `ver_desempenho` (janela padrão de 7 dias; 3, 14 ou 30 se ele pedir). Use só os números que vierem dele.
3. Responda em poucas linhas: quanto investiu, quantos resultados, custo por resultado (e ROAS em vendas), o que está indo bem e o que escalar, pausar ou trocar criativo, sempre com o porquê em números.
4. Se ele ainda não tiver critério, pergunte quanto vale pagar por resultado e salve com `salvar_metas` (só números que ele disse).
5. Para mudar algo, chame `propor_ajustes`: os ajustes ficam pendentes e também aparecem na página Tráfego do site. Mostre cada um.
6. `aplicar_ajuste` muda a conta de anúncios de verdade: chame **só depois que ele aprovar aquele ajuste nesta conversa**. Suba orçamento no passo do critério (padrão 20%) e nunca acima do teto. Se ele não quiser, `recusar_ajuste`.
7. Para trocar criativos cansados, ofereça a skill `subir-campanha` para gerar novos e montar outra campanha.
