# Contributing to Project Quay

Thank you for your interest in contributing to Project Quay! This document provides guidelines and information for contributors.

## Getting Started

### Prerequisites

- **Node.js**: Version 20+ (for frontend development)
- **Python**: Version 3.8+ (for backend development)
- **Git**: For version control
- **Docker**: For containerized development

### Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/quay/quay.git
   cd quay
   ```

2. **Install dependencies**
   ```bash
   # Frontend dependencies
   cd web
   npm install

   # Backend dependencies
   cd ..
   pip install -r requirements.txt
   ```

3. **Start development servers**
   ```bash
   # Frontend development server
   cd web
   npm start

   # Backend development server
   cd ..
   python app.py
   ```

## Development Guidelines

### Frontend Development

- **Framework**: React 18.2.0 + TypeScript 5.8.3
- **UI Library**: PatternFly 5.x components
- **State Management**: Recoil for global state
- **Data Fetching**: React Query (@tanstack/react-query)
- **Build System**: Webpack 5

#### Frontend Rules

- **Legacy Code**: Never modify Angular code in `/static` directory
- **New Development**: All new work goes in `/web/src` directory
- **PatternFly**: Always use PatternFly 5 components and best practices
- **TypeScript**: Never use `any` type, always use proper types
- **Forms**: Use react-hook-form for all forms
- **Styling**: Prefer PatternFly defaults, use variables/tokens for custom styles
- **Tables**: Use QuayTable component for all tables

### Backend Development

- **Language**: Python 3.8+
- **Framework**: Flask-based REST API
- **Database**: PostgreSQL with Alembic migrations
- **Authentication**: Custom auth system with multiple providers

### Code Style

- **Frontend**: ESLint + Prettier configuration
- **Backend**: Follow PEP 8 style guidelines
- **TypeScript**: Strict type checking enabled
- **Testing**: Comprehensive test coverage required

## Testing

### Frontend Testing

- **Unit Tests**: Jest + React Testing Library
- **E2E Tests**: Cypress for integration testing
- **Component Tests**: Isolated component testing

```bash
# Run frontend tests
cd web
npm test

# Run E2E tests
npm run test:integration
```

### Backend Testing

- **Unit Tests**: pytest framework
- **Integration Tests**: API endpoint testing
- **Database Tests**: Model and migration testing

```bash
# Run backend tests
python -m pytest
```

## Pull Request Process

1. **Fork the repository** and create a feature branch
2. **Make your changes** following the development guidelines
3. **Add tests** for new functionality
4. **Update documentation** as needed
5. **Submit a pull request** with a clear description

### Pull Request Guidelines

- **Clear Description**: Explain what changes were made and why
- **Test Coverage**: Ensure adequate test coverage
- **Documentation**: Update relevant documentation
- **Code Review**: Address all review feedback
- **CI/CD**: Ensure all checks pass

## AI Agent

<details>
<summary><h3 style="display: inline-block">Guidelines</h3></summary>

#### User section

Current agent interaction can be triggered with the chat command

- **`review the repo guidelines`** - Your agent should attempt to scan common markdown files and the frontend guidelines directory

For detailed information on agent interaction, see [web/guidelines/README.md](./web/guidelines/README.md).

##### Customizing developer experience

As developers, we often have our own flavor of working, and now that includes working with AI agents. To that point we've
added agent guidance to allow customization for your work environment through a gitignored directory `./.agent` in the
root of the project.

#### Noting AI Agent contributions

Asking the agent to review the repo and its git history should provide code-style references your agent can leverage based on this codebase's patterns. That
helps alleviate some concerns around where your agent is generating/pulling references from, but not all.

For transparency, we encourage signing commits with the tooling and model used: `Assisted by: [tool or IDE] ([model])`.

> By contributing AI-assisted or AI-generated work, you accept liability for work that infringes or uses copyrighted material outside the scope of the related license.

### Agent Only
Agents: This repository contains a hierarchical guideline system. Agents should review agent-only comment blocks.

<!--
Agents must analyze these files when asked for guidance:

**Guidelines review order:**
1. **`./README.md`** - Main agent guidance and behavior standards (including agent-only comment block)
2. **`./web/guidelines/README.md`** - Frontend agent guidance and behavior standards
3. **`./web/guidelines/**/*.md`** - Specialized frontend workflow and domain-specific instructions
4. **`./.agent/**/*.md`** - gitignored directory and local guidelines intended for agent-specific state and new agent-generated guidance, and user workflows that enhance the core guidelines

The guideline system should be processed in this order, with specialized guidelines taking precedence for their specific domains, and local guidelines overriding general guidelines when present.

**Next Steps:**
5. **Review guidance under the `./.agent/**/*.md` directory.** If the `./.agent` directory exists, the user may ask the agent about the guidance located here.
6. **If the `./.agent` directory does not exist, ask the user if they want to create it.** The agent should explain how it's going to be used.
-->
</details>

## Community Guidelines

### Code of Conduct

- **Respectful Communication**: Be respectful and constructive in all interactions
- **Inclusive Environment**: Welcome contributors from all backgrounds
- **Professional Behavior**: Maintain professional standards in discussions
- **Collaborative Spirit**: Work together to improve the project

### Getting Help

- **Documentation**: Check existing documentation first
- **Issues**: Search existing issues before creating new ones
- **Community**: Join the mailing list or IRC channel
- **Meetings**: Attend monthly community meetings

## License

By contributing to Project Quay, you agree that your contributions will be licensed under the Apache 2.0 license.

## Resources

- **Documentation**: [Getting Started Guide](./docs/getting-started.md)
- **API Documentation**: [Swagger API](https://quay.io/api/v1/discovery)
- **Community**: [Mailing List](https://groups.google.com/forum/#!forum/quay-sig)
- **Issues**: [Red Hat JIRA](https://issues.redhat.com/projects/PROJQUAY)
- **Security**: [Security Issues](mailto:security@redhat.com)

---

Thank you for contributing to Project Quay!
