# GenAI Tools for Knowledge Extraction & Technical Documentation in Enterprise Environments: A Comparative Analysis

**Authors:**  
Erik Pegoraro - Lead Software Architect & People Lead - erik.pegoraro@zuhlke.com  
Luis Sauerbronn - Lead Software Architect - luis.sauerbronn@zuhlke.com

## Abstract
This exploration evaluates AI tools for code understanding and documentation generation, focusing on privacy-preserving options for enterprise environments. We benchmarked leading commercial models (Claude Sonnet 3.7, GPT-4o, Gemini 1.5) against locally-hostable alternatives (DeepSeek-R1, codellama, Devstral, Qwen 3) using the Azure Pet Store codebase as a test environment. Our assessment of IDE integrations revealed that Cody (Sourcegraph) with DeepSeek-R1 provides the optimal balance of robust code comprehension, documentation capabilities, and data privacy. This combination enables enterprises to accelerate developer onboarding and automate documentation while maintaining control over sensitive code assets - particularly valuable for organizations in regulated industries with strict data governance requirements.

## Table of Contents
- [Project Vision](#project-vision-accelerating-developer-onboarding-with-ai)
  - [Data Privacy & Security Considerations](#data-privacy--security-considerations)
- [Methodology](#methodology)
  - [Model Evaluation](#model-evaluation)
  - [IDE Plugin and Tool Evaluation](#ide-plugin-and-tool-evaluation)
  - [Test Codebase: Azure Pet Store](#test-codebase-azure-pet-store)
  - [Hardware Considerations](#hardware-considerations)
- [Results](#results)
  - [Benchmark Prompt Comparison](#benchmark-prompt-comparison)
  - [Comprehensive Model Performance Analysis](#comprehensive-model-performance-analysis)
  - [Key Findings from Prompt Analysis](#key-findings-from-prompt-analysis)
  - [Local RAG Chatbots](#local-rag-chatbots)
  - [Commercial IDE Assistants](#commercial-ide-assistants)
  - [Third Party Tools](#third-party-tools)
- [Conclusion](#conclusion)

## Project Vision: Accelerating Developer Onboarding with AI

Consultants are often called in to work on systems they didn’t build—legacy applications that have evolved over decades, or third-party solutions where access to the original developers is limited and documentation is scarce. These environments present a unique set of challenges:

- **Missing or outdated documentation** makes onboarding slow and error-prone.
- **Limited visibility** into third-party code internals, which forces developers to rely on external support channels or make educated guesses when troubleshooting issues.
- **User interviews only work for “what,” not “how”**: We can ask stakeholders what the system does, but not how it works or how it was implemented.
- **Outdated technologies and coding practice**s increase complexity and risk.
- **High stakes for changes**, where even minor tweaks can have unexpected side effects.

For consultants, much of the job becomes detective work: trying to understand how things are wired together, what the system is doing, and how to fix or extend it safely.

This initiative into Generative AI (GenAI) tools aims at streamlining developer onboarding by automating technical documentation and runbook creation. The long-term goal is to reduce onboarding timelines by leveraging AI to understand codebases and generate support materials autonomously.

The target outcome is an AI system capable of covering **80% of documentation and support tasks**, leaving only 20% for manual input. The envisioned tool would:

- Comprehend code and business logic without full documentation
- Fill documentation gaps using code context and existing artifacts
- Act as an intelligent assistant for pair programming scenarios
- Generate architecture overviews and runbooks
- Assist new developers with rapid ramp-up and contextual support

### Data Privacy & Security Considerations

A major driver in selecting tools was **data governance**:

- **Copilot** and most GPT-based tools transmit context/code to cloud servers
- **Cody + Ollama** allows local model use, ideal for regulated industries or IP-sensitive projects
- Tools like **JetBrains AI** offer partial local processing but lack full Copilot-like depth

---

## Methodology

To assess how AI coding assistants can streamline onboarding and documentation, we tested several leading tools and models using the real-world [Azure Pet Store](https://azurepetstore.com/) application. The primary goal was to evaluate each solution's ability to understand code and business logic, assist in documentation, and accelerate developer ramp-up time.

### Model Evaluation

We selected **Claude Sonnet 3.7** as our **benchmark model** for this evaluation, which was conducted prior to the release of Claude Sonnet 4. At the time of testing, Sonnet 3.7 represented the state-of-the-art for code reasoning and summarization tasks, making it the appropriate reference point against which locally hosted models were compared. While Claude Sonnet 4 has since been released and shows promising improvements, our comprehensive evaluation framework and findings remain valid as they establish a consistent baseline for enterprise AI tool selection. Future evaluations would benefit from including Sonnet 4 as an updated benchmark.

#### Model Comparison

| Model               | Context Length | Speed     | Code Exploration Strengths                               | Limitations                                    | Local Hosting |
|---------------------|----------------|-----------|----------------------------------------------------------|------------------------------------------------|---------------|
| **Claude Sonnet 4** | 200K tokens     | ⚡ Fast     | State-of-the-art reasoning, superior code comprehension, advanced planning | Cloud-only, premium pricing                    | ❌             |
| **Claude Sonnet 3.7** | 200K tokens     | ⚡ Fast     | Excellent reasoning, long-context support, deep code comprehension | Cloud-only                                      | ❌             |
| **GPT-4o**          | 128K tokens     | ⚡⚡ Very fast | Strong NL/code interaction, solid doc generation         | Smaller context, cloud-based                   | ❌             |
| **Gemini 1.5 Pro**  | 1M tokens       | ⚡ Fast     | Efficient with large contexts, good recall               | Weaker code understanding depth                | ❌             |
| **DeepSeek-R1 32B** | 128K tokens     | 🐢 Moderate | Excellent for code understanding, strong documentation    | Requires significant GPU memory                | ✅             |
| **DeepSeek-R1 14B** | 128K tokens     | ⚡ Fast     | Good balance of performance and resource requirements    | Less nuanced than larger models                | ✅             |
| **CodeLlama 34B**   | 100K tokens     | 🐢 Moderate | Strong code completion and comprehension                 | High resource requirements, narrower reasoning | ✅             |
| **Devstral 24B**    | 128K tokens     | 🐢 Moderate | Specialized for code understanding                       | Limited general knowledge                      | ✅             |
| **Qwen 3 32B**      | 128K tokens     | 🐢 Moderate | Well-rounded performance for code and documentation      | Requires significant hardware resources        | ✅             |
| **Qwen 3 7B**       | 32K tokens      | ⚡⚡ Very fast | Efficient, low resource requirements                    | Limited context window, simpler reasoning      | ✅             |
| **DeepSeek-R1 1.5B**| 32K tokens      | ⚡⚡⚡ Ultra fast | Extremely lightweight, runs on modest hardware          | Limited comprehension of complex codebases     | ✅             |
| **Llama 3.2 3B**    | 32K tokens      | ⚡⚡ Very fast | Good for simple tasks and code suggestions              | Limited deep reasoning capabilities            | ✅             |

---

### IDE Plugin and Tool Evaluation

We focused on tools that could:
- Integrate with **VS Code** (language/framework agnostic)
- Support **local model inference**
- Offer **context-aware** or **RAG-like** capabilities

| Tool              | Local Hosting | RAG/Context Features       | Summary                                                       |
|------------------|---------------|-----------------------------|----------------------------------------------------------------|
| **TabbyML**       | ✅             | 🚫 Limited RAG             | Promising open-source project; lacks deep context integration |
| **Aider**         | ✅             | ⚠️ Basic context features | Good UX, but lacks deeper structural awareness                |
| **JetBrains AI**  | ✅             | ⚠️ Partial support        | Integrates with LLaMA & DeepSeek; promising, but still early  |
| **Cody (Sourcegraph)** | ✅       | ✅ Rich context, file-aware | Closest match to Copilot; full-featured Claude integration    |

➡️ **Winner: Cody** — paired with DeepSeek or Claude (cloud), it delivered the best mix of performance, privacy, and developer integration.

---

### Test Codebase: Azure Pet Store

Tasks executed on the [Azure Pet Store](https://azurepetstore.com/) codebase included:

- Generating documentation for Pet Store services
- Architecture and service map extraction
- Runbook synthesis
- Code chunking tests across small/large files
- Cross-file comprehension checks

---

### Hardware considerations
For local model testing, we used Apple Silicon M2 PRO with 32GB VRAM, which allowed us to run larger models like DeepSeek-R1 and CodeLlama somewhat effectively. Smaller models like Qwen 3 7B could run on lower-end GPUs (e.g., RTX 3060) but with reduced performance.

Based on [TabbyML recommendations](https://tabby.tabbyml.com/docs/models/):

- **Small models (1B-3B)**: NVIDIA T4, 10 Series, or 20 Series GPUs; Apple Silicon (M1 or newer)
- **Medium to large models (7B-13B)**: NVIDIA V100, A100, 30 Series, or 40 Series GPUs
- **Very large models (32B+)**: High-end consumer GPUs (RTX 4090) or enterprise-grade hardware

Most local models we tested could run on consumer hardware, but performance and response times varied significantly. The smaller Qwen and DeepSeek variants were usable on mid-range GPUs, while the 32B DeepSeek-R1 model required high-end hardware for reasonable inference speeds.

---

## Results
Our exploration followed three distinct paths: building custom RAG solutions from scratch, evaluating commercial IDE assistants, and testing specialized third-party tools. Each approach revealed different strengths and limitations for enterprise documentation generation and code understanding.

### Benchmark Prompt Comparison

To evaluate different models' capabilities, we leveraged the comprehensive framework provided by the [Defra AI SDLC Playbook](https://github.com/DEFRA/defra-ai-sdlc/tree/main), which offers structured guidance on integrating AI into the Software Development Lifecycle. This approach ensured we tested tools with enterprise-grade prompting techniques rather than ad-hoc queries, providing more reliable assessment of each solution's capabilities in professional environments. Furthermore, we created a standardized benchmark prompt focused on business process extraction from code:

```plaintext
You are an AI assistant analyzing an application's codebase to generate documentation. Your task is to infer and describe 
the primary business processes the application supports, based only on the provided codebase.
Application Context: Pet Store is a complete e-commerce system.

Instructions:
* Analyze the codebase, focusing on API endpoint definitions, primary classes/modules, function names, data models/schemas, 
  user interface elements (if frontend code is available), and comments.
* Identify the main user-facing features or capabilities suggested by the code.
* Infer potential user roles or types based on any authentication or authorization logic found. List the roles and their 
  likely permissions.
* Describe the key business workflows. For each workflow:
  * Give it a descriptive name (e.g., "User Registration", "Order Placement", "Report Generation").
  * Outline the likely sequence of steps from a user or system perspective.
  * Mention the main data entities involved (e.g., "Customer", "Product", "Order").
  * Reference specific code components (e.g., API endpoints, key functions/classes) that seem responsible for parts of the workflow.
...
```

This prompt was applied consistently across all tested models to ensure fair comparison.

#### Comprehensive Model Performance Analysis

Our evaluation extended beyond simple business process extraction to include five distinct documentation generation tasks, each targeting different aspects of enterprise software development. The prompts tested models' abilities to generate:

1. **General Documentation** (README/Architecture updates)
2. **API Documentation** (Comprehensive endpoint documentation)
3. **Business Logic Documentation** (Workflow and rule explanation)
4. **Repository Layer Documentation** (Data access pattern documentation)
5. **Service Layer Documentation** (Business service architecture)

Each model was evaluated across multiple dimensions using standardized prompts from the [Defra AI SDLC Playbook](https://github.com/DEFRA/defra-ai-sdlc/tree/main) framework.

##### Detailed Model Comparison

| Model | Business Process Understanding | API Documentation Quality | Technical Architecture Clarity | Code Reference Accuracy | Consistency Across Tasks |
|-------|-------------------------------|---------------------------|-------------------------------|------------------------|-------------------------|
| **Claude 3.7 Sonnet** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Copilot/Claude 4** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **DeepSeek-R1 32B** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Devstral 24B** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| **CodeLlama 34B** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| **Qwen 3 32B** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| **DeepSeek-R1 14B** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| **Llama 3.2 3B** | ⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐ |
| **DeepSeek-R1 1.5B** | ⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐ |

##### Key Findings from Prompt Analysis

**Documentation Structure & Organization:**
- **Claude 3.7 Sonnet** consistently produced well-structured documents with clear hierarchies, proper markdown formatting, and logical flow across all five documentation types
- **DeepSeek-R1 32B** generated comprehensive repository and service layer documentation but occasionally struggled with API endpoint grouping
- **Devstral 24B** excelled at technical architecture documentation but showed weaker performance in business process explanation

**Technical Accuracy & Code Comprehension:**
- **Claude models** demonstrated superior ability to trace code relationships across files, accurately identifying dependency injection patterns and transaction boundaries
- **CodeLlama 34B** showed strong code understanding for individual methods but struggled with system-wide architectural patterns
- **Smaller models (≤14B)** frequently produced generic documentation that missed project-specific implementation details

**Business Context Integration:**
Analysis of the business logic documentation prompts revealed significant differences in models' ability to connect technical implementation with business requirements:
- **Claude 3.7** consistently explained not just "how" but "why" design decisions were made
- **DeepSeek-R1 32B** provided good technical explanations but required more explicit prompting for business rationale
- **Specialized code models** (CodeLlama, Devstral) focused heavily on technical details but often missed broader business context

**Consistency Across Documentation Types:**
The multi-prompt evaluation revealed that **consistency** across different documentation tasks is a key differentiator:
- **Cloud models** (Claude, GPT-4) maintained consistent quality and style across all five documentation types
- **Local models** showed varying performance depending on the documentation type, with repository layer documentation generally being stronger than business process documentation

##### Resource Requirements vs. Output Quality

Based on actual hardware testing across the prompt suite:

| Model Size | Hardware Requirement | Documentation Quality | Time to Generate | Cost Effectiveness |
|------------|---------------------|----------------------|------------------|-------------------|
| **32B+** | RTX 4090/A100 | ⭐⭐⭐⭐ | 🐢 Slow | High (enterprise) |
| **14B-24B** | RTX 3080/4070 | ⭐⭐⭐ | ⚡ Moderate | Medium |
| **7B-13B** | RTX 3060/4060 | ⭐⭐ | ⚡⚡ Fast | High (small teams) |
| **≤3B** | Integrated GPU | ⭐⭐ | ⚡⚡⚡ Very Fast | Limited utility |

**Enterprise Deployment Considerations:**
The comprehensive prompt analysis revealed that for enterprise documentation generation:
- **32B models** provide the best balance of comprehensiveness and accuracy for complex legacy systems
- **14B models** offer sufficient quality for most documentation tasks while requiring more accessible hardware
- **Sub-7B models** are suitable only for simple code commenting and basic documentation updates

This analysis informed our recommendation that enterprises should target **14B+ parameter models** for serious documentation automation, with 32B models reserved for critical systems requiring the highest accuracy.

### Local RAG Chatbots

In our first approach, we each set out to build a retrieval augmented generation (RAG) chatbot from scratch. We began by parsing the codebase and splitting it into manageable "chunks," which we then converted into vector embeddings. With this index in place, user questions could trigger a search for the most relevant snippets of code, and both the retrieved context and the query were submitted to a locally hosted Ollama model for response generation.

As we worked, several challenges emerged. Selecting the right context proved especially tricky: deciding which files or code fragments to include in a given query was both labor-intensive and prone to error. When we broke large classes into smaller pieces, their meaning sometimes vanished; conversely, when chunks were too big, the model struggled to focus on the most critical details. On top of that, the local Ollama models we relied on had limited ability to interpret highly domain specific logic, which led to superficial or off base explanations at times. Furthermore, overcoming these context issues would require significant investment in building a more sophisticated tool—one capable of automating context selection, dynamic chunking, and seamless orchestration of information.

Despite these hurdles, our RAG prototype demonstrated the core concept's viability. It could retrieve code snippets, feed them to an LLM, and produce coherent answers—showing that a locally run RAG chatbot can, in principle, help consultants make sense of an unfamiliar codebase. However, to achieve the reliability and depth of insight we need in practice, this approach would demand more advanced prompt engineering and a smarter strategy for orchestrating and refining the contextual information fed to the model. Further refinements made on this codebase, by using storing hierarchical information as a graph, and using a graph database to store the information (GraphRAG) proved a substantial improvement, revealing that appart from the model selection, one of the most relevant factors for the performance of the chatbot is the ability to select the right context. Comparatively, following this approach was much more time-consuming than using a pre-built solution. Educationally valid, but not cost-effective for enterprise use. The sourcode for this prototype is available on this [GitHub Repo](https://github.com/ErikMPZuhlke/rag_chatbot_project).

### Commercial IDE Assistants

When we turned to **JetBrains AI Assistant**, its integration into IntelliJ immediately felt natural. As we navigated the codebase, the assistant offered inline explanations of methods and classes right next to the source, and it suggested refactorings—everything from renaming variables for clarity to extracting common logic into helper functions. This tight IDE integration made it easy to see AI feedback in real time, without context switching. However, JetBrains AI Assistant often lost track of broader workflows: when a feature spanned several packages or modules, its recommendations focused narrowly on the current file and failed to tie together related pieces elsewhere in the project.

By contrast, **GitHub Copilot** demonstrated a stronger grasp of cross file relationships and higher level structure. As we explored different parts of the codebase, Copilot not only generated clear, well commented code snippets, but also pointed us toward relevant modules we hadn't yet opened. For example, when we asked about the system's payment processing, Copilot suggested the specific service and configuration files where that logic lived—saving us the manual detective work of tracing interface calls across layers. Its explanations felt more coherent and connected, making it easier to understand how individual components fit into the overall architecture.

In terms of **overall outcome**, Copilot's cloud-based model won hands down for out of the box insight quality. Its larger, continuously trained backend delivered more nuanced answers and code suggestions without the heavy prompt tuning we needed elsewhere. The trade off, of course, is that every snippet or context window is sent to GitHub's servers for processing—a fact that raises important data privacy considerations for any consulting engagement. Nonetheless, for teams comfortable with that model, Copilot proved to be the fastest path to actionable, system wide understanding.

### Third Party Tools

Aider offered some intriguing, specialized capabilities—like customizable retrieval pipelines and quick code annotations—but its integration felt more like an early prototype than a polished tool. In practice, switching between the IDE and external windows disrupted our flow, and certain features would occasionally break or return inconsistent results.

By contrast, Sourcegraph Cody impressed with its seamless IDE integration: code suggestions, definitions, and references appeared inline as if built into the editor itself. Importantly, Cody can leverage a locally hosted LLM via Ollama, ensuring that the heavy inference work stays on-premises — although it still requires an Internet connection for licensing or metadata checks. When we paired it with a larger local model, Cody handled more complex queries with ease (for example, accurately pinpointing where the repository enforces order validation).

**Outcome:** Cody delivered the best mix of usability, security, and depth of insight. Its tight IDE integration and on prem inference struck the right balance—but before rolling it out, we'll need to verify exactly what metadata Cody sends externally and explore its fully on prem server option.

---

## Conclusion

As AI development tools mature, teams must weigh performance, context integration, and data privacy. Our comprehensive evaluation across custom RAG solutions, commercial IDE assistants, and specialized third-party tools revealed distinct trade-offs between capabilities and security concerns.

Our benchmark testing across five documentation generation tasks confirmed that:

- **Claude Sonnet 3.7** remains the gold standard for cloud-based code reasoning and documentation generation, consistently producing the most comprehensive documentation with accurate flow diagrams, precise code references, and superior business context integration across all evaluated tasks.
- **DeepSeek-R1 32B** proved the **most capable locally hosted model**, enabling high-quality AI coding support without external data transmission. While requiring significant hardware resources, it approaches cloud-model quality for complex documentation tasks and business process understanding.
- **Cody (Sourcegraph)** emerged as the clear winner among IDE assistants, offering near-Copilot functionality with deep context handling, flexible backend integration (cloud or local), and seamless editor integration that maintains developer workflow.

The exploration highlighted crucial challenges in knowledge extraction—particularly context selection and dynamic chunking—that even leading models struggle with. Our custom RAG prototypes demonstrated these difficulties firsthand, showing that while GraphRAG approaches can significantly improve context relevance, custom solutions require substantial engineering investment to match pre-built alternatives.

**Key insights from our comprehensive prompt analysis:**
- **Parameter size directly correlates with documentation quality**: Models with 14B+ parameters are essential for meaningful enterprise documentation automation
- **Consistency across documentation types** is a critical differentiator, with cloud models maintaining superior quality across diverse tasks
- **Specialized code models** (CodeLlama, Devstral) excel at technical details but struggle with broader business context integration

For enterprises seeking to combine performance with control, **Cody + DeepSeek-R1 32B** presents a production-ready pairing that accelerates onboarding, enhances documentation, and respects codebase privacy—marking a significant step toward **AI-augmented software development** while maintaining enterprise data governance requirements.

Importantly, locally-hosted models offer significant **cost advantages when centrally deployed on enterprise infrastructure**. Unlike per-seat SaaS pricing models of cloud solutions, organizations can leverage existing compute resources to serve multiple development teams from a centralized inference endpoint, providing substantial economies of scale as adoption increases across the enterprise. This economic model becomes particularly attractive for organizations with existing GPU infrastructure or those planning strategic AI investments.

**Looking Forward:** As models continue to evolve rapidly—with Claude Sonnet 4 already showing promising improvements—enterprises should focus on establishing flexible AI toolchains that can adapt to model upgrades while maintaining consistent integration patterns and data governance controls.