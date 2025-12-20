# Technical Debt Audit: FencingTogether vs FFC Single Page Template

**Date:** December 19, 2025  
**Auditor:** GitHub Copilot  
**Scope:** Comparative analysis between FencingTogether/website (Jekyll) and FreeForCharity/FFC_Single_Page_Template (Next.js)

## Executive Summary

This audit compares the FencingTogether website (a Jekyll-based static site) with the FFC Single Page Template (a modern Next.js 16/TypeScript/Tailwind CSS application). The analysis reveals significant architectural divergence, with the FencingTogether site using legacy Ruby/Jekyll tooling while the template represents a modern JavaScript/TypeScript ecosystem approach.

**Key Finding:** The FencingTogether repository represents substantial technical debt with a legacy Jekyll stack that is significantly behind modern web development practices embodied in the FFC Template.

### Priority Summary
- 🔴 **Critical Issues:** 8 items requiring immediate attention
- 🟡 **High Priority:** 9 items requiring near-term resolution (includes 2 noted as duplicates/subsets)
- 🟢 **Medium Priority:** 6 items for planned improvement
- ⚪ **Low Priority:** 3 items for long-term consideration

**Total:** 26 issues identified

---

## 1. Framework & Architecture Analysis

### Current State: FencingTogether (Jekyll)

**Technology Stack:**
- **Framework:** Jekyll (Ruby-based static site generator)
- **Language:** HTML, Liquid templates, SCSS
- **Styling:** Custom SCSS with CSS variables
- **Build Tool:** Jekyll via GitHub Pages gem
- **Ruby Version:** 3.2 (from workflow)
- **Theme:** Minima theme

**Architecture:**
```
FencingTogether/
├── _config.yml           # Jekyll configuration
├── _layouts/             # HTML templates
│   └── default.html      # Single layout file
├── _includes/            # Reusable components
│   ├── header.html       # 32 lines with inline JS
│   └── footer.html       # 66 lines with Liquid templating
├── assets/
│   ├── css/
│   │   └── style.scss    # 613 lines of custom SCSS
│   └── images/
├── index.md              # 207 lines of content + HTML
└── Gemfile               # Ruby dependencies
```

### Target State: FFC Template (Next.js)

**Technology Stack:**
- **Framework:** Next.js 16.0.7 with App Router
- **Language:** TypeScript (strict mode)
- **Styling:** Tailwind CSS 3.x
- **Build Tool:** Next.js with Turbopack
- **Node Version:** 20.x
- **Package Manager:** npm with lock file

**Architecture:**
```
FFC_Template/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── page.tsx            # Homepage (TypeScript)
│   │   ├── layout.tsx          # Root layout with metadata
│   │   ├── globals.css         # Tailwind + custom styles
│   │   ├── [policy-pages]/     # Multiple route pages
│   │   ├── sitemap.ts          # Dynamic sitemap
│   │   └── robots.ts           # SEO configuration
│   ├── components/             # 20+ organized folders
│   │   ├── header/             # TypeScript components
│   │   ├── footer/
│   │   ├── ui/                 # Reusable UI components
│   │   └── [feature-folders]/  # Domain-specific components
│   ├── data/                   # Structured data files
│   │   ├── faqs/
│   │   ├── team/
│   │   └── testimonials/
│   └── lib/                    # Utility functions
│       └── assetPath.ts        # GitHub Pages helper
├── tests/                      # Playwright E2E tests
├── next.config.ts              # TypeScript config
├── tsconfig.json               # TypeScript settings
├── eslint.config.mjs           # ESLint configuration
├── tailwind.config.ts          # Tailwind settings
└── package.json                # npm dependencies
```

### 🔴 Critical Findings

1. **Architectural Gap: Jekyll vs Next.js**
   - **Issue:** Complete framework divergence
   - **Impact:** Cannot share components, build processes, or development practices
   - **Risk:** FencingTogether cannot leverage FFC Template improvements
   - **Effort:** High (complete rewrite required)

2. **No TypeScript Usage**
   - **Issue:** FencingTogether uses only HTML/Liquid/SCSS, no type safety
   - **Impact:** Higher error rates, no IDE assistance, harder refactoring
   - **Risk:** Maintenance complexity grows with codebase size
   - **Effort:** High (requires complete migration)

