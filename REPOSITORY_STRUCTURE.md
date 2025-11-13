# Repository Structure

This document explains the organization of the AI Assessment Framework repository.

## 📁 Directory Structure

```
ai-assessment-framework/
├── .github/                          # GitHub-specific configuration
│   └── ISSUE_TEMPLATE/              # Issue templates
│       ├── bug_report.md            # Bug report template
│       ├── feature_request.md       # Feature request template
│       └── question.md              # Question template
├── docs/                            # Documentation files
│   ├── AI_Assessment_Framework_README.md         # Getting started guide
│   ├── AI_Solution_Quick_Assessment.md           # Quick assessment (5 min)
│   ├── AI_Solution_Questionnaire.md              # Full questionnaire (20 min)
│   ├── AI_Solution_Risk_Scoring_Matrix.md        # Risk scoring framework
│   ├── AI_Integration_Architecture_Templates.md  # 8 architecture templates
│   └── AI_Solution_Analytics_Template.md         # Trend tracking template
├── examples/                        # Example assessments and implementations
│   └── sample-quick-assessment.md  # Sample completed quick assessment
├── templates/                       # (Reserved for future code templates)
├── CHANGELOG.md                    # Version history and changes
├── CONTRIBUTING.md                 # Contribution guidelines
├── LICENSE                         # MIT License
├── PUBLISH_TO_GITHUB.md           # Guide for publishing to GitHub
├── README.md                       # Main repository README
├── REPOSITORY_STRUCTURE.md        # This file
└── .gitignore                     # Git ignore rules
```

## 📄 File Descriptions

### Root Directory

| File | Purpose | Audience |
|------|---------|----------|
| `README.md` | Main landing page, quick start, overview | Everyone |
| `LICENSE` | MIT License terms | Contributors |
| `CONTRIBUTING.md` | How to contribute | Contributors |
| `CHANGELOG.md` | Version history | Everyone |
| `PUBLISH_TO_GITHUB.md` | Publishing guide | Maintainers |
| `REPOSITORY_STRUCTURE.md` | This file | Contributors |
| `.gitignore` | Files to exclude from Git | Developers |

### `.github/` Directory

GitHub-specific configuration files:

