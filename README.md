import tkinter as tk
from tkinter import messagebox, ttk

# Employee database
employees = {}

# Main window
root = tk.Tk()
root.title("Employee Payroll System")
root.geometry("600x500")

# Functions
def add_employee():
    emp_id = entry_id.get()
    name = entry_name.get()
    pay_rate = entry_rate.get()
    
    if not emp_id or not name or not pay_rate:
        messagebox.showerror("Error", "Please fill all fields!")
        return
    
    try:
        pay_rate = float(pay_rate)
        employees[emp_id] = {
            'name': name,
            'pay_rate': pay_rate,
            'hours': 0,
            'deductions': 0
        }
        messagebox.showinfo("Success", f"Employee {name} added successfully!")
        clear_entries()
        update_employee_list()
    except ValueError:
        messagebox.showerror("Error", "Pay rate must be a number!")

def calculate_salary():
    emp_id = combo_emp.get()
    if not emp_id or emp_id not in employees:
        messagebox.showerror("Error", "Please select a valid employee!")
        return
    
    try:
        hours = float(entry_hours.get())
        deductions = float(entry_deductions.get())
        
        emp = employees[emp_id]
        emp['hours'] = hours
        emp['deductions'] = deductions
        
        gross_salary = hours * emp['pay_rate']
        net_salary = gross_salary - deductions
        
        result = f"Employee: {emp['name']}
"
        result += f"Hours Worked: {hours}
"
        result += f"Pay Rate: ₹{emp['pay_rate']}/hour
"
        result += f"Gross Salary: ₹{gross_salary:.2f}
"
        result += f"Deductions: ₹{deductions:.2f}
"
        result += f"Net Salary: ₹{net_salary:.2f}"
        
        messagebox.showinfo("Payroll Summary", result)
    except ValueError:
        messagebox.showerror("Error", "Hours and deductions must be numbers!")

def clear_entries():
    entry_id.delete(0, tk.END)
    entry_name.delete(0, tk.END)
    entry_rate.delete(0, tk.END)
    entry_hours.delete(0, tk.END)
    entry_deductions.delete(0, tk.END)

def update_employee_list():
    combo_emp['values'] = list(employees.keys())

# GUI Layout
frame1 = tk.LabelFrame(root, text="Add New Employee", padx=10, pady=10)
frame1.pack(padx=10, pady=10, fill="both")

tk.Label(frame1, text="Employee ID:").grid(row=0, column=0, sticky="w", pady=5)
entry_id = tk.Entry(frame1, width=30)
entry_id.grid(row=0, column=1, pady=5)

tk.Label(frame1, text="Name:").grid(row=1, column=0, sticky="w", pady=5)
entry_name = tk.Entry(frame1, width=30)
entry_name.grid(row=1, column=1, pady=5)

tk.Label(frame1, text="Pay Rate (₹/hour):").grid(row=2, column=0, sticky="w", pady=5)
entry_rate = tk.Entry(frame1, width=30)
entry_rate.grid(row=2, column=1, pady=5)

btn_add = tk.Button(frame1, text="Add Employee", command=add_employee, bg="#4CAF50", fg="white")
btn_add.grid(row=3, column=0, columnspan=2, pady=10)

frame2 = tk.LabelFrame(root, text="Calculate Payroll", padx=10, pady=10)
frame2.pack(padx=10, pady=10, fill="both")

tk.Label(frame2, text="Select Employee:").grid(row=0, column=0, sticky="w", pady=5)
combo_emp = ttk.Combobox(frame2, width=27, state="readonly")
combo_emp.grid(row=0, column=1, pady=5)

tk.Label(frame2, text="Hours Worked:").grid(row=1, column=0, sticky="w", pady=5)
entry_hours = tk.Entry(frame2, width=30)
entry_hours.grid(row=1, column=1, pady=5)

tk.Label(frame2, text="Deductions (₹):").grid(row=2, column=0, sticky="w", pady=5)
entry_deductions = tk.Entry(frame2, width=30)
entry_deductions.grid(row=2, column=1, pady=5)

btn_calculate = tk.Button(frame2, text="Calculate Salary", command=calculate_salary, bg="#2196F3", fg="white")
btn_calculate.grid(row=3, column=0, columnspan=2, pady=10)

root.mainloop()