3. **Missing Component Architecture**
   - **Issue:** Flat file structure with no component reusability
   - **Impact:** Code duplication, harder to maintain consistency
   - **Risk:** Scaling issues as site grows
   - **Effort:** Medium-High

---

## 2. Dependencies & Package Management

### FencingTogether Dependencies

**Gemfile (Ruby):**
```ruby
gem "github-pages", group: :jekyll_plugins  # Includes Jekyll + plugins
gem "jekyll-feed"
gem "jekyll-seo-tag"
```

**Issues:**
- ❌ No explicit Jekyll version pinning
- ❌ Relies on `github-pages` gem meta-package (opaque versioning)
- ❌ No dependency lock file visible in repository
- ❌ Ruby ecosystem knowledge required

### FFC Template Dependencies

**package.json (npm):**
```json
{
  "dependencies": {
    "next": "16.0.7",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "typescript": "^5.0.0",
    "tailwindcss": "^3.0.0"
  },
  "devDependencies": {
    "@playwright/test": "^1.48.2",
    "eslint": "^9.0.0",
    // ... and more
  }
}
```

**Strengths:**
- ✅ Explicit version pinning
- ✅ package-lock.json for reproducible builds
- ✅ Automated security scanning (Dependabot, CodeQL)
- ✅ Modern npm ecosystem

### 🔴 Critical Findings

1. **No Package Lock File Committed**
   - **Issue:** Gemfile.lock not in repository
   - **Impact:** Non-reproducible builds, version drift
   - **Risk:** "Works on my machine" problems
   - **Recommendation:** Commit Gemfile.lock
   - **Effort:** Low

2. **Opaque Dependency Management**
   - **Issue:** `github-pages` gem hides actual versions
   - **Impact:** Unclear what Jekyll version is running
   - **Risk:** Breaking changes without notice
   - **Effort:** Medium (requires explicit gem specification)

### 🟡 High Priority Findings

3. **No Automated Dependency Updates**
   - **Issue:** No Dependabot configuration for Ruby gems
   - **Impact:** Security vulnerabilities may go unnoticed
   - **Risk:** Outdated dependencies accumulate
   - **Comparison:** FFC Template has comprehensive Dependabot config
   - **Effort:** Low (add dependabot.yml for bundler)

---

## 3. Build Process & Deployment

### FencingTogether Build Process

**Workflow:** `.github/workflows/jekyll.yml`
```yaml
- Setup Ruby 3.2
- bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
- Deploy to GitHub Pages
```

**Characteristics:**
- ✅ Simple, GitHub-native deployment
- ✅ Automatic GitHub Pages integration
- ⚠️ Ruby 3.2 required (not documented in repo)
- ⚠️ No local build instructions in repository
- ❌ No preview/staging process
- ❌ No build artifact caching
- ❌ No performance testing

**Build Time:** Unknown (not documented)

### FFC Template Build Process

**Workflow:** `.github/workflows/deploy.yml`
```yaml
- Setup Node.js 20
- npm ci (clean install)
- NEXT_PUBLIC_BASE_PATH=/FFC_Single_Page_Template
- npm run build (~30 seconds)
- npm test (Playwright tests)
- Deploy static files from ./out
```

**Characteristics:**
- ✅ Documented build times (~30 seconds)
- ✅ Automated testing before deployment
- ✅ Explicit environment variable handling
- ✅ Local development instructions
- ✅ Preview server (`npm run preview`)
- ✅ Multiple CI workflows (ci.yml, lighthouse.yml, codeql.yml)

### 🟡 High Priority Findings

1. **Missing README/Build Documentation**
   - **Issue:** No README.md in FencingTogether repository
   - **Impact:** New contributors cannot set up local environment
   - **Risk:** Onboarding friction, inconsistent development
   - **Comparison:** FFC Template has comprehensive 400+ line README
   - **Effort:** Medium (need to document Jekyll setup process)

2. **No Local Development Instructions**
   - **Issue:** No documented way to run site locally
   - **Impact:** Contributors can't test changes before pushing
   - **Risk:** Broken deployments, slow iteration
   - **Comparison:** FFC Template: `npm run dev` (1 second startup)
   - **Effort:** Low (add to README)

