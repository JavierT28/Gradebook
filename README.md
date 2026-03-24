# Gradebook GUI Application (Python, Tkinter, OOP)

A fully‑featured gradebook application built in Python using Tkinter, designed as part of my journey toward becoming a software engineer.  
This project demonstrates practical GUI development, object‑oriented programming, data modeling, and real‑world application structure.

The application manages students, assignments, and scores through an interactive interface. It includes editing tools, GPA calculations, CSV exporting, persistent storage, and a clean class‑based architecture.

---

## 🎯 Project Overview

This project started as basic Python practice and evolved into a complete desktop application.  
It reflects my progression from fundamentals (loops, functions, conditionals) into more advanced concepts such as:

- Object‑oriented design  
- GUI programming  
- Data structures  
- File I/O  
- Application architecture  
- Packaging Python apps for distribution  

The goal was to build something functional, maintainable, and reflective of real engineering workflows.

---

## 🧱 Technical Highlights

### 🏗️ **Architecture**
- Fully object‑oriented design (`GradebookApp` class)
- Clear separation of concerns:
  - UI rendering  
  - Data management  
  - Validation  
  - Analytics (statistics, GPA, summaries)  
  - File operations  
- Modular functions for readability and maintainability
- Scalable structure suitable for future expansion (database, API, etc.)

### 🖥️ **GUI Layer (Tkinter)**
- Multi‑frame layout for clean organization  
- Scrollable listbox for large datasets  
- Dynamic UI updates (auto‑refresh on add/edit/delete)  
- Input validation with error dialogs  
- Button‑driven workflow for intuitive interaction  

### 📊 **Analytics Engine**
- Passing/failing classification  
- Highest/lowest/average score computation  
- Letter‑grade conversion  
- GPA calculation:
  - Per‑student GPA  
  - Overall GPA across all records  

### 💾 **Persistence & Exporting**
- Save/load gradebook data to text files  
- CSV export with:
  - Name  
  - Assignment  
  - Score  
  - Letter grade  
  - GPA points  
- EXE packaging using PyInstaller for Windows distribution  

---

## 🚀 Features

### Grade Management
- Add student name, assignment, and score  
- Edit existing records  
- Delete individual entries  
- Clear all records  

### Reporting Tools
- Summary report  
- Score statistics  
- Letter‑grade breakdown  
- GPA report (per student + overall)  

### File Operations
- Save gradebook to `records.txt`  
- Load saved gradebooks  
- Export full dataset to `gradebook_export.csv`  

---

## 📂 Project Structure

---

## 🛠️ Tech Stack

- **Python 3.x**
- **Tkinter** (GUI)
- **PyInstaller** (EXE packaging)
- **CSV/Text file I/O**
- **OOP principles**
---

