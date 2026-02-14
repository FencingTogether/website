# Technical Debt Audit - Quick Reference

**Repository:** FencingTogether/website  
**Audit Date:** December 19, 2025  
**Full Report:** [TECH_DEBT_AUDIT.md](./TECH_DEBT_AUDIT.md)  
**Remediation Issues:** [REMEDIATION_ISSUES.md](./REMEDIATION_ISSUES.md)

---

## At a Glance

### Current State
- **Framework:** Jekyll (Ruby-based static site generator)
- **Deployment:** GitHub Pages
- **Structure:** Flat file hierarchy, single-page site
- **Testing:** None
- **Documentation:** Minimal
- **CI/CD:** Basic build and deploy

### Target State (FFC Template)
- **Framework:** Next.js 16 with TypeScript
- **Deployment:** GitHub Pages + Custom Domain
- **Structure:** Component-based, multi-page application
- **Testing:** Playwright E2E tests
- **Documentation:** Comprehensive
- **CI/CD:** Multi-stage with testing, security, performance

### The Gap
Complete architectural divergence requiring either full migration or significant modernization of Jekyll stack.

---

## Priority Breakdown

| Priority | Count | Effort | Impact |
|----------|-------|--------|--------|
| 🔴 Critical | 8 | 30-45 hours | High - Blocks collaboration and security |
| 🟡 High | 9 | 40-60 hours | High - Impacts maintainability and SEO |
| 🟢 Medium | 6 | 35-50 hours | Medium - Improves quality and performance |
| ⚪ Low | 3 | 85-165 hours | Low - Nice to have, long-term investment |

**Total Estimated Effort:** 190-320 hours (including potential Next.js migration)

---

## Critical Issues (Do First)

1. **Missing README.md** - No onboarding documentation
2. **Missing Community Files** - No LICENSE, CODE_OF_CONDUCT, CONTRIBUTING, SECURITY
3. **Dependabot Misconfigured** - Watching npm instead of bundler (no security updates)
4. **No Gemfile.lock** - Non-reproducible builds
5. **CodeQL Wrong Language** - Scanning JavaScript but site has none
6. **No Linting** - No code quality automation
7. **No License** - Legal ambiguity
8. **No Security Process** - No SECURITY.md

**Impact:** These issues block effective collaboration, create security risks, and make the project appear abandoned.

**Time to Fix:** 1-2 weeks (30-45 hours)

---

## High Priority Issues (Do Next)

1. **No Sitemap** - SEO impact
2. **No robots.txt** - Crawler guidance missing
3. **Monolithic index.md** - 207 lines, hard to maintain
4. **No Testing** - Regressions not caught
5. **Inline JavaScript** - Not testable
6. **Unclear Dependencies** - github-pages gem hides versions
7. **Basic SEO** - Limited metadata control
8. **No Build Docs** - Can't run locally

**Impact:** These issues affect maintainability, SEO, and contributor experience.

**Time to Fix:** 2-3 weeks (40-60 hours)

---

## Technology Stack Comparison

| Aspect | FencingTogether (Current) | FFC Template (Target) | Gap |
|--------|---------------------------|------------------------|-----|
| **Framework** | Jekyll (Ruby) | Next.js 16 (Node.js) | Complete divergence |
| **Language** | HTML/Liquid/SCSS | TypeScript/TSX | No type safety |
| **Styling** | Custom SCSS (613 lines) | Tailwind CSS | Manual vs utility-first |
| **Components** | Flat includes | 20+ organized folders | No reusability |
| **Testing** | None | Playwright E2E | No quality assurance |
| **Linting** | None | ESLint + TypeScript | No code quality |
| **Build Time** | Unknown | ~30 seconds | Not documented |
| **Dev Server** | Not documented | ~1 second startup | Unknown DX |
| **SEO** | Basic (plugin defaults) | Comprehensive (custom) | Limited control |
| **Documentation** | None (no README) | 400+ line README | No onboarding |
| **CI/CD** | 2 workflows | 4 workflows + tests | Basic automation |
| **Dependencies** | Opaque (github-pages) | Explicit (package.json) | Hidden versions |

---

## File Statistics

| Metric | FencingTogether | FFC Template |
|--------|-----------------|--------------|
| **Total Files** | ~17 (HTML/CSS/MD/YML) | 100+ (TS/TSX/CSS/MD) |
| **Largest File** | 613 lines (style.scss) | Various ~100-200 lines |
| **Content File** | 207 lines (index.md) | Split across components |
| **Layouts** | 1 (default.html) | Multiple (app router) |
| **Components** | 2 (header, footer) | 20+ organized folders |
| **Test Files** | 0 | Multiple Playwright tests |
| **Config Files** | 2 (Jekyll, Dependabot) | 7+ (Next, TS, ESLint, etc) |

