# Desafio Playwright – Datacrazy QA

Crie uma suíte de testes end‑to‑end com Playwright para interagir com o ambiente `qa.datacrazy.io`. O foco é criatividade, cobertura útil e robustez. Se achar um bug real, você ganha pontos extras.

## Objetivo
- Criar uma conta nova no `qa.datacrazy.io` e, usando Playwright, automatizar interações reais no sistema.
- Demonstrar domínio de boas práticas de testes: seleção de elementos resiliente, organização do projeto, relatórios, rastreabilidade e dados de teste bem pensados.

## Escopo mínimo obrigatório
- Criar um Lead
- Editar um Lead
- Criar uma Pipeline
- Interagir com uma Pipeline (ex.: mover cards de estágio, atualizar status, atribuir responsável, comentar/registrar atividade)

Você pode (e é incentivado a) testar outras áreas do sistema além do mínimo.

## Ideias criativas (bonus)
- Fluxos alternativos: filtros de Lead, busca, paginação, ordenação, anexos, tags.
- Experiências do usuário: internacionalização/idioma, acessibilidade (roles ARIA, navegação por teclado), atalhos.
- Observabilidade do front: validar logs/erros no console, interceptar requisições para checar payloads.
- Dados realistas: massa de dados variada, casos-limite (strings grandes, caracteres especiais, números extremos).
- Resiliência: testes idempotentes com limpeza, retries estratégicos, storage state para login.
- Performance básica: tempo de carregamento de páginas-chave, custos de renderização (leve).
- Relatórios: `trace`, `video`, `screenshots`, HTML report; exportar evidências.

Se você encontrar um BUG reprodutível (com passos claros), documente e inclua a evidência no relatório: mais pontos para você.

## Regras e melhores práticas
- Use seletores estáveis: por `role`, `label`, `placeholder` ou `data-testid` quando disponível; evite XPaths frágeis.
- Mantenha testes independentes e idempotentes: crie dados próprios e, se possível, limpe ao final.
- Evite depender de tempo fixo: prefira `await expect(...).toBeVisible()`/`toHaveText()` e condições de rede.
- Estruture o projeto: páginas (Page Objects) quando fizer sentido, utilitários para massa de dados, e fixtures para login/estado.
- Armazene o estado de autenticação com `storageState` para acelerar execuções repetidas.
- Não exponha credenciais. Use variáveis de ambiente e `.env.local` (não versionado).

## Setup rápido
1) Crie um projeto Playwright (TypeScript recomendado):

```bash
npm init -y
npm i -D @playwright/test
npx playwright install
npm i -D @faker-js/faker
```

2) Configure `baseURL` e artefatos úteis no `playwright.config.ts`:

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  timeout: 30_000,
  retries: 1,
  reporter: [['html', { open: 'never' }]],
  use: {
    baseURL: 'https://qa.datacrazy.io',
    trace: 'on-first-retry',
    video: 'retain-on-failure',
    screenshot: 'only-on-failure',
    storageState: 'storageState.json',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
  ],
});
```

3) Variáveis de ambiente (login/dados):

```bash
export DC_EMAIL="seu-email-de-teste@exemplo.com"
export DC_PASSWORD="senha-segura"
```

Opcional: use `.env` e carregue com uma lib como `dotenv`.

## Fluxo sugerido
1) Cadastro/Login
- Crie uma conta ou faça login com a conta de teste criada. Automatize o login inicial e salve `storageState.json` para reuso.

2) Lead
- Criar Lead: preencher formulário com dados gerados (nome, email, telefone, origem). Validar criação (toast, listagem, detalhes).
- Editar Lead: atualizar campos chave (ex.: telefone, tags). Validar persistência e visualização.

3) Pipeline
- Criar uma Pipeline: definir estágios significativos.
- Interagir com a Pipeline: criar card/negócio, mover entre estágios, alterar status/owner, comentar.

4) Outras áreas (opcional)
- Busca, filtros, exportações; páginas de configurações; auditoria/atividades; notificações.

## Exemplo de estrutura de testes
```
frontend-tester/
  tests/
    auth.setup.spec.ts       # Gera storageState
    lead.create.spec.ts      # Criação
    lead.edit.spec.ts        # Edição
    pipeline.create.spec.ts  # Criação da pipeline
    pipeline.interact.spec.ts# Interações
  helpers/
    data.ts                  # Massa de dados
  pages/
    LoginPage.ts             # Page Object opcional
```

### Exemplo (TypeScript) – Login + storageState
```ts
import { test, expect } from '@playwright/test';

//Código de exemplo haha, não deve estar funcionando. Corrige pra gente?
test('login e salvar estado', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill(process.env.DC_EMAIL!);
  await page.getByLabel('Senha').fill(process.env.DC_PASSWORD!);
  await page.getByRole('button', { name: /entrar/i }).click();
  await expect(page.getByText(/bem-vindo|dashboard/i)).toBeVisible();
});
```

### Exemplo – Criar Lead (esboço)
```ts
import { test, expect } from '@playwright/test';
import { faker } from '@faker-js/faker';

test.describe('Lead', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/leads');
  });

  test('criar lead', async ({ page }) => {
    await page.getByRole('button', { name: /novo lead/i }).click();
    const nome = faker.person.fullName();
    const email = faker.internet.email();
    await page.getByLabel(/nome/i).fill(nome);
    await page.getByLabel(/email/i).fill(email);
    await page.getByRole('button', { name: /salvar/i }).click();
    await expect(page.getByText(nome)).toBeVisible();
  });
});
```

## Critérios de avaliação
- Criatividade: cenários relevantes, coberturas além do mínimo.
- Robustez: seletores estáveis, idempotência, poucos flakies, bons waits.
- Organização: estrutura clara (tests/helpers/pages), legibilidade, reutilização.
- Observabilidade: uso de `trace`, `video`, `screenshots`, logs, bons asserts.
- Documentação: README de execução, decisões técnicas, limitações, bugs achados.
- Cobertura mínima: todos os itens obrigatórios implementados e validados.

## Entrega
- Inclua:
  - Código dos testes dentro de `desafio-datacrazy/frontend-tester` (ou subpasta).
  - `README.md` com instruções de execução.
  - Comandos de rodar (com e sem `storageState`).
  - Evidências: relatório HTML do Playwright, traces e vídeos (falhas).

### Como rodar
```bash
# primeira execução (gera storage state)
npx playwright test tests/auth.setup.spec.ts --ui

# suíte completa
npx playwright test

# abrir relatório
npx playwright show-report
```

## Dicas Playwright
- Prefira `getByRole/getByLabel/getByPlaceholder`.
- Evite `waitForTimeout`; use expectativas baseadas em UI ou rede.
- Intercepte rede (`page.route`) para validar payloads sem mockar tudo.
- Gere dados com `@faker-js/faker` para realismo.
- Use `test.step` para melhorar leitura no trace.

## Ética e segurança
- Não compartilhe credenciais. Use conta de teste própria.
- Não execute ações destrutivas em dados de terceiros.
- Caso haja limitação/instabilidade no ambiente QA, documente e contorne com resiliência.

## Dúvidas
Se precisar de algo (ex.: rotas específicas, como acessar determinada tela), escreva no seu README e siga com o melhor entendimento — valorizamos sua autonomia e clareza.

Boa sorte e divirta‑se caçando bugs!
# Desafio Tester Playwright 
