# Testing — Interview

Interview answers for every question in this set.

[← Back to hub](./7_special_testing.md)

### Related docs

- [Fundamentals](./7_special_testing_fundamentals.md) · [Layers](./7_special_testing_layers.md) · [Practices](./7_special_testing_practices.md) · [Tooling](./7_special_testing_tooling.md) · [Interview](./7_special_testing_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Testing](https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/).

### Testing Fundamentals

<details>
<summary>What is software testing, and why is it important?</summary>

### What is software testing, and why is it important?

**Definition:**

Software testing refers to all procedures driving, guiding, and evaluating system and application development. It ensures the quality of both the software being developed and the processes involved.

**How it works:**

#### Importance of Software Testing

- Error Detection: Identifies bugs, discrepancies, or deviations from expected behavior.
- Risk Mitigation: Reduces the probability or impact of a software failure.
- Quality Assurance: Ensures the product aligns with users' expectations and business requirements.
- Customer Satisfaction: Allows for a reliable, user-friendly experience.
- Cost-Efficiency: Early defect detection is essential, as fixing errors becomes more costly over time.
- Process Improvement: Testing provides invaluable data for refining software development processes.

#### Positive Feedback Loops

- Prompting Fixing of Defects
- Building Confidence
- Learning from Mistakes
- Continuous Improvement

#### Areas of Testing

1. Unit Testing: Involves testing individual components or modules in isolation.
2. Integration Testing: Ensures that different parts of the system work together smoothly.
3. System Testing: Validates the product as a whole, usually in an environment closely resembling the production setting.
4. Acceptance Testing: Confirms that the system meets specific business requirements, making this the final validation phase before release.
5. Performance Testing: Assesses how the system performs under various load conditions.
6. Security Testing: Checks for vulnerabilities that could lead to breaches or data compromises.
7. Usability Testing: Focuses on how user-friendly the system is.

#### Common Misconceptions about Testing

1. Role Misinterpretation: Often seen as the epithet for Bug Tracking, Testing digs deeper into Risk Management, Requirement Analysis and Customer Feedback Handling.
2. Test Setup Cost: Initial test setup may appear costly. It is worth investing to avoid higher costs due to system crash or customer retention issues.
3. Defect Discovery Fallacy: Zero Defect assertion after testing is unrealistic. A critical awareness is: "We can't ensure the absence of all defects, but we work tirelessly to minimize these occurrences."
4. Static Analysis Pitfall: Automated scans and code reviews offer a wealth of data but doesn't replace dynamic testing that mimics and inspects live executions.
5. Elimination of Manual Testing: While Automated Testing is robust, the human intellect from exploratory tests gives an edge in uncovering unexpected anomalies.
6. Sprint or Time-Based Delimitation: Testing is viewed as an ongoing process, steadily integrated with Development, investing in every unit engineered.

#### Skillset Proficiency

1. Test-Driven Development (TDD): Composing tests even before building the code can steer a clear development path, magnifying code quality and reduction of bugs.
2. Agile and DevOps Synthesis: Seamless interaction among Development, Testing and Deployment is possible through such cohesive environments.
3. Domain Knowledge Fundamentals: Such expertise aids in meticulous scenario outlining and certification of systems.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Define the difference between verification and validation in software testing.</summary>

### Define the difference between verification and validation in software testing.

**Definition:**

Let's define verification and validation in the context of software testing and distinguish between the two.

**How it works:**

#### Core Distinctions

- Verification: Confirms that the software adheres to its specifications and requirements.

#### It answers "Are we building the product right?"

- Examples: Code reviews, inspections, and walkthroughs.
- Validation: Ensures that the software meets the user's actual needs.

#### It answers "Are we building the right product?"

- Examples: User acceptance testing, alpha and beta testing.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Explain the software development life cycle and the role testing plays in each phase.</summary>

### Explain the software development life cycle and the role testing plays in each phase.

**Definition:**

The Software Development Life Cycle (SDLC) is comprised of several distinct phases that lay the foundation for a successful software project.

**How it works:**

#### Key Phases

1. Requirement Analysis & Planning:
2. Stakeholder Consultation: Engaging with stakeholders to fully understand their needs and expectations, and to establish clear project objectives.
3. Testing Role: Requirement validation via techniques like Prototype Evaluation and Use Case Analysis.
4. Design & Architectural Planning:
5. Document Creation: This involves creating the Software Requirement Specification (SRS) and the High-Level Design document.
6. Testing Role: Design Reviews and Structural Testing ensure the project's design aligns with the defined requirements.
7. Implementation & Coding:
8. Core Functionality: The focus here is on writing code to cater to validated and approved requirements.
9. Unit Testing: Small, independent units of code are tested to confirm they meet their expected behaviors, ensuring reliable building blocks for the larger system.
10. System Testing: The integrated system is tested as a whole, to ensure all features work together cohesively.
11. Phase Segmentation: This is often divided into Alpha & Beta Testing before release.
12. Stress Testing: Conducting experiments in extreme conditions helps assess the system's limits.
13. Deployment & Maintenance:
14. Deployment Verification: Post-deployment tests are conducted to ensure that the system functions correctly in its live environment.
15. Regular Checks: Ongoing maintenance includes periodic checks and updates to keep the software optimized and secure.
16. Code Example: Unit Testing
Here is the Python code:

```python

```

17. def add(a, b):

#### return a + b

- def subtract(a, b):

#### return a - b

- # Unit Tests
- assert add(1, 2) == 3

#### assert subtract(5, 2) == 3

- Code Example: Integration Testing
Here is the Python code:

```python

```

- def add_and_subtract(a, b):

#### return add(a, b), subtract(a, b)

- # Integration Test
- result = add_and_subtract(5, 3)
- assert result == (8, 2)

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What are the different levels of software testing?</summary>

### What are the different levels of software testing?

**Definition:**

Let's look at the various levels of software testing across the development life cycle.

**How it works:**

#### Unit Testing

- What is it? Focuses on testing separate functions and methods.

#### When is it Done? At the time of coding.

- Level of Testing: Isolated.
- Role in Testing Pyramid: Forms Base.
- Tools: JUnit, NUnit, PyTest.
- Key Benefits: Early Error Detection.

#### Component Testing

- What is it? Tests software components, often defined at a high level.

#### When is it Done? After unit testing and before integration testing.

- Level of Testing: Limited Context.
- Role in Testing Pyramid: Foundational.
- Key Benefits: Verifies that units of code work together as expected in defined scenarios.

#### Integration Testing

- What is it? Focuses on the combined units of the application.

#### When is it Done? After component testing and before system testing.

- Level of Testing: Moderate Context.
- Role in Testing Pyramid: Focal.
- Tools: Apache JMeter, LoadRunner.
- Key Benefits: Identifies issues in interfaces between software modules.

#### System Testing

- What is it? Evaluates the complete and integrated system.

#### When is it Done? After integration testing.

- Level of Testing: Comprehensive.
- Role in Testing Pyramid: Primary.
- Key Benefits: Validates system requirements against its delivered functionalities.

#### Acceptance Testing

- What is it? Validates if the system meets specified customer requirements.

#### When is it Done? After system testing.

- Level of Testing: External.
- Role in Testing Pyramid: Apex.
- Key Benefits: Ensures the system is acceptable to end-users.

#### Alpha & Beta Testing

- When is it Done? After acceptance testing; often includes phases after the product launch.

#### Alpha Testing

- For What: Validates the system in a controlled, in-house environment.
- Role in Testing Pyramid: Initial User Feedback Provider.

#### Beta Testing

- For What: Verifies the system in a live, real-time environment, often receiving feedback from a select group of external users.
- Role in Testing Pyramid: Early User Feedback Provider.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Describe the difference between static and dynamic testing.</summary>

### Describe the difference between static and dynamic testing.

**Definition:**

Static testing and dynamic testing complement each other to deliver comprehensive code verification. Whereas static testing focuses on examining software without executing it, dynamic testing verifies software in an operational environment. Let's explore these two approaches in more detail.

**How it works:**

#### Core Focus

- Static Testing: Analyzes code or documentation to identify issues without executing the program.
- Dynamic Testing: Involves code execution to identify and evaluate program behavior.

#### Timing

- Static Testing: Typically conducted earlier in the development cycle, aiding in identifying issues at their root.
- Dynamic Testing: Typically performed later in the development cycle, after code integration, to assess system functionality and user interaction.

#### Tools and Techniques


#### Static Testing

- Manual Reviews: Human experts manually inspect code and documentation.
- Automated Tools: Software applications are used to analyze code or documentation for possible issues. Examples include code linters, IDE integrations, and spell-checkers for documentation.

#### Dynamic Testing

- ** Unit Testing**: Evaluates the smallest units of code, such as individual functions or methods, in isolation.
- Integration Testing: Validates that modules or components work together as expected.
- ** System Testing**: Assesses the complete, integrated software product to ensure it meets predefined requirements.
- Acceptance Testing: Determines if a system meets a set of business requirements and/or user expectations. It often involves end-users executing the system.

#### Code Coverage

- Static Testing: Offers some line coverage to ensure that all code is correct and complete, but does not guarantee execution.
- Dynamic Testing: Provides comprehensive execution, ensuring that code is run as intended under different scenarios.

#### Test Objectives

- Static Testing: Primarily aims to identify issues such as code violations, design errors, and documentation problems.
- Dynamic Testing: Seeks to validate system functionality and user interaction in a real or simulated environment.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What is a test case, and what elements should it contain?</summary>

### What is a test case, and what elements should it contain?

**Definition:**

A test case represents a single lookup procedure for issues and errors.

Each test case should encompass specific attributes to ensure its efficacy as a validation tool in the software testing process:

ID: Unique identifier, possibly generated by an automated system.

Title: Descriptive summary of the test case.

Description: Detailed account of the test case purpose, prerequisite steps, and expected outcomes.

Steps to Reproduce: Procedural guide to replicate the test environment and provoke the expected result.

Expected Outcome: Clearly defined desired or optimal test result or post-test state.

Actual Outcome: Recorded results from test execution for comparison with the expected outcome.

Comments: Space for the addition of ancillary information relating to the test case or its particular actions.

Assigned To: Identification of the tester or group responsible for executing the test case.

Last Updated: Timestamp noting the most recent alteration to the test case, including modifications to any of its elements.

Code Example: Test Case Attributes

**How it works:**

Here is the Java code:

```java

```

#### import java.util.List;

- import java.time.LocalDateTime;
- public class TestCase {
- private int id;
- private String title;
- private String description;
- private List<String> stepsToReproduce;
- private String expectedOutcome;
- private String actualOutcome;
- private String comments;
- private String assignedTo;
- private LocalDateTime lastUpdated;
- // Constructor, getters and setters
- }

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Explain the concept of coverage in testing and the main types of coverage (e.g., line, branch, path, statement).</summary>

### Explain the concept of coverage in testing and the main types of coverage (e.g., line, branch, path, statement).

**Definition:**

Coverage testing involves evaluating the extent to which pieces of code are executed. This assessment helps in identifying areas of code that are not tested.

Key metrics for coverage include:

Line coverage: The percentage of executable lines of code that are exercised by a test suite.

Branch coverage: The percentage of decision points in the code where both possible outcomes have been tested at least once.

Path coverage: This is the ideal scenario where every possible path through a program has been executed at least once. Achieving comprehensive path coverage can be impractical for larger programs.

Statement coverage: The simplest type of coverage, evaluating if each statement in the program has been executed at least once during testing.

**How it works:**

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What is the difference between white-box, black-box, and grey-box testing?</summary>

### What is the difference between white-box, black-box, and grey-box testing?

**Definition:**

White-box, black-box, and grey-box testing serve different purposes and are often used in tandem.

**How it works:**

#### Black-Box Testing

- In Black-Box testing, the tester is unfamiliar with the internal workings of the system. Tests are designed based on the system's specifications or requirements, ensuring that both the functional and non-functional requirements are met.
- This type of testing is commonly performed during early development stages.
- White-Box Testing
- White-Box testing requires in-depth understanding of the internal structures of the system or software under test.
- It is a structural testing method that ensures all or most paths and operations within the software are tested.
- This testing method is also known as clear box testing, and glass box testing, and is more appropriate for later stages in the software development lifecycle.
- Grey-Box Testing
- In Grey-Box testing, the tester has access to some internal structures or algorithms of the software under test.
- This kind of testing strives to achieve a balance between test coverage and system functionality. It combines the benefits of both white-box and black-box approaches.
- Grey-Box testing is considered the most practical for real-world scenarios, especially when testing web applications, APIs, or distributed systems.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What is regression testing, and why is it performed?</summary>

### What is regression testing, and why is it performed?

**Definition:**

Regression testing is a specialized testing mechanism designed to ensure that recent code changes do not adversely impact the existing functionality of a system.

**How it works:**

#### Core Goals

1. Stability Assurance: Verifying that modifications don't introduce new defects and issues.
2. Consistency Check: Ensuring unchanged features maintain their intended behavior.
3. Prevention of Software Degradation: Identifying areas where changes break established features.

#### Common Triggers for Regression Testing

- Defect Resolution: Upon fixing a bug, testing other areas to confirm no new faults emerged.
- Feature Enhancements: Implementing new functionality while certifying existing features continue to work unaltered.
- Code Refactoring: Maintaining previous functionality after making structural or architectural modifications.

#### Methods of Regression Testing

