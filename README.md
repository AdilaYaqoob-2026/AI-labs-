# AI-labs-
AI lab activity
UNIVERSITY_KB = {
    "rules": [
        {
            "id": "degree_honors_magna_cum_laude",
            "condition": {
                "cgpa": {"operator": ">=", "value": 3.70},
                "disciplinary_warnings": {"operator": "==", "value": 0},
            },
            "condition_type": "all",
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

