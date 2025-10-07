# Agent Comments

## Overview

This document provides agent templates and standards for comments in Quay frontend code.

## For Agents

### Processing Priority

Medium - This document should be processed when working with code documentation or generating comments.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for a complete list of all guidelines.

### Key Concepts

- Comment templates for different purposes
- JSDoc conventions
- Code documentation standards
- Comment formatting and style

## Comment Templates

### File Header
```tsx
/**
 * [Component Name] Component
 *
 * [Brief description of what this component does]
 *
 * @component
 * @example
 * ```tsx
 * <ComponentName prop1="value1" prop2="value2" />
 * ```
 */
```

Example:
```tsx
/**
 * UserProfile Component
 *
 * Displays user information and allows editing of user details.
 *
 * @component
 * @example
 * ```tsx
 * <UserProfile userId="123" onUpdate={handleUpdate} />
 * ```
 */
```

### Component Documentation
```tsx
/**
 * [Component Name] component props and functionality
 *
 * Key features:
 * - **[Feature 1]**: [Description of what this does and why it matters]
 * - **[Feature 2]**: [Description with examples if helpful]
 * - **[Feature 3]**: [Description of another key feature]
 */
```

Example:
```tsx
/**
 * DataTable component props and functionality
 *
 * Key features:
 * - **Sorting**: Enables column-based sorting with visual indicators
 * - **Pagination**: Handles large datasets with configurable page sizes
 * - **Filtering**: Provides search and filter capabilities
 */
```

### Function Documentation
```tsx
/**
 * [Function description]
 *
 * @param {Type} param1 - [Parameter description]
 * @param {Type} param2 - [Parameter description]
 * @returns {Type} [Return value description]
 * @example
 * ```tsx
 * const result = functionName(param1, param2);
 * ```
 */
```

Example:
```tsx
/**
 * Formats user data for display
 *
 * @param {User} user - User object to format
 * @param {FormatOptions} options - Formatting options
 * @returns {FormattedUser} Formatted user data
 * @example
 * ```tsx
 * const formatted = formatUserData(user, { includeEmail: true });
 * ```
 */
```

### Hook Documentation
```tsx
/**
 * [Hook name] - [Brief description]
 *
 * [Detailed description of what the hook does]
 *
 * @param {Type} param - [Parameter description]
 * @returns {Object} [Return object description]
 * @example
 * ```tsx
 * const { data, loading, error } = useCustomHook(param);
 * ```
 */
```

Example:
```tsx
/**
 * useUserData - Fetches and manages user data
 *
 * Provides user data with loading states and error handling.
 * Automatically refetches data when dependencies change.
 *
 * @param {string} userId - User ID to fetch data for
 * @returns {Object} { user, loading, error, refetch }
 * @example
 * ```tsx
 * const { user, loading, error } = useUserData('123');
 * ```
 */
```

### Interface Documentation
```tsx
/**
 * [Interface description]
 *
 * @interface InterfaceName
 * @property {Type} property1 - [Property description]
 * @property {Type} property2 - [Property description]
 */
```

Example:
```tsx
/**
 * User data structure
 *
 * @interface User
 * @property {string} id - Unique user identifier
 * @property {string} name - User's display name
 * @property {string} email - User's email address
 * @property {'admin' | 'user' | 'viewer'} role - User's role
 */
```

### Section Comments
```tsx
// -------------------------------------------------------------------------
// [Section Name]
// -------------------------------------------------------------------------
```

Example:
```tsx
// -------------------------------------------------------------------------
// State Management
// -------------------------------------------------------------------------
```

### Inline Comments
```tsx
// [Brief explanation of what the code does]
// [Additional context if needed]
```

Example:
```tsx
// Format date for display in user's timezone
// Convert UTC timestamp to local time string
const formattedDate = new Date(timestamp).toLocaleString();
```

## Comment Standards

### Spacing Rules
1. **NO blank line** between comment and code
2. **ONE blank line** between different code sections
3. **Consistent indentation** with code

### Basic Comment:
```tsx
// This comment explains the following code
const value = calculateValue();

// This comment explains the next section
const processedValue = processValue(value);
```

### Multi-line Comment:
```tsx
// This is a longer comment that explains
// multiple lines of code or complex logic
// that requires more detailed explanation
const complexValue = performComplexCalculation();
```

### JSDoc Comment:
```tsx
/**
 * Calculates the total value based on input parameters
 *
 * @param {number} base - Base value for calculation
 * @param {number} multiplier - Multiplier factor
 * @returns {number} Calculated total value
 */
const calculateTotal = (base: number, multiplier: number): number => {
  return base * multiplier;
};
```

## Component Comment Examples

