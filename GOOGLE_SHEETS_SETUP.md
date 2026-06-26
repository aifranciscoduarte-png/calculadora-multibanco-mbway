# Ligar os leads ao Google Sheets

A calculadora envia cada contacto submetido para uma Folha de Cálculo do Google, através de um
**Google Apps Script** publicado como Web App. Segue estes passos uma única vez.

## 1. Criar a folha

1. Vai a https://sheets.new (cria uma folha nova).
2. Dá-lhe um nome (ex.: *Leads Calculadora*). Não precisas de criar cabeçalhos — o script cria-os.

## 2. Adicionar o script

1. Na folha: menu **Extensões → Apps Script**.
2. Apaga o conteúdo que vier por defeito e cola **exatamente** isto:

```javascript
function doPost(e) {
  try {
    var lock = LockService.getScriptLock();
    lock.waitLock(20000);

    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName('Leads') || ss.getSheets()[0];

    if (sheet.getLastRow() === 0) {
      sheet.appendRow([
        'Data', 'Nome', 'Email', 'Telefone', 'Loja',
        'Encomendas/dia', 'AOV', '% MB/MB WAY', '% não pagas',
        'Encomendas não pagas/ano', 'Perda/mês (€)', 'Perda/ano (€)'
      ]);
    }

    var d = JSON.parse(e.postData.contents);
    sheet.appendRow([
      new Date(), d.nome, d.email, d.telefone, d.loja,
      d.encomendas_dia, d.valor_medio,
      d.pct_mb + '%', d.pct_nao_pagas + '%',
      d.encomendas_nao_pagas_ano,
      d.perda_mes + ' €', d.perda_ano + ' €'
    ]);

    lock.releaseLock();
    return ContentService
      .createTextOutput(JSON.stringify({ result: 'ok' }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService
      .createTextOutput(JSON.stringify({ result: 'error', error: String(err) }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. Guarda (ícone do disquete ou **Ctrl/Cmd + S**).

## 3. Publicar como Web App

1. No editor do Apps Script, canto superior direito: **Implementar → Nova implementação**.
2. Em **Selecionar tipo** (ícone da roda dentada) escolhe **Aplicação Web**.
3. Configura:
   - **Executar como:** Eu (a tua conta).
   - **Quem tem acesso:** **Qualquer pessoa** (importante — senão o site não consegue enviar).
4. Clica **Implementar**. Vai pedir para autorizares — aceita as permissões da tua conta Google.
5. Copia o **URL da aplicação Web** (termina em `/exec`). É algo como:
   `https://script.google.com/macros/s/AKfy....../exec`

## 4. Colar o URL na calculadora

Abre o `resultado.html` e na zona de config (perto do fim, no `<script>`) cola o URL:

```javascript
const LEADS_ENDPOINT = "https://script.google.com/macros/s/AKfy....../exec";
```

> Dá-me o URL e eu colo-o por ti, faço commit e push — a Vercel republica sozinha.

## Testar

1. Abre o `index.html`, preenche os dados e clica **Ver quanto estás a perder** (vai para `resultado.html`).
2. Em `resultado.html`, preenche o formulário de contacto e clica **Ver o meu resultado completo**.
3. Confirma que aparece uma nova linha na folha do Google.

> Se mudares o código do script mais tarde, tens de criar uma **Nova implementação** (ou
> *Gerir implementações → editar → Nova versão*) para o URL passar a usar a versão atualizada.
