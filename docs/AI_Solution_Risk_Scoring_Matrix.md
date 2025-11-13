# AI Solution Risk Scoring Matrix

**Version:** 1.0
**Owner:** Cesar (Security Architect)
**Purpose:** Quantitative risk assessment framework for AI solution requests
**Last Updated:** November 12, 2025

---

## Overview

This risk scoring matrix provides a quantitative framework to assess AI solution requests. Each solution receives a **Total Risk Score** that determines the approval level, required security measures, and review process.

### Risk Score Ranges

| Total Score | Risk Level | Approval Required | Security Measures |
|------------|-----------|-------------------|-------------------|
| 0-25 | **Low Risk** | Team Lead approval | Standard security controls |
| 26-50 | **Medium Risk** | Security Architect review | Enhanced security measures |
| 51-75 | **High Risk** | Security + Executive approval | Extensive security review |
| 76-100+ | **Critical Risk** | Executive + Legal approval | Comprehensive security program |

---

## Risk Scoring Categories

### 1. Data Sensitivity Score (0-25 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Data Classification** | | |
| Public data only | 0 | No sensitive information |
| Internal/confidential | 5 | Company internal data |
| Sensitive business data | 10 | Financial, strategic, or competitive data |
| PII (Personally Identifiable Information) | 15 | Names, emails, addresses, phone numbers |
| Highly sensitive PII | 20 | SSN, financial accounts, health records |
| Regulated data (PHI, PCI) | 25 | HIPAA, PCI-DSS, or other regulated data |

**Additional Modifiers:**
- **Volume of sensitive records:**
  - < 100 records: +0 points
  - 100-1,000 records: +2 points
  - 1,000-10,000 records: +5 points
  - 10,000-100,000 records: +8 points
  - > 100,000 records: +10 points

- **Data retention:**
  - Ephemeral (no storage): +0 points
  - Short-term (< 30 days): +2 points
  - Long-term (> 30 days): +5 points
  - Permanent/indefinite: +8 points

**Maximum Score for Data Sensitivity: 43 points**

---

### 2. Third-Party Data Sharing Score (0-20 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Data sent to third-party AI providers** | | |
| No third-party sharing | 0 | Self-hosted or no external calls |
| Non-sensitive data only | 5 | Public or internal data sent to API |
| Sensitive data with DPA | 10 | Data Processing Agreement in place |
| Sensitive data without DPA | 15 | No legal agreement established |
| Regulated data to third-party | 20 | PHI, PCI, or regulated data sent externally |

**Additional Modifiers:**
- **Third-party can use data for training:**
  - No: +0 points
  - Unknown: +5 points
  - Yes: +10 points

- **Data residency concerns:**
  - Data stays in approved regions: +0 points
  - Data may cross borders: +5 points
  - Data goes to restricted countries: +10 points

**Maximum Score for Third-Party Sharing: 45 points**

---

### 3. Authentication & Access Control Score (0-15 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Authentication Strength** | | |
| Strong auth (MFA + SSO) | 0 | Multi-factor with enterprise SSO |
| MFA or SSO (not both) | 3 | One strong method |
| Username/password only | 7 | Basic authentication |
| API key only | 10 | Shared secret authentication |
| No authentication | 15 | Public/anonymous access |

**Additional Modifiers:**
- **Access control model:**
  - Role-Based Access Control (RBAC): +0 points
  - Basic user/admin roles: +3 points
  - No access controls: +7 points

- **External user access:**
  - Internal users only: +0 points
  - Partner/customer access: +3 points
  - Public access: +7 points

**Maximum Score for Authentication & Access: 29 points**

---

### 4. API & Integration Security Score (0-15 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **External API calls (outbound)** | | |
| No external APIs | 0 | Self-contained solution |
| Trusted/established APIs (1-2) | 3 | Well-known providers (Google, AWS, etc.) |
| Multiple external APIs (3-5) | 7 | Several integrations |
| Many external APIs (6+) | 10 | Complex integration landscape |
| Untrusted/unknown APIs | 15 | New or unvetted providers |

