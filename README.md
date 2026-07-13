# Upscale Commerce API — Documentação

Docs em MDX pra publicar no [Mintlify](https://mintlify.com/).

## Como testar localmente

Instale a CLI do Mintlify (uma vez):

```bash
npm install -g mintlify
```

Sirva localmente (renderiza em `http://localhost:3000`):

```bash
cd docs-api
mintlify dev
```

A CLI faz hot-reload de todos os `.mdx`. Não precisa build — edita, salva, o navegador atualiza.

## Estrutura

```
docs-api/
├── mint.json                              # Config do Mintlify (paleta, navegação, links)
├── introduction.mdx                       # Página inicial
├── authentication.mdx                     # Chaves de API, X-API-Key
├── rate-limits.mdx                        # 100 req/min por chave
├── errors.mdx                             # Códigos e estratégia de retry
├── api-reference/
│   ├── produtos/
│   │   ├── criar.mdx                      # POST /produtos
│   │   └── listar.mdx                     # GET /produtos
│   ├── variantes/upsert.mdx               # POST /variantes
│   ├── estoque/atualizar.mdx              # POST /estoque
│   ├── precos/atualizar.mdx               # POST /precos
│   └── pedidos/
│       ├── listar.mdx                     # GET /pedidos
│       ├── processado.mdx                 # POST /pedidos/{codigo}/processado
│       └── rastreio.mdx                   # POST /pedidos/{codigo}/rastreio
├── guides/
│   ├── fluxo-catalogo.mdx                 # Sync inicial
│   ├── fluxo-estoque.mdx                  # Sync periódica
│   └── fluxo-pedidos.mdx                  # Pull de pedidos + rastreio
└── postman/
    └── upscale-api.postman_collection.json  # Collection pronta pra importar
```

## Publicar no Mintlify

O Mintlify tem GitHub App: cada push na branch `main` regenera a documentação hospedada.

### Passo a passo pro Antônio:

1. **Criar repo novo no GitHub**
   - Nome sugerido: `upscaledigital/api-docs` ou `upscaledigital/upscale-commerce-docs`
   - Visibilidade: **Public** (docs pública) ou **Private** (docs pra parceiros com chave)
   - Adicionar README

2. **Copiar este diretório pro novo repo**
   ```bash
   git clone git@github.com:upscaledigital/api-docs.git
   cp -r ~/path/to/docs-api/* api-docs/
   cd api-docs
   git add .
   git commit -m "initial mintlify docs"
   git push origin main
   ```

3. **Conectar Mintlify no repo**
   - Ir em https://dashboard.mintlify.com
   - Login com GitHub
   - Instalar o [Mintlify GitHub App](https://github.com/apps/mintlify) no org `upscaledigital`
   - Autorizar acesso ao repo `api-docs`
   - Escolher branch `main` como source

4. **Definir subdomínio inicial**
   - Mintlify cria `upscaledigital.mintlify.app` automaticamente
   - Confirme que o site renderiza corretamente lá

5. **Apontar domínio próprio (opcional)**
   - No dashboard Mintlify, configure Custom Domain
   - Sugestões: `api.upscaledigital.com.br`, `docs.upscaledigital.com.br`
   - Mintlify pede um CNAME apontando pro domínio deles (`upscaledigital.mintlify.app`)
   - Configurar no Cloudflare (ou provedor de DNS)
   - Aguardar propagação (~10 min) + provisioning de SSL (Mintlify usa Let's Encrypt)

6. **Configurar logo e favicon**
   - Adicionar `logo/dark.svg`, `logo/light.svg`, `favicon.svg` na raiz do repo
   - Referenciados em `mint.json` já
   - Se ainda não tem logo, Mintlify usa nome do site em texto (funciona OK)

## Trabalho manual restante do lado do Antônio

Estimativa: **1–2 horas** de setup + qualquer ajuste de branding.

- [ ] Criar repo `upscaledigital/api-docs` no GitHub (5 min)
- [ ] Copiar arquivos deste `docs-api/` pro repo (10 min)
- [ ] Instalar Mintlify GitHub App + autorizar repo (5 min)
- [ ] Confirmar renderização no `upscaledigital.mintlify.app` (10 min)
- [ ] Adicionar `logo/*` e `favicon.*` — ou desabilitar em `mint.json` (15 min se tiver logo pronto)
- [ ] Configurar Custom Domain no Mintlify + DNS (30 min se DNS é rápido)
- [ ] Revisar tom/texto pra alinhar com marca (30–60 min opcional)

## Manutenção contínua

Cada evolução da API vira PR no repo `api-docs`:

1. Fazer mudanças nos MDX
2. `mintlify dev` local pra ver preview
3. Push pra `main` → Mintlify regenera em ~1 min

Convenção: usar `<Warning>` em breaking changes, `<Note>` em avisos, `<Tip>` em boas práticas. Componentes documentados em https://mintlify.com/docs/content/components.

## Postman Collection

O arquivo `postman/upscale-api.postman_collection.json` pode ser importado direto no Postman/Insomnia. Cada endpoint tem:

- URL parametrizada com variáveis (`{{base_url}}`, `{{api_key}}`)
- Headers pré-configurados
- Body de exemplo pronto
- Query params documentados

Distribua junto com a chave de API pra parceiros novos.
