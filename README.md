# SHAP Molecular Interpreter - User Guide

## Overview

The **SHAP Molecular Interpreter** is an interactive web application designed to bridge the gap between machine learning model outputs and chemical understanding. It translates raw SHAP importance values into actionable chemical insights for chemists and medicinal chemists.

---

## Problem It Solves

**Traditional ML Output:**
```json
{
  "LogP": 0.315,
  "TPSA": 0.247,
  "MW": 0.152,
  ...
}
```

**What Chemists Need to Know:**
- What is LogP and why does it matter?
- How does this LogP value (1.19) affect my molecule?
- Is this good or bad?
- What can I do to optimize?
- Does my molecule follow drug-like properties?

**This app answers all these questions in seconds.**

---

## Getting Started

### 1. Open the Application
Download `shap_interpreter.html` and open it in any web browser (Chrome, Firefox, Safari, Edge).

### 2. Prepare Your JSON Data
Your SHAP output should follow this format:

```json
{
  "smiles": "CC(=O)OC1=CC=CC=C1C(=O)O",
  "predicted_class_or_value": -0.428,
  "confidence": 0.7,
  "feature_importance_vector": {
    "LogP": 0.315,
    "TPSA": 0.247,
    "MW": 0.152,
    "HBD": 0.098,
    "HBA": 0.062,
    "RB": 0.051,
    "HeavyAtoms": 0.042,
    "Aromatic": 0.008,
    "Charged": 0.004
  },
  "feature_values": {
    "LogP": 1.19,
    "TPSA": 37.3,
    "MW": 180.16,
    "HBD": 1,
    "HBA": 4,
    "RB": 0,
    "HeavyAtoms": 13,
    "Aromatic": 1,
    "Charged": 0
  }
}
```

**Required fields:**
- `feature_importance_vector` or `feature_importance`: Dictionary of feature names → importance scores
- `feature_values` (recommended): Dictionary of feature names → actual values for the molecule

**Optional but helpful:**
- `smiles`: Molecular structure
- `predicted_class_or_value`: Model prediction
- `confidence`: Prediction confidence (0-1)

### 3. Paste and Analyze
1. Copy your JSON
2. Paste into the text area
3. Click "Analyze & Interpret"
4. View rich chemical explanations

Or click "Load Example" to see a demo with Aspirin.

---

## Understanding the Output

### Feature Cards

Each feature gets a comprehensive card showing:

1. **Feature Name & Value**
   - Name of the molecular descriptor
   - Actual numerical value for your molecule

2. **ML Importance Bar**
   - Shows how much this feature influenced the model's prediction
   - Color-coded: Red (high) → Yellow (medium) → Green (low)

3. **Definition**
   - What the feature measures
   - Chemical significance

4. **Range Gauge**
   - Visual representation of typical ranges
   - Shows where your molecule sits
   - Color zones: Red (bad) → Yellow (okay) → Green (optimal)

5. **Interpretation**
   - Status: ✓ Good, ⚠️ Warning, or ⚠️ Concern
   - What it means for your molecule

6. **Why It Matters**
   - Chemical and biological significance
   - How it affects drug properties

7. **For Your Molecule**
   - Specific interpretation based on your value
   - Pros and cons of your molecule's value

8. **Optimization Tips**
   - How to improve this feature if needed
   - Structural modifications to consider

### Summary Section

**Top Features Driving Prediction**
- The 3 most important features for your prediction
- Understanding these helps predict model behavior on similar molecules

**Key Insights & Warnings**
- Any features with concerning or warning status
- Highlights potential issues to address

**Optimization Suggestions**
- For each important feature, specific modification suggestions
- Actionable guidance for medicinal chemists

**Lipinski's Rule of Five Assessment**
- Pass/fail for four Lipinski rules:
  - MW ≤ 500 Da
  - LogP ≤ 5
  - HBD ≤ 5
  - HBA ≤ 10
- Green box = pass, Red box = fail

**Chemical Interpretation Summary**
- Overall assessment of your molecule
- Drug-likeness classification
- Expected bioavailability

