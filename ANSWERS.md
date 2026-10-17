# Midterm 1: answers

GitHub username:aidashunayeva-sys
Session code: KETTLE-62
Fingerprint: 687a0422

## AI-use declaration
During this exam, I used Gemini AI as an assistant to support data analysis, coding, and document structuring as needed.
Specifically, I used the AI assistant for the following tasks:
1. **Scripting and Automation:** Generating and refining Python code to load `retrieval_ranks.csv` and `doc_results.csv` in Google Colab, and calculating evaluation metrics such as Recall@k, MRR@10, and overall document extraction accuracy.
2. **Statistical Calculations:** Writing functions to accurately compute 95% Wilson score confidence intervals for retrieval recall and pipeline cell accuracies.
3. **Cost Modeling and Formulas:** Verifying the token pricing logic and annual cost formulas based on daily traffic volumes, prompt token counts, and cloud server hosting fees provided in `DATA.md`.
4. **Drafting and Formatting:** Structuring the final text in Markdown format for `ANSWERS.md`, including polishing the compliance memo and ensuring all short-answer explanations adhered to the required word counts.
   
All calculations, model selection trade-offs, budget verification, and legal/ethical justifications were carefully reviewed, verified, and finalized by me.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A |0.6167 |0.7667 | 0.8333| 0.7046|
| B | 0.3333|0.5167 |0.7333 |0.4570 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 |0.7667 |0.6456 | 0.8556|
| A, k=5 | 0.8333| 0.7197|0.9069 |
| B, k=3 |0.5167 |0.3931 |0.6383 |
| B, k=5 | 0.7333|0.6099 | 0.8287|

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 18489.41|
| A, k=5 | 25231.96|
| B, k=3 | 11173.19|
| B, k=5 |13038.26 |

### 1d. Decision (at most 150 words)

Chosen candidate:A, k=3

I select Candidate A with k=3 as it achieves an overall recall of 76.67% (95% CI: [64.56%, 85.56%]) and strictly satisfies the $21,900 annual budget limit with an annual cost of $18,489.41. Option A, k=5 exceeds the budget at $25,231.96. Candidate B is cheaper but significantly underperforms in retrieval quality (51.67% at k=3 and 73.33% at k=5).

Language-specific recall breakdown for Candidate A (k=3):
- English (EN): 0.9000 (90.0%)
- Russian (RU): 0.7000 (70.0%)
- Kazakh (KK): 0.7000 (70.0%)
Candidate A with k=3 delivers superior cross-lingual performance while remaining financially viable.
### 1e. Next measurement (at most 60 words)
Measure end-to-end LLM answer generation accuracy (e.g., via RAGAS faithfulness/answer relevance metrics or human expert pass rate) and generation latency to ensure that retrieved contexts produce correct customer responses within acceptable response times.
## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H |0.8778 |KK, degraded |0.6667 |0.4878 |0.8077 |6223.32 |
| L |0.8389 | KK, degraded|0.7000 | 0.5212|0.8334 |10800 |

Personal-data pages per year sent abroad under H:273750

### 2b. Memo (at most 300 words)
To: Bank Executive Committee  
Subject: Document Extraction Pipeline Selection and Compliance Strategy

I recommend deploying Pipeline L for production document processing. 

While Pipeline H offers a slightly higher overall accuracy (87.78% vs 83.89%), it transfers approximately 273,750 document pages containing individual names and IINs to overseas data centres annually. Under Republic of Kazakhstan personal data protection laws, cross-border transmission of sensitive citizen identification data creates critical legal compliance risks and potential regulatory liabilities.
Pipeline L operates entirely on local infrastructure within Kazakhstan, guaranteeing zero external data exposure. Although Pipeline L incurs a higher fixed annual cost ($10,800 vs $6,223.32), local hosting completely mitigates regulatory non-compliance risks and protects customer data sovereignty.
Furthermore, evaluation shows that Pipeline L handles degraded Kazakh scans better than Pipeline H (70.0% vs 66.67% accuracy in its weakest cell). We will implement local pre-processing image enhancement to bridge the performance gap on clean scans.

## Part 3: Explain your own numbers

### Q1

Number:2.39
Why (at most 40 words):Output tokens are priced 6x higher than input tokens. Kazakh requires significantly more input tokens (4,445 vs 1,576) and output tokens (216 vs 138), skewing the final bill ratio above the token volume ratio.

### Q2

Number:13.5%
Why (at most 40 words): e5 expands cl100k tokens by ~2.73%. A 576-token chunk becomes ~591.7 e5 tokens, exceeding e5's 512-token context window. The trailing 79.7 tokens (~13.5%) are truncated, losing tail-end context during embedding generation.

### Q3

Number: 63.1%
Why (at most 40 words): Top-p = 0.9 keeps the top tokens whose cumulative probability reaches 90%, then renormalizes their probabilities. Top-p truncates the tail distribution dynamically, whereas lower temperature reshapes the entire logit probability distribution continuously.

### Q4

Number:0.82
Why (at most 40 words): High document frequency ($df$) penalizes ubiquitous terms across the corpus. A high $df$ severely lowers Inverse Document Frequency ($IDF$), significantly downweighting common words in BM25/TF-IDF similarity scoring relative to rare domain-specific terms.

### Q5

Number:18.70
Why (at most 40 words): The model trained on 374 tokens/parameter versus Chinchilla's compute-optimal 20 tokens/parameter (ratio 18.7). The model is over-trained relative to Chinchilla parameters; Chinchilla noted models like GPT-3 were significantly under-trained for their parameter sizes.
