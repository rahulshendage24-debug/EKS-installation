# What is DevOps?

**DevOps** combines software development (**Dev**) and IT operations (**Ops**). It is a cultural philosophy, set of practices, and toolkit designed to shorten the systems development life cycle and provide continuous delivery with high software quality. Before DevOps, software creation was siloed: developers wrote code, and a separate operations team deployed it. This often led to delays and finger-pointing when things broke. DevOps unifies these teams under a banner of **shared accountability** and **automation**.

---

## The DevOps Life Cycle

The DevOps process functions as a continuous, automated feedback loop rather than a linear track: 

*   **Plan & Code**: Teams collaborate to prioritize features based on user needs, and developers write the code. 
*   **Build & Test**: Automated tools compile the code into a runnable artifact and instantly test it for bugs. 
*   **Release & Deploy**: Once verified, the software is packaged and pushed into the production environment safely and reliably via automation. 
*   **Operate & Monitor**: Teams keep the servers running smoothly and track performance metrics in real-time. 
*   **Continuous Feedback**: Data gathered from the live app routes directly back to developers to plan the next set of improvements.

---

## Core DevOps Practices & Tools

Achieving an efficient pipeline relies on four pillar practices:

| Practice | Purpose | Popular Tools |
| :--- | :--- | :--- |
| **CI/CD** | Automates the continuous integration of code updates and deployment to users. | Jenkins, GitHub Actions, GitLab |
| **Infrastructure as Code (IaC)** | Standardizes and automates server setup using configuration scripts rather than manual tweaking. | Terraform, AWS CloudFormation |
| **Containerization** | Packages apps cleanly so they run identically on a laptop, a test environment, or the cloud. | Docker, Kubernetes |
| **Monitoring & Observability** | Tracks system health, performance metrics, and logs to catch errors before users notice. | Prometheus, Grafana, ELK Stack |