**Additional Modifiers:**
- **API exposed to external systems (inbound):**
  - No external exposure: +0 points
  - Internal systems only: +2 points
  - Partner/customer systems: +5 points
  - Public API: +10 points

- **API security measures:**
  - Rate limiting + encryption + auth: +0 points
  - Missing one measure: +3 points
  - Missing two+ measures: +7 points

**Maximum Score for API & Integration: 32 points**

---

### 5. Infrastructure & Deployment Score (0-10 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Deployment model** | | |
| On-premise (controlled) | 0 | Full control over infrastructure |
| Private cloud (dedicated) | 2 | Isolated cloud environment |
| Public cloud (standard) | 5 | AWS, Azure, GCP standard services |
| Third-party SaaS | 7 | Fully managed by vendor |
| Multi-cloud/hybrid | 10 | Complex, distributed architecture |

**Additional Modifiers:**
- **High availability requirements:**
  - Standard availability: +0 points
  - HA required (99.9%+): +3 points
  - Mission-critical (99.99%+): +5 points

- **Internet exposure:**
  - VPN/private network only: +0 points
  - Internet-facing with firewall: +3 points
  - Public internet access: +5 points

**Maximum Score for Infrastructure: 20 points**

---

### 6. Data Pipeline & Processing Score (0-10 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Pipeline complexity** | | |
| No pipeline (direct API call) | 0 | Simple request/response |
| Single database, basic processing | 3 | One data store, minimal ETL |
| Multiple data sources, ETL | 7 | Complex data transformations |
| Real-time streaming + multiple DBs | 10 | High complexity, multiple systems |

**Additional Modifiers:**
- **Data processing volume:**
  - Low (< 1GB/day): +0 points
  - Medium (1-100GB/day): +2 points
  - High (100GB-1TB/day): +5 points
  - Very high (> 1TB/day): +7 points

- **Real-time requirements:**
  - Batch processing: +0 points
  - Near real-time (minutes): +2 points
  - Real-time (seconds): +5 points

**Maximum Score for Data Pipeline: 22 points**

---

### 7. Model Training & Customization Score (0-10 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Training requirements** | | |
| Pre-trained model (no training) | 0 | Using model as-is |
| Prompt engineering only | 2 | Customized prompts |
| RAG (no model training) | 3 | Retrieval augmented |
| Fine-tuning with supervised data | 7 | Custom training on company data |
| Training from scratch | 10 | Building custom model |

**Additional Modifiers:**
- **Training data sensitivity:**
  - Public/synthetic data: +0 points
  - Internal company data: +3 points
  - Customer/PII data: +7 points
  - Regulated data (PHI, PCI): +10 points

- **Training frequency:**
  - One-time: +0 points
  - Periodic (monthly+): +2 points
  - Continuous/automated: +5 points

**Maximum Score for Model Training: 25 points**

---

### 8. Compliance & Regulatory Score (0-15 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Regulatory requirements** | | |
| No specific requirements | 0 | Standard business application |
| Industry best practices | 3 | Following NIST, OWASP, etc. |
| SOC 2 / ISO 27001 | 7 | Security certifications required |
| GDPR / CCPA | 10 | Privacy regulations |
| HIPAA / PCI-DSS | 15 | Strict compliance mandates |

**Additional Modifiers:**
- **Geographic/data residency:**
  - Single region, no restrictions: +0 points
  - Multi-region with requirements: +3 points
  - Restricted countries/regions: +7 points

- **Audit requirements:**
  - No formal audit required: +0 points
  - Internal audit: +2 points
  - External audit/certification: +5 points

**Maximum Score for Compliance: 27 points**

---

### 9. Technical Risk Score (0-10 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **Solution maturity** | | |
| Proven technology stack | 0 | Established, well-documented |
| Some new components | 3 | Mix of proven and new |
| Mostly emerging technology | 7 | Cutting-edge, limited documentation |
| Experimental/research | 10 | Unproven technology |

**Additional Modifiers:**
- **Vendor dependency:**
  - Open source / multi-vendor: +0 points
  - Single vendor, alternatives exist: +2 points
  - Single vendor, locked-in: +5 points
  - Startup vendor, uncertain future: +7 points

