# Calculadora — Encomendas MB / MB WAY não pagas

Ferramenta de vendas que mostra a uma loja online quanto dinheiro perde por ano com encomendas
geradas em **Multibanco / MB WAY** que nunca chegam a ser pagas — e quanto disso é recuperável
com lembretes automáticos por WhatsApp.

## Como abrir

Duplo-clique no `index.html` ou:

```bash
open index.html
```

É um ficheiro único (HTML + CSS + JS embutidos), sem dependências nem build.

## Como alojar

Basta enviar o `index.html` para qualquer alojamento estático (Netlify, GitHub Pages, Vercel,
ou um simples servidor). Não precisa de backend.

## Inputs

1. **Encomendas por dia** — total de encomendas feitas na loja por dia.
2. **Valor médio por encomenda** (€).
3. **% pago com Multibanco / MB WAY** — fatia das encomendas feita com estes métodos.
4. **% dessas que são pagas** — quantas das MB/MB WAY são efetivamente pagas.

## Cálculo

```
naoPagas/dia   = encomendas/dia × %MB × (1 − %pagas)
perda/dia      = naoPagas/dia × valorMédio
perda/mês      = perda/dia × 30
perda/ano      = perda/dia × 365
recuperável/ano = perda/ano × 10%   (taxa de recuperação via WhatsApp)
```

Valores em euros (formato PT-PT). Base: 30 dias/mês, 365 dias/ano.
