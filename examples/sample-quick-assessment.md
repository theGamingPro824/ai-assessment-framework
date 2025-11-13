# Sample Quick Assessment - Internal Documentation Chatbot

**Completion Time:** 7 minutes
**Completed By:** Jane Developer
**Date:** November 12, 2025
**Project Name:** TeamBot - Internal Docs Search Assistant

---

## Section 1: Basic Information

**1.1 What are you building?**
- [x] Chatbot / Q&A assistant
- [ ] Document processing
- [ ] Code assistant
- [ ] Data analysis tool
- [ ] RAG (knowledge base search)
- [ ] Other: _______________

**1.2 Who will use it?**
- [ ] Just me (< 5 users)
- [x] My team (5-20 users)
- [ ] Department (20-50 users)
- [ ] Organization-wide (50+ users)
- [ ] External users

**1.3 What problem does it solve?**

Our engineering team has documentation scattered across Confluence, Google Docs, and GitHub wikis. Engineers waste 2-3 hours per week searching for information. TeamBot will provide instant answers by searching all our internal docs using RAG.

---

## Section 2: Data & Security

**2.1 What type of data will it access?**
- [ ] Public information only
- [x] Internal company documents/data
- [ ] Customer data
- [ ] Employee data (HR, payroll, etc.)
- [ ] Financial data
- [ ] Healthcare data (PHI)
- [ ] Payment information (PCI)
- [ ] Other sensitive data: _______________

**2.2 Will data be sent to a third-party AI provider?**
- [x] Yes - Anthropic Claude
- [ ] Yes - OpenAI
- [ ] Yes - Google
- [ ] Yes - Other: _______________
- [ ] No - Self-hosted model

**2.3 Does the AI provider have a Data Processing Agreement (DPA) in place?**
- [x] Yes
- [ ] No
- [ ] Don't know

**Note:** Our legal team confirmed Anthropic DPA is signed, and they don't train on our data.

---

## Section 3: Technical Architecture

**3.1 Which AI model will you use?**
- [x] Anthropic Claude (Sonnet, Haiku)
- [ ] OpenAI (GPT-4, GPT-3.5)
- [ ] Google Gemini
- [ ] Open-source model
- [ ] Other: _______________

**Model:** Claude 3.5 Sonnet

**3.2 Will you need a database?**
- [ ] No database (API calls only)
- [ ] PostgreSQL
- [x] Vector database (for RAG): Qdrant (self-hosted)
- [ ] Other: _______________

**3.3 Where will this be deployed?**
- [ ] My local machine (dev only)
- [x] Company server (on-premise)
- [ ] Cloud (AWS, Azure, GCP)
- [ ] SaaS / fully managed
- [ ] Don't know yet

**Deployment:** Docker on internal Kubernetes cluster

**3.4 Will this make API calls to external services?** (besides the AI provider)
- [ ] No
- [x] Yes - Which services: Confluence API (read-only), GitHub API (read-only)

---

## Section 4: Access & Authentication

**4.1 How will users authenticate?**
- [x] SSO (Google Workspace, Azure AD, etc.)
- [ ] Username/password with MFA
- [ ] Username/password only
- [ ] No authentication (open access)

**SSO Provider:** Google Workspace (already configured)

**4.2 Who should have access?**
- [ ] Anyone in the company
- [x] Specific team/department: Engineering team only (35 people)
- [ ] Specific people only

---

## Section 5: Budget & Timeline

**5.1 Estimated budget**
- [ ] < $5,000
- [x] $5,000 - $15,000
- [ ] $15,000 - $25,000
- [ ] > $25,000

**Estimated:** $8,000 development + $150/month operational

**5.2 Expected monthly AI API costs**
- [x] < $50/month
- [ ] $50 - $200/month
- [ ] $200 - $500/month
- [ ] > $500/month

**Estimated:** ~$30-40/month based on 35 users, avg 20 queries/user/day

**5.3 Target completion date:** December 15, 2025 (4 weeks)

---

## Section 6: Quick Risk Assessment

**6.1 Is this for production use or just testing/development?**
- [x] Production
- [ ] Testing/development only
- [ ] Pilot (limited production)

**6.2 What happens if this solution is unavailable for a day?**
- [x] Minor inconvenience
- [ ] No impact (nice-to-have tool)
- [ ] Significant disruption
- [ ] Critical business impact

**Note:** Engineers would just search manually like they do now. Not mission-critical.

**6.3 Are you customizing/training the AI model?**
- [ ] No (using pre-trained model)
- [x] Yes - Prompt engineering only
- [ ] Yes - Fine-tuning with company data
- [ ] Yes - Training from scratch

**Customization:** Custom system prompt to format responses for technical documentation

---

## Auto-Risk Assessment Result

✅ **LOW RISK**

**Reasoning:**
- Internal tool (35 engineering users)
- Internal company docs (not customer/employee sensitive data)
- Standard tech stack (Claude, Qdrant)
- SSO authentication
- Production but low business impact
- Budget < $15,000
- No model training (pre-trained + prompt engineering)

**Next Steps:**
1. Get team lead approval ✅ (Approved by Alice Johnson, Engineering Manager)
2. Notify Cesar for security review
3. Proceed with development using Template 2 (RAG with Vector Database)

---

## Approval

**Team Lead:** Alice Johnson
**Approval Date:** November 12, 2025
**Notified Cesar:** November 12, 2025

**Selected Architecture Template:** Template 2 - RAG with Vector Database

**Security Checklist:** Following standard security checklist for low-risk projects

---

## Notes

**Additional Context:**
- Will index ~5,000 documentation pages initially
- Documents update weekly (cron job to re-index)
- Using pgvector was considered but Qdrant chosen for better performance with large doc sets
- Planning to expand to other departments if successful

**Success Metrics:**
- Reduce avg doc search time from 15 min to < 2 min
- 80% user adoption within first month
- 4/5 satisfaction rating

**Timeline:**
- Week 1: Set up infrastructure, ingest docs
- Week 2: Build RAG pipeline, basic UI
- Week 3: Integrate with SSO, polish UI
- Week 4: Testing, deployment, training

---

**Status:** ✅ Approved - Development in progress
