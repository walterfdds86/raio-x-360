# Quiz Estratégias Digitais — Diagnóstico Digital

**Data:** 2026-06-03  
**Arquivo de saída:** `quiz-ed.html`  
**Base:** `isca.html` adaptada (opção C — layout repensado para evento)

---

## Objetivo

Quiz de diagnóstico híbrido (marketing + IA) da Estratégias Digitais para ser usado em eventos presenciais. Os participantes escaneiam um QR code e respondem no celular. Resultado imediato personalizado + captura de lead + CTA para agendamento.

---

## Fluxo de Telas

1. **Intro** — Branding Estratégias Digitais, headline de impacto, "8 perguntas · 3 minutos · resultado imediato", botão "Iniciar Diagnóstico"
2. **Captura inicial** — Nome + Nome da Empresa (2 campos obrigatórios)
3. **8 Perguntas** — 4 dimensões × 2 perguntas, apresentadas uma por vez, escala 1-5
4. **Processando** — Tela animada "Analisando o perfil digital da [Empresa]..."
5. **Resultado** — Pontuação 0-100, faixa, gráfico de barras das 4 dimensões, fechamento personalizado, campo WhatsApp + botão CTA

---

## Dimensões e Perguntas

### 1. Presença Digital
- P1: Como você avalia a presença online da sua empresa? (site, redes sociais, Google)
- P2: Seu conteúdo digital gera engajamento e atrai potenciais clientes de forma consistente?

### 2. Atração de Clientes
- P3: Você tem uma estratégia clara e ativa de tráfego pago ou orgânico?
- P4: Seu funil de captação de leads gera novos contatos de forma previsível todo mês?

### 3. Conversão e Vendas
- P5: Seu processo de vendas está documentado e é seguido pela equipe?
- P6: Suas mensagens de vendas (copy) comunicam claramente o valor que você entrega?

### 4. Inteligência Artificial
- P7: Você utiliza ferramentas de IA no dia a dia da sua empresa?
- P8: Seus processos internos têm algum nível de automação implementado?

**Escala:** 1 (Quase nunca) → 5 (Sempre / Totalmente)  
**Score máximo interno:** 40 pontos (8 perguntas × 5)  
**Display:** normalizado para 0-100

---

## Faixas de Resultado

| Faixa | Score interno | Score display | Emoji | Título |
|-------|--------------|---------------|-------|--------|
| 1 | 0-16 | 0-40 | 🔴 | Negócio Analógico |
| 2 | 17-26 | 41-65 | 🟡 | Em Digitalização |
| 3 | 27-34 | 66-85 | 🟢 | Tração Digital |
| 4 | 35-40 | 86-100 | 🔵 | Máquina Digital |

**Fechamento personalizado por faixa** (usa `[Nome]` e `[Empresa]`):
- 🔴 Negócio Analógico: alerta sobre risco de ficar para trás, oportunidade imediata
- 🟡 Em Digitalização: reconhece progresso, aponta o gap entre esforço e resultado
- 🟢 Tração Digital: valida a base, aponta os pontos cegos que travam o próximo nível
- 🔵 Máquina Digital: valida a estrutura, foca em expansão estratégica com IA

---

## Captura de Lead

**Início:** Nome + Nome da Empresa  
**Resultado:** WhatsApp (campo + botão CTA)

**CTA (botão):** "Quero meu Diagnóstico Gratuito"  
**Ação:** Abre WhatsApp pré-preenchido com:  
> "Olá Walter, acabei de fazer o diagnóstico digital da [Empresa] e quero conversar sobre os resultados."  
**Número:** 61 99935-4363

---

## Google Sheets — Colunas

| # | Coluna | Valor |
|---|--------|-------|
| 1 | Data/Hora | timestamp automático |
| 2 | Nome | campo inicial |
| 3 | Empresa | campo inicial |
| 4 | WhatsApp | campo no resultado |
| 5 | Pontuação | 0-100 (display) |
| 6 | Faixa | título da faixa |
| 7 | Dimensão Mais Fraca | nome da dimensão |
| 8 | P1 - Presença Online | nota 1-5 |
| 9 | P2 - Conteúdo Digital | nota 1-5 |
| 10 | P3 - Tráfego | nota 1-5 |
| 11 | P4 - Captação de Leads | nota 1-5 |
| 12 | P5 - Processo de Vendas | nota 1-5 |
| 13 | P6 - Copy/Mensagem | nota 1-5 |
| 14 | P7 - Uso de IA | nota 1-5 |
| 15 | P8 - Automação | nota 1-5 |

**Planilha:** `https://docs.google.com/spreadsheets/d/1D1UaVxOnUhvfkEeneZQmtbiL4IKrNrku/edit`  
**Integração:** Google Apps Script (doGet com URLSearchParams, mode no-cors) — novo endpoint a criar

---

## Visual e Branding

**Paleta:**
- Fundo: `#0A0F1E` (azul-noite)
- Acento principal: `#00D4FF` (azul-elétrico / IA)
- Acento secundário: `#7B2FBE` (roxo / marketing)
- Gradiente CTAs: `#00D4FF → #7B2FBE`
- Cards/bordas: `#111827`
- Texto muted: `#8B9DB5`

**Fonte:** Inter (Google Fonts)

**Adaptações para evento:**
- Headline grande na intro — legível ao escanear QR code
- Gráfico de barras das 4 dimensões no resultado (visual, rápido de ler)
- Menos texto corrido, mais números e ícones
- Sem menu de navegação
- Sem referência ao Bruno Capanema em nenhum ponto

**Branding:**
- Nome: Estratégias Digitais
- Tagline: *Marketing com Inteligência Artificial*
- Consultor: Walter Ferreira (@_walterferreira_)

---

## Arquitetura Técnica

- HTML/CSS/JS puro (sem frameworks)
- Baseado na estrutura da `isca.html` com adaptações visuais
- `quiz-ed.html` — novo arquivo no mesmo repositório
- Deploy automático via Vercel (mesmo projeto `raio-x-360`)
- URL final: `raio-x-360.vercel.app/quiz-ed.html`
- Integração Sheets via Apps Script (novo endpoint para nova planilha)
- localStorage para evitar reenvio duplicado
