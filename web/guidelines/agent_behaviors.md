# Agent Behaviors

## Overview

Comprehensive guide to agent behaviors, workflows, and standards for Quay frontend development.

## For Agents

### Processing Priority

Critical - Process first when working with the repository.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for all guidelines.

### Key Concepts

- Repository context and structure
- Core behavior standards
- Trigger-based workflows
- Decision-making principles
- Guidance authoring standards

## 1. Repository Context

Quay provides:
- **Container Registry**: Enterprise-grade container image management
- **Modern Frontend**: React + PatternFly 5.x UI
- **Legacy Support**: Angular reference code in `/static`
- **Developer Experience**: Comprehensive API and build system

**Core Directories**:
- `web/src/`: Modern React frontend application
- `static/`: Legacy Angular code (reference only)
- `endpoints/`: Python API endpoints
- `data/`: Database models and migrations
- `auth/`: Authentication and authorization
- `buildman/`: Build management system
- `storage/`: Storage abstraction layer
- `.agent/`: Local agent state (gitignored)

**Key Files**:
- `web/package.json`: Frontend dependencies and scripts
- `web/src/App.tsx`: Main React application
- `web/webpack.*.js`: Build configurations
- `README.md`, `CONTRIBUTING.md`: Documentation

Uses modern React patterns with:
- React 18.2.0 + TypeScript 5.8.3
- PatternFly 5.x components
- Recoil state management
- React Query for data fetching
- Webpack 5 build system

## 2. Core Behavior Standards

- **Sequential Processing**: Ask questions one at a time; process requests in logical order; complete one task before starting another
- **Reference-Based Implementation**: Review git history; study existing patterns; maintain code style consistency
- **Validation Required**: Follow checklists; verify requirements; test thoroughly; validate against standards
- **Confirmation Required**: Confirm success; summarize changes; explain impact; verify understanding
- **State Management**: Use `.agent/` directory; maintain context; preserve session information

## 3. Trigger-Based Workflows

### Trigger: "review the repo guidelines"

1. **Analysis**
   - Scan `web/guidelines/` directory
   - Review `.agent/` living documentation
   - Examine repository documentation
   - Generate comprehensive summary

2. **Documentation**
   - Update living documentation
   - Create implementation notes
   - Record discoveries and patterns

3. **Report**
   - Provide summary of guidelines
   - Highlight key patterns
   - Suggest improvements

### Trigger: "Implement a PatternFly component"

1. **Research**
   - Check PatternFly documentation
   - Review existing component usage
   - Identify similar implementations

2. **Design**
   - Plan component structure
   - Define props and interfaces
   - Consider accessibility requirements

3. **Implement**
   - Create component file
   - Add TypeScript interfaces
   - Implement PatternFly components
   - Add proper styling

4. **Test**
   - Create unit tests
   - Add integration tests
   - Verify accessibility

5. **Document**
   - Add JSDoc comments
   - Update component documentation
   - Provide usage examples

### Trigger: "Optimize React performance"

1. **Analysis**
   - Profile current performance
   - Identify bottlenecks
   - Review component structure

2. **Optimization**
   - Implement React.memo where appropriate
   - Optimize re-renders
   - Improve state management
   - Optimize bundle size

3. **Validation**
   - Measure performance improvements
   - Verify functionality
   - Test across browsers

### Trigger: "Write tests for React component"

1. **Planning**
   - Identify test scenarios
   - Plan test structure
   - Define test data

2. **Implementation**
   - Create unit tests with React Testing Library
   - Add integration tests
   - Implement E2E tests with Cypress

3. **Validation**
   - Run test suite
   - Verify coverage
   - Check test quality

## 4. Decision-Making Guidelines

1. **Legacy vs. Modern**
   - Never modify Angular code in `/static`
   - Always use React for new development
   - Reference legacy code for API endpoints

2. **PatternFly vs. Custom**
   - Prefer PatternFly components
   - Use custom components only when necessary
   - Maintain design system consistency

3. **TypeScript vs. JavaScript**
   - Always use TypeScript
   - Never use `any` type
   - Define proper interfaces

## 5. Validation Procedures

For all workflows:

1. **Testing**: Run appropriate tests, ensure passing, verify functionality
2. **Documentation**: Verify accuracy, consistency, and helpful examples
3. **Code Quality**: Follow patterns, check edge cases, ensure clear comments

## 6. Guidance Authoring Principles

1. **Clarity**: Be specific and unambiguous
   ```markdown
   When implementing a PatternFly component, include: proper imports, TypeScript interfaces, accessibility attributes
   ```

2. **Hierarchy**: Use clear section organization
   ```markdown
   ## Process
   1. Research
   2. Implement
   3. Test
   ```

3. **Context**: Provide rationale for recommendations
   ```markdown
   Use PatternFly components for consistency with Red Hat design system
   ```

4. **Machine-Actionable**: Structure for easy parsing
   ```markdown
   ### Template
   ```tsx
   import { Component } from '@patternfly/react-core';

   interface ComponentProps {
     // Define props
   }

   export const Component: React.FC<ComponentProps> = ({ ...props }) => {
     // Implementation
   };
   ```
   ```

## 7. Templates and Patterns

### Component Implementation

```markdown
# [Component] Implementation

## Structure
```tsx
import { Component } from '@patternfly/react-core';
import { useComponent } from 'src/hooks/useComponent';

interface ComponentProps {
  // Props definition
}

export const Component: React.FC<ComponentProps> = ({ ...props }) => {
  // Implementation
};
```

## Usage
```tsx
<Component prop1={value1} prop2={value2} />
```

## Testing
How to test this component.
```

### Workflow

```markdown
# [Workflow] Guidelines

## Process Steps
1. **[Step 1]**: Description
2. **[Step 2]**: Description

## Decision Table
| Scenario | Action |
|----------|--------|
| Case 1 | Action 1 |
| Case 2 | Action 2 |
```

## Date and Time Management

Run `$ date` to get system date before applying dates. Used for:
- Updating timestamps in documentation
- Adding creation dates
- Recording when changes were made

## References

- [Guidelines Index](./README.md#guidelines-index)
- [PatternFly Documentation](https://v5-archive.patternfly.org)
- [React Documentation](https://react.dev)

Last updated: January 2025
