# Hotel Review Sentiment Analysis & Extraction

A sophisticated NLP pipeline that extracts key sentences from multi-sentence hotel reviews and performs sentiment classification using advanced text processing techniques and VADER sentiment analysis.

## 🎯 Project Overview

This project implements a **three-stage NLP pipeline** designed to automatically:
1. **Extract the most impactful sentence** from each multi-sentence hotel review
2. **Analyze sentiment** of extracted sentences with tuned thresholds
3. **Evaluate performance** across positive, neutral, and negative reviews

The model processes 100 hotel and travel reviews, combining tokenization, word frequency analysis, and lexicon-based sentiment classification to deliver interpretable, production-ready results.

## ✨ Key Features

- **Intelligent Sentence Extraction**: Normalized word-frequency scoring prevents longer sentences from monopolizing importance
- **Stop Word Filtering**: Removes common English words to focus on semantic content
- **Sentiment Lexicon Integration**: Custom high-value sentiment words (3x weight) for accurate importance scoring
- **Advanced Tie-Breaking**: Uses adjective density and VADER sentiment intensity to resolve scoring conflicts
- **Tuned VADER Thresholds**: Optimized classification boundaries for improved neutral sentiment detection
- **Comprehensive Evaluation**: Includes accuracy metrics, classification reports, and confusion matrices

## 📊 Dataset

- **Size**: 100 hotel and travel reviews
- **Composition**: 34 positive, 33 neutral, 33 negative reviews
- **Sentences per review**: 3-5 sentences (ensures meaningful comparative scoring)
- **Tone variation**: Ranges from highly positive to highly negative with mixed sentiments
- **Structure**: Randomized order to prevent classification bias

### Sample Data Structure

| Column | Type | Description |
|--------|------|-------------|
| `email_num` | Integer | Unique identifier (1-100) |
| `paragraph` | String | Original multi-sentence review |
| `top_sentence` | String | Extracted most important sentence |
| `predicted_sentiment` | String | Classified sentiment (Positive/Neutral/Negative) |

## 🔧 How It Works

### Stage 1: Preprocessing
```
Paragraph → Sentence Tokenization → Word Tokenization → Stop Word Removal
```
Breaks reviews into clean, meaningful words by removing common English words (is, the, was, etc.).

### Stage 2: Extract Key Sentence
Uses **normalized word frequency scoring** to identify the single most important sentence:

**Formula**: `Score = (Word Frequency Sum / Word Count) × Multipliers`

**Scoring Adjustments**:
- **3x multiplier** on high-value sentiment words (exceptional, filthy, amazing, etc.)
- **1.5x multiplier** if sentence contains many adjectives
- **Negation detection** to flag skeptical sentiments

**Tie-breaking** (when multiple sentences have equal scores):
1. Priority to negated high-value words
2. Then absolute sentiment intensity (VADER score)
3. Then first occurrence in original review

### Stage 3: Classify Sentiment
Uses **VADER** (Valence Aware Dictionary and sEntiment Reasoner) to classify the extracted sentence.

**Thresholds**:
| Sentiment | Threshold |
|-----------|-----------|
| Positive | ≥ +0.05 |
| Neutral | -0.05 to +0.05 |
| Negative | ≤ -0.05 |

## 📈 Performance Analysis

### Why Different Sentiments Show Different Accuracy

**Neutral Sentiment Challenges**:
- Inherently contains mixed language patterns ("good but slow")
- Trade-off statements ("quiet location but far from city")
- Requires nuanced threshold tuning for accurate capture
- Wide neutral window (-0.05 to +0.05) improves recall

**Negative Sentiment Complexity**:
- Often uses complex linguistic structures and indirection
- May contain qualified criticisms and sarcasm
- Lexicon-based approach struggles without full context
- Sentences like "Not as advertised" confuse word-by-word analysis

**Positive Sentiment Advantage**:
- Features strong, unambiguous sentiment words
- Consistent positive framing throughout
- Clear VADER signals with high accuracy
- Least prone to misinterpretation

## 🚀 Quick Start

### Installation
```bash
git clone https://github.com/username/hotel-review-sentiment-analyzer.git
cd hotel-review-sentiment-analyzer

pip install pandas nltk textblob scikit-learn

python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords'); nltk.download('vader_lexicon')"
```

### Run the Analysis
```bash
jupyter notebook NLP_hotel_reviews.ipynb
```

**Output Files**:
- `hotel_reviews.csv` - Generated dataset with labels
- `assignment_output.csv` - Final results with extracted sentences and predictions

## 📁 Project Structure
```
hotel-review-sentiment-analyzer/
├── README.md                      # This file
├── NLP_hotel_reviews.ipynb        # Main Jupyter notebook
├── hotel_reviews.csv              # Generated dataset with labels
├── assignment_output.csv          # Final output with predictions
└── requirements.txt               # Python dependencies
```

## 📋 Outputs

### Evaluation Metrics
- **Accuracy**: Overall classification correctness
- **Classification Report**: Precision, recall, and F1-score per sentiment class
- **Confusion Matrix**: Detailed breakdown of correct vs. incorrect predictions
- **Sentiment Distribution**: Count and percentage of each predicted sentiment

### Sample Output
```
Email #  | Paragraph                      | Top Sentence                   | Sentiment
---------|--------------------------------|--------------------------------|-----------
1        | The room was comfortable... | The room was incredibly clean. | Positive
2        | Check-in was fine but... | The location was fair overall. | Neutral
3        | Poor service and broken AC... | The AC system was broken. | Negative
```

## 🧠 Strengths & Limitations

### ✅ Strengths of This Approach
- **Interpretable**: Understand why each classification occurs
- **Fast**: No training phase; runs in linear time
- **Robust**: Works without domain-specific labeled training data
- **Production-Ready**: Minimal computational overhead
- **Effective**: High accuracy on clearly positive/negative reviews

### ❌ Limitations
- **Context-Blind**: Struggles with negation and complex sentence structures
- **Sarcasm-Deaf**: Cannot detect irony ("Best day ever...not")
- **Fixed Vocabulary**: Unknown words receive no sentiment weight
- **Balanced Language**: Mixed sentiments within single sentences are challenging

## 🎓 Educational Value

This project demonstrates:
- **NLP Fundamentals**: Tokenization, preprocessing, stop word removal
- **Text Analysis**: Word frequency analysis and importance scoring
- **Sentiment Analysis**: Lexicon-based classification and threshold tuning
- **Data Evaluation**: Metrics, confusion matrices, and performance analysis
- **Problem-Solving**: Designing scoring algorithms that handle real-world complexity

## 💡 Industry Application

**Use Case**: Hotel management teams processing high volumes of guest feedback need to:
- Quickly identify key issues from lengthy reviews
- Classify guest satisfaction automatically
- Prioritize responses to negative feedback
- Track sentiment trends over time

This pipeline provides a lightweight, interpretable solution that doesn't require extensive labeled training data.

## 📚 Dependencies

- `pandas`: Data manipulation and CSV handling
- `nltk`: Natural Language Processing toolkit
- `textblob`: Additional text processing capabilities
- `scikit-learn`: Machine learning evaluation metrics
- `collections.Counter`: Efficient word frequency counting