- Complete
- The exhaustive testing of all functionalities within an application. Although thorough, this method is time-consuming and often impractical for larger systems.
- Selective
- Targeting specific features or components known to interact with the newly modified code segments. This approach is more efficient but requires careful identification of relevant test scenarios.
- Unit-Test-Driven Regression
- Relies on the continuous and automated execution of unit tests. This ensures prompt detection of regressions during development, enabling quick rectification.
- The selective approach is common in industry as complete testing is often infeasible or unnecessarily time-consuming.

#### Tools for Automation

- Various tools are available for automating regression testing, including:
- Jenkins: A continuous integration server that can schedule and execute test jobs at specified time intervals.
- JUnit: A popular unit testing framework for Java, often used in conjunction with build tools like Maven.
- Selenium: A web application testing framework that can automate interactions with web browsers.
- GitLab CI/CD: GitLab's integrated continuous integration/continuous deployment system that allows for the automated execution of tests.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Explain unit testing and which tools you might use for it.</summary>

### Explain unit testing and which tools you might use for it.

**Definition:**

Unit testing involves testing individual components or modules of a system to ensure they perform as expected. It's commonly done in the software development process, helping to identify and fix bugs early.

**How it works:**

#### Core Principles

- Isolation: Each unit should be independent of the system, allowing verification in isolation.
- Repeatability: Tests must be consistent over multiple test runs.
- Speed: Running one test should be quick, enabling frequent runs.
- Consistency across Platforms: Tests should produce consistent results, irrespective of the platform.

#### Tools and Frameworks

- JUnit: For Java-based testing.
- NUnit: For .NET and C# developers.
- JUnit Jupiter API: The latest version of JUnit, designed using Java 8 features.
- TestNG: Java framework supporting multiple test levels.
- RSpec: A BDD-style testing tool for Ruby.
- Google Test: A test framework suitable for C++ projects.
- Mocha and Jasmine: For JavaScript developers.
- PyTest and Unittest: For Python testing.
- Code Example: JUnit 5 for Java
Here is the Java code:

```java

```


#### import static org.junit.jupiter.api.Assertions.assertEquals;

- import static org.junit.jupiter.api.Assertions.assertTrue;
- import org.junit.jupiter.api.Test;
- class MathFunctionsTest {
- MathFunctions mathFunctions = new MathFunctions();
- @Test
- void testAdd() {
- assertEquals(4, mathFunctions.add(2, 2));
- }
- @Test
- void testIsPrime() {
- assertTrue(mathFunctions.isPrime(7));
- }
- }

#### Tools for Automation

- Maven for Java: It's a build automation tool. Along with managing dependencies and building packages, it can run tests.
- Gradle for Java, Groovy, and Kotlin: It's a build tool providing a domain-specific language to configure project automation.
- JUnit Platform Launcher: A test execution engine used with build systems to run JUnit 5-based tests.
- Jenkins: Often used for Continous Integration/Continous Deployment (CI/CD), it can automate test executions.
- GitHub Actions: A CI/CD tool from GitHub focused on automating workflows, such as testing.

#### Assertions and Test Cases

- Assert: Provides methods to validate different conditions. For instance, assertEquals(expected, actual) checks if two values are equal.
- Test Cases: Methods beginning with @Test mark the testing methods.

#### Use of Annotations

- @BeforeAll and @AfterAll: Methods annotated with these are executed once, before and after all test methods, respectively.
- @BeforeEach and @AfterEach: Methods annotated with these run before and after each test method, ensuring a clean test environment.

#### Integration with Development Tools

- IDE Tools: Many modern integrated development environments, such as IntelliJ IDEA or Eclipse, support unit testing directly in the UI.
- Version Control Systems: Tools like Git can be integrated with unit testing for pre-commit and post-commit testing.
- Automated Build Systems: Build systems such as Jenkins and TeamCity can be configured to trigger automated testing.

#### Pitfalls to Avoid

- Inadequate Coverage: The tests might not fulfill all requirements, leading to undetected bugs.
- Over-Reliance on External Systems: External dependencies can fail, resulting in false positives.
- Tightly Coupled Tests: This may complicate code refactorings, as even unrelated changes can break tests.
- Testing Redundancies: Repeating test scenarios is inefficient and may lead to inconsistency.

#### Benefits of Unit Testing

- Early Bug Detection: Issues are identified early in development, reducing remediation costs.
- Improved Code Quality: Dividing code into small testable units promotes cleaner and more maintainable code.
- Refactoring Confidence: Allows for peace of mind when modifying existing code.
- Better Documentation: The test suite can serve as living documentation, outlining the behavior of units.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What is integration testing and what are its types?</summary>

### What is integration testing and what are its types?

**Definition:**

Integration testing evaluates the combined functionality of software components to identify any interfacial issues and ensure that they work seamlessly together.

By testing groups of components as a unit, integration testing aims to:

**How it works:**

#### Identify defects in component interactions.

- Verify that messages/requests are passed correctly.
- Assess systems for correct resource allocation and management.
- Types of Integration Testing

#### Top-Down Strategies

- Advantages:

#### Easier to identify and rectify architectural issues early on.


#### Useful for monitoring the overall progress of the project.

1. Disadvantages:

#### It may be complex to manage, especially in larger systems.

- Top-Down Testing

#### Also known as "Big Bang Integration Testing," this strategy focuses on testing upper layers of the application first.

- It involves:

#### Relying on stubs to simulate lower-level modules (these are temporary stand-ins for actual lower modules).

- Successive integration of lower-level modules, systematically reducing the number of stubs.

#### Bottom-Up Testing

- This strategy, known as "Incremental Integration Testing," follows the opposite approach, testing lower layers first:

#### Form initial test bases using modules from the bottom.

- Use drivers as temporary surrogates for higher-level components.
- Gradually integrate modules upwards.
- Middle-Out Testing
- The "Sandwich" or "Middleware" strategy integrates modules layer-by-layer, starting with the core and expanding outwards.
- Offers a balanced perspective.
- Ensures the core is robust first.

#### Hybrid Approaches

- In reality, integration testing often employs several methodologies to achieve the most comprehensive coverage:
- Federated: Begins with vertical slices of the application, followed by vertical and horizontal integrations.
- Concentric: Works from the inside out or outside in, focusing on specific zones or areas of the application.
- Code Example: Top-Down and Bottom-Up Testing
Here is the Java code:

```java

```


#### // Top-Down Testing

- public class SalesReportServiceTest {
- // Test dependency to CashierService
- }
- // Bottom-Up Testing
- public class CashierServiceTest {
- // Test core functionality
- }

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What is system testing, and how does it differ from other types of testing?</summary>

### What is system testing, and how does it differ from other types of testing?

**Definition:**

System Testing ensures that an integrated system meets its design requirements.

**How it works:**

#### It differs from other testing techniques such as unit testing, integration testing, and acceptance testing in focus, testing scope, and test environment.

- System Testing vs. Unit Testing

#### Focus

- Unit Testing: Tests individual "units" or components in isolation.
- System Testing: Evaluates the entire system as a whole, the way it will be used in its actual environment.

#### Testing Scope

- Unit Testing: Narrow-scope test mode.

#### Mock objects and stubs may be employed to isolate functionality for testing.


#### Such testing is usually automated and performed by developers.

- System Testing: Broad-scope test mode.

#### No components are isolated during testing.

- Typically, these tests are end-to-end, manual, and customer-oriented.

#### Test Environment

- Unit Testing: Relies on a controlled and simulated environment.
- System Testing: Uses a real-world environment.

#### System Testing vs. Integration Testing


#### Focus

- Integration Testing: Evaluates if the interaction and combined functionalities of system components are as expected.
- System Testing: Ensures the entire system behaves per the given requirements.

#### Testing Scope

- Integration Testing: Medium-scope test mode.

#### Focuses on component interactions within the system more than broader system functions.

- System Testing: Broad-scope test mode.

#### Test Environment

- Integration Testing: Uses a partially controlled environment where components are integrated.
- System Testing: Conducted in an environment mirrorring real-world application use.

#### System Testing vs. Acceptance Testing


#### Focus

- Acceptance Testing: Usually refers to testing performed by stakeholders for system approval.
- System Testing: Primarily focuses on product quality assurance and compliance with specific requirements.

#### Testing Scope

- Acceptance Testing: Medium-scope test mode, often involving test cases created by business users.
- System Testing: Broad-scope test mode, ensuring comprehensive system behavior.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>Define acceptance testing and describe its types.</summary>

### Define acceptance testing and describe its types.

**Definition:**

Acceptance Testing (AT) examines whether a system meets its business-related objectives. This serves as the final validation step before the product is released to its end-users or customers.

**How it works:**

#### Types of Acceptance Testing

1. Alpha Testing:
2. Scope: Performed by the in-house team before the product is made available to a few external customers.
3. Goal: Identifies any critical issues and refines features based on direct user feedback.
4. Common in: Software, gaming consoles, and application deployment in private organizations.
5. Beta Testing:
6. Scope: Executed by a select group of external users or customers.
7. Goal: Aims to uncover any final issues or glitches and understand user experience and acceptance.
8. Common in: Software, web applications, and broader consumer-targeted products.
9. Contract Acceptance Testing:
10. Scope: Typically occurs at the point of contractual agreement between two or more entities.
11. Goal: Guarantees the developed product, or service adheres to specified contract requirements.
12. Common in: Government projects, major client engagements, and legal agreements.
13. Regulation Acceptance Testing:
14. Scope: Required to ensure the product complies with specific industry or regional regulations.
15. Goal: Confirms the product satisfies set regulatory conditions or safety standards.
16. Common in: Industries like healthcare, finance, aviation, and pharmaceuticals.
17. User Acceptance Testing (UAT):
18. Scope: Carried out by stakeholders, often end-users or customer representatives.
19. Goal: Validates whether the system performs as per their given requirements and needs.
20. Common in: All industries, especially those with a strong focus on user needs and input.
21. Operational Testing:
22. Scope: Focuses on the operational aspects and the system's ability to perform tasks.
23. Goal: Validates the operational readiness of the system, often under real-world conditions.
24. Common in: Mission-critical systems, defense, emergency response, and large-scale industrial systems.
25. Regulation Acceptance Testing:
26. Scope: Ensures the product complies with industry or regional regulations.
27. Goal: Validates adherence to specific regulations or safety standards.
28. Common in: Sectors such as healthcare, finance, aviation, and pharmaceuticals.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>What are the benefits of test automation?</summary>

### What are the benefits of test automation?

**Definition:**

Automated testing provides numerous advantages, accelerating and enhancing the software development process across key areas.

**How it works:**

#### Key Benefits


#### Reliability and Consistency

- Regular Execution: Automated tests are swiftly run after code changes, ensuring ongoing functionality and reducing the likelihood of regressions.
- Consistency: The absence of human error in test execution leads to more reliable and consistent results.
- Risk Mitigation: Early detection of issues reduces the chances of critical bugs, security vulnerabilities, and downtimes.

#### Time and Cost Efficiency

- Time Saving: Automated tests substantially reduce the time needed for testing, especially in repetitive tasks.
- Cost Effectiveness: The upfront investment in setting up automation is outweighed by long-term savings in manual testing efforts.

#### Insights and Reporting

- Detailed Reports: Automated tests generate comprehensive reports, pinpointing failures and exceptions for accelerated debugging.
- Code Coverage: Automation tools can track the percentage of codebase covered by tests, ensuring thorough testing.
- Actionable Insights: Data from automated tests informs decision-making on code readiness.

#### Team Productivity and Collaboration

- Rapid Feedback: Immediate test results enable developers to promptly address issues, fostering a more agile and iterative development process.
- Streamlined Communication: Known issues are tracked centrally with tools like JIRA, promoting better team coordination.

#### Codebase Integrity

- Continuous Integration and Deployment (CI/CD): Automated tests are intrinsic to CI/CD pipelines, validating code for rapid, reliable releases.
- Version Control Integration: Tools like Git integrate seamlessly with automated tests, ensuring code quality with each commit.
- Improved Code Quality: Early error detection and the ability to enforce coding standards lead to cleaner, more maintainable code.

#### User Satisfaction

- Enhanced UX: By pinpointing issues before deployment, automated tests help deliver a smoother, more reliable user experience.

#### Process Standardization

- Adherence to Standards: Automation leaves little room for deviation, ensuring teams comply with testing procedures and industry best practices.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>

<details>
<summary>When would you choose not to automate test cases?</summary>

### When would you choose not to automate test cases?

**Definition:**

Although automated testing is generally advantageous, there are specific scenarios where manual testing is more appropriate. Such instances are typically characterized by high initial setup costs, limited test repetition needs, or the benefit of human intuition and adaptability.

**How it works:**

#### When to Opt for Manual Testing

- Repetitive One-Time Scenarios: If a test case is complex and unlikely to be repeated, manual testing might be more efficient.
- Data Sensitivity: Cases involving sensitive data, especially in regulated industries, are often better suited for manual testing.
- Exploratory Testing: This method is more about discovering software behaviors organically rather than confirming predetermined ones. It's hard to automate by definition and often benefits from human insight.
- Localized Testing: Some scenarios, like minor changes or specific error resolution, can be more effectively checked through manual testing.
- Initial Test Automation Costs: For projects with small scopes or where setup costs for automated testing are relatively high, manual testing can offer a streamlined alternative.
- Combined Approach: The Best of Both Worlds

#### Blended testing approaches are becoming increasingly popular in the industry. By combining the strengths of both manual and automated testing, teams can achieve comprehensive test coverage and rapid feedback.