3. **No Testing Infrastructure**
   - **Issue:** No automated tests
   - **Impact:** Manual testing required, regression risks
   - **Comparison:** FFC Template has Playwright E2E tests
   - **Effort:** Medium-High

### 🟢 Medium Priority Findings

4. **No Build Performance Monitoring**
   - **Issue:** No Lighthouse or performance CI
   - **Comparison:** FFC Template has lighthouse.yml workflow
   - **Effort:** Low (add workflow)

---

## 4. Code Organization & File Structure

### FencingTogether Structure

**Layout:**
- Flat structure: 9 files/folders at root
- 2 include files (header, footer)
- 1 layout file (default.html)
- All content in single index.md (207 lines)
- Monolithic SCSS file (613 lines)

**Issues:**
- ⚠️ No separation of concerns
- ⚠️ All content mixed in one file
- ⚠️ No reusable components beyond header/footer
- ⚠️ Inline JavaScript in HTML templates

### FFC Template Structure

**Layout:**
- Organized by feature: 20+ component folders
- Separation: app/ (routes), components/ (UI), data/ (content), lib/ (utils)
- Multiple pages: Homepage + 7 policy pages
- Small, focused files (average ~100 lines)
- Utility-first CSS with Tailwind

**Strengths:**
- ✅ Clear separation of concerns
- ✅ Reusable component library
- ✅ Data separated from presentation
- ✅ Scalable structure

### 🟡 High Priority Findings

1. **No Component Reusability**
   - **Issue:** Monolithic index.md with inline HTML sections
   - **Impact:** Cannot reuse program cards, board members, etc.
   - **Risk:** Code duplication if site expands
   - **Effort:** Medium (requires architectural changes)

2. **Mixed Concerns in index.md**
   - **Issue:** 207 lines mixing Markdown, HTML, and Liquid
   - **Impact:** Hard to maintain, edit, or test sections independently
   - **Comparison:** FFC Template separates into component files
   - **Effort:** Medium

### 🟢 Medium Priority Findings

3. **Monolithic CSS File**
   - **Issue:** 613-line SCSS file with all styles
   - **Impact:** Hard to navigate, potential for conflicts
   - **Comparison:** FFC Template uses Tailwind + component-scoped styles
   - **Effort:** Medium (refactor to modular CSS or Tailwind)

4. **Inline JavaScript**
   - **Issue:** Mobile menu JS embedded in header.html (lines 25-31)
   - **Impact:** Not testable, no separation of concerns
   - **Effort:** Low (extract to separate file)

---

## 5. Styling Methodology

### FencingTogether: Custom SCSS

**Approach:**
- CSS Custom Properties (variables)
- BEM-like class naming
- Manual responsive breakpoints
- 613 lines of hand-written CSS

**Strengths:**
- ✅ Clean, semantic class names
- ✅ CSS variables for theming
- ✅ Responsive design implemented

**Weaknesses:**
- ⚠️ Requires CSS expertise to maintain
- ⚠️ No design system or constraints
- ⚠️ Easy to introduce inconsistencies
- ⚠️ All styles must be written manually

### FFC Template: Tailwind CSS

**Approach:**
- Utility-first CSS framework
- JIT (Just-In-Time) compilation
- Design tokens built-in
- Automatic purging of unused CSS

**Strengths:**
- ✅ Rapid development
- ✅ Consistent design system
- ✅ Tiny production CSS bundle
- ✅ No CSS naming conflicts
- ✅ Responsive utilities built-in

### 🟢 Medium Priority Findings

1. **Manual CSS Maintenance**
   - **Issue:** All styles hand-written and maintained
   - **Impact:** Slower development, consistency challenges
   - **Comparison:** Tailwind provides instant utility classes
   - **Effort:** High (requires learning curve and migration)

2. **No Design System**
   - **Issue:** Color values, spacing hardcoded in CSS
   - **Impact:** Inconsistent spacing/colors across site
   - **Comparison:** Tailwind enforces design tokens
   - **Effort:** Medium (establish design system)

---

## 6. SEO & Metadata

### FencingTogether SEO

