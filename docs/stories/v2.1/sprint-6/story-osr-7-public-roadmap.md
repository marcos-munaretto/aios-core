# Story OSR-7: Public Roadmap para Comunidade

**Epic:** Open-Source Community Readiness (OSR)
**Story ID:** OSR-7
**Sprint:** 6
**Priority:** 🟡 Medium
**Points:** 5
**Effort:** 4 hours
**Status:** ⚪ Ready for Execution
**Type:** ✨ Enhancement

---

## 📋 User Story

**Como** membro da comunidade,
**Quero** ver o roadmap público do projeto,
**Para** entender a direção do projeto e planejar minhas contribuições.

---

## 🎯 Objetivo

Criar visibilidade do roadmap para a comunidade, mostrando o que está planejado sem comprometer detalhes proprietários ou estratégicos.

---

## 📊 Opções de Implementação

### Opção A: GitHub Projects (Recomendado)

**Prós:**
- Integrado nativamente com GitHub
- Fácil de manter (issues viram cards)
- Suporta diferentes views (board, table, roadmap)
- Gratuito

**Contras:**
- Requer manutenção de issues públicas
- Limitado em customização visual

**Implementação:**
1. Criar projeto público "AIOS Roadmap"
2. Configurar views: Board, Roadmap timeline
3. Usar labels para categorização
4. Linkar com issues/discussions

---

### Opção B: ROADMAP.md no Repositório

**Prós:**
- Simples de manter
- Versionado junto com código
- Fácil de contribuir

**Contras:**
- Menos visual
- Requer atualização manual
- Não interativo

---

### Opção C: Página Dedicada (docs/roadmap/)

**Prós:**
- Mais detalhado
- Pode incluir visualizações customizadas
- Separado do código

**Contras:**
- Mais trabalho de manutenção
- Pode ficar desatualizado

---

## ✅ Deliverables

### 1. GitHub Project "AIOS Public Roadmap"

**Configuração:**

| Campo | Valor |
|-------|-------|
| Nome | AIOS Public Roadmap |
| Visibilidade | Public |
| Descrição | What we're working on and planning |

**Views a criar:**

1. **Board View**
   - Colunas: Backlog | Next Up | In Progress | Done

2. **Roadmap View (Timeline)**
   - Agrupado por Quarter
   - Mostra datas estimadas

3. **Table View**
   - Filtros por área (Core, Agents, Expansion Packs)
   - Ordenação por prioridade

**Custom Fields:**

| Field | Type | Options |
|-------|------|---------|
| Quarter | Single Select | Q1 2026, Q2 2026, Q3 2026, Future |
| Area | Single Select | Core, Agents, Installer, Docs, Community |
| Size | Single Select | Small, Medium, Large, Epic |
| Status | Single Select | Exploring, Planned, In Progress, Done |

---

### 2. ROADMAP.md (Complementar)

**Arquivo:** `ROADMAP.md` na raiz

```markdown
# AIOS Roadmap

This document outlines the planned development direction for AIOS.

> 📌 For detailed tracking, see our [GitHub Project](link-to-project)

## Vision

AIOS aims to be the most comprehensive open-source AI agent framework,
enabling developers to build sophisticated AI-powered applications.

## Current Focus (Q1 2026)

### v2.1 Release
- ✅ Hybrid installer (npx + wizard)
- ✅ 4-module architecture
- ✅ Service Discovery
- 🔄 Quality Gates 3 layers
- 🔄 Template Engine
- ⏳ Open-source community infrastructure

### Community Building
- ⏳ GitHub Discussions setup
- ⏳ Contribution guides
- ⏳ Public roadmap (this!)

## Next Up (Q2 2026)

### v2.2 Planning
- Memory Layer implementation
- Enhanced agent capabilities
- Performance optimizations

### Community Features
- Expansion pack registry
- Contributor recognition system

## Future Exploration

These items are being explored but not yet committed:

- Multi-language support
- Cloud deployment options
- Visual workflow builder
- Plugin marketplace

## How to Influence the Roadmap

1. **Vote on Ideas**: React with 👍 on [Discussions](link)
2. **Propose Features**: Open an [Idea Discussion](link)
3. **Write an RFC**: For significant features, [submit an RFC](link)

## Changelog

For what's already shipped, see [CHANGELOG.md](CHANGELOG.md).

## Disclaimer

This roadmap represents our current plans and is subject to change.
Dates are estimates, not commitments.

---

*Last updated: YYYY-MM-DD*
```

---

### 3. Processo de Atualização

**Frequência:** Mensal (mínimo)

**Responsável:** @pm (Morgan) ou @po (Pax)

**Processo:**
1. Review do progresso atual
2. Atualizar status dos items
3. Adicionar novos items aprovados
4. Remover items cancelados
5. Comunicar mudanças significativas em Announcements

---

### 4. O que NÃO mostrar publicamente

**Excluir do roadmap público:**
- ❌ Detalhes de features proprietárias (Clones)
- ❌ Estratégia de monetização detalhada
- ❌ Expansion packs pagos específicos
- ❌ Timelines comprometedoras
- ❌ Informações competitivas sensíveis

**Manter vago/genérico:**
- Datas (usar quarters, não datas específicas)
- Detalhes técnicos internos
- Dependências de terceiros

---

### 5. Integração com Outros Docs

**Atualizar:**
- [ ] README.md - link para roadmap
- [ ] COMMUNITY.md - seção de roadmap
- [ ] CONTRIBUTING.md - menção ao roadmap

---

## 🎯 Acceptance Criteria

```gherkin
GIVEN a community member wanting to understand project direction
WHEN they access the public roadmap
THEN they can see:
  - Current focus areas
  - Planned features (quarters, not specific dates)
  - Status of major initiatives
  - How to influence the roadmap
AND they understand:
  - This is a plan, not a commitment
  - How to contribute to planned features
```

---

## 🔗 Dependencies

**Blocked by:**
- OSR-6: Features Process (features aprovadas vão pro roadmap)

**Blocks:**
- OSR-10: Release Checklist (roadmap precisa estar público)

---

## 📋 Definition of Done

- [ ] GitHub Project criado e público
- [ ] Views configuradas (Board, Roadmap, Table)
- [ ] Custom fields definidos
- [ ] ROADMAP.md criado
- [ ] Links em README, COMMUNITY.md
- [ ] Processo de atualização documentado
- [ ] Sem informações sensíveis expostas

---

**Criado por:** River (SM) 🌊
**Data:** 2025-12-05