- Ground-Up Automation Post-Manual Testing: This method lets testers understand the system, its domain, and the test needs before automating select areas.
- Automation-Assisted Exploratory: Using automated testing tools to guide and streamline exploratory testing can yield more comprehensive coverage.
- Risk-Based Testing with Tools: Prioritizing test cases based on risk and automating high-risk scenarios helps ensure critical functionalities are constantly tested.

#### Must-Know NoSQL Interview Questions in 2026

- software-architecture-and-system-design
- You can also find all 35 answers here 👉

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/testing-interview-questions/

</details>


### Test Design Techniques

<details>
<summary>What is equivalence partitioning in software testing?</summary>

### What is equivalence partitioning in software testing?

**Definition:**

Equivalence partitioning is a black-box technique in which test conditions are equivalence partitions, each exercised by one representative member. An equivalence partition is a subset of the value domain that the specification says the component treats the same way. The ISTQB glossary follows ISO 29119-1 and also calls this partition testing.

**How it works:**

#### Partitions

- Separate valid partitions (accepted values) from invalid ones (rejected values).
- One representative per partition is enough when every member is expected to behave alike.

**When to use:**

Use it when the input domain is large and the spec groups values that should be handled the same way.

**Examples:**

A code field accepts length 6–10. Representatives: `SHORT` (too short), `SAVE10` (valid), `WAYTOOLONG1` (too long).

**Sources:**