---

## Key Architectural Differences

### Build Process
**FencingTogether:**
```
Ruby/Jekyll → GitHub Pages → Deployment
```
- No local development documentation
- Unknown build time
- No testing phase
- No performance monitoring

**FFC Template:**
```
TypeScript → Next.js Build → Tests → Deploy
```
- Documented local dev (`npm run dev`)
- ~30 second build
- Automated testing
- Performance tracking (Lighthouse)

### Component Architecture
**FencingTogether:**
```
index.md (207 lines)
└── Inline HTML sections
    ├── Hero (hardcoded)
    ├── Mission (hardcoded)
    ├── Programs (hardcoded)
    └── Contact (hardcoded)
```
- Monolithic, no reusability
- Cannot test sections independently
- Hard to maintain consistency

**FFC Template:**
```
app/page.tsx
├── Hero (component)
├── Mission (component)
├── Programs (component, from data)
├── Team (component, from data)
└── FAQ (component, from data)
```
- Modular, reusable components
- Testable units
- Data-driven rendering

### Styling Approach
**FencingTogether:**
- 613 lines of custom SCSS
- Manual responsive design
- CSS Custom Properties
- All styles hand-written

**FFC Template:**
- Tailwind CSS utilities
- Responsive by default
- Design system enforced
- Minimal custom CSS

---

## Migration Considerations

### Option 1: Migrate to Next.js ⭐ (Recommended)
**Pros:**
- Full alignment with FFC Template
- Modern tooling and testing
- Component reusability
- Better performance and SEO
- Larger talent pool

**Cons:**
- High initial effort (80-160 hours)
- Complete rewrite required
- New tech stack to learn

**Timeline:** 2-4 weeks

### Option 2: Modernize Jekyll
**Pros:**
- Lower immediate effort
- Familiar technology
- No rewrite needed

**Cons:**
- Still diverges from template
- Limited scalability
- Technical debt remains

**Timeline:** 1-2 weeks

### Option 3: Hybrid Approach
**Phase 1:** Fix critical issues (1-2 weeks)
**Phase 2:** Modernize Jekyll (2-3 weeks)
**Phase 3:** Plan Next.js migration (TBD)

---

## Success Metrics

### Short-Term (1 month)
- [ ] All critical issues resolved
- [ ] README and community files added
- [ ] Testing infrastructure in place
- [ ] Linting configured and passing
- [ ] CI/CD improved

### Medium-Term (3 months)
- [ ] All high priority issues resolved
- [ ] Site refactored into modular structure
- [ ] SEO improved (sitemap, robots.txt, metadata)
- [ ] Performance monitoring active
- [ ] Documentation complete

### Long-Term (6 months)
- [ ] Decision on Next.js migration made
- [ ] If migrating: migration complete
- [ ] If staying: all medium priority issues resolved
- [ ] Project fully aligned with FFC standards

---

## Quick Action Items

### This Week (Critical)
1. Create README.md
2. Add LICENSE and SECURITY.md
3. Fix Dependabot configuration
4. Commit Gemfile.lock
5. Fix CodeQL configuration

### Next Week (Critical + High)
6. Add remaining community files
7. Set up linting infrastructure
8. Add sitemap and robots.txt
9. Start refactoring index.md
10. Add basic testing

### This Month (High + Medium)
11. Complete refactoring
12. Improve SEO metadata
13. Optimize images
14. Add performance monitoring
15. Document everything

---

## Resources

- **Full Audit:** [TECH_DEBT_AUDIT.md](./TECH_DEBT_AUDIT.md) (1,065 lines)
- **Issue Templates:** [REMEDIATION_ISSUES.md](./REMEDIATION_ISSUES.md) (26 issues)
- **FFC Template:** https://github.com/FreeForCharity/FFC_Single_Page_Template
- **Jekyll Docs:** https://jekyllrb.com/docs/
- **Next.js Docs:** https://nextjs.org/docs

---

## Questions & Discussion

For questions about the audit, clarifications on findings, or discussion of priorities, please comment on the original tracking issue.

**Related Issues:**
- Original Audit Request: [Link to issue]
- Individual remediation issues: To be created from REMEDIATION_ISSUES.md

---

**Last Updated:** December 19, 2025