### React Component
```tsx
/**
 * UserCard Component
 *
 * Displays user information in a card format with optional actions.
 * Supports different variants and loading states.
 *
 * @component
 * @example
 * ```tsx
 * <UserCard
 *   user={userData}
 *   variant="compact"
 *   onEdit={handleEdit}
 * />
 * ```
 */
interface UserCardProps {
  /** User data to display */
  user: User;
  /** Card variant style */
  variant?: 'default' | 'compact' | 'detailed';
  /** Callback when edit button is clicked */
  onEdit?: (user: User) => void;
}

export const UserCard: React.FC<UserCardProps> = ({
  user,
  variant = 'default',
  onEdit
}) => {
  // -------------------------------------------------------------------------
  // State Management
  // -------------------------------------------------------------------------
  const [isEditing, setIsEditing] = useState(false);

  // -------------------------------------------------------------------------
  // Event Handlers
  // -------------------------------------------------------------------------
  const handleEditClick = useCallback(() => {
    setIsEditing(true);
    onEdit?.(user);
  }, [user, onEdit]);

  // -------------------------------------------------------------------------
  // Render
  // -------------------------------------------------------------------------
  return (
    <Card>
      {/* Card content */}
    </Card>
  );
};
```

### Custom Hook
```tsx
/**
 * useUserData - Fetches and manages user data
 *
 * Provides user data with loading states, error handling, and automatic
 * refetching when dependencies change. Uses React Query for caching.
 *
 * @param {string} userId - User ID to fetch data for
 * @returns {Object} Hook return object
 * @returns {User | null} returns.user - User data or null if not loaded
 * @returns {boolean} returns.loading - Loading state
 * @returns {Error | null} returns.error - Error state
 * @returns {Function} returns.refetch - Function to manually refetch data
 *
 * @example
 * ```tsx
 * const { user, loading, error, refetch } = useUserData('123');
 *
 * if (loading) return <Spinner />;
 * if (error) return <ErrorMessage error={error} />;
 *
 * return <UserProfile user={user} onRefresh={refetch} />;
 * ```
 */
export const useUserData = (userId: string) => {
  // -------------------------------------------------------------------------
  // React Query Hook
  // -------------------------------------------------------------------------
  const {
    data: user,
    isLoading: loading,
    error,
    refetch
  } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    enabled: !!userId
  });

  return { user, loading, error, refetch };
};
```

### Utility Function
```tsx
/**
 * Formats a date string for display in the user's locale
 *
 * Converts a UTC timestamp to a localized date string using the
 * user's browser settings. Handles various date formats and timezones.
 *
 * @param {string | Date} dateInput - Date to format (ISO string or Date object)
 * @param {FormatOptions} options - Formatting options
 * @param {boolean} options.includeTime - Whether to include time in output
 * @param {string} options.locale - Locale for formatting (defaults to browser locale)
 * @returns {string} Formatted date string
 *
 * @example
 * ```tsx
 * const formatted = formatDate('2023-12-25T10:30:00Z', {
 *   includeTime: true,
 *   locale: 'en-US'
 * });
 * // Returns: "12/25/2023, 10:30:00 AM"
 * ```
 */
export const formatDate = (
  dateInput: string | Date,
  options: FormatOptions = {}
): string => {
  const { includeTime = false, locale } = options;

  // Convert input to Date object if needed
  const date = typeof dateInput === 'string' ? new Date(dateInput) : dateInput;

  // Format options based on parameters
  const formatOptions: Intl.DateTimeFormatOptions = {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    ...(includeTime && {
      hour: '2-digit',
      minute: '2-digit',
      second: '2-digit'
    })
  };

  return date.toLocaleString(locale, formatOptions);
};
```

## Comment Conciseness Guidelines

When writing comments, follow these principles to keep them concise:

- Limit explanations to 1-3 lines when possible (examples can exceed this limit if necessary)
- Focus exclusively on what the code does and the factual reason why
- Avoid restating what the code already makes obvious
- Avoid subjective opinions about the code's benefits
- Include examples only when they clarify non-obvious behavior
- Use the least amount of comment lines needed to convey essential information

### Examples of Concise vs. Verbose Comments

**Too Verbose:**
```tsx
// This function takes a user object and formats it for display in the UI.
// It's important because it ensures consistent formatting across the application
// and makes the data more readable for users. The function handles various
// edge cases like missing data and different data types.
const formatUserForDisplay = (user: User) => {
  // Implementation
};
```

**Appropriately Concise:**
```tsx
// Formats user data for consistent UI display
const formatUserForDisplay = (user: User) => {
  // Implementation
};
```

**Even More Concise:**
```tsx
// Formats user data for display
const formatUserForDisplay = (user: User) => {
  // Implementation
};
```

## Best Practices

### 1. Comment Quality
- Write clear, focused comments
- Explain the "why" not just the "what"
- Keep comments up-to-date with code changes
- Use proper grammar and spelling

### 2. JSDoc Usage
- Use JSDoc for all public APIs
- Include parameter and return types
- Provide usage examples
- Document complex algorithms

### 3. Code Organization
- Group related functionality with section comments
- Use consistent comment formatting
- Maintain proper spacing and indentation
- Follow established patterns

### 4. Maintenance
- Review comments during code reviews
- Update comments when code changes
- Remove outdated comments
- Keep comments relevant and helpful

## References

- [JSDoc Documentation](https://jsdoc.app)
- [TypeScript JSDoc](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)
- [React Documentation](https://react.dev)
- [Guidelines Index](./README.md#guidelines-index)

Last updated: January 2025
