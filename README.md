# Gradebook
import tkinter as tk
from tkinter import messagebox, END


class GradebookApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Gradebook Application")
        self.root.geometry("550x600")

        self.records = []  # Each record: {"name": ..., "assignment": ..., "score": ...}

        # Title
        title_label = tk.Label(root, text="Gradebook", font=("Arial", 20, "bold"))
        title_label.pack(pady=10)

        # -----------------------------
        # INPUT FIELDS
        # -----------------------------
        input_frame = tk.Frame(root)
        input_frame.pack(pady=5)

        tk.Label(input_frame, text="Name").grid(row=0, column=0)
        self.name_entry = tk.Entry(input_frame, font=("Arial", 12), width=14)
        self.name_entry.grid(row=1, column=0, padx=5)

        tk.Label(input_frame, text="Assignment").grid(row=0, column=1)
        self.assignment_entry = tk.Entry(input_frame, font=("Arial", 12), width=14)
        self.assignment_entry.grid(row=1, column=1, padx=5)

        tk.Label(input_frame, text="Score").grid(row=0, column=2)
        self.score_entry = tk.Entry(input_frame, font=("Arial", 12), width=6)
        self.score_entry.grid(row=1, column=2, padx=5)

        self.add_button = tk.Button(input_frame, text="Add", command=self.add_record, width=10)
        self.add_button.grid(row=1, column=3, padx=5)

        edit_button = tk.Button(input_frame, text="Edit", command=self.edit_selected, width=10)
        edit_button.grid(row=1, column=4, padx=5)

        delete_button = tk.Button(input_frame, text="Delete", command=self.delete_selected, width=10)
        delete_button.grid(row=1, column=5, padx=5)

        clear_button = tk.Button(input_frame, text="Clear All", command=self.clear_records, width=10)
        clear_button.grid(row=1, column=6, padx=5)

        # -----------------------------
        # LISTBOX
        # -----------------------------
        list_frame = tk.Frame(root)
        list_frame.pack(pady=10)

        self.listbox = tk.Listbox(list_frame, width=60, height=12, font=("Arial", 12))
        self.listbox.pack(side=tk.LEFT, padx=(0, 5))

        scrollbar = tk.Scrollbar(list_frame, orient=tk.VERTICAL, command=self.listbox.yview)
        scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
        self.listbox.config(yscrollcommand=scrollbar.set)

        # -----------------------------
        # BUTTONS
        # -----------------------------
        buttons_frame = tk.Frame(root)
        buttons_frame.pack(pady=10)

        btn_specs = [
            ("Show Passing", self.show_passing),
            ("Show Failing", self.show_failing),
            ("Statistics", self.show_statistics),
            ("Letter Grades", self.show_letter_grades),
            ("Summary", self.show_summary),
            ("Save", self.save_records),
            ("Load", self.load_records),
        ]

        for i, (text, cmd) in enumerate(btn_specs):
            tk.Button(buttons_frame, text=text, command=cmd, width=18).grid(
                row=i // 2, column=i % 2, padx=5, pady=5
            )

    # -----------------------------
    # CORE LOGIC
    # -----------------------------
    def add_record(self):
        name = self.name_entry.get().strip()
        assignment = self.assignment_entry.get().strip()
        score_value = self.score_entry.get().strip()

        if not name or not assignment or not score_value:
            messagebox.showerror("Error", "All fields are required.")
            return

        try:
            score = int(score_value)
            if not (0 <= score <= 100):
                raise ValueError
        except ValueError:
            messagebox.showerror("Error", "Score must be a whole number between 0 and 100.")
            return

        record = {"name": name, "assignment": assignment, "score": score}
        self.records.append(record)

        self.clear_inputs()
        self.refresh_listbox()

    def edit_selected(self):
        selection = self.listbox.curselection()
        if not selection:
            messagebox.showinfo("Edit", "No record selected.")
            return

        index = selection[0]
        record = self.records[index]

        # Load values into input boxes
        self.name_entry.delete(0, END)
        self.assignment_entry.delete(0, END)
        self.score_entry.delete(0, END)

        self.name_entry.insert(0, record["name"])
        self.assignment_entry.insert(0, record["assignment"])
        self.score_entry.insert(0, record["score"])

        # Switch Add button to Save Edit
        self.temp_edit_index = index
        self.add_button.config(text="Save Edit", command=self.save_edit)

    def save_edit(self):
        index = self.temp_edit_index

        name = self.name_entry.get().strip()
        assignment = self.assignment_entry.get().strip()
        score_value = self.score_entry.get().strip()

        if not name or not assignment or not score_value:
            messagebox.showerror("Error", "All fields are required.")
            return

        try:
            score = int(score_value)
            if not (0 <= score <= 100):
                raise ValueError
        except ValueError:
            messagebox.showerror("Error", "Score must be a whole number between 0 and 100.")
            return

        self.records[index] = {"name": name, "assignment": assignment, "score": score}

        self.clear_inputs()
        self.add_button.config(text="Add", command=self.add_record)
        self.refresh_listbox()

    def delete_selected(self):
        selection = self.listbox.curselection()
        if not selection:
            messagebox.showinfo("Delete", "No record selected.")
            return
        index = selection[0]
        del self.records[index]
        self.refresh_listbox()

    def clear_records(self):
        if not self.records:
            return
        if messagebox.askyesno("Clear All", "Are you sure you want to clear all records?"):
            self.records.clear()
            self.refresh_listbox()

    def clear_inputs(self):
        self.name_entry.delete(0, END)
        self.assignment_entry.delete(0, END)
        self.score_entry.delete(0, END)

    def refresh_listbox(self):
        self.listbox.delete(0, END)
        for r in self.records:
            display = f"{r['name']} | {r['assignment']} | {r['score']}"
            self.listbox.insert(END, display)

    # -----------------------------
    # ANALYSIS FUNCTIONS
    # -----------------------------
    def get_scores(self):
        return [r["score"] for r in self.records]

    def classify_scores(self):
        scores = self.get_scores()
        passing = [s for s in scores if s >= 65]
        failing = [s for s in scores if s < 65]
        return passing, failing

    def calculate_statistics(self):
        scores = self.get_scores()
        if not scores:
            return None, None, None
        return max(scores), min(scores), sum(scores) / len(scores)

    def letter_grade(self, score):
        if score >= 90:
            return "A"
        elif score >= 80:
            return "B"
        elif score >= 70:
            return "C"
        elif score >= 65:
            return "D"
        else:
            return "F"

    # -----------------------------
    # BUTTON ACTIONS
    # -----------------------------
    def show_passing(self):
        passing, _ = self.classify_scores()
        messagebox.showinfo("Passing Scores", str(passing) if passing else "No passing scores.")

    def show_failing(self):
        _, failing = self.classify_scores()
        messagebox.showinfo("Failing Scores", str(failing) if failing else "No failing scores.")

    def show_statistics(self):
        highest, lowest, average = self.calculate_statistics()
        if highest is None:
            messagebox.showinfo("Statistics", "No records available.")
            return
        messagebox.showinfo(
            "Statistics",
            f"Highest: {highest}\nLowest: {lowest}\nAverage: {average:.2f}"
        )

    def show_letter_grades(self):
        if not self.records:
            messagebox.showinfo("Letter Grades", "No records available.")
            return
        output = "\n".join(
            [f"{r['name']} ({r['assignment']}): {r['score']} → {self.letter_grade(r['score'])}"
             for r in self.records]
        )
        messagebox.showinfo("Letter Grades", output)

    def show_summary(self):
        if not self.records:
            messagebox.showinfo("Summary", "No records available.")
            return

        passing, failing = self.classify_scores()
        highest, lowest, average = self.calculate_statistics()

        summary = (
            f"Total Records: {len(self.records)}\n"
            f"Highest Score: {highest}\n"
            f"Lowest Score: {lowest}\n"
            f"Average Score: {average:.2f}\n"
            f"Passing Count: {len(passing)}\n"
            f"Failing Count: {len(failing)}"
        )
        messagebox.showinfo("Summary", summary)

    # -----------------------------
    # SAVE / LOAD
    # -----------------------------
    def save_records(self):
        if not self.records:
            messagebox.showinfo("Save", "No records to save.")
            return

        with open("records.txt", "w") as f:
            for r in self.records:
                f.write(f"{r['name']},{r['assignment']},{r['score']}\n")

        messagebox.showinfo("Saved", "Records saved to records.txt")

    def load_records(self):
        try:
            with open("records.txt", "r") as f:
                self.records.clear()
                for line in f:
                    try:
                        name, assignment, score = line.strip().split(",")
                        self.records.append({"name": name, "assignment": assignment, "score": int(score)})
                    except:
                        pass
            self.refresh_listbox()
            messagebox.showinfo("Loaded", "Records loaded.")
        except FileNotFoundError:
            messagebox.showerror("Error", "No saved records found.")


if __name__ == "__main__":
    root = tk.Tk()
    app = GradebookApp(root)
    root.mainloop()