---

## Molecular Features Explained

### LogP (Lipophilicity)
- **Definition:** Log of octanol-water partition coefficient
- **What it means:** How fat-soluble vs water-soluble your molecule is
- **Why it matters:** Affects membrane penetration, absorption, toxicity
- **Drug range:** 0-5 is typical
- **Your value interpretation:**
  - < 0: Very water-soluble (hydrophilic)
  - 0-2: Good balance for absorption
  - 2-5: Lipophilic, good penetration
  - > 5: Too lipophilic, liver toxicity risk

**Actions:**
- Increase LogP → Add aromatic rings, remove polar groups
- Decrease LogP → Add OH, NH groups, add charges

---

### TPSA (Topological Polar Surface Area)
- **Definition:** Sum of van der Waals surface area of polar atoms (N, O)
- **What it means:** How "polar" your molecule is
- **Why it matters:** Predicts oral absorption and BBB penetration
- **Drug range:** < 140 Ų for good absorption
- **Your value interpretation:**
  - < 20 Ų: Excellent penetration (BBB+)
  - 20-75 Ų: Good bioavailability
  - 75-140 Ų: Adequate absorption
  - > 140 Ų: Poor absorption, too polar

**Actions:**
- Decrease TPSA → Remove OH, NH groups
- Increase TPSA → Add polar groups if needed for specificity

---

### MW (Molecular Weight)
- **Definition:** Sum of atomic masses
- **What it means:** How "big" your molecule is
- **Why it matters:** Affects solubility, permeability, clearance
- **Drug range:** 200-500 Da (Lipinski < 500)
- **Your value interpretation:**
  - < 200 Da: Very small, excellent penetration
  - 200-500 Da: Optimal for drugs
  - 500-800 Da: Large, absorption concerns
  - > 800 Da: Very large, poor bioavailability

**Actions:**
- Increase MW → Add substituents, chain extensions
- Decrease MW → Remove groups, simplify structure

---

### HBD (Hydrogen Bond Donors)
- **Definition:** Count of N-H and O-H groups
- **What it means:** How many hydrogen bonds your molecule can donate
- **Why it matters:** Affects solubility, target binding, absorption
- **Drug range:** 0-5 (Lipinski)
- **Your value interpretation:**
  - 0: No donors, good penetration
  - 1-3: Moderate, good balance
  - 4-5: Approaching limit
  - > 5: Too many, poor absorption

**Actions:**
- Increase HBD → Add OH, NH groups
- Decrease HBD → Remove HBA groups, use esters instead of acids

---

### HBA (Hydrogen Bond Acceptors)
- **Definition:** Count of N and O atoms (not groups, atoms)
- **What it means:** How many hydrogen bonds your molecule can accept
- **Why it matters:** Affects potency, selectivity, absorption
- **Drug range:** 0-10 (Lipinski)
- **Your value interpretation:**
  - 0-3: Few acceptors, may lack potency
  - 4-7: Optimal for binding
  - 8-10: Good, but at upper limit
  - > 10: Too many, absorption issues

**Actions:**
- Increase HBA → Add N, O atoms
- Decrease HBA → Remove N, O, use isosteres

---

### RB (Rotatable Bonds)
- **Definition:** Count of non-ring single bonds that can rotate
- **What it means:** How "flexible" your molecule is
- **Why it matters:** Affects binding efficiency, conformation diversity
- **Drug range:** 0-10
- **Your value interpretation:**
  - 0-2: Rigid, locked conformation
  - 3-10: Flexible, good conformational space
  - 11-15: Very flexible, binding challenges
  - > 15: Too flexible, likely poor binding

**Actions:**
- Increase RB → Extend chains, add flexible linkers
- Decrease RB → Add rings, use constraints

---

### HeavyAtoms
- **Definition:** Count of all non-hydrogen atoms
- **What it means:** Molecular complexity
- **Why it matters:** Correlates with synthesis difficulty, cost
- **Drug range:** 10-50
- **Your value interpretation:**
  - < 10: Very simple
  - 10-30: Optimal
  - 30-50: Complex but manageable
  - > 50: Synthesis challenges

