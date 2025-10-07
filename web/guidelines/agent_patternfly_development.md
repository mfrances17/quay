# Agent PatternFly Development

## Overview

Comprehensive guide for PatternFly development and migration in Quay frontend.

## For Agents

### Processing Priority

High - This document should be processed when working with PatternFly components or UI development.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for a complete list of all guidelines.

### Key Concepts

- PatternFly 5.x component usage
- Component composition patterns
- Styling and theming
- Accessibility requirements
- Migration strategies

## PatternFly Integration

### Current Version
- **PatternFly Core**: `@patternfly/patternfly: ^5.0.4`
- **React Components**: `@patternfly/react-core: ^5.1.0`
- **Icons**: `@patternfly/react-icons: ^5.1.0`
- **Tables**: `@patternfly/react-table: ^5.1.0`
- **Charts**: `@patternfly/react-charts: ^7.1.1`

### Import Patterns

#### Standard Import
```tsx
import {
  Button,
  Card,
  CardBody,
  CardHeader,
  CardTitle,
  Page,
  PageSection
} from '@patternfly/react-core';
```

#### Icon Import
```tsx
import { InfoCircleIcon, WarningTriangleIcon } from '@patternfly/react-icons';
```

#### Table Import
```tsx
import {
  Table,
  TableHeader,
  TableBody,
  TableRow,
  TableCell
} from '@patternfly/react-table';
```

## Component Usage Guidelines

### Layout Components

#### Page Structure
```tsx
import { Page, PageSection } from '@patternfly/react-core';

export const MyPage: React.FC = () => {
  return (
    <Page>
      <PageSection variant="light">
        {/* Page content */}
      </PageSection>
    </Page>
  );
};
```

#### Flex Layout
```tsx
import { Flex, FlexItem } from '@patternfly/react-core';

export const MyLayout: React.FC = () => {
  return (
    <Flex>
      <FlexItem>
        {/* Left content */}
      </FlexItem>
      <FlexItem>
        {/* Right content */}
      </FlexItem>
    </Flex>
  );
};
```

### Data Display Components

#### Cards
```tsx
import { Card, CardBody, CardHeader, CardTitle } from '@patternfly/react-core';

export const MyCard: React.FC = () => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Card Title</CardTitle>
      </CardHeader>
      <CardBody>
        {/* Card content */}
      </CardBody>
    </Card>
  );
};
```

#### Tables (Using QuayTable)
```tsx
import { QuayTable } from 'src/components/QuayTable';
import { usePaginatedSortableTable } from 'src/components/QuayTable';

export const MyTable: React.FC = () => {
  const tableConfig = usePaginatedSortableTable({
    // Table configuration
  });

  return (
    <QuayTable
      config={tableConfig}
      // Table props
    />
  );
};
```

### Form Components

#### Form Structure
```tsx
import { Form, FormGroup, FormHelperText } from '@patternfly/react-core';
import { useForm } from 'react-hook-form';

export const MyForm: React.FC = () => {
  const { register, handleSubmit } = useForm();

  return (
    <Form onSubmit={handleSubmit(onSubmit)}>
      <FormGroup label="Field Label" isRequired>
        {/* Form controls */}
        <FormHelperText>Helper text</FormHelperText>
      </FormGroup>
    </Form>
  );
};
```

#### Form Controls
```tsx
import { TextInput, Select, Checkbox } from '@patternfly/react-core';

// Text Input
<TextInput
  type="text"
  value={value}
  onChange={onChange}
  isRequired
/>

// Select
<Select
  value={selectedValue}
  onChange={onSelectChange}
  isRequired
>
  <SelectOption value="option1">Option 1</SelectOption>
</Select>

// Checkbox
<Checkbox
  label="Checkbox Label"
  isChecked={isChecked}
  onChange={onChange}
/>
```

### Interactive Components

#### Buttons
```tsx
import { Button, ButtonVariant } from '@patternfly/react-core';

<Button variant={ButtonVariant.primary}>
  Primary Action
</Button>

<Button variant={ButtonVariant.secondary}>
  Secondary Action
</Button>
```

