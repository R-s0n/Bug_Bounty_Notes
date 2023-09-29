# Synack Bug Bounty Hunting Methodology

## Automation

1. Find vuln (nuclei or other steamrolling disruptor of services)
2. Pull analytics to check if category/endpoint has been reported
3. Create vuln report using API (standard POST method)
4. Automate screenshot (imagemagick or similar)
5. PUT method to attach screenshots
6. POST method to submit

### Types of Targets

#### Wide-Scope w/ Subdomains
- Wait for them to come to you
- Move *quick* on New/Updated

#### Large App w/ Creds 
- Go out and find the bugs
- Slow, mythodical, manual

### Enumeration
1. Exposed Admin Endpoint
    1. Admin Functions Write ($800)
    2. Admin Functions Read ($600)
2. Information Disclosure
    1. Directory Contents Disclosed ($150)
    2. Directory Structure Enumeration ($170)
    3. Identity of Network Topology ($150)
    4. Identity of Software Architecture ($150)
    5. Leaked Credentials (username/password) 
        1. High Privilege ($700)
        2. Low Privilege ($250)
    6. Leaked API Keys ($500)
    7. Sensitive Client Information ($300)
        - Compliance/Privacy Related
    8. Sensitive Directory/File Contents ($300)
    9. Sensitive Source Code ($150)

### Scanning

#### Client-side Attacks
1. Reflected Input
    1. Spoof HTML Content ($200)
        1. Cross-Site Scripting
            1. Reflected XSS ($330)
                1. + Web Cache Poisoning = $$$
        2. CSS Injection ($330)
2. Dom-Based XSS ($775)
3. Cross-Site Request Forgery
    1. CSRF -- High ($500)
    2. CSRF -- Low ($400)
    

#### Server-side Attacks
1. External Service Interaction
    1. SSRF Full ($1500)
        - Must Allow Full Interaction w/ Internal Application
    2. SSRF Limited ($500)
        - Must Show Interaction w/ Internal IP/Port
2. Improper Input Validation
    1. Bypass Client-Side Validations ($150)
        - Must be Persistant
3. File Upload
    1. Unrestricted File Upload ($180)
4. SQL Injection
    - Run SQLMap and Burp on Every Parameter
    1. Full ($3000)
        - Must Show Database Information
    2. Partial ($1500)
5. File Inclusion/Path Traversal ($850)
6. CLRF Injection ($300)
7. Host Header Injection
    1. Open Mail Relay ($400)
        - Must Send Messages to Arbitrary External Email
8. Blind XSS ($880)
9. XML External Entity
    1. XXE -- Limited ($500)
    2. XXE -- Full ($1500)

### Manual Routine
1. Account Enumeration ($150)
    1. Default Credentials Admin ($700)
    2. Default Credentails Non-Admin ($250)
2. Session Fixation ($100)
3. Captcha Bypass ($200)
4. AWS Misconfigurations ($450)
    1. S3 Bucket Plundering
5. Dependency Confusion ($750)

### Manual Creative
1. Access Control Violation
    1. Admin Functions -- Read/Write or Write Only ($800)
    2. Admin Functions -- Read Only ($600)
    3. Non-Admin Functions -- Read/Write or Write Only ($450)
        - Must Delete or Modify Data From the Personal Directory of Another User
    4. Non-Admin Functions -- Read Only ($300)
        - Must Access Data From the Personal Directory of Another User
2. In-Direct Object Reference
    1. IDOR -- Read Only ($500)
    2. IDOR -- Read and Write ($600)
3. Login Authentication Bypass ($850)
4. 2FA/MFA Authentication Bypass ($500)
5. SSO Authentication Bypass ($750)

