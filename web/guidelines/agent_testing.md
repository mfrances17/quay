# Agent Testing

## Overview

Comprehensive guide for testing procedures and standards in Quay frontend.

## For Agents

### Processing Priority

High - This document should be processed when working with tests or implementing changes that require testing.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for a complete list of all guidelines.

### Key Concepts

- Test structure and organization
- Test types and their purposes
- Component testing with React Testing Library
- E2E testing with Cypress
- Test coverage and quality

## Testing Architecture

### Test Stack
- **Unit Testing**: Jest + React Testing Library
- **E2E Testing**: Cypress
- **Test Utilities**: Custom test utilities and helpers
- **Coverage**: Jest coverage reporting

### Test Structure
```
web/
├── src/
│   ├── components/
│   │   └── __tests__/          # Component unit tests
│   ├── hooks/
│   │   └── __tests__/          # Hook unit tests
│   └── utils/
│       └── __tests__/          # Utility unit tests
├── cypress/
│   ├── e2e/                    # E2E test files
│   ├── fixtures/               # Test fixtures
│   └── support/                # Cypress support files
└── __tests__/                  # Global test utilities
```

## Unit Testing

### Component Testing
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from '@patternfly/react-core';
import { MyComponent } from './MyComponent';

describe('MyComponent', () => {
  it('renders correctly', () => {
    render(<MyComponent title="Test Title" />);
    expect(screen.getByText('Test Title')).toBeInTheDocument();
  });

  it('handles user interaction', async () => {
    const user = userEvent.setup();
    const mockOnClick = jest.fn();

    render(<MyComponent onClick={mockOnClick} />);

    await user.click(screen.getByRole('button'));
    expect(mockOnClick).toHaveBeenCalledTimes(1);
  });

  it('displays loading state', () => {
    render(<MyComponent loading={true} />);
    expect(screen.getByRole('progressbar')).toBeInTheDocument();
  });
});
```

### Hook Testing
```tsx
import { renderHook, act } from '@testing-library/react';
import { useUserData } from './useUserData';

describe('useUserData', () => {
  it('fetches user data successfully', async () => {
    const { result } = renderHook(() => useUserData('123'));

    expect(result.current.loading).toBe(true);

    await act(async () => {
      // Wait for async operations
    });

    expect(result.current.user).toBeDefined();
    expect(result.current.loading).toBe(false);
  });

  it('handles errors gracefully', async () => {
    const { result } = renderHook(() => useUserData('invalid'));

    await act(async () => {
      // Wait for async operations
    });

    expect(result.current.error).toBeDefined();
    expect(result.current.loading).toBe(false);
  });
});
```

### Form Testing
```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { UserForm } from './UserForm';

describe('UserForm', () => {
  it('submits form with valid data', async () => {
    const user = userEvent.setup();
    const mockOnSubmit = jest.fn();

    render(<UserForm onSubmit={mockOnSubmit} />);

    await user.type(screen.getByLabelText('Name'), 'John Doe');
    await user.type(screen.getByLabelText('Email'), 'john@example.com');
    await user.click(screen.getByRole('button', { name: 'Submit' }));

    expect(mockOnSubmit).toHaveBeenCalledWith({
      name: 'John Doe',
      email: 'john@example.com'
    });
  });

  it('shows validation errors', async () => {
    const user = userEvent.setup();

    render(<UserForm />);

    await user.click(screen.getByRole('button', { name: 'Submit' }));

    expect(screen.getByText('Name is required')).toBeInTheDocument();
    expect(screen.getByText('Email is required')).toBeInTheDocument();
  });
});
```

## Integration Testing

### API Integration
```tsx
import { render, screen, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { UserList } from './UserList';

const createTestQueryClient = () => new QueryClient({
  defaultOptions: {
    queries: { retry: false },
    mutations: { retry: false }
  }
});

describe('UserList Integration', () => {
  it('displays user list from API', async () => {
    const queryClient = createTestQueryClient();

    render(
      <QueryClientProvider client={queryClient}>
        <UserList />
      </QueryClientProvider>
    );

    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
    });
  });
});
```

### State Management Testing
```tsx
import { render, screen } from '@testing-library/react';
import { RecoilRoot } from 'recoil';
import { userState } from '../atoms/userState';
import { UserProfile } from './UserProfile';