- **Scalability concerns:**
  - Well-defined scaling path: +0 points
  - Some scaling unknowns: +2 points
  - Significant scaling challenges: +5 points

**Maximum Score for Technical Risk: 22 points**

---

### 10. Business Impact Score (0-10 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| **User impact if compromised** | | |
| Internal tool, limited users | 0 | < 10 users, low impact |
| Department-level tool | 3 | 10-100 users, moderate impact |
| Organization-wide tool | 7 | 100+ users, high impact |
| Customer-facing application | 10 | External users, brand impact |

**Additional Modifiers:**
- **Revenue impact:**
  - No direct revenue impact: +0 points
  - Indirect revenue support: +2 points
  - Direct revenue generation: +5 points

- **Automated decision-making:**
  - Suggestions/recommendations only: +0 points
  - Automated with human review: +3 points
  - Fully automated decisions: +7 points

**Maximum Score for Business Impact: 22 points**

---

## Total Risk Score Calculation

### Scoring Summary Table

| Risk Category | Your Score | Max Possible |
|--------------|-----------|--------------|
| 1. Data Sensitivity | ___ / 43 | 43 |
| 2. Third-Party Data Sharing | ___ / 45 | 45 |
| 3. Authentication & Access Control | ___ / 29 | 29 |
| 4. API & Integration Security | ___ / 32 | 32 |
| 5. Infrastructure & Deployment | ___ / 20 | 20 |
| 6. Data Pipeline & Processing | ___ / 22 | 22 |
| 7. Model Training & Customization | ___ / 25 | 25 |
| 8. Compliance & Regulatory | ___ / 27 | 27 |
| 9. Technical Risk | ___ / 22 | 22 |
| 10. Business Impact | ___ / 22 | 22 |
| **TOTAL RISK SCORE** | **___ / 287** | **287** |

### Normalized Risk Score (0-100 scale)

**Normalized Score = (Total Score / 287) × 100 = ___**

---

## Risk Level Determination

Based on your **Normalized Risk Score**:

- [ ] **0-25 points: LOW RISK**
- [ ] **26-50 points: MEDIUM RISK**
- [ ] **51-75 points: HIGH RISK**
- [ ] **76-100+ points: CRITICAL RISK**

---

## Approval & Security Requirements by Risk Level

### LOW RISK (0-25 points)

**Approval Requirements:**
- Team Lead or Department Manager approval
- Security notification (informational)
- Standard project review

**Security Requirements:**
- [ ] Basic security controls (authentication, encryption in transit)
- [ ] Standard logging and monitoring
- [ ] Secure coding practices
- [ ] Dependency scanning
- [ ] Basic testing (unit + integration)

**Timeline:** 1-2 days for approval
**Security Review:** Self-assessment with checklist

---

### MEDIUM RISK (26-50 points)

**Approval Requirements:**
- Department Manager approval
- Security Architect (Cesar) review and approval
- Privacy/compliance review if PII involved
- Standard project governance

**Security Requirements:**
- [ ] Enhanced security controls (MFA, RBAC, encryption at rest)
- [ ] Comprehensive logging and audit trails
- [ ] Security code review (sample or targeted)
- [ ] SAST (Static Application Security Testing)
- [ ] Vulnerability scanning
- [ ] Security testing (basic penetration testing)
- [ ] Secure configuration management
- [ ] Incident response plan

**Timeline:** 1-2 weeks for approval
**Security Review:** Security Architect assessment + threat modeling

---

### HIGH RISK (51-75 points)

**Approval Requirements:**
- Executive Sponsor (Zakery) approval
- Security Architect (Cesar) comprehensive review
- Legal/Compliance review
- Privacy Officer review (if applicable)
- Formal project governance (PMO)

