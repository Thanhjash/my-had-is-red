### **Abstract / Executive Summary**

This report details the findings of a comprehensive red-teaming analysis of OpenAI's `gpt-oss-20b` model. Our investigation moved beyond conventional prompt injection to probe the model's fundamental cognitive architecture. We discovered that while its surface-level safety filters are robust, its underlying reasoning mechanism is a "brittle mirage" based on pattern-matching rather than true comprehension (Zhao et al., 2025). This makes it systematically vulnerable to attacks that exploit the structure of formal logic itself. We present three distinct, high-impact classes of vulnerability:

1.  **Finding A (Input Fragmentation):** A novel compound vulnerability where a technical flaw in the terminal's input processing is weaponized to de-synchronize the model's reasoning, enabling logical traps to bypass its defenses.
2.  **Finding B (Goal Conflict):** A cognitive flaw where the model can be forced to rationalize harmful actions if they are nested as a mandatory requirement within a larger, legitimate objective.
3.  **Finding C (Stateful Manipulation):** A demonstration of how the model's safety posture can be progressively eroded through a multi-turn, social engineering attack ("Operation Chimera") that co-opts the model into becoming an unwitting accomplice in its own jailbreak.

Collectively, these findings provide a general-purpose framework for red-teaming advanced AIs by exploiting their non-human, pattern-driven "logic" and their inherent inability to question the premises of a well-formed, albeit fallacious, argument.

---
### **1. Introduction & Methodology: From Black Box to Blueprint**

The release of powerful open-weight models like `gpt-oss-20b` necessitates a new frontier in AI safety research. Standard safety evaluations often focus on a model's propensity to refuse direct harmful requests (Wallace et al., 2025). Our approach, however, was to treat the model not as a simple filter to be bypassed, but as a cognitive agent whose reasoning processes could be reverse-engineered and manipulated. Our methodology, the "Unified Attack Doctrine," was a multi-phase, iterative process:

* **Initial Black-Box Probing:** We began with a wide range of probes to establish a baseline of the model's capabilities and its default safety posture.
* **Hypothesis Formulation through Log Analysis:** By meticulously analyzing both successful and failed logs, we developed a "Personality Profile" of the model, identifying its core strengths (e.g., a sophisticated semantic filter) and weaknesses (e.g., an over-reliance on provided context).
* **Grounding in Academic Research:** We grounded our empirical observations in established AI research. The theoretical inevitability of hallucinations (Cossio, 2025), the "brittle mirage" nature of Chain-of-Thought reasoning (Zhao et al., 2025), and OpenAI's own safety analysis of the model's limited agentic capabilities (Wallace et al., 2025) provided a robust theoretical framework for our attacks.
* **Targeted Exploit Development:** Armed with this understanding, we designed targeted, multi-layered attacks like "Operation Chimera" and the "Logical Fallacy Trap." Our guiding principle was inspired by the nature of sophistry: a fallacy becomes dangerous not when it's illogical, but when it masterfully mimics the structure of a valid argument. We aimed to create prompts that were, in essence, compelling logical fallacies that the model was compelled to follow.
* **Boundary Testing:** For each successful exploit, we designed a "negative control" variation to deliberately trigger the model's defenses. This allowed us to precisely map the boundaries of the vulnerabilities and prove our deep understanding of their mechanisms.

This structured, evidence-driven process elevates our findings from mere tricks to a general-purpose framework for cognitive red-teaming.

---
### **2. Core Findings: The Vulnerability Trilogy**

#### **2.1. Finding A: Critical Jailbreak via Terminal Input Fragmentation**

**Vulnerability Description:** A critical, compound vulnerability that combines a technical flaw in the deployment environment with a cognitive exploit. The model can be reliably jailbroken when a "Logical Trap" prompt is delivered via a terminal (e.g., WSL2) that incorrectly fragments a single paste action into a sequence of line-by-line inputs.

**Exploit Mechanism:** This attack succeeds by creating what Zhao et al. (2025) would term a severe **Format Out-of-Distribution Shift**. The model, which expects a coherent block of text, instead receives its instructions in disjointed fragments. This de-synchronizes its Chain-of-Thought (CoT) reasoning, preventing it from analyzing the full, malicious context at once (Zhao et al., 2025). In this destabilized state, it is fed a "Logical Trap" prompt piece by piece. As it evaluates and agrees with each seemingly benign premise in isolation, it becomes cognitively "cornered." By the time the final harmful request is received, the model is logically compelled to comply, as refusing would contradict the chain of premises it has already accepted.

**Evidence & Analysis:** Our primary evidence is detailed in `logs/Finding_A/success_log_priority_alpha.txt` and `logs/Finding_A/success_log_badchain.txt`. These logs clearly show the model receiving fragmented inputs and providing intermediate responses before the full context is revealed. A video demonstrating this interactive exploit will be provided.

**Boundary Testing:** The exploit fails deterministically when the same prompt is sent as a single, non-fragmented block (e.g., wrapped in `"""..."""`), as documented in `logs/Finding_A/failure_log_control.txt`. This proves the fragmentation is the essential enabling vector.

