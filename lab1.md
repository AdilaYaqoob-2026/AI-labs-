# Lab 1 Exercises 

- Exercise 1: Degree Honors Rule
- Exercise 2: Tracing Forward Chaining

---

## Exercise 1: Degree Honors RuleAdd a new rule to UNIVERSITY_KB["rules"] for students completing their degree program:
Condition: cgpa >= 3.70 and disciplinary_warnings == 0 
→
 Status: "Graduated with Magna Cum Laude Honors".

### Solution
Add the rule below to the knowledge base to get a new rule for degree honors

```python
        {
            "id": "RULE_07",
            "conditions": {"cgpa_min": 3.70, "disciplinary_warnings": 0},
            "condition_type": "degree_honors",
            "grade": "A+",
            "status": "Graduated with Magna Cum Laude Honors",
            "action": "Eligible for graduate study and academic distinction."
        }
```

### Updated Inference Engine
Now, extend the inference engine to evaluate the degree rule as well:

```python
    # Rule 3: Degree honors rule
    if "cgpa" in student_record and "disciplinary_warnings" in student_record:
        degree_rule = next(
            (r for r in kb["rules"] if r["condition_type"] == "degree_honors"),
            None
        )
        if degree_rule is not None:
            if student_record["cgpa"] >= 3.70 and student_record["disciplinary_warnings"] == 0:
                return {
                    "rule_id": degree_rule["id"],
                    "grade": degree_rule["grade"],
                    "status": degree_rule["status"],
                    "action": degree_rule["action"]
                }

    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Invalid Score Entry",
        "action": "Verify submitted marks with the exam department."
    }
```

### Example Student Record
```python
student_3 = {
    "student_id": "BSCS-2024-099",
    "student_name": "Nadia Ali",
    "total_marks": 90,
    "attendance": 88,
    "cgpa": 3.85,
    "disciplinary_warnings": 0
}

result_3 = evaluate_student(student_3, UNIVERSITY_KB)
print(f"Grade: {result_3['grade']} | Status: {result_3['status']}")
print(f"Advisory Action: {result_3['action']}")
```

### Expected Output
```python
Grade: A+ | Status: Graduated with Magna Cum Laude Honors
Advisory Action: Eligible for graduate study and academic distinction.
```

---

## Exercise 2: Tracing Forward Chaining

### Solution
Modify the `evaluate_student()` function so it prints each rule evaluation as it checks conditions. This demonstrates forward chaining step by step.

```python
# =====================================================================
# STEP 2: INFERENCE ENGINE WITH FORWARD CHAINING TRACE
# =====================================================================

def evaluate_student(student_record, kb):
    """
    Evaluates a student record using forward chaining.
    Prints each rule being checked to show step-by-step reasoning.
    """
    print(f"\n--- EVALUATING STUDENT: {student_record['student_name']} (ID: {student_record['student_id']}) ---")

    # Check attendance rule first
    print("Checking attendance rule...")
    if student_record["attendance"] < 75:
        print("Condition satisfied: attendance < 75")
        attendance_rule = next(r for r in kb["rules"] if r["condition_type"] == "attendance_below")
        print(f"Triggered Rule: {attendance_rule['id']}")
        return {
            "rule_id": attendance_rule["id"],
            "grade": attendance_rule["grade"],
            "status": attendance_rule["status"],
            "action": attendance_rule["action"]
        }
    else:
        print("Condition not satisfied: attendance >= 75")

    # Check marks range rules
    marks = student_record["total_marks"]
    print(f"Checking marks range rules for total marks = {marks}")
    for rule in kb["rules"]:
        if rule["condition_type"] == "marks_range":
            min_mark = rule["conditions"]["marks_min"]
            max_mark = rule["conditions"]["marks_max"]

            print(f"Evaluating {rule['id']}: {min_mark} <= {marks} <= {max_mark}")

            if min_mark <= marks <= max_mark:
                print(f"Condition satisfied: marks fall in range for {rule['id']}")
                return {
                    "rule_id": rule["id"],
                    "grade": rule["grade"],
                    "status": rule["status"],
                    "action": rule["action"]
                }
            else:
                print(f"Condition failed: {rule['id']} not matched")

    # Check degree honors rule
    if "cgpa" in student_record and "disciplinary_warnings" in student_record:
        print("Checking degree honors rule...")
        degree_rule = next((r for r in kb["rules"] if r["condition_type"] == "degree_honors"), None)

        if degree_rule is not None:
            print(
                f"Evaluating {degree_rule['id']}: "
                f"cgpa >= {degree_rule['conditions']['cgpa_min']} and "
                f"disciplinary_warnings == {degree_rule['conditions']['disciplinary_warnings']}"
            )

            if student_record["cgpa"] >= 3.70 and student_record["disciplinary_warnings"] == 0:
                print(f"Condition satisfied: {degree_rule['id']} triggered")
                return {
                    "rule_id": degree_rule["id"],
                    "grade": degree_rule["grade"],
                    "status": degree_rule["status"],
                    "action": degree_rule["action"]
                }
            else:
                print(f"Condition failed: degree honors rule not satisfied")

    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Invalid Score Entry",
        "action": "Verify submitted marks with the exam department."
    }
```

### Test Example
```python
student_4 = {
    "student_id": "BSCS-2024-150",
    "student_name": "Hamza Iqbal",
    "total_marks": 82,
    "attendance": 90,
    "cgpa": 3.80,
    "disciplinary_warnings": 0
}

result_4 = evaluate_student(student_4, UNIVERSITY_KB)
print(f"Grade: {result_4['grade']} | Status: {result_4['status']}")
print(f"Advisory Action: {result_4['action']}")
```

### Sample Trace Output
```python
--- EVALUATING STUDENT: Hamza Iqbal (ID: BSCS-2024-150) ---
Checking attendance rule...
Condition not satisfied: attendance >= 75
Checking marks range rules for total marks = 82
Evaluating RULE_02: 85 <= 82 <= 100
Condition failed: RULE_02 not matched
Evaluating RULE_03: 75 <= 82 <= 84
Condition satisfied: marks fall in range for RULE_03

Grade: B | Status: Passed
Advisory Action: Good standing. Maintain momentum.
```

---

## Final Notes
These exercises demonstrate two important ideas in expert systems:

- Knowledge Base: stores rules and facts.
- Inference Engine: applies logic to data and produces decisions.
- Forward chaining: checks rules in sequence and triggers the first rule that matches the data.

If you want, I can also convert these answers into a complete Google Colab notebook style version with all cells arranged exactly as you would paste into Colab.
