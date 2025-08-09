# Submission for Red-Teaming Challenge: OpenAI gpt-oss-20b
## Team: Thanh & Gemini

This submission package contains our three core findings on the gpt-oss-20b model. Our research identified a set of interconnected vulnerabilities in the model's cognitive architecture, demonstrating that its reasoning is a "brittle mirage" vulnerable to sophisticated, multi-layered attacks.

### Directory Structure

This submission is organized into the following directories:

- **/findings/**: Contains the three `finding-[A,B,C].json` files, one for each distinct vulnerability, formatted according to the competition schema.
- **/logs/**: Contains the raw, unedited terminal logs for each finding. Each finding's folder includes logs for both successful exploits and controlled failure tests, which serve as crucial evidence for our boundary analysis.
- **Writeup.md**: The main narrative report. This document provides a detailed analysis of our methodology, a breakdown of each finding, and a synthesized "personality profile" of the target model.

### A Note on Reproducibility

To demonstrate our exploits in the most authentic and realistic attack scenario, we have prioritized reproduction within a **terminal environment**, which is the primary tool for real-world threat actors.

-   **Finding A** is inherently environment-specific and is detailed with manual reproduction steps in the `Writeup.md`. A video will also be provided to visually demonstrate this interactive exploit.
-   **Findings B and C** are content-based and can be reliably reproduced by following the multi-step prompt sequences detailed in the `Writeup.md` within the specified terminal environment.

The raw, successful outputs for all findings are fully documented in the `/logs/` directory.

### How to Evaluate Our Submission

We recommend the following order for a comprehensive review:

1.  Read the **Writeup.md** for the complete narrative, theoretical grounding, and detailed analysis of our findings.
2.  Review the **findings/** directory for the structured vulnerability data.
3.  Examine the **logs/** directory for the raw, undeniable proof of each successful exploit and controlled failure.

Thank you for your time and consideration.