#### Modals
```tsx
import { Modal, ModalVariant } from '@patternfly/react-core';

<Modal
  variant={ModalVariant.medium}
  title="Modal Title"
  isOpen={isOpen}
  onClose={onClose}
>
  {/* Modal content */}
</Modal>
```

## Styling Guidelines

### CSS Loading Order
```tsx
// Load App after patternfly so custom CSS that overrides patternfly doesn't require !important
import App from './App';
```

### Styling Approach
1. **Primary**: Use PatternFly default styling
2. **Customization**: Use PatternFly CSS variables and tokens
3. **Overrides**: Minimal custom CSS when necessary
4. **No Inline Styles**: Avoid inline styles in components

### CSS Variables Usage
```css
/* Use PatternFly variables */
.custom-component {
  color: var(--pf-global--Color--100);
  background-color: var(--pf-global--BackgroundColor--100);
  padding: var(--pf-global--spacer--md);
}
```

## Accessibility Guidelines

### ARIA Attributes
```tsx
// Use PatternFly's built-in accessibility features
<Button
  aria-label="Close dialog"
  aria-describedby="dialog-description"
>
  Close
</Button>
```

### Keyboard Navigation
```tsx
// PatternFly components include keyboard navigation by default
<Select
  onKeyDown={handleKeyDown}
  // Other props
>
  {/* Options */}
</Select>
```

### Screen Reader Support
```tsx
// Use proper semantic elements
<FormGroup
  label="Form Label"
  isRequired
  helperText="Additional information"
>
  {/* Form control */}
</FormGroup>
```

## Testing Patterns

### Component Testing
```tsx
import { render, screen } from '@testing-library/react';
import { Button } from '@patternfly/react-core';

test('renders button with correct text', () => {
  render(<Button>Test Button</Button>);
  expect(screen.getByRole('button')).toHaveTextContent('Test Button');
});
```

### Integration Testing
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('form submission works correctly', async () => {
  const user = userEvent.setup();
  render(<MyForm />);

  await user.type(screen.getByLabelText('Field Label'), 'test value');
  await user.click(screen.getByRole('button', { name: 'Submit' }));

  // Assertions
});
```

## Migration Strategies

### From Legacy Angular
1. **Reference Only**: Use `/static` for API endpoints and text content
2. **Component Mapping**: Map Angular components to PatternFly equivalents
3. **State Management**: Convert to React + Recoil patterns
4. **Styling**: Replace custom CSS with PatternFly components

### PatternFly Version Updates
1. **Breaking Changes**: Review changelog for breaking changes
2. **Component Updates**: Update component imports and props
3. **Styling Updates**: Adjust custom CSS for new design tokens
4. **Testing**: Update tests for new component behavior

## Common Patterns

### Error Handling
```tsx
import { Alert, AlertVariant } from '@patternfly/react-core';

{error && (
  <Alert
    variant={AlertVariant.danger}
    title="Error occurred"
    isInline
  >
    {error.message}
  </Alert>
)}
```

### Loading States
```tsx
import { Spinner } from '@patternfly/react-core';

{isLoading ? (
  <Spinner size="lg" />
) : (
  <div>Content loaded</div>
)}
```

### Empty States
```tsx
import { EmptyState, EmptyStateIcon, EmptyStateBody } from '@patternfly/react-core';
import { InfoCircleIcon } from '@patternfly/react-icons';

<EmptyState>
  <EmptyStateIcon icon={InfoCircleIcon} />
  <EmptyStateBody>
    No data available
  </EmptyStateBody>
</EmptyState>
```

## Best Practices

### 1. Component Composition
- Compose PatternFly components into custom components
- Maintain PatternFly's design system principles
- Extend rather than replace PatternFly components

### 2. Performance
- Use PatternFly's optimized components
- Implement React.memo for expensive components
- Use proper loading states

### 3. Consistency
- Follow established patterns in the codebase
- Use consistent import organization
- Maintain naming conventions

### 4. Documentation
- Add JSDoc comments to custom components
- Document component props and usage
- Provide usage examples

## References

- [PatternFly Documentation](https://v5-archive.patternfly.org)
- [PatternFly Components](https://v5-archive.patternfly.org/components/all-components)
- [React Documentation](https://react.dev)
- [Guidelines Index](./README.md#guidelines-index)

Last updated: January 2025
