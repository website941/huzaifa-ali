# UK Level 3 Computing Coursework Portfolio: Web Architecture & Digital Services
Author: Computing Student
Units: Web Development, Website Architecture, Cybersecurity, and User Experience (UX)
Date: 15/09/2026

=============================================================================
PORTFOLIO SPECIFICATION & TECHNICAL ANALYSIS
=============================================================================

-----------------------------------------------------------------------------
ORGANISATION 1: BRITISH BROADCASTING CORPORATION (BBC)
-----------------------------------------------------------------------------
- Sector: UK Public Media & Broadcasting
- Founded: 18 October 1922 (Royal Charter established 1927)
- Headquarters: Broadcasting House, Portland Place, London W1A 1AA
- Regulatory Body: Ofcom (Office of Communications) under the BBC Royal Charter
- Statutory Basis & Governance: BBC Board, led by Chair and Director-General Tim Davie, regulated by Ofcom
- Key Digital Mission: The BBC is the world’s leading public service broadcaster, providing impartial news, television, radio, BBC iPlayer, BBC Sounds, and educational content to millions of daily users across the UK and internationally. Its web and mobile platforms handle hundreds of millions of daily requests with strict uptime, accessibility, and editorial accuracy mandates.
- Tagline: "Nation Shall Speak Peace Unto Nation — Public Service Broadcasting & Digital Media"

1. CORE DIGITAL SERVICES
  * BBC News Multi-Platform [Category: Information & Journalism]
    Description: Continuous 24-hour breaking news, regional UK reports, live text commentaries, and impartial verified reporting.
    Target Audience: UK public and global readership (over 450m weekly users).
    Technical Delivery: Distributed microservices architecture delivering dynamic JSON feeds cached across Tier-1 CDN edge servers in sub-50ms latency.

  * BBC iPlayer Video-on-Demand [Category: Streaming Media]
    Description: Ultra HD 4K, HDR, and live TV streaming of BBC One, Two, Three, Four, and extensive boxsets with parental controls and TV Licence verification.
    Target Audience: UK licence-fee payers accessing via web browsers, smart TVs, mobile apps, and consoles.
    Technical Delivery: Adaptive Bitrate Streaming (ABR) using HLS and MPEG-DASH formats with DRM encryption and edge-cached video segments.

  * BBC Sounds Audio Platform [Category: Audio & Podcasts]
    Description: Live broadcast of 10 national radio stations, 40+ local stations, on-demand podcasts, curated music mixes, and offline downloads.
    Target Audience: Audio listeners seeking news, drama, comedy, and music curation.
    Technical Delivery: Real-time AAC/MP3 audio stream ingestion through AWS Elemental MediaStore with GraphQL metadata aggregation.

  * BBC Bitesize Educational Portal [Category: Education & Learning]
    Description: Curriculum-aligned revision materials, interactive quizzes, video tutorials, and study aids for KS1, KS2, KS3, GCSE, and National 5 students.
    Target Audience: UK primary, secondary students, teachers, and home-educating parents.
    Technical Delivery: Accessible static-generated HTML5 modules with interactive React assessment engines and high WCAG 2.2 AA accessibility scoring.

  * BBC Sport Real-Time Centre [Category: Sports & Live Telemetry]
    Description: Live scores, football tables, Premier League match commentaries, Formula 1 telemetry, and video highlights packages.
    Target Audience: Sports fans wanting instant, verified match updates.
    Technical Delivery: Low-latency WebSocket connections and Server-Sent Events (SSE) pushing instant score changes directly to client viewports.

  * BBC Weather Forecast Service [Category: Meteorological Data]
    Description: Pinpoint hourly UK and international weather forecasts, rain radars, UV warnings, and severe weather alert banners.
    Target Audience: General public and UK emergency planning authorities.
    Technical Delivery: Ingestion of Met Office gridded meteorological API data parsed into spatial GeoJSON layers and rendered via client-side vector charts.

