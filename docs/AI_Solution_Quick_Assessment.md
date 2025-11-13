# AI Solution Quick Assessment

**Version:** 1.0
**Owner:** Cesar (Security Architect)
**Purpose:** Streamlined questionnaire for low-risk, small-scale AI projects
**Last Updated:** November 12, 2025

---

## When to Use This Quick Assessment

Use this shortened version for:
- **Internal tools** with < 50 users
- **Proof of concepts** and prototypes
- **Development/testing** environments only
- **Low/no sensitive data** projects
- **Standard technology stacks** (Anthropic Claude, OpenAI, proven tech)

**If ANY of the following apply, use the full questionnaire instead:**
- [ ] Customer-facing application
- [ ] Handles PII, PHI, PCI, or regulated data
- [ ] Production deployment
- [ ] External API exposure
- [ ] Budget > $25,000
- [ ] Automated decision-making
- [ ] Compliance requirements (HIPAA, PCI-DSS, GDPR, etc.)

---

## Quick Assessment Form

**Completion Time:** 5-10 minutes
**Completed By:** [Name]
**Date:** [Date]
**Project Name:** [Name]

---

### Section 1: Basic Information

**1.1 What are you building?**
- [ ] Chatbot / Q&A assistant
- [ ] Document processing
- [ ] Code assistant
- [ ] Data analysis tool
- [ ] RAG (knowledge base search)
- [ ] Other: _______________

**1.2 Who will use it?**
- [ ] Just me (< 5 users)
- [ ] My team (5-20 users)
- [ ] Department (20-50 users)
- [ ] Organization-wide (50+ users)
- [ ] External users

**If external users selected, STOP and use full questionnaire.**

**1.3 What problem does it solve?** (1-2 sentences)

---

### Section 2: Data & Security

**2.1 What type of data will it access?**
- [ ] Public information only
- [ ] Internal company documents/data
- [ ] Customer data
- [ ] Employee data (HR, payroll, etc.)
- [ ] Financial data
- [ ] Healthcare data (PHI)
- [ ] Payment information (PCI)
- [ ] Other sensitive data: _______________

**If customer data, employee data, financial, healthcare, or payment selected, STOP and use full questionnaire.**

**2.2 Will data be sent to a third-party AI provider?**
- [ ] Yes - Anthropic Claude
- [ ] Yes - OpenAI
- [ ] Yes - Google
- [ ] Yes - Other: _______________
- [ ] No - Self-hosted model

**2.3 Does the AI provider have a Data Processing Agreement (DPA) in place?**
- [ ] Yes
- [ ] No
- [ ] Don't know

**If "No" or "Don't know" for a provider handling any internal data, STOP and use full questionnaire.**

---

### Section 3: Technical Architecture

**3.1 Which AI model will you use?**
- [ ] Anthropic Claude (Sonnet, Haiku)
- [ ] OpenAI (GPT-4, GPT-3.5)
- [ ] Google Gemini
- [ ] Open-source model
- [ ] Other: _______________

**3.2 Will you need a database?**
- [ ] No database (API calls only)
- [ ] PostgreSQL
- [ ] Vector database (for RAG): _______________
- [ ] Other: _______________

**3.3 Where will this be deployed?**
- [ ] My local machine (dev only)
- [ ] Company server (on-premise)
- [ ] Cloud (AWS, Azure, GCP)
- [ ] SaaS / fully managed
- [ ] Don't know yet

**3.4 Will this make API calls to external services?** (besides the AI provider)
- [ ] No
- [ ] Yes - Which services: _______________

---

### Section 4: Access & Authentication

**4.1 How will users authenticate?**
- [ ] SSO (Google Workspace, Azure AD, etc.)
- [ ] Username/password with MFA
- [ ] Username/password only
- [ ] No authentication (open access)

**If "No authentication" selected, STOP and use full questionnaire.**

**4.2 Who should have access?**
- [ ] Anyone in the company
- [ ] Specific team/department: _______________
- [ ] Specific people only

---

### Section 5: Budget & Timeline

