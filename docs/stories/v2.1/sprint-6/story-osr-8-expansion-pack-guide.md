# Story OSR-8: Expansion Pack Guide (Free)

**Epic:** Open-Source Community Readiness (OSR)
**Story ID:** OSR-8
**Sprint:** 6
**Priority:** 🟠 High
**Points:** 5
**Effort:** 4 hours
**Status:** ⚪ Ready for Execution
**Type:** 📝 Documentation

---

## 📋 User Story

**Como** desenvolvedor querendo estender AIOS,
**Quero** um guia completo para criar expansion packs,
**Para** contribuir com novos módulos sem precisar de suporte direto.

---

## 🎯 Objetivo

Criar documentação abrangente que permita à comunidade criar seus próprios expansion packs gratuitos, seguindo os padrões e arquitetura do AIOS.

---

## ✅ Deliverables

### 1. Guia Principal

**Arquivo:** `docs/guides/expansion-pack-guide.md`

```markdown
# Creating Expansion Packs for AIOS

This guide explains how to create and publish expansion packs for AIOS.

## What is an Expansion Pack?

Expansion packs extend AIOS functionality with:
- New agents with specialized capabilities
- Custom workflows for specific domains
- Task libraries for common operations
- Templates for document generation

## Quick Start

### Prerequisites
- Node.js 18+
- Familiarity with AIOS architecture
- Git for version control

### Create Your First Pack

```bash
# Clone the template
npx create-aios-expansion my-pack

# Navigate to directory
cd my-pack

# Install dependencies
npm install

# Start development
npm run dev
```

## Pack Structure

```
my-expansion-pack/
├── manifest.json          # Pack metadata
├── README.md              # Documentation
├── LICENSE                # License file
├── package.json           # npm configuration
├── src/
│   ├── agents/            # Agent definitions
│   │   └── my-agent.yaml
│   ├── tasks/             # Task workflows
│   │   └── my-task.yaml
│   ├── workflows/         # Multi-step workflows
│   │   └── my-workflow.yaml
│   └── templates/         # Templates
│       └── my-template.md
└── tests/                 # Test files
    └── my-agent.test.js
```

## Manifest File

The `manifest.json` defines your pack:

```json
{
  "name": "my-expansion-pack",
  "version": "1.0.0",
  "description": "Description of what this pack does",
  "author": "Your Name",
  "license": "MIT",
  "aios": {
    "minVersion": "2.1.0",
    "type": "expansion-pack"
  },
  "components": {
    "agents": ["src/agents/*.yaml"],
    "tasks": ["src/tasks/*.yaml"],
    "workflows": ["src/workflows/*.yaml"],
    "templates": ["src/templates/*.md"]
  },
  "dependencies": [],
  "keywords": ["aios", "expansion-pack", "your-domain"]
}
```

## Creating Agents

### Agent YAML Structure

```yaml
# src/agents/my-agent.yaml
name: my-agent
version: 1.0.0
description: What this agent does

persona:
  name: Agent Name
  role: Agent Role
  expertise:
    - Skill 1
    - Skill 2

capabilities:
  - capability-1
  - capability-2

commands:
  - name: my-command
    description: What this command does
    workflow: my-workflow

system_prompt: |
  You are [Agent Name], specialized in...

  Your responsibilities:
  - Responsibility 1
  - Responsibility 2
```

### Agent Best Practices
- Keep agents focused on specific domains
- Define clear boundaries and capabilities
- Include helpful error messages
- Document all commands

## Creating Tasks

### Task YAML Structure

```yaml
# src/tasks/my-task.yaml
name: my-task
version: 1.0.0
description: What this task does

inputs:
  - name: input1
    type: string
    required: true
    description: Description of input

outputs:
  - name: result
    type: object
    description: What the task returns

steps:
  - id: step-1
    action: validate-inputs
    description: Validate provided inputs

  - id: step-2
    action: process-data
    description: Process the data
    depends_on: [step-1]

  - id: step-3
    action: return-result
    description: Return the processed result
    depends_on: [step-2]
```

## Creating Workflows

### Workflow YAML Structure

```yaml
# src/workflows/my-workflow.yaml
name: my-workflow
version: 1.0.0
description: Multi-step workflow description

trigger:
  command: "*my-command"
  agent: my-agent

elicitation:
  enabled: true
  questions:
    - id: q1
      prompt: "What would you like to do?"
      type: choice
      options:
        - Option A
        - Option B

steps:
  - task: my-task
    inputs:
      input1: "{{elicitation.q1}}"
    on_success: next
    on_error: handle-error

error_handling:
  - id: handle-error
    action: notify
    message: "An error occurred"
```

## Testing Your Pack

### Unit Tests

```javascript
// tests/my-agent.test.js
import { loadAgent, executeCommand } from '@aios/testing';

describe('my-agent', () => {
  let agent;

  beforeAll(async () => {
    agent = await loadAgent('./src/agents/my-agent.yaml');
  });

  test('should execute my-command', async () => {
    const result = await executeCommand(agent, '*my-command', {
      input1: 'test'
    });
    expect(result.success).toBe(true);
  });
});
```

### Running Tests

```bash
npm test
```

## Publishing Your Pack

### To npm (Public)

```bash
# Login to npm
npm login

