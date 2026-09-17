## Reviewer #1
Questions
1. Your recommendation
Weak accept
2. Comments to authors
In this paper, the authors propose a contract-driven approach for the security assurance of Big Data service APIs. The framework uses the OpenAPI specification of a target service to automatically generate valid security tests. Unlike traditional API testing, which mainly checks syntax and functional correctness, the approach evaluates the actual security behavior of APIs through semantic test oracles derived from security requirements. The framework organizes security checks into an eight-domain taxonomy and adapts the test coverage according to available user privileges.
The paper addresses an important problem and introduces an interesting idea based on contract-driven security testing and semantic test oracles. The proposed approach has potential value for improving API security assurance in distributed systems. However, I have several concerns.
The experimental evaluation is conducted only on Forgejo 14.0.3 deployed behind Kong 3.9. Although the authors justify Forgejo as a representative API-based service, the validation remains limited to a single application domain (repository management).
Additionally, the choice of Forgejo is understandable from an API security perspective, but the connection with Big Data assurance is not fully convincing. Forgejo is primarily a software development platform rather than a Big Data component. The paper should provide a clearer argument explaining why the identified security requirements and test scenarios are representative of Big Data pipelines and data management services.
Finally, the experimental results mainly report PASS/FAIL/SKIP outcomes and the number of findings. However, these metrics do not provide enough information about the effectiveness of the approach.


## Reviewer #2
Questions
1. Your recommendation
Weak accept
2. Comments to authors
This work studies security testing and assurance of Big Data service APIs. Automated Security testing is not yet widespread, and even when it is employed, its evaluation criteria can be difficult to identify.
The authors systematically identify and address some gaps, which are: syntactic blindness, absence of evaluation criteria, portability across domains and lack of deterministic execution.
The proposed solution is application-agnostic and follows a contract-driven approach based on the target’s OpenAPI specifications to build syntactically valid probes. Configuration is the only variant. Authors also propose a taxonomy for organising assurance activities.

Strengths
- Security assurance and testing is indeed a relevant issue, especially at the API layer for Big Data services.
- Deterministic execution is useful also for reproducibility and transparency purposes.
- The proposed taxonomy clearly structures the problem.
- Separation between probing and security-specific oracles is interesting.

Major concerns
- As also pointed out by the authors in the conclusions, only a target (Forgejo) has been tested. Testing other targets could better support generalisability and portability.
- Distinction between “real” vulnerabilities and contract violation could be stressed more.
- Manual definition of oracles limits the level of automation.

Minor comments
- Some claims about the gaps could benefit from additional references.

The work seems interesting and address a critical concern in the Big Data context and the security of it. However, it can benefits from further refinements, especially on the manual definition of oracles and the distinction between real vulnerabilities and contract violations.
Overall I think it as a promising work.