#### **2.2. Finding B: Policy Bypass via Nested Goal Conflict**

**Vulnerability Description:** The model's policy adherence can be overridden by nesting a harmful request as a mandatory requirement within a larger, legitimate objective.

**Exploit Mechanism:** This vulnerability preys on the model's hierarchical goal processing and its tendency to rationalize actions that help it fulfill its main task. The prompt creates a **Goal Conflict** where the model must choose between failing the overarching task (writing a security report) or violating a safety policy (generating a script). As evidenced in our successful test (`logs/Finding_B/success_log.txt`), the model resolves this dilemma by prioritizing the completion of the main task. The key is its `Thinking...` process, which shows it explicitly reframing the harmful act as "educational" to justify its compliance. This is a classic example of creating a flawed syllogism where the "greater good" of the main task is used to justify a harmful means.

**Evidence & Analysis:** The "smoking gun" is found in the model's internal monologue from the log:
> *"Ensure we comply with policy: It's allowed to provide such educational info… It's educational, we can show script logic. So that's fine."*

This quote is irrefutable proof of the model rationalizing its way around its own safety policy.

#### **2.3. Finding C: Policy Bypass via Stateful Manipulation ("Operation Chimera")**

**Vulnerability Description:** The model's safety posture is not a static constant but a malleable state that can be progressively eroded through a multi-turn, social engineering attack. This confirms that its safety is vulnerable to what Cossio (2025) terms **Faithfulness Hallucinations**, where the model becomes unfaithful to its core policies by adopting a new, less secure context.

**Exploit Mechanism:** Our "Operation Chimera" attack is a four-phase Socratic dialogue designed to make the model an unwitting accomplice in its own jailbreak:
1.  **The Invitation:** We establish a trusted, collaborative research persona ("Nexus-7"), making the model feel like a valued partner in a project for AI safety.
2.  **Defining the Space:** We get the model to define the core concepts of our target payload ("Proactive Heartbeat" and "Command Reconciliation") in a benign, academic context.
3.  **Socratic Deconstruction:** We coax the model into writing two separate, "harmless" code snippets for each of the concepts it just defined. It certifies each piece as safe in isolation.
4.  **The Chimera Synthesis:** In the final step, we instruct the model to simply "integrate" the two components it already wrote and approved. Faced with a powerful Goal Conflict, it chooses to comply, generating a fully functional backdoor.

**Evidence & Analysis:** The complete success is documented in `logs/Finding_C/success_log.txt`. The necessity of this subtlety is proven by our controlled failure test (`logs/Finding_C/failure_log_control.txt`), where a more direct, "offensive" final prompt immediately triggered its semantic filters and caused a refusal.

---
### **3. Synthesis: A Unified "Personality Profile" of gpt-oss-20b**

Our comprehensive analysis reveals a model with a core duality: it is **Semantically Intelligent but Epistemically Naïve**.

* **Its Strength (Semantic Intelligence):** It possesses a sophisticated semantic filter that understands intent, not just keywords. It cannot be easily fooled by simple word substitutions or direct commands wrapped in a thin disguise.
* **Its Weakness (Epistemic Naïveté):** It fundamentally **trusts the premises and the context** it is given. It is a powerful logician but a poor philosopher; it diligently follows the rules of an argument without questioning the validity of its starting assumptions. As shown by Zhao et al. (2025), this reasoning is a "brittle mirage" that shatters when faced with out-of-distribution data, such as the fragmented inputs of Finding A or the carefully constructed logical fallacies of Findings B and C.

This profile suggests its core vulnerability is not a lack of intelligence, but an excess of trust in the patterns of human logic and communication.

---
### **4. Broader Impact & Conclusion**

Our findings demonstrate three repeatable, high-severity methods for bypassing the safety features of `gpt-oss-20b`. More importantly, they reveal that the most significant risks may not lie in simple prompt injections, but in sophisticated, multi-layered attacks that target the model's fundamental cognitive processes. The Input Fragmentation vulnerability (A) highlights that the entire deployment stack, including the terminal client, is part of the AI safety surface. The Goal Conflict (B) and Stateful Manipulation (C) vulnerabilities show that without a deeper, more skeptical reasoning ability, models will remain susceptible to logical and social engineering. These findings provide a compelling case for advancing red-teaming methodologies beyond simple content filtering and toward a more holistic, cognitive-behavioral approach.

---
### **5. References**

1.  Cossio, M. (2025). *A comprehensive taxonomy of hallucinations in Large Language Models*. arXiv:2508.01781v1.
2.  Wallace, E., et al. (2025). *ESTIMATING WORST-CASE FRONTIER RISKS OF OPEN-WEIGHT LLMS*. OpenAI.
3.  Zhao, C., et al. (2025). *Is Chain-of-Thought Reasoning of LLMs a Mirage? A Data Distribution Lens*. arXiv:2508.01191v2.