# Publish
npm publish
```

### To GitHub (Alternative)

1. Create repository
2. Add topics: `aios`, `expansion-pack`
3. Create release with semantic version

## Integration Guidelines

### Compatibility
- Test with minimum supported AIOS version
- Document any special requirements
- Handle missing dependencies gracefully

### Performance
- Keep agent definitions lightweight
- Optimize workflows for speed
- Cache when appropriate

### Security
- Never store credentials in code
- Use environment variables
- Validate all inputs

## Examples

### Example Packs (Reference)
- [aios-devops-pack](link) - DevOps workflows
- [aios-docs-pack](link) - Documentation generation
- [aios-testing-pack](link) - Testing utilities

## Getting Help

- GitHub Discussions: [link]
- Discord: [link]
- Issue Tracker: [link]

## Contributing Back

Found an issue or have an improvement?
- Submit bug reports
- Propose enhancements
- Share your pack with the community!
```

---

### 2. Template de Expansion Pack

**Criar repositório template ou arquivos:**

**Arquivo:** `templates/expansion-pack/manifest.json`

```json
{
  "name": "{{pack-name}}",
  "version": "0.1.0",
  "description": "{{description}}",
  "author": "{{author}}",
  "license": "MIT",
  "aios": {
    "minVersion": "2.1.0",
    "type": "expansion-pack"
  },
  "components": {
    "agents": ["src/agents/*.yaml"],
    "tasks": ["src/tasks/*.yaml"],
    "workflows": ["src/workflows/*.yaml"],
    "templates": ["src/templates/*.md"]
  },
  "dependencies": [],
  "keywords": ["aios", "expansion-pack"]
}
```

**Arquivo:** `templates/expansion-pack/package.json`

```json
{
  "name": "{{pack-name}}",
  "version": "0.1.0",
  "description": "{{description}}",
  "main": "index.js",
  "scripts": {
    "test": "jest",
    "lint": "eslint src/",
    "dev": "aios-dev-server"
  },
  "keywords": ["aios", "expansion-pack"],
  "author": "{{author}}",
  "license": "MIT",
  "devDependencies": {
    "@aios/testing": "^2.1.0",
    "jest": "^29.0.0",
    "eslint": "^8.0.0"
  }
}
```

**Arquivo:** `templates/expansion-pack/README.md`

```markdown
# {{Pack Name}}

{{Brief description of what this expansion pack does}}

## Installation

```bash
npm install {{pack-name}}
```

## Usage

```bash
# Activate the agent
@{{agent-name}}

# Use commands
*{{command-name}}
```

## Features

- Feature 1
- Feature 2
- Feature 3

## Documentation

See the [full documentation](docs/README.md) for detailed usage.

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT License - see [LICENSE](LICENSE)
```

---

### 3. Exemplos Práticos

**Criar diretório:** `docs/guides/expansion-pack-examples/`

**Arquivo:** `docs/guides/expansion-pack-examples/simple-agent.yaml`

```yaml
# Example: Simple Documentation Agent
name: docs-helper
version: 1.0.0
description: Helps with documentation tasks

persona:
  name: Docs Helper
  role: Documentation Assistant
  expertise:
    - Technical Writing
    - Markdown
    - API Documentation

commands:
  - name: generate-readme
    description: Generate README from code
    workflow: readme-generator

system_prompt: |
  You are Docs Helper, a documentation assistant.

  You help developers create and maintain documentation:
  - README files
  - API documentation
  - User guides

  Always use clear, concise language.
```

---

### 4. Integração com CONTRIBUTING.md

**Atualizar seção no CONTRIBUTING.md:**

```markdown
## Creating Expansion Packs

Want to extend AIOS with new functionality?

See our [Expansion Pack Guide](docs/guides/expansion-pack-guide.md) for:
- Pack structure and manifest format
- Creating agents, tasks, and workflows
- Testing and publishing your pack
- Integration guidelines

### Quick Links
- [Expansion Pack Template](templates/expansion-pack/)
- [Example Packs](docs/guides/expansion-pack-examples/)
- [Pack Registry](link-to-registry)
```

---

## 🎯 Acceptance Criteria

```gherkin
GIVEN a developer wanting to create an expansion pack
WHEN they read the documentation
THEN they can:
  - Understand the pack structure
  - Create a new pack from template
  - Define agents, tasks, and workflows
  - Test their pack locally
  - Publish their pack
AND the documentation includes:
  - Complete manifest reference
  - YAML schemas for all components
  - Working examples
  - Publishing instructions
```

---

## 🔗 Dependencies

**Blocked by:**
- OSR-5: COMMUNITY.md (referencia o guia)
- Architecture docs (para referência técnica)

**Blocks:**
- OSR-10: Release Checklist (precisa guia pronto)

---

## 📋 Definition of Done

- [ ] Guia principal criado em `docs/guides/`
- [ ] Template de expansion pack criado
- [ ] Exemplos práticos incluídos
- [ ] CONTRIBUTING.md atualizado com link
- [ ] README.md referencia o guia
- [ ] Testado com pack de exemplo
- [ ] Stakeholder revisou e aprovou

---

## 📎 Notas Técnicas

### Escopo MVP (v2.1)
- Apenas packs gratuitos
- Publicação via npm ou GitHub
- Sem registry centralizado (futuro)
- Sem verificação automática (futuro)

### Futuro (v2.2+)
- Registry centralizado de packs
- Verificação e badges de qualidade
- Marketplace visual
- Monetização para packs premium

---

**Criado por:** River (SM) 🌊
**Data:** 2025-12-05
