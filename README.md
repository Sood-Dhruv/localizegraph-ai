# LocalizeGraph AI Copilot: Context-Aware Multilingual Localization & Voice Pipeline

<div align='justify'>
A resource-optimized generative AI pipeline for context-aware media localization. Integrates an in-memory Semantic Knowledge Graph with a 4-bit quantized Llama-3.1-8B model and Meta's MMS-TTS framework to bypass literal
machine translation, ensuring cultural nuance and localized voice synthesis on low-VRAM hardware. Includes an automated LLM-as-a-Judge linguistic validation loop and a Gradio dashboard.
</div>
<br>
<div align='justify'>
An end-to-end, resource-optimized generative AI pipeline that translates, culturally localizes, and synthesizes English source scripts into natural target language voiceovers (Hindi and German).By integrating an in-memory
Semantic Knowledge Graph with a 4-bit quantized Local Large Language Model (Llama-3.1-8B) and Meta’s Massively Multilingual Speech (MMS-TTS) framework, this project bypasses the pitfalls of literal machine translation to
maintain cultural nuance. It also features a fully local LLM-as-a-Judge linguistic evaluation framework and an interactive Gradio content-creation dashboard.
</div>

## System Architecture

The pipeline is engineered to operate efficiently within constrained compute environments (e.g., single 15GB VRAM Tesla T4 GPU), replacing heavy fine-tuning parameters with targeted dynamic context injection.

<p align='center'>
<img width="512" height="768" alt="image" src="https://github.com/user-attachments/assets/304c11b6-df74-434a-aa4f-17f9c4334a13" />
</p>


## Key Engineering Features

### 1. Graph-Augmented Generation (GAG)
<div align = 'justify'>
Instead of relying on heavy weight updates or rigid static string matches, the system utilizes an in-memory directed semantic graph via <code>networkx</code>. When localized idioms, currencies, or regional cultural dynamics are identified, the corresponding native linguistic counterpart is retrieved along with contextual usage notes and dynamically injected into the LLM's prompt context.
</div>

### 2. Low-VRAM Hardware Optimizations
* **4-Bit Quantization:** Quantized via `BitsAndBytesConfig` (NF4, double quantization, compute dtype `torch.bfloat16`) to fit the 8 Billion parameter model into less than 6GB of active VRAM.
* **Dynamic TTS Cache Layer:** Implemented a memory management layer that dynamically routes and caches language tokenizers and `VitsModel` configurations (`facebook/mms-tts-hin` and `facebook/mms-tts-deu`), eliminating cold-start reloading spikes during runtime serialization.
* **Native Script Execution:** Discovered and leveraged native script processing within the MMS tokenization architecture, bypassing standard secondary romanization (`uroman`) pipelines for Hindi, minimizing runtime overhead.

### 3. Automated LLM-as-a-Judge Evaluation
<div align = 'justify'>
To establish data-driven validation, a local Llama-3.1-8B-Instruct instance is prompt-engineered to act as an objective, expert bilingual linguist. It programmatically processes translation output arrays and evaluates them on a strict 1-5 scale for **Cultural Naturalness**, providing automated localization quality assurance (QA).
</div>


## Experimental Evaluation Results

The pipeline was stress-tested against an evaluation dataset containing highly idiomatic English source sequences.

| Language | Average Naturalness Score (1.0 - 5.0) | Critical Research Inspected Phenomenon |
| :--- | :---: | :--- |
| **German (DEU)** | **4.0 / 5.0** | High performance in mapping idiomatic compounding transformations (e.g., transforming "piece of cake" into "*Kinderspiel*"). |
| **Hindi (HIN)** | **3.25 / 5.0** | **Semantic Drift Trapped:** The automated judge successfully penalized over-indexed cultural matching. For instance, mapping the sentence "*Honestly, that framework is not the best option*" to the aggressive idiom "*दाल में कुछ काला है*" (something is fishy) was correctly scored a **2/5**, verifying that the local judge detects forced idiom drift. |

---

## UI Parameters:

Input Box: Accepts text input containing complex Western cultural concepts, monetary expressions, or regional figures of speech.

Knowledge Graph Telemetry: Real-time logging displaying the exact contextual constraints injected from the networkx ledger.

Voice Player: Real-time streaming playback of the synthesized .wav localized files.

## Future Research Architecture
<div align = 'justify'>
LoRA Parameter-Efficient Fine-Tuning: Transitioning prompt injections into specialized Low-Rank Adaptation (LoRA) modules trained on explicit parallel colloquial corpora to reduce reliance on long-context prompt blocks.
</div>
<br>
<div align = 'justify'>
Graph-to-Text Scaling: Expanding the directed graph to dynamically fetch nodes from global ontological stores (such as Wikidata) to extend cultural mapping across broader regional dialects.
</div>
