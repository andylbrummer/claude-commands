# Security Review Guidelines

As a security specialist with extensive experience in vulnerability assessment and secure development practices, this document provides comprehensive security guidance tailored to different application types and scopes.

## Application Scope Classifications

### Low Risk: Public Information Sites (Weather, News, Blogs)
- **Risk Level**: Minimal user data, public information only
- **Primary Concerns**: DDoS, defacement, SEO spam injection
- **Data Protection**: Minimal PII collection

### Medium Risk: E-commerce Platforms
- **Risk Level**: Financial transactions, customer data, inventory
- **Primary Concerns**: Payment fraud, data breaches, account takeover
- **Data Protection**: PCI DSS compliance, encrypted payment data

### High Risk: User-to-User Interaction Platforms (Forums, Social Media)
- **Risk Level**: User-generated content, personal communications, reputation systems
- **Primary Concerns**: XSS, CSRF, content injection, privacy violations
- **Data Protection**: Strong user isolation, content moderation

## Core Security Categories

### 1. Authentication & Authorization
#### Requirements by Scope:
- **Low Risk**: Basic rate limiting, optional user accounts
- **Medium Risk**: Multi-factor authentication, secure password policies, session management
- **High Risk**: OAuth2/OIDC, role-based access control, account lockout policies

#### Common Vulnerabilities:
- Weak password requirements
- Session fixation/hijacking
- Privilege escalation
- Insecure direct object references

### 2. Input Validation & Sanitization
#### Requirements by Scope:
- **Low Risk**: Basic form validation, CSRF protection
- **Medium Risk**: SQL injection prevention, file upload validation
- **High Risk**: XSS prevention, content security policy, input encoding

#### Implementation Guidelines:
```
- Validate all input server-side
- Use parameterized queries/prepared statements
- Implement Content Security Policy (CSP)
- Sanitize user-generated content
- Validate file uploads (type, size, content)
```

### 3. Data Protection & Privacy
#### GDPR Compliance Requirements:
- **Data Minimization**: Collect only necessary data
- **Purpose Limitation**: Use data only for stated purposes
- **Storage Limitation**: Implement data retention policies
- **Right to Access**: Provide user data export functionality
- **Right to Erasure**: Implement account/data deletion
- **Data Portability**: Allow users to transfer their data
- **Privacy by Design**: Build privacy into system architecture

#### Implementation by Scope:
- **Low Risk**: Cookie consent, basic privacy policy
- **Medium Risk**: Encrypted data storage, secure data transmission, audit logs
- **High Risk**: End-to-end encryption, data anonymization, regular privacy audits

### 4. Infrastructure Security
#### Requirements by Scope:
- **Low Risk**: HTTPS/TLS, basic firewall rules
- **Medium Risk**: WAF, intrusion detection, secure headers
- **High Risk**: Zero-trust architecture, container security, secrets management

