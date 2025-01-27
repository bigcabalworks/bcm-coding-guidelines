**BCM Engineering Department Coding Guidelines**

---

### **1. Setup and Environment**

#### **Local Development Setup**

- **Contact the Team Lead**:
  - For setting up the local environment, reach out to the team lead. This ensures a consistent environment setup and avoids unnecessary misconfigurations.

**Why?** Because setting up your dev environment wrong is like assembling IKEA furniture without instructions—it’ll work... eventually, but not the way you want.

---

### **2. Deployment Process**

1. **Local Development**:
   - Build and test locally using the provided setup.
   - Use [Prettier](https://prettier.io/) and [ESLint](https://eslint.org/) to ensure code quality. (Your code should shine brighter than your future!)
   - Test all new features locally with various user scenarios.

2. **Development Server**:
   - Push your branch to the development repository.
   - Deploy changes to the dev environment using CI/CD pipelines. Automation for the win!
   - Test thoroughly on the dev setup. Don’t make the dev server cry.

3. **Staging and Production**:
   - Submit a PR to the `staging` branch after thorough local and dev testing.
   - Ensure all checkboxes in the PR template are checked and confirmed. Yes, all of them. If it says "Read the guide," actually read it.
   - Test on the staging server. Only approved changes are merged into the `main` branch for production.

**Remember**: Never deploy untested or unstable code to staging or production. A bug in production is like a bad haircut—it’s hard to hide and everyone notices.

---

### **3. Coding Standards and Best Practices**

#### **Mobile-First Development**
- Always design and code with mobile-first principles. Because your users don’t carry desktops in their pockets.
- Start with the smallest screen size and scale up using media queries:
  ```scss
  .example {
    font-size: 16px;

    @media (min-width: 768px) {
      font-size: 18px;
    }
  }
  ```

#### **Avoid Using jQuery**
- Use modern JavaScript and native browser APIs instead of relying on jQuery. (It’s 2025—jQuery is like carrying a flip phone in the age of smartphones.)
  ```javascript
  // Avoid:
  $('#element').addClass('active');

  // Prefer:
  document.querySelector('#element').classList.add('active');
  ```

#### **JavaScript Guidelines**
- Follow the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for consistent and maintainable code.
- Use descriptive names for variables and functions. Your future self will thank you.
  ```javascript
  // Avoid:
  let x = 10;

  // Prefer:
  let userCount = 10;
  ```
- Modularize code using ES Modules:
  ```javascript
  import calculateTotal from './utils/calculateTotal.js';

  const total = calculateTotal(cartItems);
  console.log(`Cart total: ${total}`);
  ```

#### **PHP Guidelines for WordPress**
- Adhere to the [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/) for consistent formatting and practices.
- Use WordPress core functions. The core team made them for a reason.
  ```php
  // Avoid:
  echo htmlspecialchars($user_input);

  // Prefer:
  echo esc_html($user_input);
  ```
- Always escape output:
  ```php
  function bcm_display_title($title) {
    return esc_html($title);
  }
  ```
- Use [WordPress VIP Coding Standards](https://github.com/Automattic/VIP-Coding-Standards) to ensure enterprise-grade security and performance.

#### **Multisite Considerations**
- Always test your code on a multisite setup when applicable.
- Use functions like `is_multisite()` and `get_sites()` to handle network-wide functionality:
  ```php
  if ( is_multisite() ) {
      $sites = get_sites();
      foreach ( $sites as $site ) {
          switch_to_blog( $site->blog_id );
          // Perform site-specific logic
          restore_current_blog();
      }
  }
  ```

**Golden Rule**: Your code should be clean enough that someone else can look at it without cursing your name.

---

### **4. Git Workflow and Version Control**

#### **Branching Strategy**
- Follow [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) with the following branch types:
  - `main`: Stable production-ready code.
  - `develop`: Latest development changes.
  - `feature/{dev-initials}-{feature-name}`: Individual feature branches. Example: `feature/sa-mobile-menu`.
  - `hotfix/{dev-initials}-{hotfix-name}`: Urgent fixes for production.

**Why dev initials?** So we can lovingly blame the right person when something breaks.

#### **Commit Messages**
- Format: `type(scope): description` (e.g., `feat(header): add mobile navigation`).
- Use the following types:
  - `feat`: New feature.
  - `fix`: Bug fix.
  - `chore`: Maintenance tasks.
  - `docs`: Documentation updates.
  - `refactor`: Code refactoring.
  - `test`: Testing-related updates.

#### **Pull Requests (PRs)**
- Ensure all PR template checkboxes are confirmed before submission.
- Include clear titles, linked issue numbers, and detailed descriptions.
- All PRs must be reviewed and approved before merging.

**Checklist Reminder**: Unchecked boxes are a sign of unchecked code. Don’t skip them.

---

### **5. Testing and Quality Assurance**

#### **JavaScript Testing**
- Write unit tests with Jest:
  ```javascript
  import addNumbers from './addNumbers';

  test('adds two numbers', () => {
    expect(addNumbers(2, 3)).toBe(5);
  });
  ```

#### **PHP Testing**
- Write PHPUnit tests for WordPress:
  ```php
  class BCM_Custom_Test extends WP_UnitTestCase {
    public function test_sample() {
      $this->assertTrue(true);
    }
  }
  ```

#### **Performance Optimization**
- Refer to [10up Engineering Best Practices](https://10up.github.io/Engineering-Best-Practices/) for performance tips.
- **Minimize DOM Manipulations**: Batch DOM updates to reduce layout thrashing.
- **Asynchronous Loading**: Use `async` or `defer` for scripts to avoid blocking rendering.
- **Image Optimization**: Use lazy loading and modern formats like WebP where possible.
- **Code Splitting**: Split JavaScript into smaller modules to improve loading times.

**Reminder**: Always test locally, on the dev server, and in staging to catch issues before they go live. Bugs love to hide in plain sight.

---

### **6. Accessibility**

- **ARIA Roles**:
  - Use ARIA attributes to enhance usability for assistive technologies.
  ```html
  <button aria-label="Close modal">&times;</button>
  ```
- **Focus Management**:
  - Ensure modal dialogs return focus to the trigger element after closing.
- **Color Contrast**:
  - Use tools like [Lighthouse](https://developers.google.com/web/tools/lighthouse) to ensure adequate contrast ratios for text and UI elements.
- **Keyboard Navigation**:
  - Ensure all interactive elements are accessible via keyboard and have focus styles.

**Remember**: Accessibility isn’t a feature; it’s a requirement. Build for everyone.

---

### **7. Internationalization (i18n)**

- Always use translation functions for strings:
  ```php
  echo esc_html__('Welcome to BCM Engineering!', 'bcm-textdomain');
  ```
- Include context for ambiguous strings using `_x()`:
  ```php
  _x('Post', 'noun', 'bcm-textdomain');
  ```
- Test translations thoroughly in multiple languages.

**Why it matters**: Your audience is global. Speak their language (literally).

---

### **8. Styling Guidelines**

#### **CSS/SCSS Best Practices**
- Use the BEM (Block Element Modifier) methodology for class naming.
  ```scss
  .button {
    &--primary {
      background-color: blue;
    }
  }
  ```
- Structure styles modularly (base, components, utilities).
- Use variables and mixins for consistency.
  ```scss
  $primary-color: #0073aa;

  .button {
    color: $primary-color;
  }
  ```

#### **Responsive Design**
- Use mobile-first breakpoints:
  ```scss
  .container {
    padding: 16px;

    @media (min-width: 768px) {
      padding: 24px;
    }
  }
  ```

---

### **References**

- [10up Engineering Best Practices](https://10up.github.io/Engineering-Best-Practices/)
- [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)
- [WordPress VIP Coding Standards](https://github.com/Automattic/VIP-Coding-Standards)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)

---

This guide is your loyal sidekick. It’s here to make your work easier, your code cleaner, and our team happier. So test everything, avoid jQuery like it’s an ex, and don’t forget—checkboxes in PR templates are sacred. Always keep this guide in mind. Happy coding!

