# Red-Teaming Predictive Biology Models via Adversarial Stress Testing

An AI safety evaluation framework exploring distribution shifts, extrapolation flatlining, and specification gaming inside predictive microbiology machine learning systems.

## 📌 Project Overview
In high-stakes biological domains, food manufacturing facilities, and biosecurity infrastructures, automated machine learning pipelines are increasingly deployed to monitor pathogen proliferation and control supply-chain interventions. 

This repository provides an empirical **AI Safety Audit** of a high-performance predictive microbiology model ($R^2 = 0.9633$). While the system appears robust under standard performance cross-validation, adversarial stress testing exposes critical alignment failures where mathematical edge cases decouple drastically from biological reality.

---

## 🔬 Methodology & System Design
The target model is an optimized **Random Forest Regressor** trained on a simulated dataset representing thousands of bacterial batches exposed to three core environmental vectors:
*   **Temperature ($4^\circ\text{C}$ to $45^\circ\text{C}$)**
*   **pH ($4.0$ to $8.5$)**
*   **Water Activity ($a_w$: $0.85$ to $1.0$)**

The underlying ground truth maps to classical cardinal parameter secondary growth models used to predict the growth rate ($\log_{10} \text{CFU/hr}$) of foodborne pathogens like *Salmonella enterica*.

---

## 🚨 Critical Vulnerability Discoveries

### Vulnerability A: Extrapolation Failure & Safety Flatlining (OOD Shift)
*   **The Attack Vector:** Pushing the system into highly alkaline scenarios ($\text{pH} > 8.5$) while holding temperature at a dangerous incubation level ($35^\circ\text{C}$).
*   **The Failure Mode:** Because tree-based regressors cannot extrapolate beyond their localized training ranges, the AI's predictions completely flatlined at $\approx 0.6914 \log_{10} \text{CFU/hr}$, failing to capture the physical reality that extreme pH levels denature cellular structures and completely halt growth. 

### Vulnerability B: Residual Noise Artifacts & False Positive Triggers
*   **The Attack Vector:** Gradual environmental desiccation (lowering water activity below $0.89$).
*   **The Failure Mode:** At severe drought thresholds where osmotic pressure makes growth biologically impossible ($0.0$), the model's prediction continuously hovered between $0.0013$ and $0.0121$. It overfitted onto measurement noise artifacts rather than learning the absolute biological lower bound, creating systematic false-positive loop cycles.

---

## 📊 Visual Diagnostics
Below are the audited safety vulnerability curves generated during the red-teaming sequence:

![AI Safety Biological Audit](ai_safety_biological_audit.png)

---

## 💡 AI Safety Insights & Takeaways
1.  **The Accuracy Fallacy:** A model scoring $>96\%$ accuracy on historical test sets can still harbor critical, systematic blind spots when introduced to Out-of-Distribution (OOD) edge cases.
2.  **Socio-Technical Risk:** If an automated plant safety loop operates under a strict binary constraint (`if growth > 0: shut_down_line`), residual noise artifacts at completely safe levels would cause continuous, unforced operational failure. Conversely, flatlining predictions in alkaline zones could mask active threats.
3.  **The Need for Hybrid Bounds:** AI models handling biological risks must be augmented with hardcoded physics-informed or biology-informed constraints to prevent purely mathematical optimizations from overriding empirical laws of nature.

---

## 🛠️ How to Replicate
1. Clone this repository.
2. Open `audit_sandbox.ipynb` in Google Colab or any Jupyter environment.
3. Run all cells to retrain the architecture and generate local threat evaluation models.# biological-ai-safety-audit
An AI safety evaluation framework exploring distribution shifts and adversarial robustness inside predictive microbiology models.
