# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date: 14-03-2026
# Register no.: 212224230111
# Aim:
To write and implement Python code that integrates with multiple AI tools 
(such as OpenAI GPT, Hugging Face, and Google Gemini) to automate API interactions, 

## AI Tools Used:
ChatGPT-5 Go

GEMINI 2.5 pro

## Project: AI Career & Skill Gap Recommender

**Project description :**  
An assistant that analyzes a candidate’s resume (text) and a desired job role, then recommends the top skills to learn, explains the skill gaps, and suggests next steps/resources. In this experiment we take **one part** of the project — the *skill-gap detection & recommendation* function — and generate two comparable code versions using **the same prompt**. We then compare the outputs and evaluate them.

---

## Prompt used for both AI tools :

"Write a Python function recommend_skills(resume_text: str, target_role: str) -> dict that:

Accepts plain-text resume and a target job role (e.g., 'Data Scientist').

Uses a simple role-to-skill mapping (hardcoded inside the function) to get required skills for the role.

Extracts skills from the resume using basic keyword matching (case-insensitive).

Returns a dictionary with: 'required_skills', 'found_skills', 'missing_skills' (top 3), and 'action_plan' with short learning suggestions for each missing skill.

Keep code simple and dependency-free (only builtin Python).

Provide a small example call at the end with sample resume text and target role, and print the result."

### OUTPUT:
#### Chatgpt:
```py
# recommender_chatgpt.py
def recommend_skills_chatgpt(resume_text: str, target_role: str) -> dict:
    # simple role -> required skills mapping
    role_skills = {
        "data scientist": ["python", "statistics", "machine learning", "deep learning", "sql", "data visualization"],
        "data analyst": ["sql", "excel", "python", "data visualization", "statistics"],
        "ml engineer": ["python", "ml engineering", "deep learning", "docker", "model deployment"],
    }
    resume_lower = resume_text.lower()
    required = role_skills.get(target_role.lower(), [])
    found = []
    for skill in required:
        # match whole word approx — basic check
        if skill in resume_lower:
            found.append(skill)
    missing = [s for s in required if s not in found]
    # pick top 3 missing (if fewer than 3, return all)
    top_missing = missing[:3]

    suggestions = {
        "python": "Practice projects, official tutorials, and 'Automate the Boring Stuff'.",
        "statistics": "Take an introductory statistics course and practice hypothesis tests.",
        "machine learning": "Complete supervised learning projects and follow scikit-learn tutorials.",
        "deep learning": "Study neural networks and try basic TensorFlow/PyTorch tutorials.",
        "sql": "Practice joins, aggregations and write queries on sample databases.",
        "data visualization": "Learn matplotlib/Seaborn or Tableau basics with projects.",
        "ml engineering": "Learn model serving, CI/CD for ML, and Docker basics.",
        "docker": "Follow Docker getting-started guides and containerize a small app.",
        "model deployment": "Explore Flask/FastAPI + simple cloud deployment tutorials."
    }

    action_plan = {m: suggestions.get(m, "Search for beginner tutorial online.") for m in top_missing}

    return {
        "target_role": target_role,
        "required_skills": required,
        "found_skills": found,
        "missing_skills": top_missing,
        "action_plan": action_plan
    }

```

#### Gemini Output:
```py
# recommender_gemini.py
def recommend_skills_gemini(resume_text: str, target_role: str) -> dict:
    # role -> skills with synonyms
    role_skills = {
        "data scientist": [
            ["python", "python3"], 
            ["statistics", "statistical analysis"], 
            ["machine learning", "ml"], 
            ["deep learning", "neural network"], 
            ["sql", "database"], 
            ["data visualization", "viz", "matplotlib", "seaborn", "tableau"]
        ]
    }
    resume_lower = resume_text.lower()
    required_flat = []
    found = []
    for group in role_skills.get(target_role.lower(), []):
        # group is list of synonyms; treat first as canonical
        canonical = group[0]
        required_flat.append(canonical)
        matched = False
        for token in group:
            if token in resume_lower:
                matched = True
                break
        if matched:
            found.append(canonical)

    missing = [s for s in required_flat if s not in found]

    # Score missing by simple heuristic: if not found and contains multiword maybe higher priority
    # We choose the top 3 by original order
    top_missing = missing[:3]

    resource_templates = {
        "python": "Build small projects and practice on code platforms (e.g., Kaggle).",
        "statistics": "Take an applied statistics course with exercises.",
        "machine learning": "Do supervised learning projects and Kaggle tutorials.",
        "deep learning": "Follow a beginner DL course and implement simple networks.",
        "sql": "Practice SQL queries on sample datasets (joins, group by).",
        "data visualization": "Build charts with matplotlib/seaborn; try Tableau basics."
    }
    action_plan = {m: resource_templates.get(m, "Look up beginner resources online.") for m in top_missing}

    return {
        "target_role": target_role,
        "required_skills": required_flat,
        "found_skills": found,
        "missing_skills": top_missing,
        "action_plan": action_plan
    }
```

### Comparison & Evaluation:
We evaluate the two code versions on **Accuracy**, **Explainability**, **Efficiency**, and **Ease of Integration** using a 1–5 scale (5 = best).  
Scores are based on the code approach, clarity, and likely usefulness when integrated into the full project.

| **Metric**               | **ChatGPT-style (Version A)** | **Gemini-style (Version B)** | **Comparision points** |
|---------------------------|------------------------------:|------------------------------:|-----------|
| Accuracy                  | 4 / 5                        | 4 / 5                        | Both use keyword matching; accuracy limited by mapping completeness. |
| Explainability            | 5 / 5                        | 4 / 5                        | Version A returns canonical names and clear suggestions; easy to read. |
| Efficiency (runtime)      | 5 / 5                        | 5 / 5                        | Both are O(n) simple text checks — very fast. |
| Robustness to synonyms    | 3 / 5                        | 4 / 5                        | Version B includes synonym groups, slightly more robust. |
| Ease of Integration       | 5 / 5                        | 4 / 5                        | Version A is straightforward; Version B needs mapping prep. |


### Conclusion:
Both versions generated functional and accurate results for skill gap detection.  
The **ChatGPT-style (Version A)** was simpler, easier to integrate, and more explainable,  
while the **Gemini-style (Version B)** handled synonyms better and was slightly more flexible.  
Combining both approaches would create an optimized, accurate, and scalable recommender module for the AI Career & Skill Gap Recommender project.

**Result:**  
The prompts were executed successfully, and both AI tool versions produced accurate skill gap recommendations for the AI Career & Skill Gap Recommender project.
