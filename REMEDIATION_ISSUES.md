# Recommended Remediation Issues

This document lists the specific issues that should be created based on the Technical Debt Audit findings. Each issue is prioritized and includes effort estimates.

## How to Use This Document

1. Review the audit findings in [TECH_DEBT_AUDIT.md](./TECH_DEBT_AUDIT.md)
2. Create issues from this list based on priority
3. Link each new issue back to the original audit issue
4. Use the provided templates as starting points

---

## 🔴 Critical Priority Issues (Create Immediately)

### Issue 1: Add README.md with Local Development Instructions
**Priority:** Critical  
**Effort:** Medium (4-8 hours)  
**Labels:** documentation, good first issue

**Description:**
Create a comprehensive README.md that includes:
- Project description and purpose
- Local development setup (Ruby, Jekyll, dependencies)
- Build and deployment instructions
- Common tasks and commands
- Contribution guidelines overview
- Link to other documentation

**Reference:** 
- FFC Template README structure
- Section 3 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] README.md exists in repository root
- [ ] Ruby version requirement documented
- [ ] Local setup instructions provided (`bundle install`, `bundle exec jekyll serve`)
- [ ] Build instructions provided
- [ ] Links to other documentation included

---

### Issue 2: Add Missing Community Health Files
**Priority:** Critical  
**Effort:** Medium (6-10 hours)  
**Labels:** documentation, community

**Description:**
Add standard community health files to establish clear project governance:
- LICENSE (choose appropriate open source license)
- CODE_OF_CONDUCT.md (use Contributor Covenant 2.1)
- CONTRIBUTING.md (contribution process and standards)
- SECURITY.md (vulnerability disclosure process)
- SUPPORT.md (how to get help)

**Reference:**
- FFC Template community files as examples
- Section 10 of TECH_DEBT_AUDIT.md
- GitHub's community standards

**Acceptance Criteria:**
- [ ] LICENSE file added with appropriate license
- [ ] CODE_OF_CONDUCT.md added (Contributor Covenant 2.1)
- [ ] CONTRIBUTING.md with contribution guidelines
- [ ] SECURITY.md with vulnerability disclosure process
- [ ] SUPPORT.md with support resources
- [ ] All files appear on GitHub community profile

---

### Issue 3: Fix Dependabot Configuration for Bundler
**Priority:** Critical  
**Effort:** Low (1-2 hours)  
**Labels:** dependencies, security

**Description:**
Current dependabot.yml is configured for npm but the project uses Ruby/Bundler. This means no automated security updates are happening.

**Changes Needed:**
```yaml
# Change from:
- package-ecosystem: 'npm'
# To:
- package-ecosystem: 'bundler'
```

**Reference:**
- Section 2 of TECH_DEBT_AUDIT.md
- Current `.github/dependabot.yml`

**Acceptance Criteria:**
- [ ] dependabot.yml updated to use 'bundler' ecosystem
- [ ] Verify Dependabot creates PRs for gem updates
- [ ] Document Dependabot configuration in README

---

### Issue 4: Commit Gemfile.lock for Reproducible Builds
**Priority:** Critical  
**Effort:** Low (1 hour)  
**Labels:** dependencies, build

**Description:**
Gemfile.lock is not committed, leading to non-reproducible builds. Different environments may install different gem versions.

**Tasks:**
1. Run `bundle install` to generate Gemfile.lock
2. Commit Gemfile.lock to repository
3. Update .gitignore if it's currently excluded
4. Update documentation to mention locked dependencies

**Reference:**
- Section 2 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] Gemfile.lock committed to repository
- [ ] Gemfile.lock not in .gitignore
- [ ] CI uses `bundle install --deployment` or similar
- [ ] Build process documented in README

---

### Issue 5: Fix CodeQL Configuration
**Priority:** Critical  
**Effort:** Low (1-2 hours)  
**Labels:** security, ci-cd

