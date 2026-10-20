# Licita Oportunit — radar de editais por CNPJ

Digite um CNPJ → o servidor lê o CNAE na Receita Federal (via BrasilAPI) → busca editais com propostas abertas no PNCP → devolve os que combinam com a atividade da empresa.

## Estrutura
- `public/` página (HTML + CSS) servida pela própria Vercel
- `api/radar.js` endpoint `GET /api/radar?cnpj=00000000000191[&uf=BA|BR]`
- `lib/` consulta de CNPJ, consulta ao PNCP e casamento CNAE ↔ objeto do edital
- `test/radar.test.js` teste offline (simula as duas APIs): `npm test`

## Publicar na Vercel
**Pelo site:** suba esta pasta para um repositório no GitHub → vercel.com → *Add New… → Project* → importe o repositório → *Deploy*. Não precisa de variáveis de ambiente nem de build.

**Pelo terminal:** `npx vercel` (teste) e `npx vercel --prod` (produção).

Opcional: `ALLOWED_ORIGIN` limita quais sites podem chamar a API (padrão: qualquer um).

## Por que um servidor no meio?
Os navegadores bloqueiam (CORS) chamadas diretas do site para BrasilAPI/PNCP. O endpoint `/api/radar` faz a consulta no servidor e a página só conversa com ele, no mesmo domínio.

## Ajustes comuns
- Sensibilidade: `MIN_COMPAT` em `api/radar.js` (40 = um termo do CNAE principal já basta).
- Sinônimos por área de atividade: `SINONIMOS` em `lib/match.js`.
- Modalidades consultadas: `MODALIDADES` em `lib/pncp.js`.