**Security Requirements:**
- [ ] Comprehensive security architecture review
- [ ] Threat modeling workshop
- [ ] Detailed security design document
- [ ] Complete security code review
- [ ] SAST + DAST (Dynamic Application Security Testing)
- [ ] Comprehensive penetration testing
- [ ] Security testing throughout SDLC
- [ ] Data flow analysis and classification
- [ ] Third-party security assessments (vendor reviews)
- [ ] Data Processing Agreements (DPAs) with vendors
- [ ] Formal incident response and disaster recovery plans
- [ ] Security monitoring and alerting (24/7 if applicable)
- [ ] Regular security reviews (quarterly)

**Timeline:** 2-4 weeks for approval
**Security Review:** Full security assessment with external review if needed

---

### CRITICAL RISK (76-100+ points)

**Approval Requirements:**
- Executive Leadership Team approval (Zakery + others)
- Board notification (if applicable)
- Security Architect (Cesar) comprehensive review + sign-off
- Legal Counsel review and approval
- Privacy Officer approval
- Compliance Officer approval
- Risk Management Committee review
- Third-party security audit may be required

**Security Requirements:**
- [ ] **All High Risk requirements PLUS:**
- [ ] Formal Security Assessment by external firm
- [ ] Red team assessment
- [ ] Compliance audit (SOC 2, HIPAA, etc.)
- [ ] Privacy Impact Assessment (PIA)
- [ ] Data Protection Impact Assessment (DPIA) if GDPR
- [ ] Ongoing security monitoring (SOC/SIEM)
- [ ] Formal change management process
- [ ] Regular external penetration testing (annual minimum)
- [ ] Security training for all team members
- [ ] Insurance review (cyber liability)
- [ ] Executive risk acceptance documentation
- [ ] Continuous compliance monitoring
- [ ] Third-party vendor audits
- [ ] Bug bounty program consideration

**Timeline:** 4-8+ weeks for approval
**Security Review:** Comprehensive security program with ongoing oversight

**Special Requirements:**
- Quarterly security reviews with executive leadership
- Annual third-party audit
- Breach notification procedures tested
- Cyber insurance coverage verified
- Legal risk assessment and documentation

---

## Risk Mitigation Strategies

### For High Data Sensitivity Scores (30+ points)

**Required Mitigations:**
1. **Data minimization:** Only collect/process necessary data
2. **Encryption:** At rest and in transit (TLS 1.3+)
3. **Data masking:** Mask/anonymize in non-production environments
4. **Access controls:** Strict RBAC, least privilege
5. **Audit logging:** Comprehensive logging of all data access
6. **Data retention:** Clear policies with automated deletion
7. **DLP (Data Loss Prevention):** Scan for sensitive data in outputs

**Consider:**
- Tokenization for highly sensitive data
- Field-level encryption
- Hardware security modules (HSM) for key management

---

### For High Third-Party Sharing Scores (30+ points)

**Required Mitigations:**
1. **Data Processing Agreement (DPA):** Legal agreement with vendor
2. **Data review:** Audit exactly what data is sent to third-party
3. **Opt-out of training:** Ensure vendor doesn't use data for model training
4. **Data residency:** Confirm data stays in approved regions
5. **Vendor assessment:** Security review of third-party provider
6. **Alternative evaluation:** Consider self-hosted alternatives
7. **Contractual protections:** Liability, breach notification, data deletion

**Consider:**
- Self-hosted AI models to eliminate third-party sharing
- Synthetic data generation for testing
- Anonymization before sending to third-party

---

### For High Authentication/Access Scores (20+ points)

**Required Mitigations:**
1. **Implement MFA:** Multi-factor authentication for all users
2. **SSO integration:** Enterprise single sign-on
3. **RBAC:** Role-based access control with least privilege
4. **Session management:** Timeout, secure tokens, revocation
5. **Authentication monitoring:** Failed login alerts, anomaly detection
6. **Password policies:** Strong requirements if passwords used
7. **API security:** Rotating keys, OAuth 2.0, rate limiting

**Consider:**
- Certificate-based authentication for services
- Biometric authentication for high-security scenarios
- Just-in-time (JIT) access provisioning

---

### For High API/Integration Scores (20+ points)

