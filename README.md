# SpotMyDeal — Sprint 3 · Front-End Design Engineering

> Gamificação de cupons de desconto com **loot tracker**. Projeto acadêmico **FIAP** em parceria conceitual com a **SoulUp** (upcycling & sustentabilidade).

Este repositório é a **evolução da Sprint 2** (HTML + CSS + JS puro) para uma **aplicação React moderna**, componentizada, tipada com TypeScript e construída com Vite — conforme os requisitos da etapa **Front-End Design Engineering**.

---

## 🚀 Stack técnica (obrigatória da Sprint)

| Tecnologia | Uso |
|---|---|
| **React 18** | Interface e componentização |
| **Vite** | Build e ambiente de desenvolvimento |
| **TypeScript** | Tipagem estática em toda a aplicação |
| **Tailwind CSS** | Estilização (100% via classes utilitárias, sem CSS externo) |
| **React Router DOM** | Navegação SPA (rotas estáticas e dinâmicas) |
| **React Hook Form** | Formulário de contato com validação tipada |
| **GitHub** | Versionamento (repositório já inicializado com commit inicial) |

---

## 📦 Como rodar o projeto

```bash
# 1. Instalar dependências
npm install

# 2. Rodar em modo desenvolvimento
npm run dev
# abre em http://localhost:5173

# 3. Build de produção
npm run build

# 4. Pré-visualizar o build de produção
npm run preview
```

## 🔗 Subindo para o GitHub

O projeto já vem com um repositório Git local inicializado e o primeiro commit feito. Para publicar:

```bash
git remote add origin <URL_DO_SEU_REPOSITORIO>
git branch -M main
git push -u origin main
```

---

## 🗺️ Páginas / Rotas

Todas as páginas obrigatórias da Sprint 2 foram migradas e mantidas, agora como uma **SPA** com `react-router-dom`:

| Rota | Página | Observação |
|---|---|---|
| `/` | Home | Hero, prévia de ranking, stats, "Como funciona", Impacto |
| `/sobre` | Sobre | Contexto SoulUp, problema, solução, roadmap |
| `/solucao` | Solução | Gatilhos psicológicos, mecanismo, cupons, benefícios |
| `/demo` | Demo interativa | Arena de cupons + ranking em tempo real (estado real) |
| `/faq` | FAQ | Acordeão por categoria |
| `/integrantes` | Equipe | Lista dos integrantes |
| `/integrantes/:id` | **Perfil do integrante** | **Rota dinâmica** (parâmetro `id`) |
| `/contato` | Contato | Formulário com React Hook Form |
| `*` | 404 | Página não encontrada |

---

## 🧠 Arquitetura de componentes

```
src/
├── assets/img/          # imagens dos integrantes (importadas como módulos)
├── components/
│   ├── ui/               # design system: Button, Badge, Card, Container,
│   │                     # SectionHeading, PageHero, Pill, GlowDivider,
│   │                     # StatHighlight, FormInput, FormSelect, FormTextarea
│   ├── layout/           # Navbar, Footer, Layout (Outlet do React Router)
│   ├── home/             # Hero, StatsBar, HowItWorks, ImpactSection, CtaBanner...
│   ├── sobre/            # AboutBlock, Roadmap, RoadmapItem
│   ├── solucao/          # TriggerCard, StepTimeline, CouponPreviewCard...
│   ├── demo/             # PlayerPanel, CouponArenaCard, Leaderboard, Toast
│   ├── faq/              # FaqAccordion, FaqAccordionItem
│   ├── integrantes/      # MemberCard
│   └── contato/          # ContactForm, ContactInfoList
├── pages/                # uma página por rota (compõem os componentes acima)
├── hooks/                # useNavbarScroll, useScrollToHash, useDemoGame
├── data/                 # conteúdo tipado (reaproveitado do site original)
├── types/                # interfaces TypeScript centrais do domínio
├── App.tsx               # definição das rotas
└── main.tsx              # entry point (BrowserRouter + StrictMode)
```