**5.1 Estimated budget**
- [ ] < $5,000
- [ ] $5,000 - $15,000
- [ ] $15,000 - $25,000
- [ ] > $25,000

**If > $25,000 selected, STOP and use full questionnaire.**

**5.2 Expected monthly AI API costs**
- [ ] < $50/month
- [ ] $50 - $200/month
- [ ] $200 - $500/month
- [ ] > $500/month

**5.3 Target completion date:** _______________

---

### Section 6: Quick Risk Assessment

**6.1 Is this for production use or just testing/development?**
- [ ] Production
- [ ] Testing/development only
- [ ] Pilot (limited production)

**If "Production" selected and handling any internal data, consider using full questionnaire.**

**6.2 What happens if this solution is unavailable for a day?**
- [ ] No impact (nice-to-have tool)
- [ ] Minor inconvenience
- [ ] Significant disruption
- [ ] Critical business impact

**If "Significant disruption" or "Critical business impact" selected, STOP and use full questionnaire.**

**6.3 Are you customizing/training the AI model?**
- [ ] No (using pre-trained model)
- [ ] Yes - Prompt engineering only
- [ ] Yes - Fine-tuning with company data
- [ ] Yes - Training from scratch

**If fine-tuning or training from scratch, STOP and use full questionnaire.**

---

## Auto-Risk Score Calculation

Based on your responses, your project is likely:

**LOW RISK** if:
- Internal tool (< 50 users)
- Public or non-sensitive internal data only
- Standard tech stack (Claude, OpenAI)
- SSO or MFA authentication
- No production deployment or low business impact
- Budget < $15,000
- No model training (pre-trained only)

**MEDIUM RISK** if:
- Internal tool but 50+ users
- Internal company data (not customer/employee)
- Production deployment with minor business impact
- Budget $15,000 - $25,000
- Prompt engineering customization

**HIGH RISK** (use full questionnaire) if:
- Any sensitive data (PII, PHI, PCI, customer, employee)
- External users
- Critical business impact
- Budget > $25,000
- Model training/fine-tuning
- Compliance requirements

---

## Quick Approval Matrix

### For LOW RISK Projects (based on quick assessment):

**Required:**
- [ ] Team lead approval
- [ ] Security notification (email to Cesar)
- [ ] Standard security checklist (see below)

**Timeline:** 1-3 days for approval

**Security Requirements:**
- [ ] SSO or MFA authentication
- [ ] TLS encryption for all communications
- [ ] No hardcoded API keys (use environment variables)
- [ ] Rate limiting (basic)
- [ ] Input validation
- [ ] Basic logging

**Proceed to development once team lead approves.**

---

### For MEDIUM RISK Projects:

**Required:**
- [ ] Complete **full AI Solution Questionnaire**
- [ ] Security Architect (Cesar) review
- [ ] Department manager approval

**Timeline:** 1-2 weeks for approval

**Additional Security Requirements:**
- [ ] Threat modeling discussion with Cesar
- [ ] Audit logging
- [ ] Data retention policy
- [ ] Security code review (sample)
- [ ] Vulnerability scanning

**Schedule meeting with Cesar before proceeding.**

---

### For HIGH RISK Projects:

**Required:**
- [ ] Complete **full AI Solution Questionnaire**
- [ ] Complete **Risk Scoring Matrix**
- [ ] Security Architect (Cesar) comprehensive review
- [ ] Executive (Zakery) approval
- [ ] Legal/compliance review (if applicable)

**Timeline:** 2-4+ weeks for approval

**Extensive Security Requirements - see full documentation**

**Contact Cesar immediately to discuss before proceeding.**

---

## Standard Security Checklist (Low Risk Projects)

Use this checklist for low-risk projects approved via quick assessment:

### Authentication & Access
- [ ] SSO integration configured (Google Workspace, Azure AD)
- [ ] MFA enabled for all users
- [ ] Session timeout set (24 hours max)
- [ ] Role-based access control (if multiple user types)

