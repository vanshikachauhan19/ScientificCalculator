import ttkbootstrap as ttk
from ttkbootstrap.constants import *
import math

# Initialize the main window with ttkbootstrap theme
root = ttk.Window(themename="darkly")
root.title("Scientific Calculator")
root.geometry("400x600")
root.resizable(False, False)

# Entry widget for the expression
entry = ttk.Entry(root, font=("Arial", 24), justify="right", bootstyle="dark")
entry.grid(row=0, column=0, columnspan=5, sticky="nsew", pady=20, padx=20)
entry.insert(0, "0")

# Functions
def clear():
    entry.delete(0, END)
    entry.insert(0, "0")

def backspace():
    current_text = entry.get()
    entry.delete(len(current_text) - 1)
    if entry.get() == "":
        entry.insert(0, "0")

def append_char(char):
    current_text = entry.get()
    if current_text == "0":
        entry.delete(0, END)
    entry.insert(END, char)

def evaluate_expression():
    try:
        # Use eval to calculate the result
        result = eval(entry.get())
        entry.delete(0, END)
        entry.insert(END, str(result))
    except Exception:
        entry.delete(0, END)
        entry.insert(END, "Error")

# Function for special operations
def calculate_special(func):
    try:
        value = float(entry.get())
        if func == "1/x":
            result = 1 / value
        elif func == "x²":
            result = value ** 2
        elif func == "√x":
            result = math.sqrt(value)
        elif func == "sin":
            result = math.sin(math.radians(value))
        elif func == "cos":
            result = math.cos(math.radians(value))
        elif func == "tan":
            result = math.tan(math.radians(value))
        elif func == "log":
            result = math.log10(value)
        elif func == "ln":
            result = math.log(value)
        elif func == "exp":
            result = math.exp(value)
        entry.delete(0, END)
        entry.insert(END, str(result))
    except Exception:
        entry.delete(0, END)
        entry.insert(END, "Error")

# Scientific calculator layout
buttons = [
    ("%", 1, 0), ("CE", 1, 1), ("C", 1, 2), ("⌫", 1, 3), ("/", 1, 4),
    ("π", 2, 0), ("sin", 2, 1), ("cos", 2, 2), ("tan", 2, 3), ("*", 2, 4),
    ("e", 3, 0), ("x²", 3, 1), ("√x", 3, 2), ("1/x", 3, 3), ("-", 3, 4),
    ("7", 4, 0), ("8", 4, 1), ("9", 4, 2), ("(", 4, 3), (")", 4, 4),
    ("4", 5, 0), ("5", 5, 1), ("6", 5, 2), ("ln", 5, 3), ("+", 5, 4),
    ("1", 6, 0), ("2", 6, 1), ("3", 6, 2), ("log", 6, 3), ("exp", 6, 4),
    ("±", 7, 0), ("0", 7, 1), (".", 7, 2), ("=", 7, 3, 7, 4),
]

# Map special button texts to their commands
special_functions = {
    "1/x": lambda: calculate_special("1/x"), "x²": lambda: calculate_special("x²"),
    "√x": lambda: calculate_special("√x"), "sin": lambda: calculate_special("sin"),
    "cos": lambda: calculate_special("cos"), "tan": lambda: calculate_special("tan"),
    "log": lambda: calculate_special("log"), "ln": lambda: calculate_special("ln"),
    "exp": lambda: calculate_special("exp"), "π": lambda: append_char(str(math.pi)),
    "e": lambda: append_char(str(math.e))
}

# Add buttons to the grid
for button in buttons:
    text, row, col, *extra = button
    rowspan = extra[0] if len(extra) > 0 else 1
    colspan = extra[1] if len(extra) > 1 else 1
    
    if text == "=":
        button = ttk.Button(root, text=text, command=evaluate_expression, bootstyle="success")
    elif text == "C":
        button = ttk.Button(root, text=text, command=clear, bootstyle="danger")
    elif text == "⌫":
        button = ttk.Button(root, text=text, command=backspace, bootstyle="warning")
    elif text in special_functions:
        button = ttk.Button(root, text=text, command=special_functions[text], bootstyle="primary")
    else:
        button = ttk.Button(root, text=text, command=lambda t=text: append_char(t), bootstyle="secondary")
    
    button.grid(row=row, column=col, rowspan=rowspan, columnspan=colspan, sticky="nsew", padx=5, pady=5)

# Configure the grid to make buttons resizable and square
for i in range(5):  # 5 columns
    root.grid_columnconfigure(i, weight=1)
for i in range(8):  # 8 rows including the entry box
    root.grid_rowconfigure(i, weight=1)

root.mainloop()


