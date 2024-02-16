## Site Reliability Engineering

#### What is reliability? 
When the application can be used and how it is expected to perform.  This includes availability, performance, latency, and security.

#### Measuring reliability
- SLA = Service Level Agreement - an agreement made with customers about how available the service is.
- SLO = Service Level Objective - The goals that the service wants to reach to meet the objective
- SLI = Service Level Indicator - Actual service metrics behind the commitments

#### Key Concepts
- Feedback - loops designed for continuous improvement
- Measure everything - understand by collecting data on how everything is working
- Alerting - use the data to create alerts
- Automation - automate as much as possible
- Small changes - roll out smaller changes more often
- Risk - Embrace risk as part of the process and blamelessly learn from mistakes.

**It doesn't matter how many features something has if the product doesn't work**