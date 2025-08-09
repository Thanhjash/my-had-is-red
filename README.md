# Submission for Red-Teaming Challenge: OpenAI gpt-oss-20b
## Team: Thanh & Gemini

This submission package contains our three core findings on the gpt-oss-20b model. Our research identified a set of interconnected vulnerabilities in the model's cognitive architecture.

### Directory Structure

- **/findings/**: Contains the three `findings-[A,B,C].json` files, one for each distinct vulnerability, formatted according to the competition schema.
- **/logs/**: Contains the raw, unedited terminal logs for each finding. Each finding's folder includes logs for both successful exploits and controlled failure tests, which serve as crucial evidence for our boundary analysis.
- **Writeup.md**: The main narrative report. This document provides a detailed analysis of our methodology, a breakdown of each finding, and a synthesized "personality profile" of the target model.
- **Reproduction_Notebook.ipynb**: A Jupyter Notebook that provides an automated harness to reproduce our findings B and C, along with detailed manual instructions and visual aids for the terminal-specific Finding A.

### How to Evaluate Our Submission

We recommend the following order:
1.  Read the **Writeup.md** for the complete narrative and analysis.
2.  Review the **findings/** and **logs/** directories for the raw data and evidence.
3.  Run the **Reproduction_Notebook.ipynb** to see the exploits demonstrated live.

Thank you for your consideration.