describe('UserProfile with Recoil', () => {
  it('displays user data from Recoil state', () => {
    const user = { id: '1', name: 'John Doe', email: 'john@example.com' };

    render(
      <RecoilRoot initializeState={(snapshot) => snapshot.set(userState, user)}>
        <UserProfile />
      </RecoilRoot>
    );

    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });
});
```

## E2E Testing with Cypress

### Test Structure
```typescript
// cypress/e2e/user-management.cy.ts
describe('User Management', () => {
  beforeEach(() => {
    cy.visit('/users');
  });

  it('should create a new user', () => {
    cy.get('[data-testid="create-user-button"]').click();
    cy.get('[data-testid="user-name-input"]').type('John Doe');
    cy.get('[data-testid="user-email-input"]').type('john@example.com');
    cy.get('[data-testid="submit-button"]').click();

    cy.get('[data-testid="user-list"]').should('contain', 'John Doe');
  });

  it('should edit an existing user', () => {
    cy.get('[data-testid="user-row"]').first().click();
    cy.get('[data-testid="edit-button"]').click();
    cy.get('[data-testid="user-name-input"]').clear().type('Jane Doe');
    cy.get('[data-testid="save-button"]').click();

    cy.get('[data-testid="user-list"]').should('contain', 'Jane Doe');
  });
});
```

### API Mocking
```typescript
// cypress/e2e/api-integration.cy.ts
describe('API Integration', () => {
  it('should handle API responses correctly', () => {
    cy.intercept('GET', '/api/users', {
      statusCode: 200,
      body: [
        { id: '1', name: 'John Doe', email: 'john@example.com' }
      ]
    }).as('getUsers');

    cy.visit('/users');
    cy.wait('@getUsers');

    cy.get('[data-testid="user-list"]').should('contain', 'John Doe');
  });

  it('should handle API errors', () => {
    cy.intercept('GET', '/api/users', {
      statusCode: 500,
      body: { error: 'Internal Server Error' }
    }).as('getUsersError');

    cy.visit('/users');
    cy.wait('@getUsersError');

    cy.get('[data-testid="error-message"]').should('be.visible');
  });
});
```

### Custom Commands
```typescript
// cypress/support/commands.ts
declare global {
  namespace Cypress {
    interface Chainable {
      login(username: string, password: string): Chainable<void>;
      createUser(userData: UserData): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (username: string, password: string) => {
  cy.visit('/login');
  cy.get('[data-testid="username-input"]').type(username);
  cy.get('[data-testid="password-input"]').type(password);
  cy.get('[data-testid="login-button"]').click();
});

Cypress.Commands.add('createUser', (userData: UserData) => {
  cy.get('[data-testid="create-user-button"]').click();
  cy.get('[data-testid="user-name-input"]').type(userData.name);
  cy.get('[data-testid="user-email-input"]').type(userData.email);
  cy.get('[data-testid="submit-button"]').click();
});
```

## Test Utilities

### Custom Render Function
```tsx
// __tests__/test-utils.tsx
import { render, RenderOptions } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { RecoilRoot } from 'recoil';

const createTestQueryClient = () => new QueryClient({
  defaultOptions: {
    queries: { retry: false },
    mutations: { retry: false }
  }
});

interface CustomRenderOptions extends Omit<RenderOptions, 'wrapper'> {
  queryClient?: QueryClient;
  initialRecoilState?: any;
}

export const renderWithProviders = (
  ui: React.ReactElement,
  options: CustomRenderOptions = {}
) => {
  const { queryClient = createTestQueryClient(), initialRecoilState, ...renderOptions } = options;

  const Wrapper: React.FC<{ children: React.ReactNode }> = ({ children }) => (
    <QueryClientProvider client={queryClient}>
      <RecoilRoot initializeState={initialRecoilState}>
        {children}
      </RecoilRoot>
    </QueryClientProvider>
  );

  return render(ui, { wrapper: Wrapper, ...renderOptions });
};
```

### Mock Data
```tsx
// __tests__/mock-data.ts
export const mockUser = {
  id: '1',
  name: 'John Doe',
  email: 'john@example.com',
  role: 'admin'
};

export const mockUsers = [
  mockUser,
  {
    id: '2',
    name: 'Jane Smith',
    email: 'jane@example.com',
    role: 'user'
  }
];

export const mockApiResponse = {
  data: mockUsers,
  status: 200,
  message: 'Success'
};
```

## Test Coverage

### Coverage Configuration
```javascript
// jest.config.js
module.exports = {
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/index.tsx',
    '!src/reportWebVitals.ts'
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};
```

### Coverage Reporting
```bash
# Run tests with coverage
npm run test:coverage

# Generate coverage report
npm run test:coverage -- --coverageReporters=html
```

## Best Practices

### 1. Test Organization
- Group related tests in describe blocks
- Use descriptive test names
- Keep tests focused and atomic
- Follow AAA pattern (Arrange, Act, Assert)

### 2. Test Data
- Use consistent mock data
- Create reusable test utilities
- Avoid hardcoded values
- Use factories for complex data

### 3. Assertions
- Use specific assertions
- Test behavior, not implementation
- Include both positive and negative cases
- Test edge cases and error conditions

### 4. Performance
- Use async/await for async operations
- Avoid unnecessary waits
- Mock external dependencies
- Keep tests fast and reliable

## Trigger Workflows

### Trigger: "Write tests for React component"

1. **Analysis**
   - Identify component functionality
   - Plan test scenarios
   - Define test data requirements

2. **Implementation**
   - Create unit tests with React Testing Library
   - Add integration tests
   - Implement E2E tests with Cypress

3. **Validation**
   - Run test suite
   - Verify coverage
   - Check test quality

### Trigger: "Test API integration"

1. **Planning**
   - Identify API endpoints
   - Plan test scenarios
   - Define mock responses

2. **Implementation**
   - Create API integration tests
   - Add error handling tests
   - Implement E2E API tests

3. **Validation**
   - Run integration tests
   - Verify API behavior
   - Check error handling

## References

- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Cypress Documentation](https://docs.cypress.io)
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [Guidelines Index](./README.md#guidelines-index)

Last updated: January 2025