- **ISSUE_TEMPLATE/** - Templates for issues
  - `bug_report.md` - Report bugs
  - `feature_request.md` - Suggest enhancements
  - `question.md` - Ask questions

### `docs/` Directory

All framework documentation:

| File | Purpose | Time to Complete |
|------|---------|------------------|
| `AI_Assessment_Framework_README.md` | Complete getting started guide | 15 min read |
| `AI_Solution_Quick_Assessment.md` | Fast intake for low-risk projects | 5-10 min |
| `AI_Solution_Questionnaire.md` | Comprehensive assessment | 20-30 min |
| `AI_Solution_Risk_Scoring_Matrix.md` | Quantitative risk calculation | 15-30 min |
| `AI_Integration_Architecture_Templates.md` | 8 pre-approved patterns | 1-2 hours |
| `AI_Solution_Analytics_Template.md` | Trend tracking & reporting | 2-3 hours |

### `examples/` Directory

Real-world examples:

- `sample-quick-assessment.md` - Completed quick assessment example
- *(More to be added based on community contributions)*

### `templates/` Directory

Reserved for future code templates and boilerplates:

- Planned: Docker configurations
- Planned: Terraform templates
- Planned: CI/CD pipeline examples
- Planned: Security control implementations

## 🗺️ Navigation Guide

### I Want To...

**...understand the framework**
→ Start with [README.md](README.md)

**...assess an AI project**
→ Go to [docs/AI_Solution_Quick_Assessment.md](docs/AI_Solution_Quick_Assessment.md) or [docs/AI_Solution_Questionnaire.md](docs/AI_Solution_Questionnaire.md)

**...implement the framework at my organization**
→ Read [docs/AI_Assessment_Framework_README.md](docs/AI_Assessment_Framework_README.md)

**...build an AI solution**
→ Browse [docs/AI_Integration_Architecture_Templates.md](docs/AI_Integration_Architecture_Templates.md)

**...contribute**
→ See [CONTRIBUTING.md](CONTRIBUTING.md)

**...report a bug**
→ Use [bug report template](.github/ISSUE_TEMPLATE/bug_report.md)

**...suggest a feature**
→ Use [feature request template](.github/ISSUE_TEMPLATE/feature_request.md)

**...see examples**
→ Check [examples/](examples/) directory

## 📊 Document Relationships

```
README.md (start here)
    ├─→ AI_Assessment_Framework_README.md (implementation guide)
    │      ├─→ AI_Solution_Quick_Assessment.md (low-risk projects)
    │      ├─→ AI_Solution_Questionnaire.md (all projects)
    │      │      └─→ AI_Solution_Risk_Scoring_Matrix.md (security review)
    │      ├─→ AI_Integration_Architecture_Templates.md (developers)
    │      └─→ AI_Solution_Analytics_Template.md (leadership)
    └─→ CONTRIBUTING.md (contributors)
```

## 🔄 Typical User Journey

### For Project Requesters

1. Read `README.md` to understand framework
2. Choose assessment type based on risk
3. Complete `AI_Solution_Quick_Assessment.md` or `AI_Solution_Questionnaire.md`
4. Submit for review
5. Upon approval, select from `AI_Integration_Architecture_Templates.md`
6. Build solution following security checklists

### For Security Teams

1. Read `AI_Assessment_Framework_README.md` for implementation
2. Review submitted assessments
3. Use `AI_Solution_Risk_Scoring_Matrix.md` to calculate risk
4. Route to appropriate approvers
5. Track projects in `AI_Solution_Analytics_Template.md`
6. Generate monthly/quarterly reports

### For Contributors

1. Read `CONTRIBUTING.md` for guidelines
2. Browse `docs/` to understand content structure
3. Check `examples/` for reference
4. Create feature branch
5. Make changes
6. Submit pull request
7. Respond to feedback

## 📝 Adding New Content

### Adding a New Architecture Template

1. Edit `docs/AI_Integration_Architecture_Templates.md`
2. Add new template section following existing structure:
   - Use cases
   - Architecture diagram
   - Technology stack
   - Security controls
   - Configuration examples
   - Deployment checklist
   - Cost estimate
3. Update template index
4. Add to template selection guide
5. Create example in `examples/` directory
6. Update `CHANGELOG.md`
7. Submit PR

### Adding a New Example

1. Create new file in `examples/` directory
2. Follow naming convention: `sample-[type]-[description].md`
3. Include all required sections
4. Add realistic data and context
5. Update `CHANGELOG.md`
6. Submit PR

### Updating Documentation

1. Identify document to update in `docs/`
2. Make changes following style guide
3. Update version number at top of document
4. Add entry to `CHANGELOG.md`
5. Submit PR

## 🏷️ Version Control

### Branch Strategy

- **main** - Stable, production-ready
- **feature/*** - New features or templates
- **fix/*** - Bug fixes
- **docs/*** - Documentation updates

### Release Process

1. Update `CHANGELOG.md` with all changes
2. Update version numbers in documents
3. Create release notes
4. Tag release (`v1.x.x`)
5. Publish GitHub release

### Version Numbering

Following [Semantic Versioning](https://semver.org/):

- **Major (v2.0.0)** - Breaking changes
- **Minor (v1.1.0)** - New features, backwards compatible
- **Patch (v1.0.1)** - Bug fixes, backwards compatible

## 🔍 Finding Information

### Search Tips

**Within repository:**
- Use GitHub's search: Press `/` then type search term
- Search in specific file: `filename:README "search term"`
- Search in docs: `path:docs/ "search term"`

**In documentation:**
- Use browser Find (Ctrl/Cmd + F)
- Check table of contents in each document
- Look for "Quick Reference" sections

### Common Searches

| Looking for... | Search for... |
|----------------|---------------|
| How to assess a project | "quick assessment" or "questionnaire" |
| Risk scoring | "risk scoring matrix" |
| Architecture examples | "architecture templates" |
| Security controls | "security controls" or "checklist" |
| Cost estimates | "cost estimate" |
| Getting started | "getting started" or "implementation guide" |

## 📈 Repository Metrics

Track repository health:

- **Issues:** Bug reports and questions
- **Pull Requests:** Contributions
- **Discussions:** Community engagement
- **Stars:** Popularity indicator
- **Forks:** Adoption rate
- **Traffic:** Visitors and views

View in: Repository > Insights

## 🤝 Community Files

Community health files in `.github/`:

- `ISSUE_TEMPLATE/` - Issue forms
- *(Future)* `PULL_REQUEST_TEMPLATE.md` - PR template
- *(Future)* `CODE_OF_CONDUCT.md` - Code of conduct
- *(Future)* `SECURITY.md` - Security policy

## 📦 Future Additions

Planned directories/files:

- `scripts/` - Automation scripts
- `tools/` - Risk calculator, converters
- `integrations/` - PM tool integrations
- `tests/` - Validation tests
- `.github/workflows/` - GitHub Actions
- `docker/` - Docker configurations

## 🎯 Best Practices

### For Maintainers

- Keep `CHANGELOG.md` updated
- Review PRs within 3 business days
- Update documentation when adding features
- Test examples before merging
- Tag releases regularly

### For Contributors

- Read `CONTRIBUTING.md` first
- Keep PRs focused (one feature/fix per PR)
- Update docs with code changes
- Add examples for new features
- Test thoroughly before submitting

### For Users

- Star the repo if you find it useful
- Report issues clearly
- Share your implementations
- Suggest improvements
- Help others in Discussions

## 📞 Support

- **Issues:** Technical problems, bugs
- **Discussions:** Questions, ideas, general chat
- **Pull Requests:** Code/doc contributions
- **Email:** cesar@alsolutions.com (maintainer)

## 📚 Additional Resources

- [GitHub Docs](https://docs.github.com)
- [Markdown Guide](https://www.markdownguide.org)
- [Semantic Versioning](https://semver.org)
- [Keep a Changelog](https://keepachangelog.com)

---

**Repository Version:** 1.0.0
**Last Updated:** November 12, 2025
**Maintainer:** Cesar (Security Architect)

---

**Questions about the structure?** Open a [Discussion](https://github.com/YOUR_ORG/ai-assessment-framework/discussions)!