**Princípios seguidos:**
- Nenhuma página é uma "parede monolítica de JSX" — cada seção visual é um componente próprio, recebendo dados via **props tipadas**.
- Conteúdo (textos, listas, dados do jogo) fica separado em `src/data/*.ts`, nunca hardcoded dentro do JSX das páginas.
- Componentes de formulário (`FormInput`, `FormSelect`, `FormTextarea`) são genéricos e reutilizáveis com qualquer `register()` do React Hook Form.
- `PageHero`, `Container`, `Button`, `Card` e `SectionHeading` são reaproveitados em praticamente todas as páginas.

---

## 🪝 Hooks & estado (React)

| Hook | Onde | O que faz |
|---|---|---|
| `useState` | Navbar, FaqAccordion, `useDemoGame` | menu mobile, item aberto do FAQ, estado do jogo |
| `useEffect` | `useNavbarScroll`, `useScrollToHash`, `useDemoGame` | scroll da navbar, navegação fluida por âncora, `setInterval` com **cleanup** simulando outros players |
| `useMemo` | `useDemoGame` | ranking ordenado e progresso de nível, recalculados só quando necessário |
| `useCallback` | `useDemoGame` | handlers estáveis (`collectCoupon`, `addPoints`, `triggerToast`) |
| `useNavigate` | `MemberCard`, `MemberDetail` | navegação programática para o perfil e botão "Voltar" |
| `useParams` | `MemberDetail` | leitura do parâmetro dinâmico `:id` da rota |

A página **Demo** é o ponto alto da interatividade: todo o estado do "loot tracker" (pontos, inventário, cupons coletados, ranking ao vivo, toast) vive no hook `useDemoGame`, mantendo os componentes de apresentação (`PlayerPanel`, `CouponArenaCard`, `Leaderboard`) livres de lógica — eles só recebem dados e callbacks via props.

---

## ✅ Checklist — Critérios de Avaliação (Sprint 3)

- [x] **Bloco 1 — Conversão para React + Vite + TypeScript**: todas as páginas obrigatórias convertidas, SPA com `react-router-dom`, estrutura em `/src/components` e `/src/pages`, TypeScript em 100% dos arquivos.
- [x] **Bloco 2 — Componentização, modularidade e reutilização**: +50 componentes pequenos e reutilizáveis, divisão lógica por domínio, nomenclatura consistente.
- [x] **Bloco 3 — Hooks, props e navegação de dados**: `useState` e `useEffect` em múltiplos componentes, `useNavigate`/`useParams`, rotas estáticas e **rota dinâmica** (`/integrantes/:id`), props tipadas em todo lugar.
- [x] **Bloco 4 — Estilização e responsividade com Tailwind**: 100% Tailwind (sem CSS externo além do `@tailwind` base), responsivo em mobile (≤480px), tablet (768px) e desktop (992px+).
- [x] **Bloco 5 — Formulários com React Hook Form**: `useForm` tipado com `ContactFormValues`, validações (obrigatório, e-mail, mínimo de caracteres), mensagens de erro, estado de envio.

---

## 🔁 O que mudou em relação ao site HTML original

- Removida a dependência do **Google Translate / i18n** (fora do escopo desta sprint, focada em React/TS/Tailwind); o conteúdo permanece 100% em pt-BR.
- Scripts soltos (`main.js`, `faq.js`, `contato.js`, `troca.js`) foram substituídos por **hooks e estado React**.
- CSS customizado (`style.css`, `responsive.css`) foi substituído por **Tailwind CSS** utilitário.
- Todo o conteúdo textual (Sobre, Solução, FAQ, Integrantes) foi **preservado e reaproveitado**, apenas reorganizado em dados tipados (`src/data`).

---

## 👥 Equipe

| Nome | RM | Papel |
|---|---|---|
| Gabriel Augusto | 573120 | Front-End · UI/UX · Gamificação |
| Nycolas Escobar | 573052 | Produto & Estratégia |
| Rodrigo Banharelli | 570539 | Back-end |
| Tayna Jimenes | 569337 | UX Research · Conteúdo |

**Turma:** 1TDSPW · FIAP 2026
