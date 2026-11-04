# AI-labs-
AI lab activity
# lab 1
##Exercise 1: Degree Honors RuleAdd a new rule to UNIVERSITY_KB["rules"] for students completing their degree program:
#Add the code below in knowledge base 
Condition: cgpa >= 3.70 and disciplinary_warnings == 0 
→
 Status: "Graduated with Magna Cum Laude Honors".
UNIVERSITY_KB = {
    "rules": [
        {
            "id": "RULE 7",
            "condition": {
                "cgpa": {"operator": ">=", "value": 3.70},
                "disciplinary_warnings": {"operator": "==", "value": 0},
            },
            "condition_type": "award",
            "grade": "Magna Cum Laude",
            "status": "Graduated with Magna Cum Laude Honors",
            "action": "Award Magna Cum Laude honors",
        }
    ]
}


def evaluate_condition(student, field_condition):
    """Evaluate a single field condition against a student's record."""
    for field, comparison in field_condition.items():
        if field not in student:
            return False

        value = student[field]
        operator = comparison["operator"]
        target = comparison["value"]

        if operator == ">=" and not value >= target:
            return False
        if operator == "==" and not value == target:
            return False
    return True


def evaluate_rules(student, knowledge_base=UNIVERSITY_KB):
    """Return the first matching rule's result, or None if none match."""
    for rule in knowledge_base["rules"]:
        if rule["condition_type"] == "all":
            if evaluate_condition(student, rule["condition"]):
                return {
                    "id": rule["id"],
                    "grade": rule["grade"],
                    "status": rule["status"],
                    "action": rule["action"],
                }

    return None


# Example
student = {
    "cgpa": 3.85,
    "disciplinary_warnings": 0,
}

result = evaluate_rules(student)
print(result)
# {'id': 'degree_honors_magna_cum_laude',
#  'grade': 'Magna Cum Laude',
#  'status': 'Graduated with Magna Cum Laude Honors',
#  'action': 'Award Magna Cum Laude honors'}