2. WEB ARCHITECTURE & TECHNOLOGY STACK (LEVEL 3 ANALYSIS)
- Architecture Summary: The BBC operates one of the world’s most sophisticated cloud-native web architectures. Built upon its internal Global Experience Language (GEL) and micro-frontends framework, the platform decouples editorial content authoring from multi-channel delivery using headless CMS engines, AWS multi-region infrastructure, and geo-distributed CDN caching.
- Data Pipeline Flow: Editorial staff publish through the CPS/Nitro CMS -> Stored in DynamoDB & Aurora -> GraphQL Federation layer aggregates entities -> Edge CDN nodes (Fastly & Akamai) cache HTML/JSON -> Client browser receives semantic HTML5 + modern CSS/JS bundles with progressive enhancement.
- Core Architectural Components Analyzed:
  * Component: HTML5 Semantic Markup & Accessibility
    Technology Stack: Semantic HTML5, WAI-ARIA 1.2, Microdata schema.org
    Role & Explanation: HTML forms the foundational skeleton of every BBC webpage. The BBC uses strict semantic tags such as <header>, <nav>, <main>, <article>, <section>, and <footer>. This semantic clarity ensures that assistive technologies (screen readers like JAWS, NVDA, and VoiceOver) can interpret page hierarchies effortlessly. Headings (H1 to H6) follow strict sequential numbering without skipping levels.
    Level 3 Grading Criteria: Level 3 Assessment Note: Semantic HTML improves search engine ranking (SEO) by defining meaningful content boundaries, enables web crawlers to categorize news topics, and complies with UK Public Sector Bodies Accessibility Regulations (PSBAR 2018).

  * Component: Cascading Style Sheets (CSS3 & GEL)
    Technology Stack: CSS3, Modern CSS Grid, Flexbox, BBC Global Experience Language (GEL)
    Role & Explanation: CSS controls typography, visual hierarchy, colour contrast, and responsive layout across phones, tablets, smart TVs, and desktop monitors. The BBC GEL framework enforces mathematical font sizing (BBC Reith font family), consistent 8px spatial grid gutters, and strict 4.5:1 minimum colour contrast ratios to guarantee readability.
    Level 3 Grading Criteria: Level 3 Assessment Note: Fluid CSS Grid and media queries (@media (min-width: 768px)) ensure responsive reflow without horizontal scrolling, reducing mobile bounce rates and data consumption.

  * Component: JavaScript & Progressive Enhancement
    Technology Stack: TypeScript, ES2022, React, Server-Side Rendering (SSR), Web Workers
    Role & Explanation: JavaScript powers interactive components such as the iPlayer video scrubber, live score tickers, navigation drawers, and weather search autocomplete. Crucially, the BBC follows the "Progressive Enhancement" philosophy: core news text is pre-rendered on the server so if a user has JavaScript disabled or suffers a slow connection, the article remains fully readable.
    Level 3 Grading Criteria: Level 3 Assessment Note: Code-splitting and tree-shaking prevent oversized JS bundles, keeping initial Time-to-Interactive (TTI) under 2.0 seconds over 4G mobile networks.

  * Component: Application Programming Interfaces (APIs)
    Technology Stack: RESTful JSON APIs, GraphQL Apollo Federation, WebSocket Streams
    Role & Explanation: APIs connect the user-facing web applications to back-end editorial databases and media streaming encoders. The BBC uses GraphQL federation to allow front-end components to query only the precise fields needed (e.g. headline, thumbnail, publishedTimestamp), eliminating over-fetching.
    Level 3 Grading Criteria: Level 3 Assessment Note: Rate limiting and API gateway throttling prevent denial-of-service spikes during major breaking news broadcasts (e.g., General Elections).

  * Component: Content Management System (CMS)
    Technology Stack: BBC CPS (Content Production System), Headless Nitro Editorial Engine
    Role & Explanation: Journalists in London, Salford, Glasgow, and worldwide author articles, audio clips, and video bulletins within CPS. The CMS validates editorial standards, spelling, copyright licensing, and legal defamation checks before pushing articles to the publishing pipeline.
    Level 3 Grading Criteria: Level 3 Assessment Note: Decoupled (headless) CMS architecture separates the authoring UI from presentation servers, preventing editorial bottlenecks from affecting public-facing web uptime.

  * Component: Database Layer & Data Storage
    Technology Stack: Amazon Aurora PostgreSQL, AWS DynamoDB, Redis Cluster, Amazon S3
    Role & Explanation: The BBC uses a polyglot persistence strategy. Structured relational data (user accounts, permissions, TV licence validation) resides in Amazon Aurora PostgreSQL with multi-Availability Zone replication. High-velocity unstructured data (live article comments, telemetry, programme metadata) is stored in Amazon DynamoDB for millisecond key-value retrieval, backed by Redis in-memory caching.
    Level 3 Grading Criteria: Level 3 Assessment Note: ACID compliance in PostgreSQL guarantees that account data cannot become corrupted during simultaneous transactions.

  * Component: Cloud Hosting & Infrastructure
    Technology Stack: Amazon Web Services (AWS eu-west-1 & eu-west-2), Kubernetes (EKS)
    Role & Explanation: Rather than maintaining physical on-premise servers for web delivery, the BBC hosts its digital estate in AWS multi-region infrastructure. Containerised microservices run inside Amazon Elastic Kubernetes Service (EKS) pods that automatically scale up or down based on incoming CPU load and network traffic.
    Level 3 Grading Criteria: Level 3 Assessment Note: Auto-scaling allows the BBC to surge from 10,000 requests per second to 250,000 requests per second within three minutes during major national events.

  * Component: Content Delivery Network (CDN) & Edge Caching
    Technology Stack: Fastly & Akamai Multi-CDN Architecture, Anycast DNS, Varnish (VCL)
    Role & Explanation: The BBC utilizes a multi-CDN strategy combining Fastly and Akamai. When a user requests an article or an iPlayer video segment, the request is routed to the geographically nearest CDN Point of Presence (PoP) in London, Manchester, Edinburgh, or Belfast. 95%+ of all requests are served directly from cache memory.
    Level 3 Grading Criteria: Level 3 Assessment Note: CDNs absorb distributed denial-of-service (DDoS) volumetric attacks before malicious requests can touch the BBC origin servers.

  * Component: HTTPS, SSL/TLS 1.3 & Transport Security
    Technology Stack: TLS 1.3, Strict Transport Security (HSTS), Perfect Forward Secrecy
    Role & Explanation: Every connection to bbc.co.uk is enforced over HTTPS using TLS 1.3 encryption. HTTP Strict Transport Security (HSTS) with preloading in modern browsers instructs the browser to never communicate over unencrypted HTTP, preventing Man-in-the-Middle (MitM) eavesdropping.
    Level 3 Grading Criteria: Level 3 Assessment Note: Encryption protects users reading sensitive political, health, or investigative journalism from being monitored by third parties on open Wi-Fi networks.

  * Component: User Authentication & BBC ID
    Technology Stack: BBC Account Identity Service, OAuth 2.0, OpenID Connect (OIDC), JWT
    Role & Explanation: BBC ID allows users to sign in, verify their TV Licence declaration, save programmes to their iPlayer watchlist, and resume podcast playback across devices. Passwords are salted and hashed using Argon2/bcrypt algorithms. JSON Web Tokens (JWT) manage short-lived stateless sessions.
    Level 3 Grading Criteria: Level 3 Assessment Note: Token-based stateless authentication allows millions of users to remain authenticated across auto-scaled microservices without central session database locks.

  * Component: System Monitoring, Telemetry & Observability
    Technology Stack: Datadog, Prometheus, Grafana, CloudWatch, Synthetic User Monitors
    Role & Explanation: BBC engineering teams monitor thousands of metrics per second: HTTP 5xx error rates, page load latency (LCP/FID/CLS), video buffer ratios, and database connection pools. Automated synthetic probes test login and playback every 60 seconds from external UK internet providers.
    Level 3 Grading Criteria: Level 3 Assessment Note: Real-time alerting via PagerDuty notifies on-call DevOps engineers when error rates exceed 0.05% of total traffic.

  * Component: Disaster Recovery & Automated Backup Strategy
    Technology Stack: Multi-Region Active-Active Replication, Automated Daily Snapshots, RTO/RPO
    Role & Explanation: To satisfy public service broadcasting resilience mandates, BBC data is replicated continuously between AWS London (eu-west-2) and AWS Ireland (eu-west-1). If an entire cloud data centre experiences catastrophic failure, automated DNS failover redirects traffic within 30 seconds. Database snapshots are stored in immutable write-once-read-many (WORM) S3 buckets.
    Level 3 Grading Criteria: Level 3 Assessment Note: Recovery Time Objective (RTO) is under 5 minutes for core news services, and Recovery Point Objective (RPO) is zero data loss for news copy.

