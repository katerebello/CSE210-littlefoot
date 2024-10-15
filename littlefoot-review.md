# LittleFoot Code Review

# Working Demo -

<!-- place holder for adding demo video -->

# Package Architecture

- **TypeScript and Strong Typing**:

  - The package is written in TypeScript, which enhances maintainability and reduces errors through strong typing.
  - Example: `src/types.ts` defines interfaces like `Settings` and `Footnote`, improving type safety and ensuring proper usage of properties.

- **Event-Driven Architecture**:

  - Custom events are used for internal communication (e.g., in `src/dom/events.ts`), promoting loose coupling between components.
  - This approach allows different parts of the system to communicate without direct dependencies, leading to better flexibility and maintainability.

- **"Decisions, Not Options" Philosophy**:

  - Functions are implemented with default settings that can be changed, eliminating the need for users to manually enable optional features.
  - This design simplifies the user experience by providing sensible defaults.

- **Responsive Design with CSS**:

  - Littlefoot no longer relies on JavaScript for handling screen size breakpoints.
  - Instead, CSS-based media queries are used to adjust layouts for different screen sizes, improving performance and responsiveness.

- **Testability**:

  - Automated unit tests are performed using the `vitest` framework whenever a push event occurs.
  - These tests run on an Ubuntu runner, and all unit tests are stored in the `test/` folder with a `.test.ts` extension.

- **Progressive Enhancement**:
  - The library follows a progressive enhancement approach, ensuring that footnotes are accessible even when JavaScript is disabled.
  - This design improves accessibility and usability across various environments, including those with limited or disabled JavaScript support.

# Code Organization and Quality

- **Logical Module Organization**:

  - The code is well-organized into logical modules, each with a clear and distinct purpose, making the project easy to navigate and maintain.

- **Extensive Use of TypeScript Interfaces**:

  - The use of TypeScript interfaces and types throughout the code enhances reliability by enforcing strong typing and reducing potential errors.
  - Example: Types like `Settings` and `Footnote` are defined to ensure consistent handling of core components.

- **Descriptive Naming Conventions**:

  - File and function names are descriptive and follow a consistent pattern, improving readability and making it easier for developers to understand the codebase quickly.

- **Large Files**:

  - Some files, such as `littlefoot.ts`, are relatively long and could benefit from being broken down into smaller, more focused modules for improved maintainability.

- **Code Self-Documentation**:

  - The code is generally self-documenting, meaning it is written in a way that its purpose and functionality can be inferred from the structure and naming. However, adding more inline comments would help clarify complex logic, improving the learning curve for new contributors.

- **SVG Optimization**:

  - A notable change from Bigfoot to Littlefoot is the optimization of the footnote button. Littlefoot uses a single `<svg>` element with 3 circles instead of three separate `<svg>` elements, reducing redundancy and improving efficiency.

- **Security Practices**:

  - The team follows good security practices by performing static code analysis using GitHub's `codeql-analysis.yml`. This workflow is triggered when a new branch is pushed, scanning the code for vulnerabilities and ensuring a secure codebase.

- **Code Length Optimization**:

  - Some functions could be refactored to improve brevity and clarity.  
    Example:
    ```typescript
    dismiss(id, delay = settings.dismissDelay) {
        if (id === undefined) {
            useCases.dismissAll();
        } else {
            useCases.dismiss(id, delay);
        }
    }
    ```
    could be refactored to:
    ```typescript
    dismiss(id, delay = settings.dismissDelay) {
        useCases.dismiss(id === undefined ? undefined : id, delay);
    }
    ```

- **Lack of Comments**:
  - There are minimal comments explaining the individual parts of the code. While the code is generally easy to follow, adding more detailed comments would further improve code comprehension, especially for complex sections.

# Pattern and Language Use

## Factory Pattern

The main `littlefoot` function in `src/index.ts` acts as a factory for creating and configuring Littlefoot instances. This allows for the easy instantiation of the library with customizable settings.

```typescript
default function littlefoot(
  userSettings: Partial<Settings> = {}
): LittlefootInstance {
  const settings = { ...DEFAULT_SETTINGS, ...userSettings };
  return new Littlefoot(settings);
}
```

# Module Pattern

The code is organized into modules, each with a specific responsibility. This modular structure enhances maintainability and separates concerns for better clarity.

- **`src/dom.ts`**: Handles all DOM-related operations, such as manipulating elements, classes, and event listeners.
- **`src/footnotes.ts`**: Manages footnote-specific logic, including how footnotes are displayed and how interactions with them (like clicks) are handled.

