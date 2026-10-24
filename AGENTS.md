# AGENTS.md — Noremati (site)

Site institucional da **Noremati Pilates Clássico** (Ponta da Praia, Santos-SP). Objetivo: autoridade, SEO local e conversão para agendamento da primeira aula via WhatsApp. **Repositório público e site em produção.**

## Stack
Site estático de página única. Sem framework, build, dependências ou CI.
- `index.html` — HTML, CSS e JS inline (~65 KB)
- `assets/img/` — logo e fotos (webp)
- `robots.txt`, `sitemap.xml` (domínio `noremati.com`)

## Rodar local
`python3 -m http.server 8080` e abrir `http://localhost:8080`.

## Deploy e produção
- Push na `main` dispara `.github/workflows/deploy.yml`: autentica na AWS por OIDC, faz `aws s3 sync . --delete` (excluindo `.git`, `.github`, `README.md`, `Old-v1`) e invalida o CloudFront (`/*`). **Merge em `main` = produção imediata.**
- Tudo que fica na raiz e não está excluído é **publicado** e público, inclusive `.deploy/`, `scripts/` e este arquivo. Nunca colocar secret, dado de cliente ou informação interna no repositório.
- Não há CI. Antes de pedir OK de merge, validar localmente em 360, 768 e 1280 px.

## Aprovação e merge
- Peça OK do PH antes de **todo** merge. A regra de "merge sem OK para tarefa pequena" exige CI verde e **não se aplica** aqui, porque o repositório não tem CI.
- Alteração em workflow, bucket, CloudFront ou IAM: OK explícito.

## Workflows de patch (cuidado)
`apply-conversion-patch.yml`, `apply-noremati-final.yml`, `fix-approved-home.yml` e `fix-whatsapp-icon.yml` reescrevem o `index.html` e fazem commit direto com `contents: write`, ao alterar o próprio arquivo ou por disparo manual. Pelo conteúdo, parecem patches pontuais já aplicados (hipótese). **Não editar, renomear nem disparar sem OK do PH**; remover é decisão dele.

## Conteúdo e marca
- Preservar identidade visual, logo e posicionamento aprovados. Não redesenhar áreas aprovadas sem pedido.
- Não inventar números, clientes, avaliações, endereço ou horário. Confirmar endereço e horário antes de colocá-los em Schema.org.
- CTA principal: "agendar primeira experiência" → WhatsApp (botão flutuante `.whatsapp-float` com o ícone oficial, verde `#25D366`; links marcados com `data-booking-open`). Não quebrar esse fluxo.
- SEO já existente (meta, Open Graph, Schema.org, FAQ estruturado, eventos GA4): preservar. Mudando o domínio, atualizar canonical, `robots.txt` e `sitemap.xml`.
- Pendência registrada no README: `G-XXXXXXXXXX` (GA4) ainda é placeholder em `index.html`. Não inventar o ID; pedir o Measurement ID real ao PH.

## UI
- Tokens em `:root` do `index.html`: `--bg #f7f2e8`, `--paper #fffdf8`, `--ink #2c2117`, `--gold #a77a34`, `--gold-btn #8f6a2e`, `--gold2 #c6a260`, `--dark #2d2116`. Reutilizar, não criar cores novas.
- Breakpoints em uso: 960, 900, 760, 620, 520 px. Sem overflow horizontal; manter `prefers-reduced-motion`.

## Nunca
- Colocar secret, token ou dado pessoal no repositório (ele é público).
- Criar V2/New/Final, páginas ou arquivos paralelos, ou adicionar dependências sem necessidade.
