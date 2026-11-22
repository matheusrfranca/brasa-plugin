# Brasa para o Claude Code

Plugin que liga o Claude Code ao **Brasa**. Com ele, o cliente conversa com o Claude e sai com a campanha de Facebook e Instagram pronta: criativos gerados a partir das fotos dele, plano da campanha e tudo **publicado pausado** na Meta. Ele também lê o desempenho dos anúncios e sugere o que escalar, pausar ou trocar.

Uma instalação traz:

| Parte | O que faz |
|---|---|
| Conexão `brasa` (MCP) | liga o Claude ao servidor do Brasa com o login do Brasa; não precisa copiar chave |
| Skill `/brasa:subir-campanha` | do material do cliente até a campanha publicada pausada |
| Skill `/brasa:analisar-anuncios` | lê gasto, resultados, custo por resultado e ROAS e propõe ajustes (só aplica com aprovação) |

Tudo o que o Claude faz aparece também no site do Brasa, como uma conversa.

## Instalar (para o cliente)

No Claude Code:

```
/plugin marketplace add matheusrfranca/brasa-plugin
/plugin install brasa@brasa
```

Se o Claude Code pedir, reinicie a sessão.

Pelo terminal, sem abrir o Claude Code:

```
claude plugin marketplace add matheusrfranca/brasa-plugin
claude plugin install brasa@brasa
```

## Entrar no Brasa

1. No Claude Code, rode `/mcp` e escolha o servidor `brasa`.
2. Entre na sua conta do Brasa no navegador que abrir e clique em **Permitir**.
3. Volte ao Claude Code. Pronto.

Sem conta? Crie em https://ca-brasa-dev.calmdesert-42ad1cf1.brazilsouth.azurecontainerapps.io/#criar-conta.

## Usar

Peça em português, na pasta onde estão as fotos:

> sobe uma campanha de vendas da minha garrafa térmica com as fotos desta pasta, R$ 30 por dia, link https://minhaloja.com.br/garrafa

Ou chame direto `/brasa:subir-campanha`. O Claude:

1. envia as fotos para o Brasa;
2. gera os criativos (4:5 para o feed, 9:16 para stories);
3. monta o plano (objetivo, orçamento, público, textos, títulos e botão) e mostra para você;
4. **só publica depois do seu "sim"**, e tudo sobe **pausado**: você revisa e ativa no Gerenciador de Anúncios.

Para acompanhar: "como estão meus anúncios nos últimos 7 dias?" ou `/brasa:analisar-anuncios`.

## O que precisa estar ligado para publicar

- **Na conta do cliente:** Facebook conectado e conta de anúncios e página escolhidas na tela Conta do site do Brasa (para campanha de vendas, também o pixel).
- **No servidor do Brasa:** o app da Meta configurado (`brasa-studio/scripts/definir-meta.ps1`). Enquanto ele não estiver, o plugin faz tudo até o plano e entrega o pacote (imagens e textos) para subir pelo Gerenciador de Anúncios.

## No claude.ai e nos apps do Claude

O plugin é do Claude Code. No claude.ai:

- a conexão é um **conector personalizado** com o endereço `https://ca-brasa-dev.calmdesert-42ad1cf1.brazilsouth.azurecontainerapps.io/mcp` (Configurações, Conectores);
- as skills podem ser enviadas como zip: compacte a pasta `plugins/brasa/skills/subir-campanha` (e `analisar-anuncios`) e envie na parte de Skills das configurações.

No claude.ai o Claude não envia arquivos do computador: o cliente anexa as fotos na conversa do projeto no site do Brasa.

## Manutenção

| Quando | O que mudar |
|---|---|
| O app do Brasa mudar de endereço | `url` em `plugins/brasa/.mcp.json` e os links deste README |
| Nova versão do plugin | `version` em `plugins/brasa/.claude-plugin/plugin.json` e em `.claude-plugin/marketplace.json` |
| Validar antes de publicar | `npx -y @anthropic-ai/claude-code plugin validate ./plugins/brasa` |

Estrutura:

```
.claude-plugin/marketplace.json        catálogo (marketplace "brasa")
plugins/brasa/.claude-plugin/plugin.json
plugins/brasa/.claude-plugin/icon.png
plugins/brasa/.mcp.json                conexão com o servidor do Brasa (login OAuth)
plugins/brasa/skills/subir-campanha/SKILL.md
plugins/brasa/skills/analisar-anuncios/SKILL.md
```

Para enviar ao catálogo público de plugins da Anthropic, ainda faltam uma licença (`LICENSE` ou `license` no plugin.json) e o link da política de privacidade (`privacyPolicyUrl`).
