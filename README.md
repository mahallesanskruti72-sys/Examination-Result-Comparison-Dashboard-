
# Examination Result Comparison Dashboard
# Google Colab Compatible

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# -----------------------------
# 1. Create Examination Dataset
# -----------------------------

data = {
    "Student": ["Aarav", "Priya", "Rahul", "Sneha", "Rohan",
                "Anjali", "Vivek", "Neha", "Karan", "Pooja"],

    "Maths": [85, 72, 65, 90, 58, 78, 88, 69, 75, 92],

    "Science": [80, 75, 70, 88, 62, 81, 85, 72, 78, 89],

    "English": [78, 82, 68, 91, 60, 76, 84, 75, 70, 94],

    "Computer": [92, 85, 75, 95, 65, 88, 90, 79, 82, 96]
}

df = pd.DataFrame(data)

# -----------------------------
# 2. Calculate Total & Percentage
# -----------------------------

subjects = ["Maths", "Science", "English", "Computer"]

df["Total"] = df[subjects].sum(axis=1)
df["Percentage"] = df["Total"] / len(subjects)

# Pass/Fail condition
df["Result"] = df[subjects].apply(
    lambda row: "Pass" if all(row >= 35) else "Fail",
    axis=1
)

print("EXAMINATION RESULT DATA")
print(df)

# -----------------------------
# 3. Dashboard
# -----------------------------

plt.figure(figsize=(16, 12))

# Chart 1: Student Percentage Comparison
plt.subplot(2, 2, 1)

sns.barplot(
    data=df,
    x="Student",
    y="Percentage"
)

plt.title("Student Percentage Comparison")
plt.xlabel("Students")
plt.ylabel("Percentage (%)")
plt.xticks(rotation=45)

# -----------------------------
# Chart 2: Subject-wise Average
# -----------------------------

plt.subplot(2, 2, 2)

average_marks = df[subjects].mean()

sns.barplot(
    x=average_marks.index,
    y=average_marks.values
)

plt.title("Average Marks by Subject")
plt.xlabel("Subjects")
plt.ylabel("Average Marks")

# -----------------------------
# Chart 3: Total Marks Comparison
# -----------------------------

plt.subplot(2, 2, 3)

sns.lineplot(
    x=df["Student"],
    y=df["Total"],
    marker="o"
)

plt.title("Total Marks Comparison")
plt.xlabel("Students")
plt.ylabel("Total Marks")
plt.xticks(rotation=45)

# -----------------------------
# Chart 4: Pass / Fail
# -----------------------------

plt.subplot(2, 2, 4)

result_count = df["Result"].value_counts()

plt.pie(
    result_count.values,
    labels=result_count.index,
    autopct="%1.1f%%",
    startangle=90
)

plt.title("Pass / Fail Distribution")

plt.tight_layout()
plt.show()

# -----------------------------
# 4. Display Top Performer
# -----------------------------

top_student = df.loc[df["Percentage"].idxmax()]

print("\nTOP PERFORMER")
print("Student:", top_student["Student"])
print("Percentage:", top_student["Percentage"], "%")
print("Result:", top_student["Result"])

# -----------------------------
# 5. Subject-wise Performance
# -----------------------------

print("\nAVERAGE MARKS IN EACH SUBJECT")

for subject in subjects:
    print(subject, ":", round(df[subject].mean(), 2))
