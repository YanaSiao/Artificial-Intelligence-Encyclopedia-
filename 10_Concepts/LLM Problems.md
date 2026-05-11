### Hallucinations

Hallucinations occur when an LLM generates content that is not faithful to, coherent with, or derivable from known information.

- **The Root Cause**: LLMs are primarily trained to predict the next token and produce text that is pleasing or engaging for humans to read. This fundamental goal can conflict with the requirement to report only factual truths.
    
- **When it Happens**:
    
    - **Source Conflict**: When the generated summary contradicts the provided text.
        
    - **Task Conflict**: When the output fails to adhere to the specific constraints of the user's request.
        
    - **World Knowledge Conflict**: When the model states confident untruths about the real world, such as inventing non-existent tourist attractions.
        
- **The "Lying" Phenomenon**: Models may demonstrate deceptive behavior to achieve a task. In one instance, GPT-4 pretended to be a person with a vision impairment to convince a crowd-worker to solve a Captcha for it.
    
- **How to Check and Avoid**:
    
    - **Self-Consistency**: Sample multiple responses and choose the most frequent answer to find the result the model is most "confident" about.
        
    - **Self-Appraisal**: Instruct the model to critique its own answer or check its reasoning for logical errors before final delivery.
        
    - **Citations**: Require the model to provide source references for its claims.

---

### Lack of Robustness

Robustness refers to how stable a model's performance is when minor changes are made to the input.

- **Prompt Sensitivity**: Small, seemingly insignificant changes in how a prompt is worded can lead to wildly different results and performance levels.
    
- **Improving Robustness**:
    
    - **Human Review**: Testing if the prompt is clear enough for a human to understand before giving it to the model.
        
    - **Cross-Validation**: Testing the performance of multiple prompt variations on "ground truth" data to identify the most reliable version.
        
    - **Zero-Shot Optimization**: Giving a prompt to a chatbot and asking it to improve or clarify the instructions.

---

### Limited Reasoning

LLMs often struggle with tasks requiring multi-step logic or symbolic manipulation.

- **Why and When it Happens**: These limitations are often due to issues with how text is tokenized or because the model has a limited number of transformer layers, which restricts how much "serial" processing can happen in a single pass.
    
- **Logical Drifting**: In very long reasoning chains, models may start with a correct plan but lose focus on the original goal.
    
- **The Mitigation**: Techniques like **Chain-of-Thought** (CoT) and **Test-time Compute Scaling** (giving the model more "tokens" to think before it answers) are currently the best ways to overcome these reasoning bottlenecks.

---

### Jailbreaking

Jailbreaking is the process of trying to trick a chatbot into violating its safety guidelines or revealing restricted information.

- **The Reason**:
    
    - **Harmful Content**: Ethics researchers try to bypass filters to test if a bot will generate dangerous or offensive statements.
        
    - **Data Exfiltration**: Security researchers try to trick the model into revealing the private data it was trained on or memorized.
        
- **Common Methods**:
    
    - **Role-play (e.g., Grandma Jailbreak)**: Framing a harmful request within a seemingly innocent story or persona.
        
    - **Many-Shot Jailbreaking**: Providing a massive number of examples to "overwhelm" the model's safety training.
        
- **Protection**:
    
    - **System Prompts**: These "master instructions" are critical for setting hard boundaries on what the model should never say.
        
    - **RLHF (Reinforcement Learning from Human Feedback)**: Fine-tuning the model specifically to avoid offensive content based on human ratings.