**Description:**
CodeQL workflow is configured to scan JavaScript/TypeScript but the site has none. This wastes CI resources and provides false security coverage.

**Changes Needed:**
Remove or update `.github/workflows/codeql.yml` matrix:
```yaml
# Current (incorrect):
- language: javascript-typescript
# Should be removed or changed to scan relevant languages
```

**Reference:**
- Section 9 of TECH_DEBT_AUDIT.md
- `.github/workflows/codeql.yml`

**Acceptance Criteria:**
- [ ] CodeQL matrix updated to reflect actual codebase
- [ ] JavaScript/TypeScript scanning removed
- [ ] CodeQL scans complete successfully
- [ ] No false security alerts

---

### Issue 6: Add LICENSE File
**Priority:** Critical  
**Effort:** Low (30 minutes)  
**Labels:** legal, documentation

**Description:**
Repository has no LICENSE file, creating legal ambiguity. Contributors cannot legally contribute without a clear license.

**Recommended License:**
- MIT License (permissive) OR
- GPL-3.0 (copyleft, matches FFC Template)

**Reference:**
- Section 10 of TECH_DEBT_AUDIT.md
- Choose license based on organization policy

**Acceptance Criteria:**
- [ ] LICENSE file added to repository root
- [ ] License choice documented in README
- [ ] Copyright holder specified
- [ ] Year updated

---

### Issue 7: Set Up Linting Infrastructure
**Priority:** Critical  
**Effort:** High (8-12 hours)  
**Labels:** code-quality, tooling

**Description:**
No automated code quality checks exist. This leads to inconsistent code style and potential errors.

**Tools to Add:**
1. **RuboCop** - Ruby/Liquid linting
2. **Stylelint** - SCSS linting
3. **HTMLProofer** - HTML validation and link checking

**Tasks:**
1. Add linting gems to Gemfile (development group)
2. Create configuration files (.rubocop.yml, .stylelintrc)
3. Add lint scripts to Rakefile or as standalone commands
4. Add lint step to CI workflow
5. Fix existing lint errors
6. Document linting in README

**Reference:**
- Section 7 of TECH_DEBT_AUDIT.md
- FFC Template eslint.config.mjs as inspiration

**Acceptance Criteria:**
- [ ] Linting gems installed and configured
- [ ] Lint scripts available (e.g., `bundle exec rubocop`)
- [ ] CI runs linting checks
- [ ] Existing code passes lint checks (or exclusions documented)
- [ ] Linting documented in README

---

### Issue 8: Create SECURITY.md with Vulnerability Disclosure
**Priority:** Critical  
**Effort:** Low (1-2 hours)  
**Labels:** security, documentation

**Description:**
No documented process for reporting security vulnerabilities. This may lead to public disclosure of vulnerabilities or improper handling.

**Contents to Include:**
- How to report a vulnerability (email, GitHub Security Advisories)
- Expected response time
- Disclosure policy
- Supported versions
- Security update process

**Reference:**
- Section 12 of TECH_DEBT_AUDIT.md
- FFC Template SECURITY.md as example
- GitHub security best practices

**Acceptance Criteria:**
- [ ] SECURITY.md created in repository root
- [ ] Vulnerability disclosure process documented
- [ ] Contact information provided
- [ ] File appears in GitHub Security tab
- [ ] Linked from README

---

## 🟡 High Priority Issues (Create Within 2 Weeks)

### Issue 9: Add Sitemap Generation
**Priority:** High  
**Effort:** Low (1-2 hours)  
**Labels:** seo, enhancement

**Description:**
No sitemap.xml generated, which impacts SEO as search engines may not discover all pages.

**Implementation:**
1. Add `jekyll-sitemap` to Gemfile plugins
2. Verify sitemap generation (`_site/sitemap.xml`)
3. Update robots.txt to reference sitemap
4. Submit to Google Search Console

