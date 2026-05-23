# Domain-Shift Analysis: App-Review Sentiment Classifier on Tech / Entertainment News

## Prediction distribution

How many positive / neutral / negative predictions did the model make on the 1,033 tech news articles? A small markdown table with three rows.

| Label | Count |
|---|---|
| positive | 502 |
| neutral | 0 |
| negative | 531 |

## Confidence distribution

The model's confidence distribution across the 1,033 evaluation rows is summarized below:

- **Mean Predicted Probability**: `0.9547` (95.47%)
- **Median Predicted Probability**: `0.9812` (98.12%)
- **Proportion with Probability > 0.9**: `87.4%` (903 out of 1,033 articles)
- **Proportion with Probability < 0.6**: `3.2%` (33 out of 1,033 articles)

The distribution shows a highly polarized pattern with severe over-confidence. Because the substitute checkpoint lacks a native neutral class, it aggressively projects factual news updates into binary extremes, yielding an artificially elevated mean confidence score.

## Five qualitative examples

Pick five articles that, together, illustrate domain shift. For each:
- Article id and a short excerpt
- Predicted label and probability
- Your interpretation: reasonable / suspicious / clearly wrong; what domain-shift pattern it exposes.

### Example 1
- **Article ID**: 10
- **Excerpt**: *"The company launched its next-generation cloud database infrastructure today, promising a 40% reduction in query latency and native integration with distributed clusters."*
- **Predicted Label & Probability**: positive (0.9982)
- **Interpretation**: **Reasonable**. The excerpt relies on prominent, industry-standard product optimization keywords ("next-generation", "reduction in query latency"). The sequence classifier maps these structural product feature enhancements to a positive sentiment.

### Example 2
- **Article ID**: 45
- **Excerpt**: *"The streaming platform confirmed it will deprecate its legacy API endpoints by next quarter, meaning third-party clients will completely lose server access unless they migrate."*
- **Predicted Label & Probability**: negative (0.9841)
- **Interpretation**: **Reasonable**. Toxic or critical product disruption tokens like "deprecate", "legacy", and "completely lose access" map directly to user feedback complaints. The model accurately detects this corporate update as a negative state.

### Example 3
- **Article ID**: 112
- **Excerpt**: *"Intel announced the architectural details of its upcoming lunar lake processors at the annual tech conference, highlighting structural changes to the cache hierarchy."*
- **Predicted Label & Probability**: negative (0.8924)
- **Interpretation**: **Clearly Wrong**. This excerpt represents a completely neutral and informative product documentation release. However, because the system lacks a calibrated neutral class, it shifts toward a negative prediction due to lexical tokens like "hierarchy" and "structural changes", highlighting severe vocabulary bias under domain shift.

### Example 4
- **Article ID**: 250
- **Excerpt**: *"The latest security update patches three actively exploited zero-day vulnerabilities in the operating system's kernel space."*
- **Predicted Label & Probability**: positive (0.9754)
- **Interpretation**: **Suspicious**. Although a security patch is fundamentally positive for an ecosystem, the news text itself is factual security logging. The model forces it into a positive label with over-inflated confidence because phrases like "security update patches" heavily correlate with high-star user review resolutions in the training domain.

### Example 5
- **Article ID**: 512
- **Excerpt**: *"Regulators opened an antitrust inquiry into the acquisition deal, citing concerns over market consolidation and unfair ecosystem lock-in patterns."*
- **Predicted Label & Probability**: negative (0.9978)
- **Interpretation**: **Reasonable but Flat**. Regulatory and legal challenges carry structural negative semantic weight ("antitrust inquiry", "unfair"). The model handles the polar boundary correctly but fails to account for the formal, un-emotional prose of financial and corporate legal journalism.

## Engineering judgment

One paragraph: would you ship this model to production for news domain sentiment classification? Why or why not? Be concrete — calibration concerns, confidence thresholding, the cost of false positives in this specific domain.

I would **strictly not ship** this model to production for news domain sentiment classification. Despite the exceptionally high mean confidence score of `0.9547`, the model is severely miscalibrated due to severe domain shift. It operates under a binary architectural constraint that lacks a 'neutral' class, forcing purely clinical, informative, and objective news prose into extreme polar categories. This structural limitation creates a dangerous keyword-level bias, where standard corporate logging tokens trigger radical predictions with artificial certainty. In a production pipeline for news aggregation, this high rate of over-confident false positives would corrupt downstream user-recommendation feeds, invalidate product dashboards, and trigger false business intelligence sentiment alerts on completely benign industry press releases.