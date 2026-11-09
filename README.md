# Agroseg Connect — Página de redirecionamento após checkout

Página premium para o comprador que retorna do Mercado Pago. Usa a identidade grafite e dourado e a **logo oficial do Agroseg Connect**.

## Funcionalidades

- Layout responsivo e adaptado ao celular.
- Instruções de próximos passos, confirmação no Mercado Pago e uso do produto.
- Botão para abrir o Agroseg Connect: https://agrocalc-one.vercel.app/
- Botão para consultar o Mercado Pago.
- Páginas de retorno: `/`, `/sucesso`, `/pendente` e `/falha`.
- As rotas também interpretam parâmetros típicos de retorno (`status`, `collection_status`, `payment_status` e `retorno`).
- `robots: noindex,nofollow` para evitar indexar páginas de checkout e retornos pessoais.
- Nenhum dado de pagamento ou identificador é exibido ou enviado a outros serviços.

## Publicação na Vercel

1. Acesse https://vercel.com/new.
2. Importe o repositório `testeagroseg-eng/REDIRECIONAMENTOAGC`.
3. Selecione **Other** como framework; não informe comando de build.
4. Deixe o diretório raiz como padrão e publique.
5. Copie a **URL real gerada pela Vercel**. Essa URL será o destino de retorno, caso o meio de checkout utilizado permita configurar uma URL de redirecionamento.

### Rotas depois da publicação

- `https://SEU-PROJETO.vercel.app/` — retorno genérico.
- `https://SEU-PROJETO.vercel.app/sucesso` — orientação para um retorno de conclusão, **sem afirmar aprovação verificada**.
- `https://SEU-PROJETO.vercel.app/pendente` — orientação para pagamento possivelmente pendente.
- `https://SEU-PROJETO.vercel.app/falha` — orientação para retorno de falha/abandono.

A configuração das rotas é feita pelo `vercel.json`.

## Configuração no Mercado Pago

O checkout usado na landing page foi um link curto `https://mpago.la/2aUkW2B`.

**Atenção: nem todo link simples do Mercado Pago oferece campo editável para URL de retorno.** O código deste repositório **não altera automaticamente o checkout do Mercado Pago**. Verifique no painel da cobrança se existe opção de página de retorno, pós-pagamento ou redirecionamento. Se não existir, será necessário adotar um fluxo de checkout/integracão que suporte `back_urls` e, quando disponível, `auto_return`.

Em uma integração de checkout via API, configure as URLs reais da página para os retornos `success`, `pending` e `failure` conforme a documentação vigente do Mercado Pago. **Nunca use o retorno do navegador como evidência de aprovação.**

## Segurança e acesso ao SaaS

O visitante pode abrir esta página diretamente ou editar os parâmetros de URL. Por isso:

- A página **não confirma** que o pagamento foi aprovado.
- A página **não cria usuários** e **não libera acesso**.
- Para acesso exclusivo de pagantes, configure backend com credenciais privadas, notificações oficiais e consulta do pagamento na API do Mercado Pago. Só após confirmação, habilite a conta no Agroseg Connect.
- Nunca coloque access tokens privados no JavaScript ou no HTML do navegador.
- Para mostrar uma mensagem de **pagamento confirmado**, a informação deve vir de sessão verificada pelo servidor, não de `?status=approved`.

## Arquivos

- `index.html` — página de retorno.
- `vercel.json` — rotas e cabeçalhos.
- `assets/agroseg-connect.webp` — logo oficial.