---

### Aromatic
- **Definition:** Count of aromatic ring systems
- **What it means:** How much aromatic character your molecule has
- **Why it matters:** Affects potency, metabolic stability, selectivity
- **Drug range:** 1-2 typically
- **Your value interpretation:**
  - 0: Non-aromatic, may lack potency
  - 1-2: Good aromatic content
  - 3: Well-aromatic, watch metabolism
  - > 3: Over-aromatic, metabolic issues

**Actions:**
- Increase Aromatic → Add benzene/pyridine rings
- Decrease Aromatic → Replace with aliphatic, saturated rings

---

### Charged
- **Definition:** Net formal charge on the molecule
- **What it means:** Is molecule protonated/deprotonated?
- **Why it matters:** Affects solubility, permeability, binding
- **Drug range:** -1 to +1 (neutral preferred)
- **Your value interpretation:**
  - 0: Neutral, good absorption
  - ±1: Singly charged, acceptable
  - ±2: Doubly charged, absorption concerns
  - > ±2: Highly charged, poor bioavailability

---

## Workflow Examples

### Example 1: Drug Lead Optimization

You have a lead compound and want to understand what drives the model's prediction.

**Workflow:**
1. Generate SMILES for your lead
2. Calculate molecular descriptors
3. Get SHAP importance from your model
4. Paste JSON into interpreter
5. Review top 3 features
6. Check Lipinski assessment
7. Use optimization tips to guide next synthesis

**Output:** "LogP is the main driver. Reducing aromatic rings will lower LogP and improve absorption."

---

### Example 2: Toxicity Prediction

You have a SHAP model predicting drug-induced liver injury (DILI).

**Workflow:**
1. Get SHAP explanation for DILI prediction
2. Review which features increase/decrease DILI risk
3. Understand that high LogP (> 3) is a risk factor
4. Identify which substituents to remove

**Output:** "Your molecule's high LogP (3.8) is the main driver of DILI risk. Consider adding polar groups or removing lipophilic substituents."

---

### Example 3: Target Selectivity

You want to understand what makes your molecule selective for a target.

**Workflow:**
1. Train SHAP model on target binding
2. Explain predictions for related molecules
3. Identify key selectivity features (e.g., aromatic rings, HBA)
4. Use insights to design next generation

**Output:** "TPSA is critical for selectivity. Your molecule's low TPSA (25) enables BBB penetration, distinguishing it from off-target effects."

---

## Tips & Best Practices

### For Best Results

1. **Include all features** in your JSON that your model uses
2. **Provide feature_values** so the gauge shows where your molecule sits
3. **Include SMILES** for context and structure verification
4. **Use consistent feature names** that match the database (see Supported Features)

### Interpreting Warnings

- ✓ **Green Status:** Good, your molecule is well-optimized
- ⚠️ **Yellow Status:** Warning, consider adjustments
- ⚠️ **Red Status:** Concern, likely needs modification

### Lipinski's Rules

The summary shows Lipinski assessment:
- **0 violations:** Excellent drug-like properties
- **1 violation:** Acceptable, but monitor this property
- **2+ violations:** May have bioavailability issues

---

## Supported Features

The interpreter includes built-in explanations for:

| Feature | Type | Range | Notes |
|---------|------|-------|-------|
| LogP | Lipophilicity | -2 to 10 | Partition coefficient |
| TPSA | Polarity | 0-200+ Ų | Polar surface area |
| MW | Size | 50-1000 Da | Molecular weight |
| HBD | Donors | 0-10 | Hydrogen bond donors |
| HBA | Acceptors | 0-20 | Hydrogen bond acceptors |
| RB | Flexibility | 0-20 | Rotatable bonds |
| HeavyAtoms | Complexity | 1-100 | Non-H atom count |
| Aromatic | Rings | 0-6 | Aromatic ring count |
| Charged | Charge | -5 to +5 | Net formal charge |

---

## Limitations