3. CYBERSECURITY RISK ASSESSMENT & UK LEGAL FRAMEWORKS
- Security Summary: As a high-profile national broadcaster and critical UK digital infrastructure provider, the BBC is continuously targeted by nation-state cyber adversaries, hacktivists, and phishing fraudsters. The BBC operates a dedicated Cyber Security Operations Centre (SOC) enforcing Defence-in-Depth, UK GDPR compliance, and ISO/IEC 27001 standards.
- Statutory Standards & Certifications: UK General Data Protection Regulation (UK GDPR), Data Protection Act 2018, ISO/IEC 27001 Information Security Management, Ofcom Broadcasting Code Security Standards, Cyber Essentials Plus Certification, PCI-DSS Level 1 (Commercial BBC Shop Services)
- Threat Matrix & Mitigations:
  * Threat: Brand Impersonation & TV Licence Phishing Scams [Severity: CRITICAL]
    Attack Vector: Email spoofing, SMS smishing, typo-squatted domain names (e.g. bbc-licence-renew.co.uk).
    Operational Impact: Criminal syndicates send fraudulent emails and SMS messages claiming the recipient’s "BBC TV Licence has expired" or "Payment failed", directing victims to counterfeit lookalike sites that steal banking details.
    Mitigation Strategy: Deployment of DMARC (Domain-based Message Authentication) with strict p=reject policy, active brand monitoring and legal takedown teams, customer education campaigns, and domain blocklisting via the National Cyber Security Centre (NCSC) Protective DNS.
    UK Legal Act: Computer Misuse Act 1990 (Section 1 & 3), Fraud Act 2006.

  * Threat: Distributed Denial of Service (DDoS) on Election Nights [Severity: CRITICAL]
    Attack Vector: SYN floods, UDP amplification, Layer 7 HTTP GET floods attacking un-cached search and live results endpoints.
    Operational Impact: Malicious botnets attempt to flood BBC web servers with gigabits of garbage traffic during general elections or major royal broadcasts, aiming to censor news reporting and sow public panic.
    Mitigation Strategy: Multi-layered DDoS protection via Fastly and Akamai Edge Scrubbing centres, rate limiting per IP, automatic CAPTCHA challenges for anomalous volumetric bursts, and direct peering with major UK ISPs.
    UK Legal Act: Computer Misuse Act 1990 Section 3 (Unauthorised act with intent to impair computer operation).

  * Threat: Cross-Site Scripting (XSS) & Content Injection [Severity: HIGH]
    Attack Vector: Reflected or stored XSS vulnerabilities in user-submitted comments or search input forms.
    Operational Impact: Attackers attempt to inject malicious JavaScript into live news comment sections, feedback forms, or search query parameters, which could steal session tokens or deface reputable headlines.
    Mitigation Strategy: Strict Content Security Policy (CSP) blocking unauthorized script execution, automated HTML entity sanitisation (DOMPurify), React JSX automatic escaping, and disabling eval() functions.
    UK Legal Act: Data Protection Act 2018, OWASP Top 10 A03: Injection.

  * Threat: Account Takeover & Credential Stuffing on BBC ID [Severity: HIGH]
    Attack Vector: Distributed credential stuffing attacks utilizing rotating residential proxy networks.
    Operational Impact: Automated bots test millions of stolen username/password pairs obtained from third-party data breaches against the BBC ID login endpoint to hijack viewer accounts and scrape viewing habits.
    Mitigation Strategy: Web Application Firewall (WAF) bot detection, biometric and behavioural analysis, rate limiting, mandatory multi-factor authentication (MFA) for staff accounts, and integration with "Have I Been Pwned" APIs to block compromised passwords.
    UK Legal Act: UK GDPR Article 32 (Security of Processing), Data Protection Act 2018.

  * Threat: Third-Party Software Supply Chain Vulnerabilities [Severity: HIGH]
    Attack Vector: Malicious dependencies in node_modules, compromised CI/CD build pipelines, outdated third-party scripts.
    Operational Impact: Compromise of an open-source npm library or analytics SDK used on the BBC website could allow attackers to execute arbitrary code or spy on user sessions.
    Mitigation Strategy: Automated Dependabot and Snyk vulnerability scanning in GitHub Actions, Software Bill of Materials (SBOM) tracking, Subresource Integrity (SRI) hashes on external assets, and strict vendor security audits.
    UK Legal Act: ISO 27001 Annex A.15 (Supplier Relationships), NCSC Supply Chain Security Guidance.
