# React Best Practices


### Code Splitting
Code splitting is a technique used to optimize the performance of a React application by dividing the code into smaller chunks that can be loaded on demand. Implementing code splitting helps improve the initial load time of the app by only loading the necessary code upfront.

#### Implementation
- **React.lazy and Suspense**: Use `React.lazy()` and `Suspense` to dynamically load components as they are needed.
  
  ```jsx
  import React, { Suspense } from 'react';

  const HomePage = React.lazy(() => import('./views/HomePage'));
  const ProfilePage = React.lazy(() => import('./views/ProfilePage'));
  const FriendsPage = React.lazy(() => import('./views/FriendsPage'));

  function App() {
    return (
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route exact path="/" component={HomePage} />
          <Route path="/profile" component={ProfilePage} />
          <Route path="/friends" component={FriendsPage} />
        </Switch>
      </Suspense>
    );
  }

  export default App;

- Webpack and import(): Use webpack’s import() function to dynamically import code chunks at runtime.

### Linting
Linting involves automatically checking your code for errors and inconsistencies. It helps enforce coding standards and best practices.

Implementation
- ESLint Configuration:
  ```jsx

    {
        "extends": [
            "eslint:recommended",
            "plugin:react/recommended"
        ],
        "plugins": [
            "react"
        ],
        "parser": "babel-eslint",
        "parserOptions": {
            "ecmaVersion": 6,
            "sourceType": "module",
            "ecmaFeatures": {
            "jsx": true
            }
        },
        "rules": {
            "react/prop-types": "off"
        }
    }

### Unidirectional Codebase Architecture
Maintain a unidirectional flow in the codebase to ensure predictability and ease of understanding.

Implementation

- ESLint Configuration for Unidirectional Architecture:

  ```jsx

    'import/no-restricted-paths': [
    'error',
    {
        zones: [
        // Enforce unidirectional codebase
        {
            target: './src/features',
            from: './src/app',
        },
        {
            target: [
            './src/components',
            './src/hooks',
            './src/lib',
            './src/types',
            './src/utils',
            ],
            from: ['./src/features', './src/app'],
        },
        ],
    },
    ],

# Folder Structure
Organize the codebase into a feature-based structure for easy scalability and maintainability. Each feature folder should contain code specific to that feature.

### Root-Level Structure

  ```jsx

    src
    |
    +-- app               # Application layer containing:
    |   |
    |   +-- routes        # Application routes/pages
        +-- app.tsx       # Main application component
        +-- provider.tsx  # Application provider
        +-- router.tsx    # Router configuration
    +-- assets            # Static files (images, fonts, etc.)
    |
    +-- components        # Shared components
    |
    +-- config            # Global configurations
    |
    +-- features          # Feature-based modules
    |
    +-- hooks             # Shared hooks
    |
    +-- lib               # Reusable libraries
    |
    +-- stores            # Global state stores
    |
    +-- test              # Test utilities and mocks
    |
    +-- types             # Shared types
    |
    +-- utils             # Shared utility functions
  ```



### Feature-Based Structure
Each feature folder may contain:

  ```jsx
    src/features/awesome-feature
    |
    +-- api         # API requests and hooks for the feature
    |
    +-- assets      # Assets specific to the feature
    |
    +-- components  # Components specific to the feature
    |
    +-- hooks       # Hooks specific to the feature
    |
    +-- stores      # State stores for the feature
    |
    +-- types       # TypeScript types for the feature
    |
    +-- utils       # Utility functions for the feature
  ```

Notes
- Folder Necessity: Include only the folders that are necessary for each feature.
- Shared API Calls: For many shared API calls, consider using a dedicated api folder outside of the features folder.

### Import Restrictions
To enforce feature independence and prevent cross-feature imports, use ESLint configurations:
  ```jsx

    'import/no-restricted-paths': [
    'error',
    {
        zones: [
        // Disable cross-feature imports
        {
            target: './src/features/auth',
            from: './src/features',
            except: ['./auth'],
        },
        {
            target: './src/features/comments',
            from: './src/features',
            except: ['./comments'],
        },
        {
            target: './src/features/discussions',
            from: './src/features',
            except: ['./discussions'],
        },
        {
            target: './src/features/teams',
            from: './src/features',
            except: ['./teams'],
        },
        {
            target: './src/features/users',
            from: './src/features',
            except: ['./users'],
        },
        ],
    },
    ],