**Implementation:**
```html
<!-- _layouts/default.html -->
<meta name="description" content="{{ site.description }}">
<title>{% if page.title %}{{ page.title }} | {% endif %}{{ site.title }}</title>
{% seo %}
{% feed_meta %}
```

**Plugins:**
- jekyll-seo-tag
- jekyll-feed (RSS)

**Issues:**
- ⚠️ Basic SEO via plugin
- ⚠️ No custom metadata per page
- ❌ No sitemap.xml
- ❌ No robots.txt
- ❌ No structured data (JSON-LD)

### FFC Template SEO

**Implementation:**
- Comprehensive metadata object in layout.tsx
- Dynamic sitemap.ts generation
- robots.ts configuration
- Multiple pages with individual metadata
- Open Graph and Twitter cards

**Strengths:**
- ✅ Complete SEO setup
- ✅ Dynamic sitemap
- ✅ Per-page metadata customization
- ✅ Social media optimization

### 🟡 High Priority Findings

1. **Missing Sitemap**
   - **Issue:** No sitemap.xml generated
   - **Impact:** Search engines may not discover all pages
   - **Comparison:** FFC Template has dynamic sitemap generation
   - **Effort:** Low (Jekyll can generate with plugin)

2. **Missing robots.txt**
   - **Issue:** No crawler instructions
   - **Impact:** Suboptimal crawling behavior
   - **Effort:** Low (add static file)

3. **Basic SEO Implementation**
   - **Issue:** Relies entirely on jekyll-seo-tag defaults
   - **Impact:** Limited control over metadata
   - **Comparison:** FFC Template has fine-grained control
   - **Effort:** Medium

---

## 7. Developer Experience & Tooling

### FencingTogether Tooling

**Available:**
- Jekyll build system
- GitHub Actions deployment
- CodeQL security scanning

**Missing:**
- ❌ No linting (Ruby, HTML, CSS)
- ❌ No code formatting (Prettier, RuboCop)
- ❌ No type checking
- ❌ No testing framework
- ❌ No development server instructions
- ❌ No hot reload documentation

### FFC Template Tooling

**Available:**
- ESLint with Next.js rules
- TypeScript strict mode
- Playwright testing framework
- Hot reload with Turbopack (~1 sec)
- npm scripts for all tasks
- Comprehensive documentation

**Strengths:**
- ✅ Fast development server
- ✅ Instant feedback on errors
- ✅ Automated testing
- ✅ Consistent code formatting

### 🔴 Critical Findings

1. **No Linting or Code Quality Tools**
   - **Issue:** No automated code quality checks
   - **Impact:** Inconsistent code style, potential errors
   - **Risk:** Technical debt accumulates
   - **Comparison:** FFC Template has ESLint + TypeScript
   - **Effort:** Medium (set up RuboCop, HTMLProofer, Stylelint)

### 🟡 High Priority Findings

2. **No Testing Infrastructure**
   - **Issue:** No automated tests
   - **Impact:** Regressions not caught, manual testing required
   - **Comparison:** FFC Template has Playwright E2E tests
   - **Effort:** High (set up testing framework)

3. **Undocumented Development Workflow**
   - **Issue:** No instructions for local development
   - **Impact:** Contributor friction
   - **Comparison:** FFC Template has step-by-step guide
   - **Effort:** Low (document in README)

---

## 8. JavaScript & Interactivity

### FencingTogether JavaScript

**Current State:**
```html
<!-- Inline in _includes/header.html -->
<script>
  document.querySelector('.mobile-menu-btn').addEventListener('click', function() {
    document.querySelector('.nav-links').classList.toggle('active');
    this.classList.toggle('active');
  });
</script>
```

**Issues:**
- ⚠️ 7 lines of vanilla JS inline in HTML
- ⚠️ No build process for JS
- ⚠️ No modules or organization
- ⚠️ No testing possible

### FFC Template JavaScript/TypeScript

**Implementation:**
- TypeScript throughout
- React components with hooks
- Client-side state management
- Global popup system (PopupProvider)
- Modular, testable code

**Strengths:**
- ✅ Type-safe JavaScript
- ✅ Component-based architecture
- ✅ Testable units
- ✅ Modern ES6+ features

### 🟢 Medium Priority Findings