### API Security
- [ ] All API keys stored in environment variables (not in code)
- [ ] Use secrets manager (AWS Secrets Manager, HashiCorp Vault, or similar)
- [ ] API rate limiting configured (10-20 requests/minute per user)
- [ ] Input validation on all user inputs
- [ ] Output encoding to prevent XSS

### Data Protection
- [ ] TLS 1.3 for all communications
- [ ] HTTPS only (no HTTP)
- [ ] API keys transmitted securely
- [ ] No sensitive data logged
- [ ] Session data encrypted (if stored)

### Infrastructure
- [ ] VPN access if internal tool (Tailscale recommended)
- [ ] Firewall rules configured
- [ ] Monitoring and alerting set up
- [ ] Backup procedures documented

### AI Provider Security
- [ ] Confirmed AI provider has DPA
- [ ] Verified data usage policy (no training on your data)
- [ ] Set up usage alerts and budget caps
- [ ] API key rotation plan documented

### Code & Deployment
- [ ] No secrets in code repository
- [ ] Dependencies scanned for vulnerabilities
- [ ] Docker containers used (if applicable)
- [ ] Deployment documented (runbook)

### Monitoring & Logging
- [ ] Error logging configured
- [ ] API usage tracked
- [ ] Cost monitoring enabled
- [ ] Uptime monitoring (if production)

### Documentation
- [ ] User documentation written
- [ ] Architecture diagram created
- [ ] Security decisions documented
- [ ] Contact info for issues (on-call)

---

## Quick Start Templates

### Template 1: Simple Chatbot (Low Risk)

**Use Case:** Internal team Q&A bot, no sensitive data

**Tech Stack:**
- Frontend: React + TypeScript
- Backend: Python FastAPI
- AI: Anthropic Claude API
- Auth: SSO (Google Workspace)
- Deployment: Docker on company server

**Estimated Time:** 2-3 weeks
**Estimated Cost:** $5,000 - $10,000 + $50-200/month

**Security:** Standard checklist above

**Approval:** Team lead → Start building

---

### Template 2: Document Search (RAG) (Low Risk)

**Use Case:** Search internal documentation, non-sensitive

**Tech Stack:**
- Frontend: React + TypeScript
- Backend: Python FastAPI
- Database: PostgreSQL with pgvector
- AI: Anthropic Claude API
- Auth: SSO (Google Workspace)
- Deployment: Docker on company server

**Estimated Time:** 4-6 weeks
**Estimated Cost:** $15,000 - $25,000 + $100-300/month

**Security:** Standard checklist + document access controls

**Approval:** Team lead + Cesar notification → Start building

---

### Template 3: Data Analysis Assistant (Medium Risk)

**Use Case:** Query internal databases with natural language

**Tech Stack:**
- Frontend: React + TypeScript
- Backend: Python FastAPI
- Database: PostgreSQL (read-only access)
- AI: Anthropic Claude API
- Auth: SSO (Google Workspace)
- Deployment: Docker on company server

**Estimated Time:** 6-8 weeks
**Estimated Cost:** $20,000 - $35,000 + $150-400/month

**Security:** Enhanced checklist (use full questionnaire)

**Approval:** Cesar review + Department manager → Proceed after approval

---

## FAQs

**Q: My project seems borderline. Should I use quick assessment or full questionnaire?**
A: When in doubt, use the full questionnaire. It's better to be thorough than to miss important security considerations.

**Q: Can I start building while waiting for approval?**
A: For low-risk projects with team lead approval, yes. For medium/high-risk, wait for security review.

**Q: What if my requirements change during development?**
A: Notify Cesar immediately if scope, data types, or users change. May require re-assessment.

**Q: I'm not sure which AI provider to use. Can you help?**
A: Yes! Contact Chris (AI Specialist) for model selection guidance, or Cesar for security/compliance considerations.

**Q: My project failed the quick assessment (too high risk). What now?**
A: Complete the full AI Solution Questionnaire and Risk Scoring Matrix, then schedule a meeting with Cesar to discuss your project.

**Q: Can I use this quick assessment for customer-facing applications?**
A: No. Customer-facing applications always require the full questionnaire and comprehensive security review.

