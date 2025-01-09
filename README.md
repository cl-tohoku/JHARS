# JHARS (Japanese Hallucination Assessment in RAG Settings)

JHARS is a comprehensive benchmark dataset for evaluating hallucinations (the phenomenon of generating content not present in given information sources) in Japanese Large Language Models (LLMs) in Retrieval-Augmented Generation (RAG) settings.

## Overview

Hallucination is a critical challenge in the practical application of Large Language Models. JHARS was developed to quantitatively evaluate and understand the characteristics of hallucinations in Japanese LLMs.

### Key Features

- Sentence level annotations on 450 Japanese LLM responses in RAG settings
- Evaluation of multiple state-of-the-art models (including GPT-4o)
- Performance assessment of hallucination detection methods

## Dataset

This dataset includes:

- 450 annotated LLM responses
- Scripts for hallucination evaluation
  
## Key Findings

1. Relatively low hallucination rate in LLM responses
2. Evidence of critical hallucinations that warrant fact-checking 
3. Difficulty in achieving both high precision and recall in automatic detection
4. High recall possible for critical hallucinations


## Dataset Structure
```js
{
    "id": number,            // Unique identifier for each QA pair
    "question": string,      // Question in Japanese
    "reference_text": string,// Reference text used to generate answer
    "[model_name]": {        // Model response object (gpt-4o, gpt-4o-mini, Llama-3.1-Swallow-8B-Instruct-v0.1)
        "response": string,  // Model's answer in Japanese
        "annotations": {     // Annotation data
            "aggregated": {  // Consensus from multiple annotators
                "is_valid_answer": boolean,     // Whether response is valid
                "sentence_annotations": [        // Array of sentence-level annotations
                    {
                        "sentence": string,      // Target sentence
                        "annotation_status": string,     // e.g., "completed"
                        "hallucination_type": string,    // "No_hallucination", "Contradictory", "Unverifiable" 
                        "hallucination_text": string[],  // Identified hallucination text
                        "hallucination_text_start_offset": number[], // Start positions of hallucination text
                        "hallucination_text_end_offset": number[],   // End positions of hallucination text
                        "verification_uncertainty_reason": string[][], // Reasons for verification uncertainty
                        "contradiction_uncertainty_reason": string[][], // Reasons for contradiction uncertainty
                        "agreement_status": string       // "unanimous", "majority", "disputed"
                    }
                ]
            }
        }
    }
}
```

Note on hallucination types:
  - "No_hallucination": No hallucination detected
  - "Contradictory": Intrinsic hallucination - content that contradicts the reference text
  - "Contradictory_uncertain": Annotator uncertain about contradiction with reference
  - "Unverifiable": Extrinsic hallucination - content that cannot be verified using the reference text
  - "Unverifiable_uncertain": Annotator uncertain about verifiability from reference


The dataset includes two evaluation settings for hallucination types:
- Relaxed setting: Treats uncertain cases as their base types
  - "Contradictory_uncertain" → "Contradictory"
  - "Unverifiable_uncertain" → "Unverifiable"
- Strict setting: Maintains distinction between all five hallucination types listed above


## Usage

```bash
# Clone the repository
git clone https://github.com/cl-tohoku/JHARS.git
cd JHARS
```


```python
import pandas as pd

# Load and analyze JHARS dataset
df = pd.read_json('data/sentence_annotation/annotated_data_relaxed.jsonl', lines=True)
print(df.head())
``` 


<!-- 
## Citation

If you use this research in your work, please cite using the following BibTeX:

```bibtex
@inproceedings{JHARS2024,
    title = "JHARS: Japanese Hallucination Assessment in RAG Settings",
    author = "...",  # Author names will be updated upon paper publication
    booktitle = "Proceedings of ...",  # Conference name will be updated upon paper publication
    year = "2024"
}
``` 
-->


## License

This project is licensed under Apache License 2.0. See the `LICENSE` file for details.

<!-- ## Contributing

1. Fork the project
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Create a Pull Request -->

## Acknowledgments

This work was supported through a research collaboration between AI Shift Inc. and Tohoku University.


## Contact

- Research inquiries:
  - ryohei.kamei.s4 at dc.tohoku.ac.jp
  - sakata.masaki.s5 at dc.tohoku.ac.jp
- Bug reports & feature requests: GitHub Issues

---
