import re
from tkinter import *
from tkinter import messagebox
from tkinter import font as tkfont 

# --- CONFIGURATION CONSTANTS ---
BACKGROUND_COLOR = '#2C3E50' 
FOREGROUND_COLOR = '#ECF0F1' 
BUTTON_COLOR = '#3498DB'     
FONT_SIZE = 12               
BUTTON_FONT_SIZE = 10        
CHECK_FONT_SIZE = 10         
TICK = "\u2713"              
SPACE = " "                  
CHECKBOX_START = "["
CHECKBOX_END = "] "

# --- HI-DPI FIX ---
try:
    from ctypes import windll
    windll.shcore.SetProcessDpiAwareness(1)
except:
    pass
# ------------------

# --- CORE LOGIC (Remains the same) ---
def check_password_criteria(password):
    results = {}
    results['length'] = len(password) >= 10
    results['uppercase'] = bool(re.search(r"[A-Z]", password))
    results['digit'] = bool(re.search(r"\d", password))
    results['symbol'] = bool(re.search(r"[^a-zA-Z0-9]", password))
    return results

# --- VALIDATION AND UI UPDATE FUNCTION (Remains the same) ---
def validate_password():
    entered_password = password_entry.get()
    results = check_password_criteria(entered_password)
    
    req_texts = {
        'length': "10 characters long",
        'uppercase': "Minimum one uppercase letter",
        'digit': "Include numbers (digits)",
        'symbol': "Include symbols (special characters)"
    }

    overall_success = True
    
    requirement_labels = [
        (lbl_length, req_texts['length'], 'length'),
        (lbl_uppercase, req_texts['uppercase'], 'uppercase'),
        (lbl_digit, req_texts['digit'], 'digit'),
        (lbl_symbol, req_texts['symbol'], 'symbol')
    ]

    for lbl, text, key in requirement_labels:
        if results[key]:
            box_content = TICK
            color = "lime green"
        else:
            box_content = SPACE
            color = "red"
            overall_success = False

        new_text = f"{CHECKBOX_START}{box_content}{CHECKBOX_END}{text}"
        lbl.config(text=new_text, fg=color)

    if overall_success:
        messagebox.showinfo("Password Strength Result", "Strong password! All conditions met.")
    

# --- TOGGLE VISIBILITY FUNCTION (Updated icons and styling) ---

def toggle_password_visibility():
    """
    Toggles the 'show' attribute and changes the icon text to reflect the state.
    Uses universal Unicode characters for better compatibility.
    """
    current_show = password_entry.cget('show')
    
    if current_show == '*':
        # Unmask and change icon to 'Open Eye'
        password_entry.config(show='')
        toggle_icon.config(text="\U0001F441", fg=BUTTON_COLOR) # Open Eye
    else:
        # Mask and change icon to 'Lock'
        password_entry.config(show='*')
        toggle_icon.config(text="\U0001F512", fg=FOREGROUND_COLOR) # Lock


# --- TKINTER GUI SETUP ---

root = Tk()
root.title("PASSWORD CHECKER") 
root.geometry("350x380") 
root.config(bg=BACKGROUND_COLOR) 

# Define custom fonts
title_font = tkfont.Font(family="Helvetica", size=FONT_SIZE, weight="bold")
button_font = tkfont.Font(family="Helvetica", size=BUTTON_FONT_SIZE, weight="bold")
check_font = tkfont.Font(family="Helvetica", size=CHECK_FONT_SIZE)
# Use a standard font for icons to ensure broad support
icon_font = tkfont.Font(family="Segoe UI Symbol", size=14) 

# 1. Main Instruction Label
label = Label(root, 
              text="ENTER YOUR PASSWORD",
              bg=BACKGROUND_COLOR,
              fg=FOREGROUND_COLOR,
              font=title_font) 
label.pack(pady=10)

# 2. Entry and Icon Frame (to group the entry box and the icon)
# Frame BG is set to the Entry BG color to create the illusion of a single bar
entry_frame = Frame(root, bg=BACKGROUND_COLOR)
entry_frame.pack(pady=5, padx=10)

# Password Entry Widget
password_entry = Entry(entry_frame, 
                       width=30,   
                       show='*',
                       bg=BACKGROUND_COLOR, 
                       fg=FOREGROUND_COLOR,
                       font=title_font, 
                       insertbackground=FOREGROUND_COLOR,
                       relief=FLAT) # Make the entry FLAT so the icon looks 'inside'
password_entry.grid(row=0, column=0, padx=(0, 0))

# Eye Icon Button (placed inside the entry_frame)
toggle_icon = Button(entry_frame,
                     text="\U0001F512", # Default icon: Lock
                     command=toggle_password_visibility,
                     bg=BACKGROUND_COLOR,
                     fg=FOREGROUND_COLOR, # Default icon color is subtle
                     activebackground=BACKGROUND_COLOR,
                     activeforeground=BUTTON_COLOR,
                     relief=FLAT, # MUST be FLAT to merge visually
                     cursor="hand2",
                     font=icon_font,
                     padx=5, # Minimal padding
                     pady=0)
toggle_icon.grid(row=0, column=1, padx=0) 


# 3. Check Strength Button
check_button = Button(root, 
                      text="Check Strength", 
                      command=validate_password, 
                      bg=BUTTON_COLOR,          
                      fg=FOREGROUND_COLOR,       
                      font=button_font,           
                      relief=RAISED,              
                      cursor="hand2",             
                      padx=10, 
                      pady=5)
check_button.pack(pady=10)

# 4. Requirements Frame
requirements_frame = Frame(root, bg=BACKGROUND_COLOR)
requirements_frame.pack(pady=10, padx=10, fill='x')

# Label Creation (simplified for presentation)
initial_text_length = f"{CHECKBOX_START}{SPACE}{CHECKBOX_END}10 characters long"
initial_text_uppercase = f"{CHECKBOX_START}{SPACE}{CHECKBOX_END}Minimum one uppercase letter"
initial_text_digit = f"{CHECKBOX_START}{SPACE}{CHECKBOX_END}Include numbers (digits)"
initial_text_symbol = f"{CHECKBOX_START}{SPACE}{CHECKBOX_END}Include symbols (special characters)"

lbl_length = Label(requirements_frame, text=initial_text_length, anchor='w', justify=LEFT, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=check_font)
lbl_uppercase = Label(requirements_frame, text=initial_text_uppercase, anchor='w', justify=LEFT, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=check_font)
lbl_digit = Label(requirements_frame, text=initial_text_digit, anchor='w', justify=LEFT, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=check_font)
lbl_symbol = Label(requirements_frame, text=initial_text_symbol, anchor='w', justify=LEFT, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=check_font)

lbl_length.grid(row=0, column=0, sticky='w', padx=5, pady=2)
lbl_uppercase.grid(row=1, column=0, sticky='w', padx=5, pady=2)
lbl_digit.grid(row=2, column=0, sticky='w', padx=5, pady=2)
lbl_symbol.grid(row=3, column=0, sticky='w', padx=5, pady=2)

root.mainloop()