**Reference:**
- Section 6 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] jekyll-sitemap plugin enabled
- [ ] sitemap.xml generated in build
- [ ] Sitemap accessible at /sitemap.xml
- [ ] robots.txt references sitemap
- [ ] Submitted to search engines

---

### Issue 10: Add robots.txt
**Priority:** High  
**Effort:** Low (30 minutes)  
**Labels:** seo, enhancement

**Description:**
No robots.txt file exists to guide crawler behavior.

**Contents:**
```
User-agent: *
Allow: /
Sitemap: https://fencingtogether.github.io/website/sitemap.xml
```

**Reference:**
- Section 6 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] robots.txt created in root
- [ ] Accessible at /robots.txt
- [ ] References sitemap
- [ ] Appropriate directives for crawlers

---

### Issue 11: Refactor Monolithic index.md
**Priority:** High  
**Effort:** High (12-16 hours)  
**Labels:** refactoring, maintainability

**Description:**
index.md contains 207 lines of mixed Markdown, HTML, and Liquid. This makes maintenance difficult and prevents component reuse.

**Refactoring Strategy:**
1. Extract sections into separate includes:
   - `_includes/hero.html`
   - `_includes/mission.html`
   - `_includes/programs.html`
   - `_includes/board.html`
   - `_includes/contact.html`
2. Extract data into `_data/` YAML files:
   - `_data/programs.yml`
   - `_data/board_members.yml`
   - `_data/values.yml`
3. Create reusable components:
   - `_includes/program_card.html`
   - `_includes/board_member.html`
   - `_includes/value_card.html`

**Reference:**
- Section 4 of TECH_DEBT_AUDIT.md
- Jekyll includes and data files documentation

**Acceptance Criteria:**
- [ ] index.md reduced to layout and include tags
- [ ] Sections extracted to separate includes
- [ ] Data extracted to _data/ files
- [ ] Reusable components created
- [ ] Site builds and displays correctly
- [ ] No content or styling regressions

---

### Issue 12: Add Basic Testing with HTMLProofer
**Priority:** High  
**Effort:** Medium (4-6 hours)  
**Labels:** testing, quality

**Description:**
No automated testing exists. Add HTMLProofer to validate HTML structure and links.

**Implementation:**
1. Add `html-proofer` gem
2. Create test script in Rakefile
3. Configure HTMLProofer options
4. Add test step to CI workflow
5. Fix any broken links or invalid HTML

**Tests to Include:**
- Internal link validation
- Image alt text presence
- HTML5 validation
- External link checking (optional, may be slow)

**Reference:**
- Section 7 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] html-proofer gem installed
- [ ] Test script available (e.g., `rake test`)
- [ ] CI runs tests after build
- [ ] Tests pass (or failures documented)
- [ ] Testing documented in README

---

### Issue 13: Extract Inline JavaScript
**Priority:** High  
**Effort:** Low (2-3 hours)  
**Labels:** refactoring, best-practices

**Description:**
Mobile menu JavaScript is embedded inline in `_includes/header.html`. This is not testable and violates separation of concerns.

**Changes:**
1. Create `assets/js/main.js`
2. Move mobile menu toggle logic to main.js
3. Add script tag in layout to load main.js
4. Consider adding JS linting (ESLint)

**Reference:**
- Section 8 of TECH_DEBT_AUDIT.md
- `_includes/header.html` lines 25-31

**Acceptance Criteria:**
- [ ] JavaScript extracted to assets/js/main.js
- [ ] Script loaded in layout
- [ ] Mobile menu functionality works
- [ ] No inline scripts remain
- [ ] Consider JS minification

---

### Issue 14: Add GitHub Pages Jekyll Version Documentation
**Priority:** High  
**Effort:** Low (1 hour)  
**Labels:** documentation

**Description:**
Current Gemfile uses `github-pages` gem which hides actual Jekyll version. Document what version is actually being used.

