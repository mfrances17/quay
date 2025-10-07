# Agent React Development

## Overview

Comprehensive guide for React development and optimization in Quay frontend.

## For Agents

### Processing Priority

High - This document should be processed when working with React components or optimization.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for a complete list of all guidelines.

### Key Concepts

- React 18.2.0 patterns and best practices
- TypeScript integration
- State management with Recoil
- Performance optimization
- Component architecture

## React Architecture

### Current Stack
- **React**: 18.2.0 with modern patterns
- **TypeScript**: 5.8.3 for type safety
- **State Management**: Recoil for global state
- **Data Fetching**: React Query (@tanstack/react-query)
- **Routing**: React Router DOM 6.15.0

### Component Structure
```tsx
import React, { useState, useEffect } from 'react';
import { useRecoilState } from 'recoil';
import { useQuery } from '@tanstack/react-query';

interface ComponentProps {
  // Define props with proper types
  id: string;
  title: string;
  onAction?: () => void;
}

export const Component: React.FC<ComponentProps> = ({
  id,
  title,
  onAction
}) => {
  // Component implementation
};
```

## TypeScript Guidelines

### Interface Definitions
```tsx
// Always define proper interfaces
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'viewer';
}

// Use generic types when appropriate
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

// Never use 'any' type
// Bad: const data: any = response.data;
// Good: const data: User = response.data;
```

### Component Props
```tsx
// Define component props with proper types
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  children: React.ReactNode;
  onClick: () => void;
}

export const Button: React.FC<ButtonProps> = ({
  variant,
  size = 'md',
  disabled = false,
  children,
  onClick
}) => {
  // Implementation
};
```

### Event Handlers
```tsx
// Use proper event types
const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  setValue(event.target.value);
};

const handleFormSubmit = (event: React.FormEvent<HTMLFormElement>) => {
  event.preventDefault();
  // Handle submission
};

const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
  // Handle click
};
```

## State Management

### Recoil State
```tsx
import { atom, useRecoilState, useRecoilValue } from 'recoil';

// Define atoms
const userState = atom<User | null>({
  key: 'userState',
  default: null
});

const loadingState = atom<boolean>({
  key: 'loadingState',
  default: false
});

// Use in components
export const UserProfile: React.FC = () => {
  const [user, setUser] = useRecoilState(userState);
  const isLoading = useRecoilValue(loadingState);

  // Component logic
};
```

### Local State
```tsx
import { useState, useCallback } from 'react';

export const FormComponent: React.FC = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });

  const handleInputChange = useCallback((field: string, value: string) => {
    setFormData(prev => ({
      ...prev,
      [field]: value
    }));
  }, []);

  // Component logic
};
```

## Data Fetching

### React Query Integration
```tsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

// Query hook
const useUserData = (userId: string) => {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    enabled: !!userId
  });
};

// Mutation hook
const useUpdateUser = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: updateUser,
    onSuccess: (data) => {
      queryClient.invalidateQueries({ queryKey: ['user', data.id] });
    }
  });
};

// Usage in component
export const UserComponent: React.FC<{ userId: string }> = ({ userId }) => {
  const { data: user, isLoading, error } = useUserData(userId);
  const updateUserMutation = useUpdateUser();

  if (isLoading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;

  return (
    <div>
      {/* Render user data */}
    </div>
  );
};
```

## Forms with react-hook-form

### Form Implementation
```tsx
import { useForm, Controller } from 'react-hook-form';
import { TextInput, Select } from '@patternfly/react-core';

interface FormData {
  name: string;
  email: string;
  role: string;
}

export const UserForm: React.FC = () => {
  const {
    control,
    handleSubmit,
    formState: { errors, isSubmitting }
  } = useForm<FormData>({
    defaultValues: {
      name: '',
      email: '',
      role: 'user'
    }
  });

  const onSubmit = async (data: FormData) => {
    try {
      await createUser(data);
    } catch (error) {
      // Handle error
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="name"
        control={control}
        rules={{ required: 'Name is required' }}
        render={({ field }) => (
          <TextInput
            {...field}
            isRequired
            validated={errors.name ? 'error' : 'default'}
          />
        )}
      />

      <Controller
        name="role"
        control={control}
        render={({ field }) => (
          <Select
            {...field}
            isRequired
          >
            <SelectOption value="admin">Admin</SelectOption>
            <SelectOption value="user">User</SelectOption>
          </Select>
        )}
      />

      <Button type="submit" isDisabled={isSubmitting}>
        Submit
      </Button>
    </form>
  );
};
```