**Q: How often do I need to re-assess?**
A: Re-assess whenever:
  - Scope changes significantly
  - User base grows (especially if reaching external users)
  - Data types change (especially if adding sensitive data)
  - Moving from dev/test to production
  - Adding new integrations or features
  - At least annually for ongoing projects

---

## Decision Tree: Which Assessment Should I Use?

```
Start: I want to build an AI solution
    ↓
Is it customer-facing or public?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Does it handle PII, PHI, PCI, or regulated data?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Is budget > $25,000?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Is it mission-critical (major impact if down)?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Will it make automated decisions affecting people?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Is it for production use with > 50 users?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
Are you training/fine-tuning the model?
    ├─ Yes → USE FULL QUESTIONNAIRE
    └─ No → Continue
        ↓
✓ USE QUICK ASSESSMENT
```

---

## Submission & Review Process

### For Low Risk Projects (Quick Assessment)

1. **Complete this quick assessment**
2. **Get team lead approval** (email or Slack)
3. **Notify Cesar** (email with completed assessment)
4. **Wait 1-3 business days** for security acknowledgment
5. **Proceed with development** using standard security checklist
6. **Update Cesar when deployed** to production

**Email Template:**
```
To: cesar@alsolutions.com
Subject: AI Project Notification - [Project Name]

Hi Cesar,

I've completed the AI Solution Quick Assessment for [project name].
Team lead [name] has approved.

Summary:
- Project type: [chatbot/RAG/etc.]
- Users: [number] internal users
- Data: [public/internal non-sensitive]
- Budget: $[amount]
- Timeline: [date]

Quick assessment attached. This appears to be low risk.
I'll follow the standard security checklist during development.

Please acknowledge receipt. Let me know if you need any additional info.

Thanks,
[Your name]
```

### For Medium/High Risk Projects

1. **Complete full AI Solution Questionnaire**
2. **Complete Risk Scoring Matrix**
3. **Email both to Cesar**
4. **Schedule meeting to discuss** (30-60 minutes)
5. **Wait for security review** (1-4 weeks depending on risk)
6. **Address any security requirements**
7. **Get formal approval** before starting development
8. **Follow comprehensive security program**

---

## Appendix: Quick Reference

### Low Risk Indicators
✓ < 50 internal users
✓ Public or internal non-sensitive data
✓ Standard AI providers (Claude, OpenAI)
✓ SSO/MFA authentication
✓ Dev/test environment
✓ Budget < $15,000
✓ Pre-trained model (no training)
✓ Low business impact

### High Risk Indicators (Use Full Questionnaire)
✗ External/public users
✗ PII, PHI, PCI, regulated data
✗ Customer or employee data
✗ Production + critical business impact
✗ Budget > $25,000
✗ Model training/fine-tuning
✗ Automated decision-making
✗ Compliance requirements
✗ Complex integrations

### Key Security Requirements (All Projects)
- SSO or MFA authentication
- TLS encryption (HTTPS only)
- No hardcoded secrets
- API rate limiting
- Input validation
- Basic logging
- Secrets in environment variables

### Who to Contact
- **Cesar (Security):** cesar@alsolutions.com - Security questions, risk assessment
- **Chris (AI):** [email] - AI model selection, implementation questions
- **Chase (PM):** [email] - Project coordination, resource questions
- **Chad (Ops):** [email] - Budget, infrastructure, vendor questions

---

**Document Version:** 1.0
**Last Updated:** November 12, 2025
**Next Review:** February 12, 2026
**Owner:** Cesar (Security Architect)

**Questions? Contact cesar@alsolutions.com**

---

## Related Documents

- **Full Assessment:** [AI_Solution_Questionnaire.md](AI_Solution_Questionnaire.md)
- **Risk Scoring:** [AI_Solution_Risk_Scoring_Matrix.md](AI_Solution_Risk_Scoring_Matrix.md)
- **Architecture Templates:** [AI_Integration_Architecture_Templates.md](AI_Integration_Architecture_Templates.md)
- **Analytics Template:** [AI_Solution_Analytics_Template.md](AI_Solution_Analytics_Template.md)