## Language Use

In the `src/` folder of Littlefoot, the primary languages used are:

- **TypeScript**: The main language for writing the library's code, providing strong typing and enhancing code maintainability.
- **CSS**: Used for styling footnotes and references within the library, ensuring a consistent and accessible visual appearance.

## How It Works

- **HTML Structure**:  
  Littlefoot uses a specific HTML structure to identify footnotes. Footnotes are typically marked with a specific class or attribute, which Littlefoot scans to apply enhancements.

- **Initialization**:  
  After including the Littlefoot.js script in your project, initializing the library automatically scans the page for footnotes and enhances them, making them interactive and accessible to users.

# Repo Organization

- **Well-Structured README**:  
  The README file is well-organized and includes clear usage instructions, making it easy for users to understand how to use the library quickly and efficiently.

- **Clear Commit History**:  
  The commit history is clear, with meaningful commit messages that make it simple to track changes and understand project updates.

- **Semantic Versioning**:  
  The repository uses semantic versioning, which clearly indicates the type of changes (patch, minor, major) in each release. This helps manage dependencies and version control effectively.

- **Stylesheets**:  
  Unlike Bigfoot, which uses multiple stylesheets (likely `.scss` files), Littlefoot simplifies its styling with just one stylesheet.

- **Configuration Files**:  
  Configuration files are dispersed in the root directory, such as `postcss.config.js` and `commitlint.config.js`. Instead of organizing them into a unique config folder, these files are maintained at the root, which could be improved by having a more structured configuration folder.

- **Structured Directories**:  
  The repository is organized into distinct directories such as:

  - `src/`: Contains the source code for the library.
  - `dist/`: Contains the distribution files for the library.
  - `test/`: Contains unit tests to ensure code quality.
  - `cypress/`: Contains end-to-end tests to simulate user interactions and ensure overall functionality.

- **Key Configuration Files**:
  - **`package.json`**: Defines project dependencies, scripts, and metadata for managing the library.
  - **`.husky`**: Manages Git hooks for maintaining consistent development workflows.

# Quality / Tool Quality

- **End-to-End Testing with Cypress**:  
  Littlefoot.js utilizes the `/cypress` directory for end-to-end testing. These tests simulate real user interactions in a browser environment to validate the overall functionality of the application. This ensures that users experience the intended features seamlessly.

- **Unit Testing with Vitest**:  
  Additionally, the `/test` directory contains unit tests that focus on evaluating individual components in isolation. The library leverages **Vitest**, a JavaScript unit testing framework, for testing individual components and functionalities. This combination of testing strategies enhances the library's reliability and robustness, ensuring high-quality performance across different scenarios.

# Challenges

1. **Performance Issues with Many Footnotes**

   - **Why?**  
     Each footnote requires DOM manipulation, which can be costly when dealing with a large number of footnotes. Activating all footnotes at once can lead to significant memory usage, and rendering/layout calculations for many footnotes can slow down the page.
   - **How to Fix:**
     - Implement lazy loading of footnote content.
     - Use virtual scrolling for a large number of footnotes.
     - Optimize DOM manipulations.

2. **Integration with Modern JavaScript Libraries**

   - **Why?**  
     Littlefoot.js is framework-agnostic and operates directly on the DOM. However, modern frameworks often use a virtual DOM or have their own rendering cycles, making lifecycle management between the library and framework components complex.
   - **How to Fix:**
     - Create wrapper components or custom hooks for each major framework (e.g., React, Vue, Angular).
     - Provide clear documentation on integration methods.

3. **Extensive Customization Options**

   - **Why?**  
     The library offers a large number of configuration options to cater to various use cases. Some options may have interdependencies or non-obvious effects, and there is a lack of clear examples or use-case-based documentation.
   - **How to Fix:**
     - Provide sensible defaults.
     - Create preset configurations for common use cases.
     - Improve documentation with examples and use cases.

4. **Changes Needed for Cross-Platform Build in `package.json`**

   - **macOS:**

     ```json
     "clean": "rm -rf coverage dist",
     "build": "npm run clean && mkdir -p dist && concurrently 'npm run build:types' 'npm run build:scripts' 'npm run build:styles'"
     ```

   - **Windows:**
     ```json
     "clean": "rimraf coverage dist",
     "build": "npm run clean && npx mkdirp dist && concurrently \"npm run build:types\" \"npm run build:scripts\" \"npm run build:styles\""
     ```
