# Calculadora — Encomendas MB / MB WAY não pagas

Lead magnet (estilo *A Marca que Converte*) que mostra a uma loja online quanto dinheiro perde por
ano com encomendas **Multibanco / MB WAY** que nunca chegam a ser pagas, capturando o contacto do
visitante para depois o abordar.

## Estrutura (2 páginas + estilos)

- **`index.html`** — calculadora: o utilizador preenche os números e clica em *Ver quanto estás a perder*.
- **`resultado.html`** — recebe os dados por URL, mostra o resultado **desfocado** e um formulário de
  contacto (Nome, Email, Telemóvel/WhatsApp, Loja). Ao submeter, envia o lead e **revela** os valores.
- **`styles.css`** — sistema visual partilhado (identidade de marca).

## Como abrir

```bash
open index.html
```

Sem dependências nem build — site estático. Aloja em qualquer serviço estático (Vercel, GitHub Pages…).

## Fluxo

1. `index.html` → inputs → submete → redireciona para `resultado.html?orders=…&avg=…&mb=…&unpaid=…`
2. `resultado.html` → resultado desfocado + número-isco visível (encomendas não pagas/ano)
3. Formulário de contacto → envia o lead para Google Sheets → revela perda/ano e perda/mês

## Inputs

1. **Encomendas por dia** — total de encomendas recebidas por dia.
2. **Valor médio por encomenda (AOV)** (€).
3. **% de encomendas em Multibanco / MB WAY**.
4. **% dessas que *não* são pagas**.

## Cálculo

```
naoPagas/dia = encomendas/dia × %MB × %naoPagas
perda/dia    = naoPagas/dia × AOV
perda/mês    = perda/dia × 30
perda/ano    = perda/dia × 365
```

Valores em euros (formato PT-PT). Base: 30 dias/mês, 365 dias/ano.

## Captura de leads

Os contactos são gravados numa Folha do Google via Apps Script. Configuração em
**`GOOGLE_SHEETS_SETUP.md`** — falta colar o URL do Web App em `LEADS_ENDPOINT` (topo do `<script>`
em `resultado.html`).

## Identidade visual

Segue o *Brand Guidelines v1.0*: Inter + JetBrains Mono; azul cobalto `#0052CC`, elétrico `#0066FF`
(CTAs), quase-preto `#0A0F1E` (hero), ciano `#00B4D8`, glacial `#D6E4FF`, surface `#F0F4FF`.
Cantos 6–8px (cards) / 4px (botões); gradiente azul apenas em hero/fundo.
