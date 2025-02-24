# Context-Sensitive Spelling Correction with N-gram Language Models  
**Jupyter Notebook Implementation**  

This repository contains a **context-aware spelling correction system** implemented in a Jupyter Notebook. The system uses bigram language models and beam search to correct spelling mistakes while considering surrounding context (e.g., "dking sport" → "doing sport" vs. "dking species" → "dying species").  

---

## 📊 Key Results  
| Metric                                  | Accuracy |
|-----------------------------------------|----------|
| **Norvig's Unigram Corrector**          | 70.6%    |
| **Context-Sensitive Corrector (Ours)**  | 92.1%    |

**Test Set**: Noisy COCA Fivegrams (30% noise probability)  

---

## 🚀 Quick Start  

### 1. Prerequisites  
- Python 3.7+  
- Jupyter Notebook  

```bash
pip install jupyter numpy matplotlib
```

### 2. Run the Notebook  
1. Download the datasets from this repo.

2. Launch the notebook:  
```bash
jupyter notebook context_spelling_correction.ipynb
```

---

## 🧠 Implementation Highlights  

### 1. Language Model  
- **Datasets**: Combines `big.txt` (Norvig), `coca_all_links.txt` (COCA), and custom bigrams.  
- **Weighting**: GloVe-inspired frequency weighting (`x_max=68,298` for unigrams, `alpha=0.55`) to balance common/rare words.  
- **Smoothing**: Add-1 Laplace smoothing for unseen bigrams.  

### 2. Context-Aware Corrector  
- **Candidate Generation**: Damerau-Levenshtein edit distance (up to 3 edits).  
- **Beam Search**: Width=25, edit penalty `λ=0.3` to prioritize contextually likely corrections.  
- **Example**:  
  ```python
  spell_checker.correct("dking sport")  # → "doing sport"
  spell_checker.correct("dking species")  # → "dying species"
  ```

---

## 📝 Justification  

### Why Our Model Outperforms Norvig’s (92.1% vs 70.6%)  
1. **Context Sensitivity**:  
   - Uses bigram probabilities (e.g., "doing sport" vs. "dying species") instead of isolated word frequencies.  
2. **Weighted Frequencies**:  
   - Limits dominance of ultra-frequent words (e.g., "the", "and") via GloVe-style weighting.  
3. **Beam Search Efficiency**:  
   - Explores 25 most promising correction paths while penalizing excessive edits.  

### Tradeoffs  
- **Speed**: Beam search adds computational cost vs. Norvig’s unigram model.  
- **Vocabulary Bias**: Relies on training data coverage (fails on OOV words like rare proper nouns).  

---

## 🔍 Evaluation Details  

### Test Set Generation  
- **Source**: 1,000 clean five-word sequences from COCA.  
- **Noise**: 30% probability of edits (deletions, transpositions, replacements, insertions).  

### Error Analysis  
| Error Type                | Example Input       | Norvig Output       | Our Output          |
|---------------------------|---------------------|---------------------|---------------------|
| **Contextual Disambiguation** | "dking sport"       | "dying sport" ❌     | "doing sport" ✅    |
| **Multi-Edit Corrections**    | "tihs examppel"     | "this example" ✅    | "this example" ✅    |

---

## 🛠 Future Improvements  
1. **Transformer Integration**: Leverage pretrained models (e.g., BERT) for long-range context.  
2. **Dynamic Edit Penalty**: Adjust `λ` based on error type (e.g., prioritize transpositions for common typos).  
3. **Subword Modeling**: Handle rare/OOV words via byte-pair encoding (BPE).  

---

## 📚 Resources  
- [Norvig’s Spell Corrector](https://norvig.com/spell-correct.html)  
- [COCA N-grams](https://www.ngrams.info/download_coca.asp)  
- [Damerau-Levenshtein Algorithm](https://en.wikipedia.org/wiki/Damerau–Levenshtein_distance)  

---

**Note**: The Jupyter Notebook includes interactive visualizations of unigram/bigram distributions and step-by-step correction examples.