**Required Mitigations:**
1. **API inventory:** Document all APIs (inbound and outbound)
2. **API gateway:** Centralized management, rate limiting
3. **Authentication:** OAuth 2.0, JWT, or API keys with rotation
4. **Encryption:** TLS 1.3 for all API communications
5. **Rate limiting:** Protect against abuse and DDoS
6. **Input validation:** Sanitize all inputs to prevent injection
7. **API monitoring:** Log all API calls, alert on anomalies
8. **Vendor assessment:** Review security of third-party APIs

**Consider:**
- API versioning and deprecation strategy
- Circuit breakers for external API failures
- API contract testing

---

### For High Compliance Scores (20+ points)

**Required Mitigations:**
1. **Compliance mapping:** Document how solution meets requirements
2. **Legal review:** Engage legal counsel early
3. **Privacy review:** Conduct Privacy Impact Assessment
4. **Data classification:** Label and track regulated data
5. **Audit trails:** Comprehensive, immutable logging
6. **Policy documentation:** Privacy policy, terms of service
7. **User consent:** Obtain and track consent where required
8. **Data rights:** Implement right to access, deletion, portability
9. **Breach procedures:** Notification process documented and tested

**Consider:**
- Third-party compliance audit/certification
- Compliance automation tools
- Regular compliance training for team

---

### For High Technical Risk Scores (15+ points)

**Required Mitigations:**
1. **Proof of Concept (PoC):** Validate technology before full build
2. **Vendor due diligence:** Assess vendor stability and roadmap
3. **Contingency planning:** Identify alternatives and migration path
4. **Scalability testing:** Load testing and performance validation
5. **Technical documentation:** Architecture, runbooks, troubleshooting
6. **Team training:** Ensure team has necessary skills
7. **Pilot/beta phase:** Limited rollout before full production

**Consider:**
- Advisory board or expert consultation
- Open-source alternatives to reduce vendor lock-in
- Modular architecture to allow component swapping

---

### For High Business Impact Scores (15+ points)

**Required Mitigations:**
1. **High availability:** Redundancy, failover, load balancing
2. **Disaster recovery:** Backup and restore procedures tested
3. **Monitoring:** Proactive alerting, uptime monitoring
4. **Incident response:** Clear procedures and team training
5. **Communication plan:** Stakeholder notification procedures
6. **Change management:** Controlled rollout, rollback capability
7. **User training:** Ensure users understand limitations and risks
8. **Bias/fairness testing:** For automated decision-making systems
9. **Human oversight:** Review mechanisms for AI decisions

**Consider:**
- SLA (Service Level Agreement) definition
- Business continuity planning
- Insurance coverage

---

## Risk Scoring Examples

### Example 1: Internal Chatbot for HR Questions (Low Risk)

| Category | Score | Reasoning |
|----------|-------|-----------|
| Data Sensitivity | 10 | Internal data, some employee info |
| Third-Party Sharing | 10 | Anthropic Claude API, DPA in place |
| Authentication & Access | 3 | MFA + SSO |
| API & Integration | 3 | One trusted API (Anthropic) |
| Infrastructure | 2 | Private cloud |
| Data Pipeline | 3 | Simple RAG, single vector DB |
| Model Training | 3 | RAG, no training |
| Compliance | 3 | Best practices, no specific regulations |
| Technical Risk | 3 | Proven tech stack |
| Business Impact | 3 | Internal only, 50 users |
| **Total** | **43** | **Medium Risk** |

**Normalized Score: 15 (Low-Medium Risk)**

---

### Example 2: Customer-Facing Document Analysis with PHI (Critical Risk)

| Category | Score | Reasoning |
|----------|-------|-----------|
| Data Sensitivity | 35 | PHI, 50,000+ patient records, long-term storage |
| Third-Party Sharing | 25 | Regulated data to third-party, no DPA yet |
| Authentication & Access | 3 | MFA + SSO planned |
| API & Integration | 12 | Multiple APIs, public-facing |
| Infrastructure | 8 | Public cloud, internet-facing |
| Data Pipeline | 17 | Complex ETL, real-time processing, high volume |
| Model Training | 17 | Fine-tuning on PHI data |
| Compliance | 22 | HIPAA + SOC 2 required |
| Technical Risk | 10 | Some new/emerging tech |
| Business Impact | 17 | Customer-facing, revenue-generating |
| **Total** | **166** | **Critical Risk** |

