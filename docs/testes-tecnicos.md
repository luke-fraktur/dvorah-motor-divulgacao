# Testes técnicos e evidências de validação

## Objetivo

Este documento registra as verificações realizadas no laboratório web para apoiar uma apresentação acadêmica. A validação cobre compilação, build de produção, renderização visual responsiva e fluxos funcionais essenciais da calculadora e da simulação.

Os testes foram executados em **05/10/2026** contra a aplicação publicada em [dvorahai-ezadihcd.manus.space](https://dvorahai-ezadihcd.manus.space). O repositório não possui backend nem integração automática com plataformas musicais; por isso, a validação é de software e de método, não uma prova de desempenho de campanha.

## Resumo dos resultados

| Grupo | Verificação | Resultado |
|---|---|---|
| Tipagem | `pnpm check` / TypeScript sem emissão | **PASS** |
| Build | `pnpm build` / Vite + esbuild | **PASS** |
| Renderização | Dashboard desktop, 1440 px | **PASS** |
| Responsividade | Dashboard mobile, 390 px | **PASS** |
| Calculadora | Projeção, fórmula e indicadores derivados | **PASS** |
| Persistência | Registro em `localStorage` | **PASS** |
| Experimentos | Geração de hipótese no caderno local | **PASS** |
| Simulação | Execução da hipótese EXP-07 | **PASS** |
| Segurança documental | PDFs rastreados no Git | **PASS — 0 arquivos** |

## Testes automatizados de compilação

### TST-001 — Verificação TypeScript

**Comando:**

```bash
pnpm check
```

**Critério de aprovação:** o compilador termina com código de saída `0` e não apresenta erros de tipo.

**Resultado observado:** aprovado.

### TST-002 — Build de produção

**Comando:**

```bash
pnpm build
```

**Critério de aprovação:** Vite gera os artefatos de frontend e esbuild gera o servidor em `dist/`.

**Resultado observado:** aprovado. O build transformou 2.221 módulos e gerou os artefatos de produção. O bundler exibiu um aviso informativo de tamanho de chunk JavaScript acima de 500 kB; isso não interrompeu o build nem impediu a publicação.

## Smoke test funcional no navegador

O smoke test foi executado em uma instância Chromium headless, usando a aplicação publicada. Os controles foram acionados por DOM e os estados resultantes foram lidos no navegador.

| ID | Ação | Critério esperado | Resultado |
|---|---|---|---|
| SMK-001 | Abrir a aplicação | Documento renderizado sem falha | **PASS** |
| SMK-002 | Selecionar `Calculadora` | Módulo e seis campos numéricos aparecem | **PASS** |
| SMK-003 | Usar parâmetros padrão | Projeção de seguidores igual a `147` | **PASS** |
| SMK-004 | Verificar streams projetados | Valor igual a `705` | **PASS** |
| SMK-005 | Clicar em `Gerar experimento` | Novo registro é criado | **PASS** |
| SMK-006 | Consultar persistência | `dvorah-experiments` contém um registro | **PASS** |
| SMK-007 | Voltar para `Visão geral` | Dashboard é exibido | **PASS** |
| SMK-008 | Clicar em `Rodar experimento de 14 dias` | Estado visual indica `EXP-07 recalculado` | **PASS** |

### Conferência matemática do cenário padrão

Com os valores iniciais do laboratório:

- baseline de seguidores: `71`;
- streams atuais: `340`;
- crescimento semanal: `20%`;
- ciclo: `4` semanas;
- conversão estimada: `2,5%`.

A aplicação utiliza a fórmula:

```text
projeção = baseline × (1 + crescimento semanal) ^ semanas
```

Assim, `71 × 1,2^4 = 147,456`, arredondado para **147 seguidores projetados**. Para streams, `340 × 1,2^4 = 704,9664`, arredondado para **705 streams/mês**. Os cliques estimados são `705 × 2,5% = 17,625`, arredondados para **18**.

O campo **Meta de seguidores** aparece como referência de planejamento, mas não participa da fórmula atual de projeção. Essa distinção evita apresentar a meta como se fosse uma variável causal do cálculo.

### Persistência local verificada

A aplicação utiliza duas chaves de armazenamento no navegador:

| Chave | Conteúdo |
|---|---|
| `dvorah-calculator` | Parâmetros do cenário salvo |
| `dvorah-experiments` | Lista de hipóteses geradas e seus metadados |

O teste confirmou a geração de um experimento e sua leitura posterior por `localStorage`. Como essa persistência é local, os dados não são sincronizados entre navegadores ou dispositivos.

## Testes visuais e responsivos

Foram capturadas evidências da aplicação renderizada com Chromium em produção. As imagens estão versionadas em [`docs/assets/screenshots/`](assets/screenshots/).

| Evidência | Viewport | Escopo | Resultado |
|---|---:|---|---|
| Dashboard desktop | 1440 × 1635 | Página completa, navegação, métricas, gráfico e mix de formatos | **PASS** |
| Calculadora desktop | 1440 × 1256 | Formulário, projeção, fórmula e caderno local | **PASS** |
| Experimentos desktop | 1440 × 1000 | Lista de hipóteses, detalhe e ação de simulação | **PASS** |
| Público desktop | 1440 × 1000 | Personas, cartão público e leitura do motor | **PASS** |
| Dashboard mobile | 390 × 3042 | Refluxo vertical, cartões e gráfico em tela estreita | **PASS** |

A inspeção visual procurou especificamente imagens espremidas, overflow horizontal, perda de contraste, quebra dos cartões e sobreposição da navegação. Nenhum desses problemas foi observado nos viewports registrados.

## O que estes testes demonstram

Os testes demonstram que o artefato compila, inicia, renderiza os módulos principais, calcula o cenário padrão, registra uma hipótese e altera o estado de simulação. Também demonstram que a interface pode ser apresentada em desktop e mobile sem depender de uma única largura de tela.

Isso é diferente de demonstrar que uma campanha musical funcionará no mundo real. O laboratório não coleta métricas nativas, não executa publicação em redes sociais e não valida causalidade de crescimento. Os números exibidos continuam sendo **simulações didáticas**.

## Limitações e próximos testes

Ainda não há uma suíte de testes unitários ou end-to-end integrada ao pipeline CI. A próxima evolução técnica pode extrair a fórmula para funções puras, adicionar testes com Vitest, validar exportações CSV/JSON e criar testes automatizados de acessibilidade com foco em teclado, rótulos e contraste.

Também seria necessário um estudo empírico separado para comparar projeções com métricas observadas. Esse estudo deveria registrar plataforma, janela de coleta, definição da métrica, tamanho da amostra e decisão tomada, mantendo os dados reais separados do modelo didático.

## Reprodutibilidade

Para reproduzir a validação básica localmente:

```bash
pnpm install
pnpm check
pnpm build
pnpm dev
```

Depois, abra `http://localhost:3000`, selecione **Calculadora**, confirme os valores padrão, gere uma hipótese e execute a simulação na **Visão geral**. A validação manual deve observar os mesmos valores descritos acima, salvo diferenças causadas por dados já persistidos no navegador.