1. **Inline JavaScript**
   - **Issue:** JS embedded in HTML templates
   - **Impact:** Not testable, not minified
   - **Effort:** Low (extract to separate file)

2. **No JavaScript Build Process**
   - **Issue:** No transpilation, bundling, or minification
   - **Impact:** Limited to basic JS features
   - **Comparison:** FFC Template uses modern build pipeline
   - **Effort:** Medium-High (requires framework change)

---

## 9. CI/CD & Automation

### FencingTogether CI/CD

**Workflows:**
1. `jekyll.yml` - Build and deploy to GitHub Pages
2. `codeql.yml` - Security scanning (JavaScript/TypeScript + Actions)

**Issues:**
- ⚠️ CodeQL configured for JavaScript/TypeScript but site has none
- ⚠️ No testing in CI
- ⚠️ No lint checks
- ⚠️ No performance monitoring
- ❌ No preview deployments

### FFC Template CI/CD

**Workflows:**
1. `ci.yml` - Run tests and lint
2. `deploy.yml` - Build, test, deploy
3. `codeql.yml` - Security scanning
4. `lighthouse.yml` - Performance monitoring

**Strengths:**
- ✅ Multi-stage CI pipeline
- ✅ Testing before deployment
- ✅ Performance tracking
- ✅ Security scanning

### 🟡 High Priority Findings

1. **CodeQL Misconfiguration**
   - **Issue:** CodeQL scanning for JavaScript/TypeScript but site has none
   - **Impact:** Wasted CI resources, confusing security alerts
   - **Risk:** False sense of security
   - **Effort:** Low (update codeql.yml to scan relevant languages)

2. **No CI Quality Gates**
   - **Issue:** Deployment happens without testing
   - **Impact:** Broken code can reach production
   - **Comparison:** FFC Template tests before deploy
   - **Effort:** Medium (add test step)

### 🟢 Medium Priority Findings

3. **No Performance Monitoring**
   - **Issue:** No Lighthouse CI or similar
   - **Impact:** Performance regressions unnoticed
   - **Effort:** Low (add lighthouse workflow)

---

## 10. Community Health & Documentation

### FencingTogether Community Files

**Present:**
- ✅ `.github/CODEOWNERS`
- ✅ `.github/FUNDING.yml`
- ✅ `.github/PULL_REQUEST_TEMPLATE.md`
- ✅ `.github/ISSUE_TEMPLATE/` (4 templates + config)
- ✅ `.github/dependabot.yml` (configured for npm only)
- ✅ `.github/copilot-instructions.md` (references FFC Template)

**Missing:**
- ❌ README.md
- ❌ LICENSE
- ❌ CODE_OF_CONDUCT.md
- ❌ CONTRIBUTING.md
- ❌ SECURITY.md
- ❌ SUPPORT.md
- ❌ CHANGELOG.md
- ❌ CITATION.cff

### FFC Template Community Files

**Complete Set:**
- ✅ All files listed above present
- ✅ Comprehensive README (400+ lines)
- ✅ Detailed contribution guidelines
- ✅ Security policy with disclosure process
- ✅ Academic citation support

### 🔴 Critical Findings

1. **Missing README.md**
   - **Issue:** No project documentation
   - **Impact:** Cannot onboard contributors, unclear purpose
   - **Risk:** Abandoned appearance, low contribution
   - **Effort:** Medium (write comprehensive README)

2. **No LICENSE File**
   - **Issue:** Unclear legal status of code
   - **Impact:** Cannot legally use or contribute
   - **Risk:** Legal ambiguity
   - **Effort:** Low (add appropriate license)

### 🟡 High Priority Findings

3. **Missing Standard Community Files**
   - **Issue:** No CODE_OF_CONDUCT, CONTRIBUTING, SECURITY
   - **Impact:** Unclear contribution process, no security disclosure
   - **Comparison:** FFC Template has complete set
   - **Effort:** Low-Medium (copy and adapt from template)

4. **Dependabot Configured for npm**
   - **Issue:** dependabot.yml references npm but site uses Ruby/Jekyll
   - **Impact:** No dependency updates happen
   - **Risk:** Outdated dependencies accumulate
   - **Effort:** Low (change to bundler ecosystem)

### ⚪ Low Priority Findings