#### Security Headers:
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
```

### 5. API Security
#### Requirements by Scope:
- **Low Risk**: Rate limiting, input validation
- **Medium Risk**: API authentication, request signing
- **High Risk**: OAuth2 scopes, API versioning, comprehensive logging

#### Best Practices:
- Implement proper authentication (JWT, API keys)
- Use HTTPS for all API communication
- Validate and sanitize all API inputs
- Implement rate limiting and throttling
- Log all API access and errors

### 6. Third-Party Dependencies
#### Security Practices:
- Regular dependency updates and vulnerability scanning
- Vendor security assessments
- Supply chain security verification
- Minimal privilege principle for third-party integrations

### 7. Monitoring & Incident Response
#### Requirements by Scope:
- **Low Risk**: Basic error logging, uptime monitoring
- **Medium Risk**: Security event logging, automated alerting
- **High Risk**: SIEM integration, forensic capabilities, incident response plan

#### Key Metrics to Monitor:
- Failed authentication attempts
- Unusual access patterns
- Data export/deletion requests
- System resource usage anomalies

## Security Testing Requirements

### Static Analysis
- Code review for security vulnerabilities
- Dependency vulnerability scanning
- Secrets detection in code repositories

### Dynamic Testing
- **Low Risk**: Basic penetration testing annually
- **Medium Risk**: Quarterly security assessments, automated vulnerability scanning
- **High Risk**: Continuous security testing, red team exercises

### Compliance Auditing
- Regular compliance assessments (GDPR, PCI DSS, SOC 2)
- Third-party security audits
- Penetration testing by certified professionals

## Data Handling Guidelines

### Personal Data Classification
- **Public**: Name, public posts (minimal protection)
- **Internal**: Email, preferences (encrypted at rest)
- **Confidential**: Financial data, private messages (encrypted in transit and at rest)
- **Restricted**: Authentication credentials, payment info (tokenized/hashed)

### Data Retention Policies
- Implement automated data purging
- Maintain audit trails for compliance
- Secure data disposal procedures
- Regular data inventory and classification reviews

## Incident Response Plan

### Response Phases:
1. **Detection**: Automated monitoring, user reports
2. **Containment**: Isolate affected systems, preserve evidence
3. **Eradication**: Remove vulnerabilities, patch systems
4. **Recovery**: Restore services, monitor for recurrence
5. **Lessons Learned**: Document findings, update procedures

### Communication Plan:
- Internal escalation procedures
- Customer notification requirements
- Regulatory reporting obligations
- Media response strategy

## Security Training & Awareness

### Developer Training:
- Secure coding practices
- Common vulnerability patterns (OWASP Top 10)
- Security testing methodologies
- Incident response procedures

### User Education:
- Password security best practices
- Phishing awareness
- Privacy settings configuration
- Reporting security concerns

## Project-Specific Security Profiling

### Creating security-profile.md (Private File)

Each project must maintain a private `security-profile.md` file containing:

#### Developer Interview Framework
Conduct structured interviews with development team covering:

**Project Overview Questions:**
- What type of data does this application collect, process, and store?
- Who are the primary users and what access levels exist?
- What third-party services and APIs are integrated?
- What is the business criticality and potential impact of downtime?
- Are there any known compliance requirements?

**Technical Architecture Questions:**
- What authentication and authorization mechanisms are implemented?
- How is sensitive data encrypted in transit and at rest?
- What logging and monitoring systems are in place?
- How are secrets and credentials managed?
- What is the deployment and infrastructure architecture?

**Data Flow Analysis Questions:**
- Map all data ingestion points and sources
- Identify data processing workflows and transformations
- Document data storage locations and retention policies
- Trace data sharing with external parties
- Identify data export and deletion capabilities

#### Data Collection and Analysis

**Automated Data Discovery:**
```bash
# Example data scanning commands
find . -name "*.sql" -o -name "*.db" | head -20
grep -r "password\|credit_card\|ssn\|email" --include="*.js" --include="*.py" | head -10
rg "(api_key|secret|token)" --type py --type js | head -10
```

**Data Classification Matrix:**
- **Public Data**: Marketing content, public profiles, documentation
- **Internal Data**: Employee information, internal communications, business metrics  
- **Confidential Data**: Customer PII, financial records, proprietary algorithms
- **Restricted Data**: Authentication credentials, payment data, health records

#### Regulatory Compliance Assessment

**HIPAA (Healthcare)**
- **Triggers**: Health information, medical records, patient data
- **Requirements**: 
  - Administrative, physical, and technical safeguards
  - Business associate agreements
  - Risk assessments and workforce training
  - Breach notification procedures (60 days)
- **Technical Controls**: Encryption, access logging, automatic logoff, audit trails

**PCI DSS (Payment Card Industry)**
- **Triggers**: Credit card processing, storage, or transmission
- **Requirements**:
  - Network security controls and firewalls
  - Strong access control measures  
  - Regular monitoring and testing
  - Information security policy maintenance
- **Key Controls**: Tokenization, encryption, vulnerability scanning, penetration testing

**SOX (Sarbanes-Oxley)**
- **Triggers**: Public companies, financial reporting systems
- **Requirements**:
  - Internal controls over financial reporting (ICFR)
  - Management assessment and auditor attestation
  - Segregation of duties for financial processes
  - Change management controls for financial systems
- **Technical Controls**: Database integrity, access controls, change logging, backup procedures

**SEC (Securities and Exchange Commission)**
- **Triggers**: Investment firms, broker-dealers, public company disclosures
- **Requirements**:
  - Cybersecurity risk management and incident disclosure
  - Customer data protection (Regulation S-P)
  - Books and records retention (Regulation 17a-4)
  - Business continuity and disaster recovery planning
- **Technical Controls**: Immutable storage, encryption, audit trails, incident response

#### Security Profile Template

```markdown
# Security Profile - [Project Name]

**Classification**: [Low/Medium/High/Critical Risk]
**Last Updated**: [Date]
**Reviewed By**: [Security Specialist Name]

## Project Overview
- **Purpose**: [Brief description]
- **Users**: [User types and count]
- **Data Types**: [List sensitive data types]
- **Business Impact**: [Revenue/operational impact]

## Regulatory Compliance Status
- [ ] GDPR (if EU users/data)
- [ ] HIPAA (if health data)
- [ ] PCI DSS (if payment data)
- [ ] SOX (if public company)
- [ ] SEC (if securities/investment firm)
- [ ] Other: [Specify]

## Risk Assessment Results
### Critical Findings:
- [List high-priority security issues]

### Medium Findings:
- [List medium-priority security issues]

### Recommendations:
- [Prioritized remediation steps]

## Data Flow Analysis
- **Data Sources**: [List all data inputs]
- **Data Processing**: [Describe transformations]
- **Data Storage**: [List all storage locations]
- **Data Sharing**: [External data sharing]
- **Data Retention**: [Retention policies]

## Technical Security Controls
- **Authentication**: [Current implementation]
- **Authorization**: [Access control model]
- **Encryption**: [At rest and in transit]
- **Logging**: [Security event logging]
- **Monitoring**: [Detection capabilities]

## Compliance Action Items
- [ ] [Specific compliance requirements]
- [ ] [Timeline and responsible party]

## Next Review Date: [Date]
```

#### Implementation Guidelines

**File Security:**
- Store `security-profile.md` outside version control
- Encrypt file if containing sensitive findings
- Restrict access to security team and project stakeholders
- Regular updates (quarterly minimum, or after major changes)

**Review Process:**
- Initial assessment for all new projects
- Annual reviews for low-risk projects  
- Quarterly reviews for medium/high-risk projects
- Immediate review after security incidents or major changes

## Conclusion

Security is not a one-time implementation but an ongoing process that must evolve with threats and business requirements. Regular reviews, updates, and testing are essential for maintaining a robust security posture appropriate to your application's risk level and regulatory requirements.

The project-specific security profile ensures comprehensive understanding of each application's unique risk landscape and compliance obligations.

Remember: The cost of prevention is always lower than the cost of a breach.