1. **Feature interpretations are general** - specific domains (e.g., antibiotic vs anticancer) may have different rules
2. **No molecular structure visualization** - bring mol_visualizer.py separately for images
3. **No batch processing** - analyze one molecule at a time
4. **Assumes standard molecular features** - custom features require manual interpretation

---

## Advanced Usage

### Comparing Multiple Molecules

1. Analyze molecule A
2. Screenshot/note key features
3. Analyze molecule B
4. Compare their feature profiles

### Extracting for Reports

Features and interpretations can be copied directly into:
- Research papers
- Patent applications
- Internal reports
- Presentations

### Integration with ML Workflows

```python
# After SHAP analysis
import json

explanation = {
    "smiles": molecule_smiles,
    "predicted_class_or_value": model.predict(X),
    "confidence": probability,
    "feature_importance_vector": {feat: shap_val for feat, shap_val in zip(feature_names, shap_values)},
    "feature_values": {feat: X[feat] for feat in feature_names}
}

# Export for interpretation
with open("shap_explanation.json", "w") as f:
    json.dump(explanation, f, indent=2)

# Open shap_interpreter.html in browser and paste JSON
```

---

## FAQ

**Q: Can I add my own features?**
A: The app includes the most common molecular descriptors. For custom features, you'll need to modify the `featureDatabase` JavaScript object.

**Q: What if my features have different names?**
A: Feature names must match exactly (LogP, not log_p or logp). Use consistent naming from your model.

**Q: Can I save the interpretation?**
A: Use browser print-to-PDF functionality or screenshot the results.

**Q: Does this replace medicinal chemistry expertise?**
A: No - this tool helps explain ML models. Always validate with domain expertise and experimental data.

**Q: What about other molecular features (e.g., rotatable bonds, ring count)?**
A: The tool supports 9 common descriptors. Others can be added - contact for custom versions.

---

## Technical Details

### Data Processing
- All calculations are client-side (no data leaves your browser)
- JSON is parsed and visualized instantly
- No external API calls required

### Browser Support
- Chrome/Chromium: ✓
- Firefox: ✓
- Safari: ✓
- Edge: ✓
- Mobile browsers: ✓ (responsive design)

### File Size
- Single HTML file: ~50 KB
- No dependencies or downloads required
- Works offline

---

## Support & Improvements

The interpreter can be extended with:
- More molecular features
- Domain-specific guidelines (drugs vs agrochemicals vs cosmetics)
- Batch processing for multiple molecules
- PDF export functionality
- Integration with molecular drawing tools

---

## Example: Understanding Your First SHAP Output

**Scenario:** You trained a model to predict drug absorption. SHAP explains your lead compound:

```json
{
  "smiles": "CC(=O)OC1=CC=CC=C1C(=O)O",  // Aspirin
  "predicted_class_or_value": 0.75,  // High absorption predicted
  "confidence": 0.82,
  "feature_importance_vector": {
    "TPSA": 0.35,  // Most important
    "LogP": 0.28,
    "MW": 0.18,
    "HBD": 0.12,
    "RB": 0.07
  },
  "feature_values": {
    "TPSA": 37.3,  // Excellent value
    "LogP": 1.19,  // Perfect range
    "MW": 180,     // Optimal
    "HBD": 1,      // Good
    "RB": 0        // Rigid - good
  }
}
```

**Interpretation:**

1. **Model predicts high absorption (0.75)** with good confidence
2. **TPSA is most important (0.35)** → Your molecule's TPSA of 37 is in the sweet spot (20-75)
3. **LogP is important (0.28)** → Your molecule's LogP of 1.19 is perfect (0-5 range)
4. **MW matters (0.18)** → Your molecule is small (180 Da) - easy to absorb
5. **Overall assessment:** This is a well-optimized drug-like molecule for absorption

**Actionable insight:** "Your lead has excellent absorption properties. The main drivers are balanced LogP and moderate polarity (TPSA)."

---

**Created for chemists by ML practitioners. Bridge the gap between data and chemistry! 🧬🔬**