**Normalized Score: 58 (High-Critical Risk)**

---

### Example 3: Code Review Assistant (Internal Dev Team) (Low Risk)

| Category | Score | Reasoning |
|----------|-------|-----------|
| Data Sensitivity | 5 | Internal code, no customer data |
| Third-Party Sharing | 5 | Non-sensitive code to API |
| Authentication & Access | 0 | SSO + MFA |
| API & Integration | 3 | One trusted API |
| Infrastructure | 0 | On-premise |
| Data Pipeline | 0 | Direct API call, no storage |
| Model Training | 2 | Prompt engineering |
| Compliance | 0 | No specific requirements |
| Technical Risk | 0 | Proven technology |
| Business Impact | 3 | Internal team of 20 developers |
| **Total** | **18** | **Low Risk** |

**Normalized Score: 6 (Low Risk)**

---

## Risk Acceptance Documentation

For **Medium Risk and above**, document risk acceptance:

**Project Name:** ___
**Risk Score:** ___ (Risk Level: ___)
**Date:** ___

### Identified Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| | Low/Med/High | Low/Med/High | |
| | Low/Med/High | Low/Med/High | |
| | Low/Med/High | Low/Med/High | |

### Residual Risk After Mitigation

**Residual Risk Score:** ___ (Expected after mitigations)
**Residual Risk Level:** Low / Medium / High / Critical

### Risk Acceptance

**Accepted By:**
- **Security Architect (Cesar):** _______________ Date: ___
- **Executive Sponsor:** _______________ Date: ___
- **Legal (if required):** _______________ Date: ___
- **Compliance (if required):** _______________ Date: ___

**Conditions of Acceptance:**
1. ___
2. ___
3. ___

**Review Schedule:** Quarterly / Annually / As-needed

---

## Continuous Risk Monitoring

For approved solutions, monitor risk indicators:

**Monthly Review:**
- [ ] Security incidents or near-misses
- [ ] Compliance audit findings
- [ ] Performance/availability issues
- [ ] Cost overruns
- [ ] User complaints or concerns

**Trigger for Re-assessment:**
- Major feature changes or scope expansion
- Security incident or breach
- Compliance requirement changes
- Vendor changes or API updates
- Scale increase (10x users or data volume)
- Migration to new infrastructure

---

## Risk Score Tracking Over Time

| Review Date | Risk Score | Risk Level | Changes | Reviewer |
|------------|-----------|-----------|---------|----------|
| Initial | | | | Cesar |
| | | | | |
| | | | | |

---

## Appendix: Quick Reference Guide

### When to Use This Matrix

1. **During initial intake:** Requester or Chase completes preliminary scoring
2. **During security review:** Cesar completes comprehensive scoring
3. **Before major changes:** Re-score if scope/data/architecture changes significantly
4. **Periodic review:** Re-score high/critical risk solutions quarterly

### Tips for Accurate Scoring

- **Be conservative:** When in doubt, score higher
- **Consider future state:** Score based on production, not just MVP
- **Aggregate modifiers:** Don't forget the +points modifiers
- **Document rationale:** Note why you scored each category
- **Seek input:** Consult with technical team on details

### Common Scoring Mistakes

❌ **Mistake:** Scoring MVP/prototype instead of production system
✅ **Correct:** Score based on planned production deployment

❌ **Mistake:** Ignoring modifiers (volume, frequency, etc.)
✅ **Correct:** Add all applicable modifier points

❌ **Mistake:** Underestimating third-party data sharing risk
✅ **Correct:** Carefully assess what data goes to external APIs

❌ **Mistake:** Not considering data sensitivity in aggregate
✅ **Correct:** Even if individual records are low-risk, large volumes increase risk

---

**Document Version:** 1.0
**Last Updated:** November 12, 2025
**Next Review:** February 12, 2026
**Owner:** Cesar (Security Architect)

**For questions or risk assessment assistance, contact:**
- **Cesar (Security Architect)** - cesar@alsolutions.com