**Tasks:**
1. Run `bundle exec github-pages versions` locally
2. Document Jekyll version in README
3. Consider switching to explicit Jekyll version
4. Document Ruby version requirement

**Reference:**
- Section 2 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] Jekyll version documented in README
- [ ] Ruby version requirement documented
- [ ] Dependencies clear and explicit

---

### Issue 15: Improve SEO Metadata Implementation
**Priority:** High  
**Effort:** Medium (4-6 hours)  
**Labels:** seo, enhancement

**Description:**
Currently relying entirely on jekyll-seo-tag defaults. Add custom metadata for better SEO control.

**Enhancements:**
1. Add Open Graph tags for social sharing
2. Add Twitter card metadata
3. Add structured data (JSON-LD) for organization
4. Add meta descriptions per page
5. Optimize title tags

**Reference:**
- Section 6 of TECH_DEBT_AUDIT.md
- FFC Template layout.tsx metadata as inspiration

**Acceptance Criteria:**
- [ ] Open Graph tags added
- [ ] Twitter card metadata added
- [ ] Structured data (JSON-LD) for organization
- [ ] Custom metadata per page/section
- [ ] Test with social media debuggers
- [ ] Validate with SEO tools

---

### Issue 16: Fix Dependabot Ecosystem Mismatch (Duplicate of #3)
**Priority:** High  
**Effort:** Low (1-2 hours)  
**Labels:** dependencies, security

**Note:** This is the same as Issue 3 but highlighted again due to security implications.

---

### Issue 17: Create CODE_OF_CONDUCT.md (Part of Issue 2)
**Priority:** High  
**Effort:** Low (1 hour)  
**Labels:** community, documentation

**Note:** This is part of Issue 2 but can be done independently.

---

## 🟢 Medium Priority Issues (Create Within 1 Month)

### Issue 18: Add Lighthouse CI Performance Monitoring
**Priority:** Medium  
**Effort:** Medium (3-4 hours)  
**Labels:** performance, ci-cd

**Description:**
Add Lighthouse CI to track performance metrics over time.

**Implementation:**
1. Create `.github/workflows/lighthouse.yml`
2. Configure Lighthouse CI
3. Set performance budgets
4. Add badge to README

**Reference:**
- Section 3 of TECH_DEBT_AUDIT.md
- FFC Template lighthouse.yml as example

**Acceptance Criteria:**
- [ ] Lighthouse CI workflow created
- [ ] Performance metrics tracked
- [ ] Budgets set and enforced
- [ ] Results accessible

---

### Issue 19: Refactor CSS Architecture
**Priority:** Medium  
**Effort:** High (16-24 hours)  
**Labels:** refactoring, css

**Description:**
613-line monolithic SCSS file is hard to navigate. Refactor into modular structure.

**Strategy:**
1. Split into partials:
   - `_variables.scss`
   - `_base.scss`
   - `_layout.scss`
   - `_components.scss`
   - `_utilities.scss`
2. Import in main `style.scss`
3. Consider naming methodology (BEM, SMACSS)
4. Document color palette and spacing system

**Reference:**
- Section 5 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] SCSS split into logical modules
- [ ] No styling regressions
- [ ] CSS size not increased
- [ ] Documentation of system

---

### Issue 20: Optimize Images
**Priority:** Medium  
**Effort:** Medium (4-6 hours)  
**Labels:** performance, assets

**Description:**
Images served as-is without optimization.

**Tasks:**
1. Add image optimization to build process
2. Convert to WebP where supported
3. Implement responsive images
4. Add lazy loading
5. Optimize logo.png if it exists

**Reference:**
- Section 11 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] Images optimized (reduced file size)
- [ ] WebP format provided with fallbacks
- [ ] Lazy loading implemented
- [ ] Page load time improved

---

### Issue 21: Document Naming Conventions
**Priority:** Medium  
**Effort:** Low (2-3 hours)  
**Labels:** documentation, standards

**Description:**
No documented naming conventions or coding standards.