## Performance Optimization

### React.memo Usage
```tsx
import React, { memo } from 'react';

// Use memo for expensive components
export const ExpensiveComponent = memo<ComponentProps>(({ data }) => {
  // Expensive rendering logic
  return <div>{/* Complex rendering */}</div>;
});

// Use memo with custom comparison
export const CustomMemoComponent = memo<ComponentProps>(
  ({ data }) => {
    return <div>{data.name}</div>;
  },
  (prevProps, nextProps) => {
    return prevProps.data.id === nextProps.data.id;
  }
);
```

### useCallback and useMemo
```tsx
import { useCallback, useMemo } from 'react';

export const OptimizedComponent: React.FC<Props> = ({ items, onItemClick }) => {
  // Memoize expensive calculations
  const processedItems = useMemo(() => {
    return items.map(item => ({
      ...item,
      processed: true
    }));
  }, [items]);

  // Memoize event handlers
  const handleItemClick = useCallback((id: string) => {
    onItemClick(id);
  }, [onItemClick]);

  return (
    <div>
      {processedItems.map(item => (
        <ItemComponent
          key={item.id}
          item={item}
          onClick={handleItemClick}
        />
      ))}
    </div>
  );
};
```

### Lazy Loading
```tsx
import { lazy, Suspense } from 'react';
import { Spinner } from '@patternfly/react-core';

// Lazy load components
const LazyComponent = lazy(() => import('./LazyComponent'));

export const ParentComponent: React.FC = () => {
  return (
    <Suspense fallback={<Spinner />}>
      <LazyComponent />
    </Suspense>
  );
};
```

## Component Architecture

### Custom Hooks
```tsx
// Create reusable hooks
export const useUserData = (userId: string) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    const fetchUser = async () => {
      setLoading(true);
      try {
        const userData = await fetchUserById(userId);
        setUser(userData);
      } catch (err) {
        setError(err as Error);
      } finally {
        setLoading(false);
      }
    };

    if (userId) {
      fetchUser();
    }
  }, [userId]);

  return { user, loading, error };
};
```

### Higher-Order Components
```tsx
// Create HOCs for common functionality
export const withErrorBoundary = <P extends object>(
  Component: React.ComponentType<P>
) => {
  return (props: P) => (
    <ErrorBoundary>
      <Component {...props} />
    </ErrorBoundary>
  );
};
```

### Context Usage
```tsx
import { createContext, useContext, ReactNode } from 'react';

interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  const toggleTheme = useCallback(() => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  }, []);

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
};
```

## Testing Patterns

### Component Testing
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Component } from './Component';

describe('Component', () => {
  it('renders correctly', () => {
    render(<Component title="Test Title" />);
    expect(screen.getByText('Test Title')).toBeInTheDocument();
  });

  it('handles user interaction', async () => {
    const user = userEvent.setup();
    const mockOnClick = jest.fn();

    render(<Component onClick={mockOnClick} />);

    await user.click(screen.getByRole('button'));
    expect(mockOnClick).toHaveBeenCalledTimes(1);
  });
});
```

### Hook Testing
```tsx
import { renderHook, act } from '@testing-library/react';
import { useUserData } from './useUserData';

describe('useUserData', () => {
  it('fetches user data', async () => {
    const { result } = renderHook(() => useUserData('123'));

    expect(result.current.loading).toBe(true);

    await act(async () => {
      // Wait for async operations
    });

    expect(result.current.user).toBeDefined();
    expect(result.current.loading).toBe(false);
  });
});
```

## Best Practices

### 1. Component Design
- Use functional components with hooks
- Keep components focused and single-purpose
- Use proper TypeScript types
- Implement proper error boundaries

### 2. State Management
- Use Recoil for global state
- Use local state for component-specific data
- Use React Query for server state
- Avoid prop drilling

### 3. Performance
- Use React.memo for expensive components
- Implement proper memoization
- Use lazy loading for large components
- Optimize re-renders

### 4. Code Organization
- Group related functionality
- Use custom hooks for reusable logic
- Maintain consistent file structure
- Follow naming conventions

## References

- [React Documentation](https://react.dev)
- [TypeScript Documentation](https://www.typescriptlang.org/docs)
- [Recoil Documentation](https://recoiljs.org)
- [React Query Documentation](https://tanstack.com/query)
- [Guidelines Index](./README.md#guidelines-index)

Last updated: January 2025
