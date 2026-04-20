# CS-305-Software-Security
CS-305 Software Security

Artemis Financial is a consulting company that develops individualized financial plans for its customers covering savings, retirement, investments, and insurance. They wanted to modernize their web application by adding secure communication mechanisms, 
specifically a file verification step in the form of a checksum to ensure data integrity during transfers. The existing application had no encryption, no SSL certificate, and no data verification. All of which needed to be implemented.

One thing done well was systematically working through the vulnerability assessment process flow rather than jumping straight to fixes. Running the OWASP Dependency-Check first established a baseline of known vulnerabilities in third-party libraries, 
and distinguishing confirmed false positives from real findings kept the report accurate and actionable. Secure coding matters because vulnerabilities in financial software don't just affect the company, they expose client data that people trust the company to protect. 
For Artemis specifically, a breach involving savings or retirement data would cause both financial and reputational damage that is difficult to recover from.

The dependency-check process was both the most challenging and the most educational part. Managing the NVD API rate limiting, updating the plugin version, and then correctly identifying false positives. 
Particularly CVE-2022-22965 (Spring4Shell) which looked alarming but didn't apply because the project runs on Java 1.8, required understanding both the tool and the underlying vulnerability details rather than just suppressing everything. 
That process of actually reading CVE descriptions and matching them to the application's specific configuration was the most valuable part of the assignment.

Security was added in three layers. First, transport security was established with TLS via a JKS keystore generated with Java's keytool, ensuring all communication runs over HTTPS on port 8443. 
Second, data integrity was addressed at the application layer by implementing SHA-256 hashing using Java's MessageDigest class, giving the recipient a verifiable fingerprint of the transmitted data. 
Third, dependency security was addressed at the build layer by integrating OWASP Dependency-Check into the Maven build pipeline with a documented suppressions file. 
In the future, the same vulnerability assessment process flow would be a useful starting point, beginning with architecture review to scope the effort, then working through static analysis tools like Dependency-Check before moving to manual code review for the areas that actually require it.

Functional testing was done through a combination of JUnit tests (1/1 passing, zero errors) and manual browser verification at https://localhost:8443/hash, confirming the SHA-256 hash returned correctly with the expected data string. 
To verify no new vulnerabilities were introduced by the refactored code, the OWASP Dependency-Check was run again after refactoring. Because the refactoring added no new dependencies, only using java.security.MessageDigest from the standard JDK — no new CVEs appeared in the output. 
The suppressions file also remained valid, confirming the false positive classifications held.

The most reusable tools and practices from this project are the OWASP Dependency-Check Maven plugin for automated library vulnerability scanning, Java's keytool for certificate and keystore management, and the java.security.MessageDigest API for standards-compliant cryptographic hashing. 
The practice of maintaining a documented suppressions.xml file with written justifications for each suppressed finding is something that carries directly into any project that uses dependency scanning, it creates an auditable record of security decisions rather than just silencing the tool. 
The vulnerability assessment process flow diagram is also a useful framework for scoping security reviews on future projects.

The most demonstrable artifacts from this project are the refactored ServerController implementation showing proper SHA-256 hashing with exception handling, the suppressions.xml file with documented false positive justifications showing the ability to read and reason about CVEs rather than just run tools, 
and the practices for secure software report itself demonstrating the ability to communicate security decisions in writing to a non-technical audience. Together these show both technical implementation skill and the analytical thinking required to make informed security decisions, which is more valuable to an employer than just being able to run a scanner.