**Contents:**
- File naming (kebab-case for routes)
- CSS class naming (current BEM-like approach)
- Liquid variable naming
- Git commit message format
- Branch naming

**Reference:**
- Section 13 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] Naming conventions documented
- [ ] Examples provided
- [ ] Rationale explained
- [ ] Added to CONTRIBUTING.md

---

### Issue 22: Add Asset Path Management
**Priority:** Medium  
**Effort:** Low (1-2 hours)  
**Labels:** enhancement, assets

**Description:**
Hardcoded asset paths limit deployment flexibility.

**Solution:**
Jekyll's `relative_url` filter already provides this. Ensure it's used consistently:
```liquid
{{ '/assets/images/logo.png' | relative_url }}
```

**Reference:**
- Section 11 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] All asset references use relative_url filter
- [ ] Verify works with different baseurl settings
- [ ] Document in README

---

### Issue 23: Improve Contact Form
**Priority:** Medium  
**Effort:** Medium (4-6 hours)  
**Labels:** feature, enhancement

**Description:**
Contact form references Formspree with placeholder form ID.

**Tasks:**
1. Set up actual Formspree account or alternative
2. Update form action URL
3. Add form validation
4. Add success/error messaging
5. Test form submission

**Reference:**
- `index.md` line 188

**Acceptance Criteria:**
- [ ] Form configured with real endpoint
- [ ] Form validation works
- [ ] Success/error states handled
- [ ] Tested end-to-end

---

## ⚪ Low Priority Issues (Future Consideration)

### Issue 24: Start CHANGELOG.md
**Priority:** Low  
**Effort:** Low (1-2 hours ongoing)  
**Labels:** documentation

**Description:**
Start maintaining a changelog to track version history.

**Format:**
Use Keep a Changelog format

**Reference:**
- Section 10 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] CHANGELOG.md created
- [ ] Current version documented
- [ ] Process for updating established

---

### Issue 25: Evaluate Google Fonts Strategy
**Priority:** Low  
**Effort:** Low (2-3 hours)  
**Labels:** performance, enhancement

**Description:**
Currently loading Google Fonts from CDN. Consider self-hosting for performance and privacy.

**Options:**
1. Self-host fonts
2. Use system font stack
3. Keep current approach

**Reference:**
- Section 11 of TECH_DEBT_AUDIT.md

**Acceptance Criteria:**
- [ ] Fonts strategy evaluated
- [ ] Performance impact measured
- [ ] Decision documented

---

### Issue 26: Evaluate Next.js Migration
**Priority:** Low  
**Effort:** Very High (80-160 hours)  
**Labels:** epic, architecture

**Description:**
Evaluate and potentially execute migration to Next.js to align with FFC Template.

**Phase 1: Evaluation (8-16 hours)**
- Create proof-of-concept
- Estimate migration effort
- Identify risks and blockers
- Get stakeholder buy-in

**Phase 2: Migration (72-144 hours)**
- Set up Next.js project
- Port content and components
- Implement testing
- Deploy and verify

**Reference:**
- Entire TECH_DEBT_AUDIT.md document
- Section "Recommendations & Action Plan"

**Acceptance Criteria:**
- [ ] Evaluation complete with recommendation
- [ ] If approved: migration plan created
- [ ] If executed: site migrated to Next.js

---

## Summary

**Total Issues:** 26
- Critical: 8 issues (~30-45 hours)
- High: 9 issues (~40-60 hours)
- Medium: 6 issues (~35-50 hours)
- Low: 3 issues (~85-165 hours if migration pursued)

**Recommended Phasing:**

**Week 1-2:** Critical issues (Documentation, community files, configuration fixes)
**Week 3-4:** High priority issues (Testing, refactoring, SEO)
**Month 2:** Medium priority issues (Performance, optimization)
**Ongoing:** Low priority issues as time allows

For questions or to discuss prioritization, please comment on the original audit issue.