5. **No CHANGELOG.md**
   - **Issue:** No version history
   - **Impact:** Unclear what changed between updates
   - **Effort:** Low (start maintaining changelog)

---

## 11. Asset Management & Performance

### FencingTogether Assets

**Structure:**
```
assets/
├── css/
│   └── style.scss
├── images/
│   └── logo.png (referenced but may not exist)
└── favicon.ico (referenced in layout)
```

**Issues:**
- ⚠️ Google Fonts loaded from CDN (network dependency)
- ⚠️ No image optimization
- ⚠️ No asset versioning/cache busting
- ❌ No basePath handling for deployment flexibility

### FFC Template Assets

**Structure:**
```
public/
├── icons/
├── images/
└── [various assets]

src/lib/assetPath.ts  # Helper for dual deployment
```

**Features:**
- ✅ assetPath() helper for GitHub Pages/custom domain
- ✅ Next.js automatic asset optimization
- ✅ Configurable basePath support
- ✅ Environment-aware asset loading

### 🟢 Medium Priority Findings

1. **No Asset Path Flexibility**
   - **Issue:** Hardcoded asset paths
   - **Impact:** Difficult to move to different hosting
   - **Comparison:** FFC Template has assetPath() helper
   - **Effort:** Low (can use Jekyll's relative_url filter)

2. **No Image Optimization**
   - **Issue:** Images served as-is
   - **Impact:** Slower page loads, wasted bandwidth
   - **Comparison:** Next.js optimizes images automatically
   - **Effort:** Medium (use imageOptim or similar)

---

## 12. Security Considerations

### FencingTogether Security

**Current:**
- ✅ CodeQL scanning (though misconfigured)
- ✅ GitHub Actions security
- ⚠️ Dependabot configured but for wrong ecosystem
- ❌ No SECURITY.md
- ❌ No dependency lock file

### FFC Template Security

**Implementation:**
- ✅ CodeQL properly configured
- ✅ Dependabot for npm and GitHub Actions
- ✅ SECURITY.md with disclosure process
- ✅ package-lock.json committed
- ✅ Security Acknowledgements page

### 🔴 Critical Findings

1. **No Security Disclosure Process**
   - **Issue:** No SECURITY.md file
   - **Impact:** Unclear how to report vulnerabilities
   - **Risk:** Security issues handled improperly
   - **Effort:** Low (create SECURITY.md)

### 🟡 High Priority Findings

2. **Dependabot Ecosystem Mismatch**
   - **Issue:** Configured for npm but uses bundler
   - **Impact:** No automated security updates
   - **Risk:** Vulnerable dependencies unpatched
   - **Effort:** Low (fix dependabot.yml)

---

## 13. Naming Conventions & Standards

### FencingTogether Conventions

**Observed:**
- Kebab-case for files: `_config.yml`, `style.scss`
- Lowercase HTML templates
- CSS classes: BEM-like naming

**Issues:**
- ⚠️ No documented standards
- ⚠️ Inconsistent Liquid variable usage

### FFC Template Conventions

**Standards:**
- **CRITICAL RULE:** All folders MUST use kebab-case
- TypeScript files: PascalCase for components
- Utility files: camelCase
- Routes: kebab-case (SEO requirement)

**Documentation:**
- ✅ Explicit naming rules in README
- ✅ SEO rationale explained
- ✅ Accessibility reasoning provided

### 🟢 Medium Priority Findings

1. **No Documented Naming Conventions**
   - **Issue:** Conventions not written down
   - **Impact:** Inconsistent contributions
   - **Comparison:** FFC Template has detailed guidelines
   - **Effort:** Low (document current practices)

---

## Summary of Findings by Priority

### 🔴 Critical (Immediate Attention Required)

1. **Architectural Gap**: Jekyll vs Next.js complete divergence
2. **No TypeScript**: Missing type safety and modern tooling
3. **Missing Component Architecture**: Flat structure limits scalability
4. **No Package Lock File**: Non-reproducible builds
5. **Missing README.md**: No documentation for contributors
6. **No LICENSE File**: Legal ambiguity
7. **No Linting**: No code quality automation
8. **No Security Disclosure Process**: Missing SECURITY.md

### 🟡 High Priority (Near-Term Resolution)

1. **Opaque Dependency Management**: github-pages gem hides versions
2. **No Automated Dependency Updates**: Dependabot misconfigured
3. **No Build Documentation**: Missing local setup instructions
4. **No Testing Infrastructure**: No automated tests
5. **No Component Reusability**: Monolithic index.md
6. **Missing Sitemap**: SEO impact
7. **CodeQL Misconfiguration**: Scanning wrong languages
8. **Missing Community Files**: CODE_OF_CONDUCT, CONTRIBUTING, SECURITY

### 🟢 Medium Priority (Planned Improvement)

1. **No Build Performance Monitoring**: No Lighthouse CI
2. **Monolithic CSS**: 613-line SCSS file
3. **Inline JavaScript**: Not testable or maintainable
4. **Manual CSS Maintenance**: No design system
5. **Basic SEO Implementation**: Limited control
6. **No Asset Optimization**: Images not optimized

### ⚪ Low Priority (Long-Term Consideration)

1. **No CHANGELOG.md**: Version history not tracked
2. **Google Fonts CDN**: Network dependency
3. **No Documentation Standards**: Naming conventions not written

---

## Recommendations & Action Plan

### Option 1: Migrate to Next.js (Recommended)

**Rationale:** Achieve full alignment with FFC Template and modern practices.

**Benefits:**
- ✅ Share components and patterns with FFC Template
- ✅ Gain TypeScript, testing, and modern tooling
- ✅ Unified development experience across FFC projects
- ✅ Better performance and SEO
- ✅ Easier onboarding for developers

**Effort:** High (2-4 weeks)
**Risk:** Medium (requires complete rewrite)

**Migration Steps:**
1. Set up Next.js project from FFC Template
2. Port content from index.md to React components
3. Convert SCSS to Tailwind CSS classes
4. Implement testing for key flows
5. Set up CI/CD pipeline
6. Deploy and verify

### Option 2: Modernize Jekyll (Band-Aid)

**Rationale:** Quick fixes while deferring major architectural changes.

**Benefits:**
- ✅ Lower immediate effort
- ✅ Familiar technology stack
- ✅ No rewrite required

**Drawbacks:**
- ❌ Still diverges from FFC Template
- ❌ Limited long-term scalability
- ❌ Technical debt remains

**Effort:** Low-Medium (1-2 weeks)
**Risk:** Low

**Improvement Steps:**
1. Create README.md with setup instructions
2. Add LICENSE, CODE_OF_CONDUCT, CONTRIBUTING, SECURITY.md
3. Fix Dependabot configuration for bundler
4. Add sitemap.xml and robots.txt
5. Extract inline JS to separate file
6. Add basic testing with HTMLProofer
7. Document naming conventions
8. Add linting (RuboCop, Stylelint)

### Option 3: Hybrid Approach

**Rationale:** Improve Jekyll site while planning gradual migration.

**Phase 1 (Immediate - 1 week):**
- Add missing community health files
- Fix Dependabot and CodeQL configurations
- Create comprehensive README
- Add sitemap and robots.txt

**Phase 2 (Near-Term - 2-3 weeks):**
- Set up linting and testing
- Refactor index.md into reusable includes
- Add development documentation
- Improve CI/CD pipeline

**Phase 3 (Long-Term - TBD):**
- Plan Next.js migration
- Build component library in parallel
- Gradual feature migration
- Final cutover to Next.js

---

## Cost-Benefit Analysis

### Staying with Jekyll

**Costs:**
- Ongoing divergence from FFC Template
- Cannot share components or practices
- Limited tooling and testing
- Ruby/Jekyll expertise required
- Slower development velocity
- Higher maintenance burden

**Benefits:**
- No immediate migration effort
- Familiar to current maintainers
- GitHub Pages native support
- Simple deployment

### Migrating to Next.js

**Costs:**
- 2-4 weeks initial migration effort
- Learning curve for new stack
- Requires Node.js expertise
- More complex build process

**Benefits:**
- Full alignment with FFC Template
- Modern tooling (TypeScript, testing, linting)
- Component reusability across projects
- Better performance and SEO
- Larger talent pool (React vs Jekyll)
- Future-proof technology choice
- Shared documentation and practices

---

## Specific Remediation Issues to Create

Based on this audit, the following individual issues should be created:

### Immediate (Critical)

1. **[Critical] Add README.md with Local Development Instructions**
   - Document Jekyll setup, Ruby requirements, local server
   - Include build and deployment instructions
   - Reference: FFC Template README structure

2. **[Critical] Add Missing Community Health Files**
   - Add LICENSE (choose appropriate license)
   - Add CODE_OF_CONDUCT.md
   - Add CONTRIBUTING.md
   - Add SECURITY.md with vulnerability disclosure process
   - Reference: FFC Template community files

3. **[Critical] Fix Dependabot Configuration**
   - Change from npm to bundler ecosystem
   - Add Bundler to dependabot.yml
   - Test that PRs are created for gem updates

4. **[Critical] Commit Gemfile.lock for Reproducible Builds**
   - Generate and commit Gemfile.lock
   - Document Ruby version requirement
   - Ensure CI uses locked dependencies

5. **[Critical] Fix CodeQL Configuration**
   - Remove JavaScript/TypeScript scanning
   - Add Ruby scanning if available
   - Update matrix to reflect actual codebase

### Near-Term (High Priority)

6. **[High] Add Sitemap Generation**
   - Enable jekyll-sitemap plugin
   - Verify sitemap.xml generation
   - Submit to Google Search Console

7. **[High] Add robots.txt**
   - Create robots.txt in root
   - Configure crawler instructions
   - Link to sitemap

8. **[High] Set Up Linting Infrastructure**
   - Add RuboCop for Ruby/Liquid
   - Add Stylelint for SCSS
   - Add HTMLProofer for HTML validation
   - Add lint step to CI

9. **[High] Refactor Monolithic index.md**
   - Split into separate Jekyll includes
   - Create reusable components for program cards, board members
   - Extract sections into data files where appropriate

10. **[High] Add Basic Testing**
    - Set up HTMLProofer
    - Test internal links
    - Validate HTML structure
    - Add to CI pipeline

11. **[High] Extract Inline JavaScript**
    - Move mobile menu JS to separate file
    - Set up basic JS linting (ESLint or similar)
    - Consider adding simple build process

### Planned (Medium Priority)

12. **[Medium] Add Lighthouse CI**
    - Set up lighthouse-ci workflow
    - Track performance metrics
    - Set performance budgets

13. **[Medium] Refactor CSS Architecture**
    - Split monolithic SCSS into modules
    - Consider Tailwind CSS migration path
    - Document color palette and spacing system

14. **[Medium] Optimize Images**
    - Add image optimization to build process
    - Consider WebP format
    - Implement lazy loading

15. **[Medium] Improve SEO Implementation**
    - Add custom metadata per section
    - Implement structured data (JSON-LD)
    - Add Open Graph and Twitter card tags

### Future (Low Priority)

16. **[Low] Start CHANGELOG.md**
    - Document version history
    - Establish versioning scheme
    - Include in release process

17. **[Low] Evaluate Next.js Migration**
    - Create proof-of-concept
    - Estimate effort and timeline
    - Plan migration strategy

---

## Conclusion

The FencingTogether website represents significant technical debt compared to the FFC Single Page Template. The core issue is the architectural divergence between Jekyll (Ruby) and Next.js (TypeScript/React), which prevents code sharing and limits the site's scalability.

**Key Recommendation:** Migrate to Next.js to achieve full alignment with the FFC Template. This will enable component reuse, modern tooling, and a unified development experience across FFC projects.

**Alternative:** If immediate migration is not feasible, implement the "Hybrid Approach" to address critical gaps (documentation, community files, testing, linting) while planning for eventual Next.js migration.

**Priority Actions:**
1. Add README.md and community health files (1-2 days)
2. Fix Dependabot and CodeQL configurations (1 day)
3. Add testing and linting infrastructure (3-5 days)
4. Refactor monolithic structure (1 week)
5. Plan and execute Next.js migration (2-4 weeks)

**Success Metrics:**
- All critical issues resolved within 2 weeks
- High priority issues resolved within 1 month
- Decision on Next.js migration made within 2 months
- If migrating: Complete migration within 3 months

---

**End of Audit Report**

*For questions or clarifications, please comment on the tracking issue.*