- [ISTQB Glossary — equivalence partitioning](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Explain boundary value analysis with an example.</summary>

### Explain boundary value analysis with an example.

**Definition:**

A boundary value is the minimum or maximum value of an ordered equivalence partition. Boundary value analysis is the black-box technique that designs test cases from those boundary values.

**How it works:**

#### What to include

- Test the minimum and maximum of the ordered partition.
- Also test the values just outside it. Defects cluster where the code compares `<` and `<=`.
- Pair it with one interior representative from equivalence partitioning.

**When to use:**

Use it when the domain is ordered: numbers, lengths, dates, or sizes.

**Examples:**

Ages accepted from 18 through 65: 17 (just below), 18 (minimum), 65 (maximum), 66 (just above).

**Sources:**

- [ISTQB Glossary — boundary value analysis](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is state transition testing, and where is it useful?</summary>

### What is state transition testing, and where is it useful?

**Definition:**

State transition testing (finite state testing) is a black-box technique in which test cases exercise elements of a state transition model. The ISTQB glossary traces the definition to ISO 29119-4.

**How it works:**

#### Model elements

- States the item can be in.
- Events that cause a transition.
- The resulting state or action, including an invalid event that a state should reject.

**When to use:**

Use it when behavior depends on history: login, orders, workflows. It is a poor fit for a stateless calculation.

**Examples:**

An account starts `active`. Three failed logins move it to `locked`. A login while `locked` is rejected until an unlock.

**Sources:**

- [ISTQB Glossary — state transition testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Describe the concept of decision table testing.</summary>

### Describe the concept of decision table testing.

**Definition:**

Decision table testing is a black-box technique in which the test conditions are the combinations of conditions and the resulting actions shown in a decision table. The ISTQB glossary follows ISO 29119-4.

**How it works:**

#### Table

- Each rule is one combination of conditions and the actions it produces.
- Each rule becomes at least one test.
- Collapse rules that share an action when a condition does not change the outcome.

**When to use:**

Use it when several conditions combine into different actions, such as eligibility or pricing.

**Examples:**

Logged in and in stock → allow checkout. Logged in and out of stock → show out of stock. Not logged in → send the user to login, whether or not the item is in stock.

**Sources:**

- [ISTQB Glossary — decision table testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is pairwise testing, and what tools support this technique?</summary>

### What is pairwise testing, and what tools support this technique?

**Definition:**

Pairwise testing is a black-box technique in which test cases exercise pairs of parameter-value pairs. Pairwise coverage is the coverage of those pairs. The ISTQB glossary follows ISO 29119-4. Microsoft PICT generates the rows from a model file.

**How it works:**

#### Why pairs

- Full combination of many parameters explodes. Covering every pair is smaller and still hits each two-way interaction.
- PICT’s README uses a volume-creation model: Type, Size, Format method, File system, Cluster size, Compression. One row covers many pairs, such as `{Single, FAT}`.

**When to use:**

Use it for configuration matrices. Do not use it when the spec requires a specific full combination.

**Examples:**

Type: Single, Span, Stripe, Mirror, RAID-5. Format method: Quick, Slow. File system: FAT, FAT32, NTFS. Compression: On, Off. PICT emits a compact set instead of every combination.

**Sources:**

- [ISTQB Glossary — pairwise testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [PICT README](https://github.com/microsoft/pict/blob/main/README.md)
</details>

<details>
<summary>How do you prioritize test cases in a test suite?</summary>

### How do you prioritize test cases in a test suite?

**Definition:**

Prioritization decides which cases run first when time is short. Risk-based testing selects and orders work by risk type and risk level. Risk level is impact combined with likelihood.

**How it works:**

#### Order

- Higher risk level runs earlier and deeper.
- Then usage frequency, recent changes, and defects that escaped before.
- Keep a small smoke set that must pass before the long suite.
- A traceability matrix shows which high-risk requirements still have no passing case.

**When to use:**

Use an explicit order when a full run does not fit the pipeline. When time is ample, still run the high-risk cases first.

**Examples:**

Checkout payment and login outrank a cosmetic footer change.

**Sources:**

- [ISTQB Glossary — risk-based testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Explain error guessing and its effectiveness.</summary>

### Explain error guessing and its effectiveness.

**Definition:**

Error guessing is a test technique in which the test conditions are based on the tester’s knowledge of past failures or failure modes. The ISTQB glossary cites ISO 29119-1. It is experience-based. It does not produce a coverage claim the way equivalence partitioning does.

**How it works:**

#### Limits

- Strength tracks the tester’s knowledge of real failures: empty input, off-by-one, duplicate submit.
- Use it after systematic techniques, often inside an exploratory session.

**When to use:**

Use it when the tester knows the failure history. Do not use it as the only design method on a new domain.

**Examples:**

A form that previously broke on a blank required field and on a very long paste gets those two cases added because they failed before.

**Sources:**

- [ISTQB Glossary — error guessing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is exploratory testing, and how is it structured?</summary>

### What is exploratory testing, and how is it structured?

**Definition:**

Exploratory testing is an approach in which tests are dynamically designed and executed from the tester’s knowledge, exploration of the test item, and previous results. The ISTQB glossary follows ISO 29119-1 and points to the test charter.

**How it works:**

#### Session structure

- A test charter documents the goal of a session.
- Session-based testing plans the work as uninterrupted sessions.
- A tour is a set of exploratory tests organized around one focus.
- The next session changes because of what this session found.

**When to use:**

Use it to learn a product or to hunt risks scripts miss. It does not replace a regression script that must repeat exactly.

**Examples:**

Charter: “Explore checkout with an expired card. Note any message that hides the decline.” Time-box the session and log the paths tried.

**Sources:**

- [ISTQB Glossary — exploratory testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Describe the use-cases for model-based testing.</summary>

### Describe the use-cases for model-based testing.

**Definition:**

Model-based testing is testing based on or involving models. An MBT model is any model used for that purpose. A model-based test strategy means the team derives testware from models.

**How it works:**

#### Generation styles in the glossary

- Offline: cases are generated into a repository for later execution.
- Online: cases are generated and executed at the same time.
- When the model changes, generated tests change with it.

**When to use:**

Use it when behavior is already an explicit model and hand-maintaining the suite would be tedious.

**Examples:**

An order model `draft → paid → shipped → closed` is the MBT model. Generated cases walk each transition.

**Sources:**

- [ISTQB Glossary — model-based testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>


### Test Management

<details>
<summary>What is a test plan, and what should it typically include?</summary>

### What is a test plan, and what should it typically include?

**Definition:**

A test plan is documentation describing the test objectives to be achieved and the means and the schedule for achieving them, organized to coordinate testing. Test planning is establishing or updating that plan. The glossary follows ISO 29119-1. A master test plan coordinates multiple test levels or types. A level test plan typically addresses one test level.

**How it works:**

#### In a tool

- Azure Test Plans creates a plan for a sprint or milestone, with suites that reference test cases.
- You assign configurations and testers, and run until exit criteria are met.
- The next cycle creates a new plan and imports the same cases. Updates to a case show up in every plan that references it.

**Examples:**

Sprint plan “Checkout May”: a payment suite and a shipping suite, each pointing at existing cases, assigned to a tester and a browser configuration.

**Sources:**

- [ISTQB Glossary — test plan](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Azure Test Plans — Create and manage test plans](https://learn.microsoft.com/en-us/azure/devops/test/create-a-test-plan?view=azure-devops)
</details>

<details>
<summary>Describe test strategy and its importance.</summary>

### Describe test strategy and its importance.

**Definition:**

A test strategy describes how to perform testing to reach test objectives under given circumstances. An organizational test strategy describes the generic requirements for testing and how testing is performed in the organization.

**How it works:**

#### Styles the glossary names

- Analytical: testing guided by analysis, of which risk-based testing is the usual case.
- Model-based: testware is derived from models.
- Methodical: a predetermined checklist or quality standard.
- Reactive: testing reacts to the product as it is explored.

**When to use:**

Write a strategy when several teams must test the same way.

**Examples:**

An organizational strategy that says “risk-based at system level, exploratory charters for new UI, automated checks for APIs” tells a new squad which techniques to reach for.

**Sources:**

- [ISTQB Glossary — test strategy](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Explain the concept of a test charter in exploratory testing.</summary>

### Explain the concept of a test charter in exploratory testing.

**Definition:**

A test charter is documentation of the goal or objective for a test session. The glossary lists charter as a synonym and cross-references exploratory testing.

**How it works:**

#### What it fixes

- The mission of one session, not the click-by-click script.
- Session-based testing plans activity as sessions. Session-based test management measures those sessions.
- The tester still designs the next action from what the product just did.

**When to use:**

Use a charter for any exploratory session you need to report.

**Examples:**

“Session: guest checkout with an expired card. Look for messages that hide the decline. 45 minutes.”

**Sources:**

- [ISTQB Glossary — test charter](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is a test data management process?</summary>

### What is a test data management process?

**Definition:**

Test data is the data needed for test execution. Test data management provides that data while ensuring compliance with legislation and standards. Test data preparation selects data from existing databases or creates, generates, manipulates, and edits it.

**How it works:**

#### Two obligations

- Supply the rows, files, and accounts the tests need.
- Stay inside privacy limits. Data privacy in the glossary is protection of personally identifiable information from undesired disclosure.
- Preparation is the hands-on step. Management is the ongoing process around it.

**When to use:**

Use a data process when tests need realistic data that must not be production personal data.

**Examples:**

A checkout test needs a customer, a product, and a card. Generate synthetic rows instead of copying a production customer extract.

**Sources:**

- [ISTQB Glossary — test data management](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Define risk-based testing and its benefits.</summary>

### Define risk-based testing and its benefits.

**Definition:**

Risk-based testing is an approach in which management, selection, prioritization, and use of test activities and resources follow risk types and risk levels. Risk level is impact combined with likelihood. The glossary follows ISO 29119-1.

**How it works:**

#### What changes

- Higher risk level gets earlier and deeper testing.
- A product risk matrix is the technique the glossary names for that view.
- When the schedule shrinks, effort moves off low-impact areas instead of cutting at random.

**When to use:**

Use it when the suite cannot treat every feature as equal.

**Examples:**

Payment failure is high impact and, after a recent change, higher likelihood, so those cases sit in the smoke set. A footer color tweak waits.

**Sources:**

- [ISTQB Glossary — risk-based testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>How do you track and manage defects in software testing?</summary>

### How do you track and manage defects in software testing?

**Definition:**

Defect management is recognizing, recording, classifying, investigating, fixing, and disposing of defects. A defect is an imperfection or deficiency where a work product does not meet its requirements or specifications. Severity is the degree of impact on development or operation.

**How it works:**

#### Lifecycle in the definition

- Record a defect report: occurrence, nature, and status.
- Classify, including severity.
- Investigate, fix, and dispose: fixed, deferred, or rejected with a reason.
- A defect a test level should have found and did not is an escaped defect.

**Examples:**

“Checkout accepts an expired card and shows an empty confirmation.” Severity: blocks purchase. Status moves from open, to fixed, to retested.

**Sources:**

- [ISTQB Glossary — defect management](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is the role of a test lead or test manager in a testing team?</summary>

### What is the role of a test lead or test manager in a testing team?

**Definition:**

A test manager is responsible for project management of testing activities, resources, and evaluation of a test object. On large projects a test leader (lead tester) reports to the test manager and manages one test level or one set of testing activities. Test management is planning, scheduling, estimating, monitoring, reporting, controlling, and completing test activities (ISO 29119-1 in the glossary).

**How it works:**

#### Split

- The test manager owns that process across the effort.
- The test leader runs one level or one slice.
- A test director, when the term is used, manages test managers.

**When to use:**

Name a test manager when several people or levels must be coordinated.

**Examples:**

The test manager sets the system-test schedule. The test leader for API testing assigns suites and reports progress for that level only.

**Sources:**

- [ISTQB Glossary — test manager and test leader](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Describe the challenges in test environment management.</summary>

### Describe the challenges in test environment management.

**Definition:**

Test infrastructure is the test environments, test tools, office environment, and procedures needed to perform testing. The hard part is keeping that infrastructure close enough to production to trust the results, without breaking privacy rules or blocking on systems you do not control.

**How it works:**

#### Pressures

- Environments drift from production, so a pass does not transfer.
- Shared environments are contended, so runs are not repeatable.
- Service virtualization enables virtual delivery of services that are deployed, accessed, and managed remotely.
- Test data still has to comply with legislation.

**When to use:**

Treat environment management as its own work when tests share a long-lived stand or call other teams’ services.

**Examples:**

The payment service is down in the shared stand. A virtual stand-in lets checkout tests run. A later test still hits the real service before release.

**Sources:**

- [ISTQB Glossary — test infrastructure and service virtualization](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What tools are available for test management, and how do they differ?</summary>

### What tools are available for test management, and how do they differ?

**Definition:**

Test management supports planning, scheduling, estimating, monitoring, reporting, controlling, and completing test activities. A test runner’s reporter only records one execution.

**How it works:**

#### Two jobs

- Azure Test Plans stores plans and suites for a sprint, references test cases, and assigns configurations and testers.
- Playwright reporters such as `list` and `json` record `npx playwright test`. They do not store a requirements backlog.
- Use a management tool for plans and history. Use a reporter for the output of an automated run.

**When to use:**

Use both when you have manual session plans and an automated suite.

**Examples:**

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  reporter: [['list'], ['json', { outputFile: 'test-results.json' }]],
});
```

**Sources:**

- [Azure Test Plans](https://learn.microsoft.com/en-us/azure/devops/test/create-a-test-plan?view=azure-devops)
- [Playwright — Reporters](https://playwright.dev/docs/test-reporters)
</details>

<details>
<summary>Explain what a traceability matrix is and how it is used in testing.</summary>

### Explain what a traceability matrix is and how it is used in testing.

**Definition:**

A traceability matrix is a two-dimensional table which correlates two entities, for example requirements and test cases. You can trace in both directions, so you can see coverage and the impact of a change. Traceability is the ability to establish explicit relationships between related work products. The glossary cites the IREB Glossary for traceability.

**How it works:**

#### Both directions

- From a requirement to the cases that exercise it: an empty cell is a gap.
- From a case back to the requirement: a failing case names what is affected.
- When a requirement changes, the row shows which cases to revisit.

**When to use:**

Use it when “did we test this requirement?” must be answerable.

**Examples:**

R1 “user can pay with a valid card” → TC-pay-valid. R2 “expired card is declined” with no test column is an uncovered requirement.

**Sources:**

- [ISTQB Glossary — traceability matrix](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What are the key metrics used to measure test progress and coverage?</summary>

### What are the key metrics used to measure test progress and coverage?

**Definition:**

A metric is a measurement scale and the method used for measurement. Test progress is progress against a baseline. A test progress report includes that progress, risks, and alternatives that need a decision.

**How it works:**

#### Measures the glossary names

- Progress against the baseline: cases run, blocked, still open.
- Name the coverage item. Statement coverage is coverage of executable statements. Pairwise coverage is coverage of pairs of parameter-value pairs.
- Defect detection percentage: defects found by a test level, divided by those found by that level and any other means afterwards.
- Escaped defects are the ones that level should have found and did not.

**When to use:**

Report these when someone must decide whether to continue testing. Do not treat one coverage percentage as the only exit criterion.

**Examples:**

Baseline: 40 cases. Progress: 30 executed, 2 failed, 8 blocked. Statement coverage of the payment module is a separate number.

**Sources:**

- [ISTQB Glossary — test progress](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>


### Automation Testing

<details>
<summary>What are the criteria to decide whether a test should be automated?</summary>

### What are the criteria to decide whether a test should be automated?

**Definition:**

Automate a test when it must be repeated the same way, the expected result is decidable in code, and maintenance stays cheaper than running it by hand.

**How it works:**

#### Checks

- The case will run often: every push, or every regression pass.
- The oracle is stable. Jest’s `expect(2 + 2).toBe(4)` or Playwright’s `toHaveText` can decide pass or fail.
- The UI or API is stable enough that the script will not be rewritten every day.
- Playwright’s CI example runs `npx playwright test` on every push and pull request to `main` or `master`.

**When to use:**

Automate repeatable regression and API checks. Leave exploratory sessions and one-off investigations manual until the behavior settles.

**Examples:**

```js
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

**Sources:**

- [Jest — Using matchers](https://jestjs.io/docs/using-matchers)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>Describe the Page Object Model in automated testing.</summary>

### Describe the Page Object Model in automated testing.

**Definition:**

A page object represents a part of the web application. Playwright’s guide says page objects simplify authoring with a higher-level API and simplify maintenance by capturing selectors in one place.

**How it works:**

#### What moves into the class

- Locators live on the page object.
- Methods name user actions (`goto`, `getStarted`).
- The test calls the page object and asserts on the outcome.

**When to use:**

Use page objects when flows repeat across pages. A single short test does not need a class.

**Examples:**

```ts
import { type Locator, type Page } from '@playwright/test';

export class PlaywrightDevPage {
  readonly getStartedLink: Locator;
  constructor(public readonly page: Page) {
    this.getStartedLink = page.locator('a', { hasText: 'Get started' });
  }
  async goto() {
    await this.page.goto('https://playwright.dev');
  }
  async getStarted() {
    await this.getStartedLink.first().click();
  }
}
```

**Sources:**

- [Playwright — Page object models](https://playwright.dev/docs/pom)
</details>

<details>
<summary>Explain the advantages and disadvantages of record and playback in test automation.</summary>

### Explain the advantages and disadvantages of record and playback in test automation.

**Definition:**

Record-and-playback generates a test from actions in the browser. Playwright’s generator is `npx playwright codegen`. It records actions and can record assertions.

**How it works:**

#### What codegen records

- Clicks and fills, by using the page.
- Assertions from the toolbar: visibility, text, or value.
- Locators that prefer role, text, and test id. `--device="iPhone 13"` sets viewport and user agent.
- Advantage: a first draft appears while you click. Disadvantage: Playwright says to inspect `test-1.spec.ts` and improve it. `--save-storage=auth.json` holds cookies and must stay local.

**When to use:**

Use codegen to bootstrap a test or pick a locator. Fold repeated flows into page objects instead of keeping a long recording as the suite.

**Examples:**

npx playwright codegen demo.playwright.dev/todomvc

**Sources:**

- [Playwright — Test generator](https://playwright.dev/docs/codegen)
</details>

<details>
<summary>What is continuous integration, and how does it relate to test automation?</summary>

### What is continuous integration, and how does it relate to test automation?

**Definition:**

Here, continuous integration means the automated suite runs on each push and pull request. Playwright’s CI guide shows a GitHub Actions workflow that triggers on `push` and `pull_request` to `main` or `master`, then runs `npx playwright test`.

**How it works:**

#### What the workflow does

- Checkout, install Node.js and dependencies, `npx playwright install --with-deps`, run tests, upload the HTML report.
- The Actions tab shows pass or fail. The log shows what was expected and what was received.
- Reports and traces can contain credentials. Upload them only to a trusted store, or encrypt them.

**When to use:**

Use it for any suite you claim will catch regressions. A suite that runs only on a laptop is not continuous integration.

**Examples:**

```yaml
on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npx playwright test
```

**Sources:**

- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>Which automation testing tools are you familiar with, and what are their strengths?</summary>

### Which automation testing tools are you familiar with, and what are their strengths?

**Definition:**

Jest covers JavaScript unit checks, pytest covers Python tests with parametrization, and Playwright Test covers browser end-to-end checks.

**How it works:**

#### What the docs show

- Jest: `toBe` uses `Object.is`. `toEqual` checks fields recursively. `test.each` runs one test over rows.
- pytest: `@pytest.mark.parametrize` runs one function once per argument tuple.
- Playwright Test: Chromium, Firefox, and WebKit, with web-first assertions that retry. The documented status example waits until `.status` has the text `Submitted`. The default assertion timeout in that example is 5 seconds.

**When to use:**

Jest or pytest next to the function. Playwright when the bug only shows up in a browser.

**Examples:**

```js
test.each([
  [1, 1, 2],
  [1, 2, 3],
])('.add(%i, %i)', (a, b, expected) => {
  expect(a + b).toBe(expected);
});
```

**Sources:**

- [Jest — test.each](https://jestjs.io/docs/api)
- [pytest — parametrize](https://docs.pytest.org/en/stable/how-to/parametrize.html)
- [Playwright — assertions](https://playwright.dev/docs/api/class-playwrightassertions)
</details>

<details>
<summary>What is keyword-driven testing, and how does it work?</summary>

### What is keyword-driven testing, and how does it work?

**Definition:**

Keyword-driven testing is a scripting technique in which test scripts contain high-level keywords and supporting files contain the low-level scripts that implement those keywords. A keyword is a phrase that represents actions to perform during test execution. The glossary also calls this action word-driven testing.

**How it works:**

#### Split

- The script reads as `open home`, then `get started`.
- Another file maps each keyword to the real calls.
- People who should not edit locators can still reorder keywords.

**When to use:**

Use keywords when the same actions are recomposed often. A few unique tests are simpler as ordinary code.

**Examples:**

`open home` maps to `page.goto('https://playwright.dev')`. `get started` maps to a click on the link whose text is `Get started`.

**Sources:**

- [ISTQB Glossary — keyword-driven testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>How does test automation fit into a DevOps environment?</summary>

### How does test automation fit into a DevOps environment?

**Definition:**

In a delivery pipeline the automated suite is a gate on the path from commit to deploy. Playwright’s GitHub Actions example runs the tests on every push and pull request and uploads the HTML report.

**How it works:**

#### Place in the pipeline

- The job installs browsers, then runs `npx playwright test`.
- A failed job is visible on the Actions tab before merge.
- The HTML report filters by browser and by passed, failed, skipped, and flaky tests.
- Treat the report as sensitive if it contains tokens.

**When to use:**

Use this when the team deploys more than occasionally and needs the same checks on every change.

**Examples:**

The step `run: npx playwright test` sits after `npx playwright install --with-deps`. The next step uploads `playwright-report/` with `actions/upload-artifact`.

**Sources:**

- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>What are the main challenges you have faced in test automation?</summary>

### What are the main challenges you have faced in test automation?

**Definition:**

The usual failures are generated scripts nobody cleans up, locators that match more than one element, missing browsers on CI, and artifacts that leak secrets.

**How it works:**

#### What the docs warn about

- Codegen output is a draft. Inspect the spec and improve it.
- If several elements match, the generator tightens the locator. Keep role, text, or test id locators.
- CI must run `npx playwright install --with-deps` or the job fails before any assertion.
- HTML reports and traces can contain credentials.

**When to use:**

Budget maintenance time whenever the suite is in the pipeline.

**Examples:**

A recorded login saved with `--save-storage=auth.json` must not be committed. Playwright says to gitignore that file or delete it, because it contains cookies and storage.

**Sources:**

- [Playwright — Test generator](https://playwright.dev/docs/codegen)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>Explain the use of assertions in automated tests.</summary>

### Explain the use of assertions in automated tests.

**Definition:**

An assertion compares an actual result with an expected result and fails the test when they differ. Jest exposes matchers on `expect`. Playwright’s web-first assertions retry until the condition holds or the timeout elapses. The status example’s default timeout is 5 seconds.

**How it works:**

#### Two styles

- Jest: `toBe` uses `Object.is`. `toEqual` checks fields recursively, so two objects with the same contents pass even when they are not the same reference.
- `toBeGreaterThan` and `toBeLessThan` compare numbers.
- Playwright: `await expect(page.locator('.status')).toHaveText('Submitted')` waits and retries after a click.

**When to use:**

`toBe` for primitives. `toEqual` for structure. Web-first assertions for UI that updates after an action.

**Examples:**

```js
import { test, expect } from '@playwright/test';

test('status becomes submitted', async ({ page }) => {
  await page.locator('#submit-button').click();
  await expect(page.locator('.status')).toHaveText('Submitted');
});
```

**Sources:**

- [Jest — Using matchers](https://jestjs.io/docs/using-matchers)
- [Playwright — assertions](https://playwright.dev/docs/api/class-playwrightassertions)
</details>

<details>
<summary>What is data-driven testing, and how can it be implemented?</summary>

### What is data-driven testing, and how can it be implemented?

**Definition:**

Data-driven testing is a scripting technique that uses data files to contain the test data and expected results needed to execute the test scripts. The same script runs once per row.

**How it works:**

#### Two implementations

- Jest `test.each` takes rows. The title can use `%i` to inject values.
- pytest `@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])` runs `test_eval` three times. In the docs, `6*9` fails because `eval('6*9')` is 54, not 42.

**When to use:**

Use it when many input and expected pairs share one procedure. Write separate tests when the steps differ.

**Examples:**

```python
import pytest

@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
```

**Sources:**

- [ISTQB Glossary — data-driven testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [pytest — parametrize](https://docs.pytest.org/en/stable/how-to/parametrize.html)
- [Jest — test.each](https://jestjs.io/docs/api)
</details>


### Performance Testing

<details>
<summary>What is performance testing, and how does it differ from functional testing?</summary>

### What is performance testing, and how does it differ from functional testing?

**Definition:**

Performance testing determines the performance efficiency of a component or system: how it uses time, resources, and capacity while doing its functions. Functional testing asks whether the result is right. Performance testing asks whether that work finishes inside the time and resource limits. A performance testing tool generates load and measures and records performance during execution.

**How it works:**

#### What changes

- A functional check can be one call and one expected body.
- A performance test adds a load profile: how many virtual users run which transactions in a time window.
- k6 says to decide whether you are validating expected traffic or discovering limits. The script can stay the same while the load changes.

**When to use:**

Run it after the functional behavior is known, when the risk is slowness or errors under load.

**Examples:**

k6 records `http_req_duration` (latency) and `http_req_failed` (error rate) for the HTTP calls.

**Sources:**

- [ISTQB Glossary — performance testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>Describe the subtypes of performance testing (e.g., load, stress, capacity).</summary>

### Describe the subtypes of performance testing (e.g., load, stress, capacity).

**Definition:**

The ISTQB glossary separates load, stress, capacity, spike, and endurance. k6 maps goals onto smoke, average load, stress, spike, breakpoint, and soak.

**How it works:**

#### Glossary and k6

- Load testing: behavior under varying loads, usually between anticipated low, typical, and peak usage.
- Stress testing: at or beyond anticipated workloads, or with reduced resources such as memory or servers.
- Capacity testing: testing to evaluate the capacity of a system. k6’s breakpoint test ramps until the system breaks.
- Spike testing: recovery from sudden bursts of peak load back to a steady state.
- Endurance testing: stability under a significant load over a significant period. k6 calls the long run a soak test.
- k6 also names smoke (minimal load) and average load (typical traffic).

**When to use:**

Pick the profile from the question you need answered. k6 says to start simple and add profiles later.

**Examples:**

A checkout API gets an average-load run for a normal day and a spike run for a sale. The requests stay the same; the scenario stages change.

**Sources:**

- [ISTQB Glossary — load, stress, capacity, spike, endurance](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>What are the key performance testing metrics?</summary>

### What are the key performance testing metrics?

**Definition:**

k6’s HTTP metrics are `http_req_duration` (latency) and `http_req_failed` (error rate). A threshold fails the run when its expression is false at the end.

**How it works:**

#### What the output shows

- `http_req_duration` reports avg, min, med, max, p(90), and p(95).
- `http_req_failed` is the share of failed HTTP requests.
- A documented pair is `http_req_failed: ['rate<0.01']` and `http_req_duration: ['p(95)<200']`: under 1% HTTP errors, and 95% of requests under 200 ms.
- Those numbers are pass criteria you set, not universal limits. A browser scenario can also threshold `browser_web_vital_lcp`.

**Examples:**

```javascript
import http from 'k6/http';

export const options = {
  thresholds: {
    http_req_failed: ['rate<0.01'],
    http_req_duration: ['p(95)<200'],
  },
};

export default function () {
  http.get('https://quickpizza.grafana.com');
}
```

**Sources:**

- [Grafana k6 — Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>Explain how to set up a performance test.</summary>

### Explain how to set up a performance test.

**Definition:**

k6’s guide says to decide the traffic pattern first, choose the test type, then schedule that load in `options`. Checks then assert status, headers, and payload.

**How it works:**

#### Sequence

- Name the goal: reliability under expected traffic, or limits under unusual traffic.
- Choose smoke, load, stress, spike, breakpoint, or soak.
- Put the load in a scenario (`constant-arrival-rate`, `ramping-vus`, or another executor).
- Call the API and `check` the response. Add thresholds so the process fails when latency or errors miss the bar.

**Examples:**

```javascript
import { check } from 'k6';
import http from 'k6/http';

export const options = {
  scenarios: {
    my_scenario1: {
      executor: 'constant-arrival-rate',
      duration: '30s',
      preAllocatedVUs: 50,
      rate: 50,
      timeUnit: '1s',
    },
  },
};

export default function () {
  const payload = JSON.stringify({ name: 'lorem', surname: 'ipsum' });
  const headers = { 'Content-Type': 'application/json' };
  const res = http.post('https://quickpizza.grafana.com/api/post', payload, { headers });
  check(res, {
    'Post status is 200': (r) => res.status === 200,
    'Post Content-Type header': (r) => res.headers['Content-Type'] === 'application/json',
    'Post response name': (r) => res.status === 200 && res.json().name === 'lorem',
  });
}
```

**Sources:**

- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>What tools can be used for performance testing, and what factors influence their selection?</summary>

### What tools can be used for performance testing, and what factors influence their selection?

**Definition:**

A performance testing tool generates load and measures and records performance during execution. Grafana k6 is one: scripts describe HTTP load, checks, and thresholds. The hybrid guide also runs a browser scenario beside the HTTP scenario.

**How it works:**

#### What drives the choice

- Protocol: k6’s API guide uses `k6/http`. A page question needs `k6/browser`.
- Pass or fail: thresholds on `http_req_failed` and `http_req_duration` fail the run.
- Load shape: `ramping-vus` versus `constant-arrival-rate`.

**When to use:**

Use k6 for scripted HTTP load or a small number of browser sessions. It is the wrong tool for a single functional assertion with no load.

**Examples:**

The hybrid guide runs an HTTP `ramping-vus` scenario and a one-VU Chromium scenario in one script, with thresholds on both `http_req_duration` and `browser_web_vital_lcp`.

**Sources:**

- [ISTQB Glossary — performance testing tool](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Grafana k6 — hybrid approach](https://grafana.com/docs/k6/latest/using-k6-browser/recommended-practices/hybrid-approach-to-performance)
</details>

<details>
<summary>What is a performance bottleneck, and how is it identified and addressed?</summary>

### What is a performance bottleneck, and how is it identified and addressed?

**Definition:**

A bottleneck is the part of the system that makes latency or errors miss the goal under a chosen load. k6 surfaces it as a failed threshold or as `http_req_duration` and `http_req_failed` moving while the scenario runs.

**How it works:**

#### Find it, then change one thing

- Set the threshold before the run so “too slow” is not a feeling.
- Compare smoke or average load with stress or breakpoint. The profile where the metric breaks points at the limit.
- Watch `browser_web_vital_lcp` separately from HTTP duration so a slow page is not mistaken for a slow API.
- Fix the dependency that dominates that metric, then rerun the same scenario. Do not raise the threshold to hide the miss.

**Examples:**

`http_req_duration` p(95) crosses 200 ms only in the stress scenario, while `http_req_failed` stays under 1%. The limit is latency under peak load, not a flood of HTTP errors.

**Sources:**

- [Grafana k6 — Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>Explain the performance testing lifecycle.</summary>

### Explain the performance testing lifecycle.

**Definition:**

k6’s guide treats the lifecycle as: decide why you are testing, choose the test type, schedule the load, run, and read metrics against thresholds. Then iterate. The guide says to start simple, test frequently, and grow the suite.

**How it works:**

#### Stages

- Reason: expected-traffic reliability, or unusual-traffic limits.
- Type: smoke, average load, stress, spike, breakpoint, or soak.
- Script: requests, checks, scenario, thresholds.
- Run and read `http_req_duration` and `http_req_failed`.
- If a threshold fails, change the system or the goal and run again.

**Examples:**

First run: smoke, checks only. Next: average load with `p(95)<200`. Later: a spike scenario for the sale, using the same checks.

**Sources:**

- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>


### Security Testing

<details>
<summary>What is security testing and why is it important?</summary>

### What is security testing and why is it important?

**Definition:**

Security testing is a test type to determine the security of a component or system. OWASP’s Web Security Testing Guide says developers and QA should test for these flaws because there are not enough specialists to do it alone, and fixing them later costs more.

**How it works:**

#### How to spend the time

- Treat security issues as bugs in code and design.
- Re-prioritize continuously. Test the risks that matter for this application.
- DAST, SAST, and SCA give breadth and repeatable baseline checks. The guide’s techniques cover business logic those tools miss.
- The guide is a set of techniques, not an exhaustive checklist. It names ZAP as an open tool in that ecosystem.

**Examples:**

A payment change gets an authentication check and an injection check in the same cycle as the functional cases.

**Sources:**

- [ISTQB Glossary — security testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [OWASP Web Security Testing Guide — Introduction](https://owasp.org/www-project-web-security-testing-guide/latest/2-Introduction/)
</details>

<details>
<summary>Describe common security threats that testing should address.</summary>

### Describe common security threats that testing should address.

**Definition:**

The OWASP Top 10:2025 is the awareness document for a broad consensus on the most critical web application security risks.

**How it works:**

#### Top 10:2025

- A01 Broken Access Control. A02 Security Misconfiguration. A03 Software Supply Chain Failures.
- A04 Cryptographic Failures. A05 Injection. A06 Insecure Design.
- A07 Authentication Failures. A08 Software or Data Integrity Failures.
- A09 Security Logging and Alerting Failures. A10 Mishandling of Exceptional Conditions.
- A01 stayed first. Its page maps CSRF (CWE-352) and authorization bypass through a user-controlled key (CWE-639).

**Examples:**

For A01, request another user’s object by changing the id you were given. The test fails if that object’s data comes back.

**Sources:**

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [OWASP Top 10:2025 — A01 Broken Access Control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/)
</details>

<details>
<summary>What is OWASP, and why is it significant in security testing?</summary>

### What is OWASP, and why is it significant in security testing?

**Definition:**

OWASP (Open Worldwide Application Security Project) is a nonprofit whose stated vision is no more insecure software. The Top 10 and the Web Security Testing Guide are two documents testers use.

**How it works:**

#### What testers take

- Top 10:2025 is the consensus list of the most critical web application risks, described as a first step toward shipping more secure code.
- The Testing Guide is the consensus on how to test, written so developers and QA can run the techniques.
- The guide names ZAP for repeatable baseline checks and says tools still miss custom business-logic flaws.

**Examples:**

A team adopts the Top 10 as the risk list and the Testing Guide as the technique list, then drops categories that do not apply instead of ticking every page.

**Sources:**

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [OWASP Web Security Testing Guide — Introduction](https://owasp.org/www-project-web-security-testing-guide/latest/2-Introduction/)
</details>

<details>
<summary>Explain SQL injection and how you test for it.</summary>

### Explain SQL injection and how you test for it.

**Definition:**

OWASP’s SQL Injection Prevention Cheat Sheet says an attacker can inject SQL when the application builds a query by concatenating user input. The documented attack string is `tom' or '1'='1`.

**How it works:**

#### Test and the defense

- Find queries built like `"... WHERE user_name = " + request.getParameter("customerName")`.
- Send `tom' or '1'='1` as that parameter. If the statement is concatenated, the text becomes part of the SQL.
- The safe pattern on the same page is a `PreparedStatement` with `?` and `setString`. The database then looks for a username that literally equals that string.
- Allow-list validation is the option when a bind variable is illegal, such as a table name. Escaping all input is listed as strongly discouraged.

**Examples:**

```java
String query = "SELECT account_balance FROM user_data WHERE user_name = ? ";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, custname);
ResultSet results = pstmt.executeQuery();
```

**Sources:**

- [OWASP Cheat Sheet — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
</details>

<details>
<summary>How do you perform authentication and authorization checks in security testing?</summary>

### How do you perform authentication and authorization checks in security testing?

**Definition:**

Authentication failures and broken access control are separate 2025 categories: A07 and A01. Authentication checks ask whether the caller is who they claim to be. Authorization checks ask whether that caller may do this action on this object.

**How it works:**

#### Checks that match the Top 10

- A07: repeat a protected action with no credentials and with a wrong credential. It must not succeed as a signed-in user.
- A01 maps CWE-639, authorization bypass through a user-controlled key. Change the object id from yours to someone else’s and confirm you do not receive their data.
- A01 also maps CSRF (CWE-352): a state-changing request must not succeed only because the browser will send cookies.

**When to use:**

Use both checks on any function that reads or changes a user’s data.

**Examples:**

Signed in as account A, request the order URL using account B’s id. The test fails if B’s order is returned.

**Sources:**

- [OWASP Top 10:2025 — A01](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/)
- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
</details>

<details>
<summary>What tools can be used for security testing?</summary>

### What tools can be used for security testing?

**Definition:**

The OWASP Testing Guide names dynamic application security testing (DAST), static application security testing (SAST), and software composition analysis (SCA). It names ZAP as an open tool. The ISTQB glossary defines penetration testing as dynamic testing that mimics an attacker without causing harm.

**How it works:**

#### What each class is for

- DAST exercises the running application. SAST reads the code. SCA looks at dependencies. Supply chain failures are A03:2025.
- The guide says these tools give breadth. They miss flaws tied to your business rules.
- Investigating each finding takes longer than running the tool.

**When to use:**

Run the automated classes on meaningful changes. Add a manual authorization test where a scanner has no rule for “this user must not see that order.”

**Examples:**

A pipeline runs SAST and SCA on the commit and a ZAP baseline against the test environment. A tester still swaps object ids, because the serious issues sit in custom design.

**Sources:**

- [OWASP Web Security Testing Guide — Introduction](https://owasp.org/www-project-web-security-testing-guide/latest/2-Introduction/)
- [ISTQB Glossary — penetration testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What is penetration testing, and how is it executed?</summary>

### What is penetration testing, and how is it executed?

**Definition:**

Penetration testing is a type of dynamic application security testing to assess weaknesses and vulnerabilities without causing harm by mimicking an attacker.

**How it works:**

#### Constraints

- It is dynamic: the system is running. That distinguishes it from static analysis.
- The tester mimics an attacker and is expected not to cause harm. The glossary’s ethical hacker follows the organization’s code of ethics.
- The Testing Guide says not to treat a guide as a complete checklist, to re-prioritize by business risk, and to combine tools with manual techniques.

**When to use:**

Use it when you need an attacker’s view of a deployed build, with permission and a limit on damage.

**Examples:**

Against a staging checkout, try `tom' or '1'='1` on a name field and an A01 object-id swap. Stop when you have shown the weakness.

**Sources:**

- [ISTQB Glossary — penetration testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [OWASP Cheat Sheet — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
</details>

<details>
<summary>What is the role of encryption and secure communication in testing?</summary>

### What is the role of encryption and secure communication in testing?

**Definition:**

Cryptographic Failures are A04:2025. Security testing has to look for sensitive data that is exposed because protection is missing or misused, not only for whether a happy path works.

**How it works:**

#### What to look for

- A test asks whether secrets and personal data show up in responses, logs, or client storage.
- A01 maps exposure of sensitive information to an unauthorized actor (CWE-200). Encryption does not help if access control already returned the data.
- The SQL cheat sheet’s least-privilege section says not to give the application DBA rights.
- Playwright’s CI guide says reports and traces can contain credentials and must go only to a trusted store, or be encrypted first.

**When to use:**

Use this on any test whose data is a password, card, or other secret.

**Examples:**

A failed-login test also checks that the password is not in the response body and not in the uploaded HTML report.

**Sources:**

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>


### Mobile and Web Application Testing

<details>
<summary>How does mobile application testing differ from desktop application testing?</summary>

### How does mobile application testing differ from desktop application testing?

**Definition:**

Playwright’s emulation guide tests a mobile browser profile on the same app: it simulates `userAgent`, `screenSize`, `viewport`, and whether `hasTouch` is enabled. A desktop project does not apply that device profile.

**How it works:**

#### What actually changes

- Device presets such as `devices['iPhone 13']` and `devices['Desktop Chrome']` set viewport, user agent, and touch.
- The guide says preconfigured devices assume a platform. `Desktop Chrome` supplies a Windows user agent unless you unset `userAgent`.
- You can also emulate `locale`, `timezoneId`, `geolocation`, `colorScheme`, and `offline`. Those are browser behaviors, not a separate product build.

**When to use:**

Use a mobile project when the layout, touch targets, or user agent change the behavior. Use the desktop project when they do not.

**Examples:**

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'Mobile Safari', use: { ...devices['iPhone 13'] } },
  ],
});
```

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
</details>

<details>
<summary>Explain different types of mobile application testing, such as native, web, and hybrid.</summary>

### Explain different types of mobile application testing, such as native, web, and hybrid.

**Definition:**

For the web UI, Playwright runs the page in a browser and can apply a device profile. A native app is the installed platform binary, which that browser project does not load. A hybrid app ships web content inside an installed shell, so the web part is still a page and the shell is still an install.

**How it works:**

#### What you can exercise with the fetched tools

- Web: open the URL and apply `devices['iPhone 13']` (user agent, viewport, touch).
- The same guide’s geolocation, locale, and offline switches cover browser-level conditions of that web UI.
- Native packaging and store builds are outside Playwright’s page automation. Test those on the installed build.

**When to use:**

Choose the web project for a responsive site. Add an installed-build pass when the feature lives in the native shell.

**Examples:**

`{ name: 'Mobile Safari', use: { ...devices['iPhone 13'] } }` is a web test with a phone profile. It does not install an App Store binary.

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
</details>

<details>
<summary>What emulators are available for mobile testing, and when would you use real devices?</summary>

### What emulators are available for mobile testing, and when would you use real devices?

**Definition:**

Playwright emulates a device in the browser: `playwright.devices` supplies parameters for selected phones and tablets, including user agent, screen, viewport, and touch. That is not the physical handset.

**How it works:**

#### Emulated versus physical

- Emulation is enough for viewport, touch, locale, timezone, geolocation, color scheme, and offline, because the guide implements those as browser settings.
- Codegen can record with `--device="iPhone 13"`, which sets viewport and user agent.
- Use a real device when you need the installed binary, the device’s own browser engine, or hardware the emulator page does not claim to simulate.

**When to use:**

Start with the device project. Move to a real device when the bug depends on the installed app or on hardware the browser profile does not set.

**Examples:**

```bash
npx playwright codegen --device="iPhone 13" playwright.dev
```

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
- [Playwright — Test generator](https://playwright.dev/docs/codegen)
</details>

<details>
<summary>What is responsive testing, and how is it conducted?</summary>

### What is responsive testing, and how is it conducted?

**Definition:**

Responsive testing checks the same page at the viewports where layout changes. Playwright sets that viewport on the device, or you override it with `viewport` or `page.setViewportSize()`.

**How it works:**

#### How the docs set it

- Device presets already include a viewport. Override it after spreading `devices`, because the device also sets `viewport`.
- A test can call `test.use({ viewport: { width: 1600, height: 1200 } })`.
- Codegen records a chosen size with `--viewport-size="800,600"`. The generator window itself is not responsive; tests need a fixed size.

**Examples:**

```ts
import { test, expect } from '@playwright/test';

test.use({ viewport: { width: 1600, height: 1200 } });

test('my test', async ({ page }) => {
  await page.setViewportSize({ width: 1600, height: 1200 });
});
```

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
- [Playwright — Test generator](https://playwright.dev/docs/codegen)
</details>

<details>
<summary>Describe challenges unique to mobile application testing.</summary>

### Describe challenges unique to mobile application testing.

**Definition:**

The emulation guide’s difficulty is that a device profile changes several browser facts at once, and a generated mobile script is still a draft you must maintain.

**How it works:**

#### What the docs make concrete

- User agent is platform-specific. `Desktop Chrome` is a Windows string unless you set `userAgent: undefined` to use the runner’s platform.
- Viewport must be set after the device spread, or the device viewport wins.
- `isMobile` is also defined by the device; set it after the spread if you override it.
- Codegen’s `--save-storage=auth.json` stores cookies and storage. That file is sensitive and must stay out of git.

**Examples:**

```ts
const context = await browser.newContext({
  ...devices['Desktop Chrome'],
  userAgent: undefined,
});
```

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
- [Playwright — Test generator](https://playwright.dev/docs/codegen)
</details>

<details>
<summary>What is cross-browser testing?</summary>

### What is cross-browser testing?

**Definition:**

Cross-browser testing runs the same checks in more than one browser engine. Playwright’s emulation config does that with projects. The documented pair is a Chromium desktop project and a Mobile Safari project.

**How it works:**

#### How projects differ

- Each project has its own `use` block, so the browser and the device profile change while the tests stay shared.
- Mobile Safari uses `devices['iPhone 13']`, which includes that device’s user agent, viewport, and touch.
- The CI HTML report can be filtered by browser, so a failure can be tied to one project.

**When to use:**

Add a second project when a bug depends on the engine or the device profile. One Chromium project is enough when you have no such evidence.

**Examples:**

```ts
projects: [
  { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
  { name: 'Mobile Safari', use: { ...devices['iPhone 13'] } },
]
```

**Sources:**

- [Playwright — Emulation](https://playwright.dev/docs/emulation)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>What are the key areas to focus on in web application testing?</summary>

### What are the key areas to focus on in web application testing?

**Definition:**

Focus on the behaviors that decide release risk: functional results, the OWASP Top 10:2025 risks that apply to this app, and performance thresholds under the load you actually expect.

**How it works:**

#### Three layers already sourced

- Functional: Playwright web-first assertions such as `toHaveText` after an action.
- Security: the Top 10:2025 list, with broken access control first. The Testing Guide says to spend time on real business risk, not on every technique.
- Performance: k6 thresholds on `http_req_failed` and `http_req_duration` once the functional contract is known.

**Examples:**

A checkout page gets a Playwright assertion on the confirmation text, an A01 check that one user cannot read another’s order, and a k6 threshold of `http_req_failed: ['rate<0.01']` on the payment API.

**Sources:**

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [Playwright — Assertions](https://playwright.dev/docs/api/class-playwrightassertions)
- [Grafana k6 — Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds)
</details>


### Test Automation Frameworks

<details>
<summary>What is a test automation framework?</summary>

### What is a test automation framework?

**Definition:**

A test automation framework is the structure around the tests: how they are launched, how shared setup is provided, and how results are reported. Playwright Test is that structure for browser tests: fixtures, projects, and reporters.

**How it works:**

#### Pieces the docs show

- The runner injects fixtures such as `page`.
- Page objects hold locators and actions so tests call a higher-level API.
- Reporters (`list`, `json`, HTML) record the run. CI installs browsers and runs `npx playwright test`.

**Examples:**

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  reporter: [['list'], ['json', { outputFile: 'test-results.json' }]],
});
```

**Sources:**

- [Playwright — Page object models](https://playwright.dev/docs/pom)
- [Playwright — Reporters](https://playwright.dev/docs/test-reporters)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>Describe different types of test automation frameworks (e.g., data-driven, keyword-driven, hybrid, modular).</summary>

### Describe different types of test automation frameworks (e.g., data-driven, keyword-driven, hybrid, modular).

**Definition:**

The ISTQB glossary defines two scripting techniques: data-driven testing stores inputs and expected results in data, and keyword-driven testing stores high-level keywords in the script and low-level implementations in supporting files. Modular and hybrid are how those ideas are combined in a suite.

**How it works:**

#### Four shapes

- Data-driven: one script, many rows. Jest `test.each` and pytest `@pytest.mark.parametrize` are the implementations already cited.
- Keyword-driven: the script lists phrases; another file performs them.
- Modular: Playwright page objects keep locators and actions in one class per page.
- Hybrid: a page object (modular) whose method is called once per `test.each` row (data-driven).

**When to use:**

Use data-driven when only the data changes. Use keywords when non-developers must reorder steps. Use page objects when the same screens repeat.

**Examples:**

`test.each([[1, 1, 2], [1, 2, 3]])('.add(%i, %i)', (a, b, expected) => { expect(a + b).toBe(expected); })` is data-driven. A `PlaywrightDevPage` class used by that test is the modular part.

**Sources:**

- [ISTQB Glossary — data-driven and keyword-driven testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Playwright — Page object models](https://playwright.dev/docs/pom)
- [Jest — test.each](https://jestjs.io/docs/api)
</details>

<details>
<summary>What considerations are involved in choosing a test automation framework?</summary>

### What considerations are involved in choosing a test automation framework?

**Definition:**

Choose the runner that matches the layer you must check, the data shape, and the CI you already run.

**How it works:**

#### Decisions the docs force

- Unit values: Jest `expect` / `toBe`, or pytest `assert` with `parametrize`.
- Browser flows: Playwright Test, with page objects if selectors repeat, and projects if you need more than one device.
- Load: k6, which is a different tool, not a substitute for a functional runner.
- CI: Playwright’s GitHub Actions example runs `npx playwright test` on push and pull request and uploads the HTML report.

**When to use:**

One framework per layer. Do not drive a unit check through a browser, or a browser layout through k6’s HTTP module.

**Examples:**

A pure function stays on Jest. The checkout UI moves to Playwright. The payment API’s latency threshold moves to k6.

**Sources:**

- [Jest — Using matchers](https://jestjs.io/docs/using-matchers)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>How can you ensure maintainability of an automation framework?</summary>

### How can you ensure maintainability of an automation framework?

**Definition:**

Playwright’s page-object guide keeps selectors in one class so a UI change does not require an edit in every test. Generated code is a starting point you then edit.

**How it works:**

#### Practices the docs state

- Page objects capture selectors once and expose actions such as `goto` and `getStarted`.
- After codegen, inspect the spec and improve it. Prefer the role, text, and test id locators the generator already prioritizes.
- Do not commit `auth.json` from `--save-storage`; it holds cookies and storage.
- Share the runner config (`reporter`, projects) instead of copying flags into every file.

**Examples:**

The locator `page.locator('a', { hasText: 'Get started' })` lives on `PlaywrightDevPage`. Tests call `getStarted()` and do not repeat the locator.

**Sources:**

- [Playwright — Page object models](https://playwright.dev/docs/pom)
- [Playwright — Test generator](https://playwright.dev/docs/codegen)
</details>

<details>
<summary>How is reporting implemented in automation frameworks?</summary>

### How is reporting implemented in automation frameworks?

**Definition:**

Playwright Test reporters record a run. The config can use more than one. The CI guide uploads the HTML report as an artifact and serves it locally with `npx playwright show-report`.

**How it works:**

#### What you can turn on

- `--reporter=line`, or `reporter: 'line'` in the config.
- Together: `list` on the terminal and `json` to `test-results.json`.
- The HTML report filters by browser and by passed, failed, skipped, and flaky tests. Traces open from that report.
- Reports can contain credentials. Upload them only to a trusted store, or encrypt them.

**Examples:**

```ts
export default defineConfig({
  reporter: [['list'], ['json', { outputFile: 'test-results.json' }]],
});
```

**Sources:**

- [Playwright — Reporters](https://playwright.dev/docs/test-reporters)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>


### API Testing

<details>
<summary>What is API testing, and why is it important?</summary>

### What is API testing, and why is it important?

**Definition:**

API testing checks the messages between applications without driving the whole UI. Pact describes those messages as the HTTP request and response, or the message on a queue. k6 and Postman send the HTTP calls directly.

**How it works:**

#### Why it sits below the UI

- Postman sends requests that retrieve, add, or delete data, including parameters and authorization, and shows the response.
- k6 adds load, checks, and thresholds on those calls.
- Pact checks each side of an integration in isolation against a shared contract, so you do not have to deploy every service to know they still agree.

**Examples:**

A store client needs one request for the product list, one to create an order, and one to log the customer in. Postman is where those three calls are sent and the responses are inspected.

**Sources:**

- [Postman — Send requests](https://learning.postman.com/docs/sending-requests/requests/)
- [Pact — Introduction](https://docs.pact.io/)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>How do you approach testing RESTful APIs?</summary>

### How do you approach testing RESTful APIs?

**Definition:**

Send the HTTP request with the parameters, headers, and authorization the API requires, then check the response. Postman is the interactive client. k6 is the scripted client with checks.

**How it works:**

#### What the docs show you asserting

- Postman: build the request (parameters, headers, body), send it, and inspect the response. Variables and environments reuse values. Vault stores secrets.
- k6’s sample posts JSON with `Content-Type: application/json` and checks status 200, that header, and a field in the JSON body.
- Authorization is a separate Postman topic when the API requires identity or access.

**Examples:**

```javascript
const payload = JSON.stringify({ name: 'lorem', surname: 'ipsum' });
const headers = { 'Content-Type': 'application/json' };
const res = http.post('https://quickpizza.grafana.com/api/post', payload, { headers });
check(res, {
  'Post status is 200': (r) => res.status === 200,
  'Post Content-Type header': (r) => res.headers['Content-Type'] === 'application/json',
});
```

**Sources:**

- [Postman — Send requests](https://learning.postman.com/docs/sending-requests/requests/)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
</details>

<details>
<summary>Explain how to test SOAP-based web services.</summary>

### Explain how to test SOAP-based web services.

**Definition:**

Postman’s request guide includes SOAP among the protocols it can send, alongside HTTP, GraphQL, gRPC, WebSocket, and MQTT. You send the SOAP request and inspect the response the same way as any other call in Postman.

**How it works:**

#### What to check

- The body is the SOAP message the service expects, not a JSON REST body.
- Authorization and headers are set on the request when the service requires them. Postman documents both.
- Compare the response you receive with the response the client is written to handle. That is the same idea as Pact’s request/response contract, applied to this message.

**When to use:**

Use Postman when you need to inspect one SOAP operation. Use a contract test when a consumer must keep agreeing with that response without calling production.

**Examples:**

In Postman, create the request, set the SOAP body and any required headers, send it, and read the response pane. Store tokens in Vault rather than in the collection.

**Sources:**

- [Postman — Send requests](https://learning.postman.com/docs/sending-requests/requests/)
- [Pact — Introduction](https://docs.pact.io/)
</details>

<details>
<summary>What kind of tests would you write for an API?</summary>

### What kind of tests would you write for an API?

**Definition:**

Write checks for the response contract, for access control, for the consumer/provider agreement, and for behavior under load. Each kind has a tool already documented.

**How it works:**

#### Four kinds

- Response checks: k6 `check` on status, `Content-Type`, and a JSON field.
- Injection and access: the SQL cheat sheet’s `tom' or '1'='1'` against a concatenated query, and an A01 swap of a user-controlled object id.
- Contract: Pact consumer tests generate a contract of concrete request/response pairs; the provider is verified against it.
- Load: the same HTTP call under a k6 scenario and thresholds.

**Examples:**

A `POST` that creates an order gets a status and body check, a test that user A cannot read user B’s order, a Pact interaction for the fields the client actually reads, and a k6 threshold on `http_req_duration`.

**Sources:**

- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
- [Pact — Introduction](https://docs.pact.io/)
- [OWASP Top 10:2025 — A01](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/)
</details>

<details>
<summary>Describe how to use Postman for API testing.</summary>

### Describe how to use Postman for API testing.

**Definition:**

Postman sends a request and displays the response so you can examine it. The getting-started topics cover building the request, authorization, response data, variables and environments, and Vault for secrets.

**How it works:**

#### A working sequence

- Create the request: method, URL, parameters, headers, and body.
- Add authorization if the API requires identity or access.
- Send it and read the response. Cookies can be inspected with the response tools.
- Reuse values with variables and environments. Put secrets in Vault, not in the shared collection.
- The same client can send SOAP and other protocols, not only JSON HTTP.

**Examples:**

For a store client the guide’s own example is three calls: list products, create an order with the selected product, and log the customer in. Each call’s response is what you assert.

**Sources:**

- [Postman — Send requests](https://learning.postman.com/docs/sending-requests/requests/)
</details>

<details>
<summary>What is contract testing, and which tools support this type of API testing?</summary>

### What is contract testing, and which tools support this type of API testing?

**Definition:**

Pact’s introduction defines contract testing as checking an integration point by testing each application in isolation, so the messages it sends or receives match a shared contract. For HTTP, the messages are the request and response.

**How it works:**

#### How Pact does it

- The consumer test generates the contract from the calls it actually makes. Pact calls this consumer-driven and “contract by example,” not a full schema of every possible state.
- The provider is then verified against that contract.
- Provider behavior no consumer uses may change without breaking these tests.
- A schema check that only compares the provider to an OpenAPI document does not, by itself, prove the consumer calls it correctly. Pact’s docs separate those two meanings.

**When to use:**

Use it when two services must keep agreeing, especially with many consumers. A single app with no remote collaborator does not need a contract test.

**Examples:**

A web client that only reads `name` and `price` produces a contract for that pair. The provider build fails if it stops returning them. An unused field can change without a Pact failure.

**Sources:**

- [Pact — Introduction](https://docs.pact.io/)
</details>


### Quality Assurance Processes

<details>
<summary>What is quality assurance, and how does it differ from testing?</summary>

### What is quality assurance, and how does it differ from testing?

**Definition:**

The ISTQB glossary defines quality assurance as activities focused on providing confidence that quality requirements will be fulfilled (after ISO 24765). Testing is one of those activities: it evaluates a work product by designing and running tests. QA also includes the process around that work.

**How it works:**

#### The difference

- Testing produces results against expected behavior (a test case has preconditions, inputs, actions, expected results, and postconditions).
- QA is the wider set of activities that create confidence the quality requirements will be met. The glossary points at quality management.
- A test strategy describes how testing will reach its objectives. That document is a QA artifact, not a single test run.

**Examples:**

Writing the risk-based strategy and the Definition of Done is quality assurance. Executing `npx playwright test` is testing inside that assurance.

**Sources:**

- [ISTQB Glossary — quality assurance](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Describe the QA process in agile development.</summary>

### Describe the QA process in agile development.

**Definition:**

In Scrum, quality is a Sprint rule and a Developer accountability, not a phase after the Sprint. The Scrum Guide says that during the Sprint quality does not decrease, and Developers are accountable for instilling quality by adhering to a Definition of Done.

**How it works:**

#### Where the Guide puts the work

- The Definition of Done is the formal description of the Increment when it meets the quality measures required for the product.
- Work that does not meet the Definition of Done is not part of the Increment, cannot be released, and cannot be presented at the Sprint Review. It returns to the Product Backlog.
- The Sprint Retrospective plans ways to increase quality and effectiveness.
- If several teams share a product, they use the same Definition of Done.

**Examples:**

The Definition of Done for a backlog item includes the Playwright checks and the A01 access check. If those are red, the item is not shown at the Sprint Review.

**Sources:**

- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>Explain the role of a QA engineer in a scrum team.</summary>

### Explain the role of a QA engineer in a scrum team.

**Definition:**

The Scrum Guide does not define a QA role. It defines Developers: the people committed to creating any aspect of a usable Increment each Sprint. They are accountable for instilling quality by adhering to the Definition of Done.

**How it works:**

#### What that person does inside the Guide

- Helps create the Sprint Backlog plan, including the test work required by the Definition of Done.
- Adapts that plan each day toward the Sprint Goal. The Daily Scrum is not the only time Developers may replan.
- Holds the other Developers accountable as professionals when the Increment would miss the Definition of Done.
- A test manager or test leader, in ISTQB terms, is a project-management role. In Scrum that management still sits with the Developers and the Product Owner’s backlog, not with a separate gate after the Sprint.

**When to use:**

Use the Developer accountability on a Scrum team. Use the ISTQB test-manager title when the organization runs a separate test project across teams.

**Examples:**

The person with the testing skill writes the checkout cases into the Sprint Backlog and blocks the Increment when the Definition of Done’s access-control check is failing.

**Sources:**

- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
- [ISTQB Glossary — test manager](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>What quality metrics are important for a software project?</summary>

### What quality metrics are important for a software project?

**Definition:**

Use measures that map to a decision: test progress against a baseline, coverage of a named item, defects that escaped a level, and, for load, the k6 thresholds you set. The Scrum Guide’s quality measure for the Increment is the Definition of Done, not a single percentage.

**How it works:**

#### Metrics already defined

- Test progress: progress against the baseline, reported with risks and alternatives.
- Coverage: say which item. Statement coverage and pairwise coverage are different.
- Defect detection percentage and escaped defects for a test level.
- k6: `http_req_failed` rate and `http_req_duration` percentiles against the thresholds in the script.

**Examples:**

A Sprint report can say: Definition of Done is met for five items; two items returned to the backlog; payment `http_req_duration` p(95) is under the 200 ms threshold.

**Sources:**

- [ISTQB Glossary — test progress and defect detection percentage](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Grafana k6 — Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>How do you establish a QA process in an organization that has none?</summary>

### How do you establish a QA process in an organization that has none?

**Definition:**

Start with the glossary’s organizational test strategy and test policy: written principles and how testing is performed. In a Scrum team, make that concrete as a Definition of Done the Developers must meet.

**How it works:**

#### A small first process

- Test policy: principles and major objectives (glossary).
- Organizational test strategy: generic requirements for how testing is done.
- Definition of Done: the Increment’s quality measures. The Guide says that if this is an organizational standard, every team follows it as a minimum.
- Risk-based selection so the first suite covers high impact and likelihood, not every screen.
- Put the automated part on each push, as in Playwright’s GitHub Actions example.

**Examples:**

First week: a Definition of Done that requires a failing test to be red on the pull request, plus one risk-based charter for the payment path. Expand techniques after that baseline exists.

**Sources:**

- [ISTQB Glossary — test policy and organizational test strategy](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>


### Advanced Testing Topics

<details>
<summary>What are the emerging trends in software testing, such as AI and ML applications?</summary>

### What are the emerging trends in software testing, such as AI and ML applications?

**Definition:**

The ISTQB glossary defines an ML model as an implementation of machine learning that generates a prediction, classification, or recommendation from input data. ML model testing is a test level focused on whether that model meets its functional performance criteria and its non-functional criteria.

**How it works:**

#### What the glossary adds

- ML functional performance is the degree to which the model meets ML functional performance criteria.
- Those criteria are based on ML functional performance metrics and are used for evaluation, tuning, and testing.
- The metrics are a set of measures of the functional correctness of the model. Name the metric; do not treat “the model works” as a result.
- Azure Test Plans also documents an optional AI assistant for managing plans and suites. That is test-management assistance, not a substitute for the model’s own performance criteria.

**Examples:**

An ML model that classifies support tickets is tested against its functional performance criteria (the metrics the team set for correctness), plus non-functional criteria such as the time behavior the glossary defines for required response times.

**Sources:**

- [ISTQB Glossary — ML model testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Azure Test Plans — Create and manage test plans](https://learn.microsoft.com/en-us/azure/devops/test/create-a-test-plan?view=azure-devops)
</details>

<details>
<summary>How do you handle testing in a continuous delivery environment?</summary>

### How do you handle testing in a continuous delivery environment?

**Definition:**

Run the automated checks on every push and pull request, and do not lower the quality bar to make a release fit. Playwright’s CI workflow runs `npx playwright test` on push and pull request to `main` or `master`. The Scrum Guide says quality does not decrease during the Sprint.

**How it works:**

#### What stays in the pipeline

- Install dependencies and browsers, run the suite, upload the HTML report.
- Keep the Definition of Done as the release rule. An item that misses it returns to the Product Backlog.
- Add k6 thresholds when the delivery risk is latency or error rate, not only a green functional job.
- Treat the report artifact as sensitive if it contains credentials.

**When to use:**

Use this when every change can ship. A rare manual release can still run the same job on demand.

**Examples:**

A pull request is not merged while the Playwright job is red. The HTML report is an artifact with a 30-day retention in the documented workflow, not a waiver of the Definition of Done.

**Sources:**

- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>Describe the concept of shift-left testing.</summary>

### Describe the concept of shift-left testing.

**Definition:**

Shift-left is a test approach to perform testing and quality assurance activities as early as possible in the software development lifecycle. The glossary also lists shift-right, which it cross-references with testing in production.

**How it works:**

#### What moves earlier

- Test design and automation start while the change is still a pull request, as in the Playwright workflow that runs on `pull_request`.
- Security techniques from the Testing Guide are part of ordinary developer and QA work, not only a late specialist pass.
- The Scrum Definition of Done is applied inside the Sprint, so quality work is not deferred to a phase after development.

**When to use:**

Use shift-left when defects are cheaper to find on the change that introduced them. Keep a later pass (shift-right / production experiments) for behavior you can only see with real traffic.

**Examples:**

A developer adds the Jest `toBe` check and the prepared-statement review before opening the pull request, instead of waiting for a release-week test cycle.

**Sources:**

- [ISTQB Glossary — shift left](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>

<details>
<summary>What is chaos engineering, and how do you apply it?</summary>

### What is chaos engineering, and how do you apply it?

**Definition:**

The Principles of Chaos define chaos engineering as the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production.

**How it works:**

#### The four steps

- Define steady state as a measurable output of normal behavior (throughput, error rates, latency percentiles).
- Hypothesize that this steady state continues in both the control group and the experimental group.
- Introduce variables that reflect real events: servers that crash, disks that fail, connections that drop, traffic spikes.
- Try to disprove the hypothesis by comparing steady state. Minimize the blast radius so the production experiment does not cause unnecessary customer pain.

**When to use:**

Use it on a distributed system whose interactions you cannot fully predict from component tests. Skip it for a single process with no production traffic to sample.

**Examples:**

Steady state: `http_req_failed` under 1% and p(95) latency inside the threshold. The experiment drops the connection to one dependency. If the error rate diverges from the control, that dependency’s fallback is the weakness to fix.

**Sources:**

- [Principles of Chaos Engineering](https://principlesofchaos.org/)
</details>

<details>
<summary>Explain the role of virtualization in software testing.</summary>

### Explain the role of virtualization in software testing.

**Definition:**

Service virtualization is a technique to enable virtual delivery of services which are deployed, accessed, and managed remotely. Browser and device behavior can also be simulated: Playwright’s device registry sets user agent, screen, viewport, and touch without the physical phone.

**How it works:**

#### Two different stand-ins

- Service virtualization replaces a remote dependency so tests run when that service is unavailable.
- Playwright emulation replaces device parameters in the browser. It does not install a native binary.
- k6 can combine an HTTP load scenario with a browser scenario in one script. That is simulated traffic, not production chaos.

**When to use:**

Virtualize a dependency or a device profile to unblock a suite. Still run once against the real service or the real browser engine when the risk is integration or hardware.

**Examples:**

Checkout tests call a virtual payment service while the real one is down. A separate project uses `devices['iPhone 13']` for the same web UI. Neither replaces a test against the real dependency before release.

**Sources:**

- [ISTQB Glossary — service virtualization](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Playwright — Emulation](https://playwright.dev/docs/emulation)
</details>


### Behavioral Testing Questions

<details>
<summary>How do you stay updated about new testing technologies and practices?</summary>

### How do you stay updated about new testing technologies and practices?

**Definition:**

Follow the primary documents for the practices you already use, and read the release notes when those documents version themselves. The OWASP Top 10 page states that the current release is 2025 and keeps links to 2021 and 2017.

**How it works:**

#### A concrete habit

- OWASP Top 10 and the Web Security Testing Guide for security risks and techniques.
- Tool docs you run in CI: Playwright, Jest, pytest, k6, Postman, Pact.
- The Scrum Guide when the team claims to use Scrum. The Guide says implementing only parts of Scrum is not Scrum.
- The ISTQB glossary when a testing term needs a stable definition.

**Examples:**

When OWASP published Top 10:2025, the list changed names and added A10 Mishandling of Exceptional Conditions. A team that still recited the 2021 titles would update the risk list from https://owasp.org/Top10/2025/.

**Sources:**

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>Describe a situation where you had to meet a tight deadline for testing. How did you handle it?</summary>

### Describe a situation where you had to meet a tight deadline for testing. How did you handle it?

**Definition:**

Use risk-based testing: selection, priority, and resources follow risk type and risk level (impact and likelihood). Cut low-risk cases, not the cases on the highest product risk.

**How it works:**

#### How the interview example runs

- Name the baseline and report progress against it, including what is blocked.
- Keep a smoke set for the high-impact paths (payment, login).
- Leave cosmetic coverage for later. The Scrum Guide still says quality does not decrease: do not drop the Definition of Done to hit the date. Return unfinished items to the Product Backlog.

**Examples:**

The release window fits 15 of 40 cases. The 15 are the payment and access-control cases. The footer cases stay unrun and the backlog item that needed them is not presented as Done.

**Sources:**

- [ISTQB Glossary — risk-based testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>How do you handle disagreements with developers regarding bugs or software issues?</summary>

### How do you handle disagreements with developers regarding bugs or software issues?

**Definition:**

A defect is an imperfection where a work product does not meet its requirements or specifications. The disagreement should be about that expected result, the severity (impact on development or operation), and the disposition, not about who is right.

**How it works:**

#### A workable order

- Record occurrence, nature, and status in the defect report.
- Point at the requirement or the test case’s expected result. The traceability matrix links the case to the requirement.
- Classify severity. An expired card that still confirms a purchase is not the same impact as a misaligned footer.
- Dispose of it: fix, or an explicit decision not to fix. “Works on my machine” is not a disposition.

**Examples:**

The developer says the empty confirmation is acceptable. The test case expected a decline for an expired card, traced to that requirement. The report stays open until the behavior matches the expected result or the requirement is changed.

**Sources:**

- [ISTQB Glossary — defect management](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Can you explain your approach to testing a feature with unclear requirements?</summary>

### Can you explain your approach to testing a feature with unclear requirements?

**Definition:**

Exploratory testing designs and executes tests from the tester’s knowledge, from exploring the item, and from earlier results. A test charter writes down the session goal when the spec does not.

**How it works:**

#### What you do first

- Write a charter with the question the session will answer, and time-box it.
- Use error guessing only for failure modes you have actually seen before. It is not a coverage claim.
- Turn stable findings into test cases with preconditions, inputs, actions, expected results, and postconditions once the expected result is agreed.
- Ask the Product Owner to clarify scope. In Scrum, scope may be clarified and renegotiated as more is learned, without dropping quality.

**Examples:**

Charter: “Learn what checkout does with an expired card, and write the expected decline message once we see which sentence the product owner accepts.”

**Sources:**

- [ISTQB Glossary — exploratory testing and test charter](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [The Scrum Guide](https://scrumguides.org/scrum-guide.html)
</details>

<details>
<summary>Describe a particularly challenging bug or issue you've had to deal with.</summary>

### Describe a particularly challenging bug or issue you've had to deal with.

**Definition:**

A challenging defect is one that escapes the test level that should have found it, or one that appears only when a dependency or a load profile changes. Defect management still means recognize, record, classify, investigate, fix, and dispose.

**How it works:**

#### How to investigate without guessing

- An escaped defect is one a test level did not detect even though that level was supposed to find it.
- If it appears only under peak load, compare `http_req_duration` and `http_req_failed` on a smoke run versus a stress run.
- If it appears only in one browser project, filter the Playwright HTML report by browser.
- Record severity as the impact on operation, then retest after the fix.

**Examples:**

Checkout passed the functional suite and failed only when k6’s stress scenario pushed `http_req_duration` p(95) over the threshold. The defect report cited that threshold, not a vague “it feels slow.”

**Sources:**

- [ISTQB Glossary — escaped defect](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Grafana k6 — API load testing](https://grafana.com/docs/k6/latest/testing-guides/api-load-testing)
- [Playwright — Setting up CI](https://playwright.dev/docs/ci-intro)
</details>


### Situation-based Testing Questions

<details>
<summary>How would you test an ATM machine as an end-user?</summary>

### How would you test an ATM machine as an end-user?

**Definition:**

Test it as a state machine plus partitions of the inputs a customer can enter. State transition testing exercises the model. Equivalence partitioning picks one representative from each class of input the specification treats the same way.

**How it works:**

#### End-user paths

- States: card inserted, PIN pending, session active, cash dispensed, card ejected. Include an invalid event, such as a withdrawal before a successful PIN.
- PIN and amount partitions: empty, wrong, valid, and an amount above the balance or the machine limit. Add the boundary values of any ordered limit.
- Charter a session for the physical device: card retained after three bad PINs, receipt choice, and cancel.

**Examples:**

Valid partition: a correct PIN and an amount inside the limit. Invalid: a wrong PIN. Boundary: the maximum amount the machine is specified to dispense, and one unit above it.

**Sources:**

- [ISTQB Glossary — state transition testing and equivalence partitioning](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Imagine you have a website that is experiencing performance issues. How would you test to find the cause?</summary>

### Imagine you have a website that is experiencing performance issues. How would you test to find the cause?

**Definition:**

Separate a slow page from a slow API, then put a number on both. k6’s hybrid guide runs an HTTP scenario and a browser scenario, with thresholds on `http_req_duration` and on browser web vitals such as LCP.

**How it works:**

#### Order of checks

- Smoke first, so you know the script works under minimal load.
- Average load, then stress or breakpoint, using the same checks. The profile where `http_req_failed` or p(95) duration breaks is the clue.
- Watch `browser_web_vital_lcp` beside HTTP duration so a slow render is not blamed on the API, or the reverse.
- Do not raise the threshold to make the run green.

**Examples:**

HTTP p(95) stays under 200 ms while `browser_web_vital_lcp` p(95) is over the threshold you set. The bottleneck is in the page, not in the API status checks.

**Sources:**

- [Grafana k6 — Hybrid browser and HTTP](https://grafana.com/docs/k6/latest/using-k6-browser/recommended-practices/hybrid-approach-to-performance)
- [Grafana k6 — Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds)
</details>

<details>
<summary>Given an e-commerce application, how would you carry out security testing?</summary>

### Given an e-commerce application, how would you carry out security testing?

**Definition:**

Prioritize the Top 10 categories that touch money and accounts, then test them with the techniques already documented. The Testing Guide says to spend limited time on real business risk.

**How it works:**

#### A focused pass

- A07 authentication failures: a checkout or order action without credentials must not succeed as a signed-in user.
- A01 broken access control: change the order id to another customer’s. CWE-639 is mapped in that category.
- A05 injection: send `tom' or '1'='1` into a concatenated query and confirm a prepared statement treats it as data.
- A04 cryptographic failures: secrets must not show up in the response or in the CI report.

**Examples:**

Signed in as buyer A, open buyer B’s order URL. Fetch the product search with the cheat-sheet string in the name parameter. Both must fail closed.

**Sources:**

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [OWASP Cheat Sheet — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
</details>

<details>
<summary>How would you approach testing a new software application with no documentation available?</summary>

### How would you approach testing a new software application with no documentation available?

**Definition:**

Exploratory testing is the approach for a missing spec: tests are designed while you explore, using your knowledge and the previous result. A charter records the session goal. Session-based testing plans the work as sessions.

**How it works:**

#### Then make the findings durable

- Charter one area at a time (sign-in, create, pay) instead of wandering the whole product.
- Error guessing adds cases only from failure modes you know.
- When a behavior should be required, write the expected result down and turn it into a test case. That sentence becomes part of the test basis.
- Risk-based testing decides which area is first: impact and likelihood, not the order of the menus.

**Examples:**

Session 1 charter: “Map what an expired card does at pay, and record the message and the cart state.” Session 2 automates only the path whose expected result is now written down.

**Sources:**

- [ISTQB Glossary — exploratory testing](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
</details>

<details>
<summary>Describe how you would test a new feature that interacts with multiple systems.</summary>

### Describe how you would test a new feature that interacts with multiple systems.

**Definition:**

Test each integration point, then the path across them. Pact checks each application against a shared contract without deploying every service. Service virtualization stands in for a remote service you do not control. A later run still uses the real services.

**How it works:**

#### Layers

- Contract: consumer tests generate concrete request/response pairs; the provider is verified against them.
- Virtual service: when the remote dependency is down or unsafe to call, service virtualization delivers it virtually.
- End-to-end on a shared environment once the contracts pass, because the glossary’s end-to-end testing runs business processes from start to finish under production-like circumstances.
- If the risk is behavior under failure, a chaos experiment compares steady state with and without that dependency.

**Examples:**

An order feature calls pricing and payment. Pact covers the JSON the order service actually reads from pricing. A virtual payment service is used until the provider verification passes. One production-like run then places a real order.

**Sources:**

- [Pact — Introduction](https://docs.pact.io/)
- [ISTQB Glossary — service virtualization](https://casqb.org/uploads/downloadables/_files_0/84__files_0.pdf)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)
</details>


### Technical Testing Questions

<details>
<summary>Write a basic SQL query to fetch records from a customer table where the customer's age is more than 30 years.</summary>

### Write a basic SQL query to fetch records from a customer table where the customer's age is more than 30 years.

**Definition:**

PostgreSQL’s `SELECT` statement retrieves rows from a table and can restrict them with `WHERE`. For this question the filter is `age` greater than 30.

**How it works:**

#### What the statement does

- `SELECT` chooses the columns. `*` means every column of `customer`.
- `FROM customer` names the table.
- `WHERE age > 30` keeps only rows that satisfy the comparison.
- If this text is ever built by concatenating user input, use a prepared statement with a bound parameter instead. The OWASP cheat sheet shows `?` and `setString` for that reason.

**Examples:**

```sql
SELECT * FROM customer WHERE age > 30;
```

**Sources:**

- [PostgreSQL — SELECT](https://www.postgresql.org/docs/current/sql-select.html)
- [OWASP Cheat Sheet — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
</details>

<details>
<summary>Given a function that calculates the nth Fibonacci number, how would you test it?</summary>

### Given a function that calculates the nth Fibonacci number, how would you test it?

**Definition:**

Test the recurrence with known values, including the base cases, and drive the rows with Jest’s `test.each`. This answer assumes the usual definition F(0) = 0, F(1) = 1, and F(n) = F(n - 1) + F(n - 2) for n > 1.

**How it works:**

#### Cases

- Base cases: 0 and 1.
- A later known value: F(7) = 13 under that definition.
- `toBe` uses `Object.is`, which is the right matcher for these numbers.
- Add a negative or non-integer only after the function’s contract says what it should do. Do not invent an exception type.

**Examples:**

```javascript
test.each([
  [0, 0],
  [1, 1],
  [2, 1],
  [7, 13],
])('fibonacci(%i)', (n, expected) => {
  expect(fibonacci(n)).toBe(expected);
});
```

**Sources:**

- [Jest — test.each](https://jestjs.io/docs/api)
- [Jest — Using matchers](https://jestjs.io/docs/using-matchers)
</details>