- Incident Response Protocol (NCSC 5-Stage Framework):
The BBC CSIRT follows the NCSC 5-phase Incident Response Framework: 1) Identification & Triage (24/7 SOC automated alerting); 2) Containment (network isolation, edge CDN blocking); 3) Eradication (root-cause patch deployment, credential revocation); 4) Recovery (phased service restoration with active synthetic monitoring); 5) Post-Incident Lessons Learned and statutory reporting to the Information Commissioner’s Office (ICO) within 72 hours where personal data is implicated.

4. USER FLOW ARCHITECTURE
- Workflow Scenario: BBC iPlayer Programme Playback & TV Licence Verification Flow
- Objective: Map the complete technical journey of an audience member searching for a documentary, authenticating their BBC Account, verifying licence eligibility, and establishing an encrypted adaptive bitrate video stream.
- End-to-End Steps:
  * Step 1 [Actor: User]: User Lands on BBC iPlayer & Enters Search Query
    User Action: Navigates to bbc.co.uk/iplayer and searches for "Planet Earth III" in the search bar.
    Technical Execution & APIs: Browser sends client-side debounced GET request to /api/v1/search?q=planet+earth with auto-suggest JSON response.

  * Step 2 [Actor: Client Frontend]: Frontend Requests Programme Metadata
    User Action: Transmits GraphQL query to the BBC Apollo Federation Gateway.
    Technical Execution & APIs: GraphQL fetches programme UUID, synopsis, age rating (12), 4K video asset URLs, and subtitle tracks from Nitro CMS repository.

  * Step 3 [Actor: Backend Service]: BBC ID Authentication & TV Licence Verification Check
    User Action: Checks for active session JWT token and verifies TV Licence declaration status.
    Technical Execution & APIs: If no valid token is found, user is redirected to /account/signin with returnUrl query parameter. Postgres database checks user record.

  * Step 4 [Actor: API Gateway]: DRM Token Issuance & Geo-IP Location Validation
    User Action: Validates UK IP address via MaxMind GeoIP database and issues Widevine/FairPlay DRM license key.
    Technical Execution & APIs: Confirms client IP originates from the United Kingdom to comply with rights licensing. Issues encrypted session token.

  * Step 5 [Actor: Database / External]: Adaptive Bitrate Manifest Ingestion & Video Playback
    User Action: Client HTML5 video player requests m3u8 (HLS) or mpd (DASH) manifest from Fastly CDN.
    Technical Execution & APIs: Player measures available bandwidth every 2 seconds, dynamically adjusting bitrate between 1080p, 4K, and 720p chunks to eliminate buffering.

  * Step 6 [Actor: Backend Service]: Telemetry & Viewing Position Save to Redis
    User Action: Background beacon periodically transmits playback timestamp back to BBC Account service.
    Technical Execution & APIs: Saves resume timestamp (e.g., 24m:15s) into Redis cache so user can continue seamlessly on smart TV.

5. INFORMATION ARCHITECTURE & SITE MAP
- Root Domain: bbc.org.uk
- Routes Implemented: Home, About, Services, Architecture, Security, Reviews, Site Map, User Flow, Contact.
