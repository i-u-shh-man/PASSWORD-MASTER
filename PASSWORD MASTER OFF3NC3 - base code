import re
import os
from tkinter import *
from tkinter import messagebox
from tkinter import filedialog
from tkinter import font as tkfont 

# --- CONFIGURATION CONSTANTS ---
BACKGROUND_COLOR = '#2C3E50' 
FOREGROUND_COLOR = '#ECF0F1' 
BUTTON_COLOR = '#3498DB'     
FONT_SIZE = 12               
BUTTON_FONT_SIZE = 10        

# --- HI-DPI FIX ---
try:
    from ctypes import windll
    windll.shcore.SetProcessDpiAwareness(1)
except:
    pass
# ------------------

# --- CORE GENERATION LOGIC ---

LEET_MAP = {
    'a': '4', 'A': '4', 
    'e': '3', 'E': '3', 
    's': '5', 'S': '5', 
    'i': '1', 'I': '1', 
    'o': '0', 'O': '0', 
    't': '7', 'T': '7', 
    'g': '9', 'G': '9'
}

COMMON_YEARS = [
    '2025', '2024', '2023', '2022', '2021', '2020', 
    '1999', '1990', '1980', '123', '321', '69'
]

def generate_leetspeak_variations(word):
    """Generates basic leetspeak variations of a single word."""
    variations = {word, word.lower(), word.capitalize()}
    
    # 1. Simple Leet Substitution (e.g., "password" -> "pa55w0rd")
    leeted_word = word.lower()
    for char, leet in LEET_MAP.items():
        leeted_word = leeted_word.replace(char, leet)
    variations.add(leeted_word)
    
    # 2. Capitalize Leeted Word
    if leeted_word:
        variations.add(leeted_word.capitalize())
        
    return variations

def generate_wordlist():
    """Collects inputs, generates all variations, and initiates the save dialogue."""
    
    # 1. Collect and clean inputs
    name = entry_name.get().strip()
    pet = entry_pet.get().strip()
    year_input = entry_year.get().strip()

    if not name and not pet:
        messagebox.showerror("Input Error", "Please enter at least a Name or a Pet's Name.")
        return

    # 2. Define base words set
    base_words = set()
    
    # Generate variations for Name
    if name:
        base_words.update(generate_leetspeak_variations(name))
        
    # Generate variations for Pet
    if pet:
        base_words.update(generate_leetspeak_variations(pet))
        
    # 3. Generate combined words
    if name and pet:
        base_words.update(generate_leetspeak_variations(name + pet))
        base_words.update(generate_leetspeak_variations(pet + name))
        
    # 4. Final list of words (base words + year appendages)
    final_wordlist = set()
    
    # Handle user-defined year input
    if year_input and year_input.isdigit():
        COMMON_YEARS.append(year_input)
        
    # Generate all year appendages (full year and 2-digit abbreviation)
    append_suffixes = set(COMMON_YEARS)
    for y in COMMON_YEARS:
        if len(y) == 4 and y.startswith(('19', '20')):
            append_suffixes.add(y[2:]) # e.g., '2024' -> '24'
    
    # Combine all base words with all suffixes
    for word in base_words:
        final_wordlist.add(word) # Include the base word alone
        for suffix in append_suffixes:
            final_wordlist.add(word + suffix)
            
    # Remove any empty strings that might have crept in
    final_wordlist.discard('')
    
    # 5. Initiate file save
    if final_wordlist:
        save_wordlist_to_file(final_wordlist)
    else:
        messagebox.showinfo("Generation Complete", "No valid words were generated.")


def save_wordlist_to_file(wordlist):
    """Opens a file dialogue and saves the wordlist to the selected path."""
    
    file_path = filedialog.asksaveasfilename(
        defaultextension=".txt",
        filetypes=[("Text files", "*.txt")],
        title="Save Wordlist"
    )

    if file_path:
        try:
            with open(file_path, 'w', encoding='utf-8') as f:
                # Write each word on a new line
                for word in sorted(list(wordlist)):
                    f.write(word + '\n')
            
            messagebox.showinfo("Success", 
                                f"Wordlist saved successfully!\nGenerated {len(wordlist)} unique words.")
        except Exception as e:
            messagebox.showerror("Error Saving File", f"An error occurred: {e}")


# --- TKINTER GUI SETUP ---

root = Tk()
root.title("PASSWORD WORDLIST GENERATOR") 
root.geometry("450x400") 
root.config(bg=BACKGROUND_COLOR) 

# Define custom fonts
title_font = tkfont.Font(family="Helvetica", size=FONT_SIZE, weight="bold")
button_font = tkfont.Font(family="Helvetica", size=BUTTON_FONT_SIZE, weight="bold")

# Main Label
Label(root, 
      text="CUSTOM WORDLIST GENERATOR (PM)",
      bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=title_font).pack(pady=15)

# Input Frame (for Name, Pet, Year)
input_frame = Frame(root, bg=BACKGROUND_COLOR)
input_frame.pack(pady=5, padx=20)

# 1. Name Input
Label(input_frame, text="Name/Alias:", 
      bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=button_font).grid(row=0, column=0, sticky='w', pady=5, padx=10)
entry_name = Entry(input_frame, width=30, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, insertbackground=FOREGROUND_COLOR, font=title_font)
entry_name.grid(row=0, column=1, padx=10)

# 2. Pet Input
Label(input_frame, text="Pet's Name:", 
      bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=button_font).grid(row=1, column=0, sticky='w', pady=5, padx=10)
entry_pet = Entry(input_frame, width=30, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, insertbackground=FOREGROUND_COLOR, font=title_font)
entry_pet.grid(row=1, column=1, padx=10)

# 3. Date/Year Input
Label(input_frame, text="Custom Year (Optional):", 
      bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, font=button_font).grid(row=2, column=0, sticky='w', pady=5, padx=10)
entry_year = Entry(input_frame, width=30, bg=BACKGROUND_COLOR, fg=FOREGROUND_COLOR, insertbackground=FOREGROUND_COLOR, font=title_font)
entry_year.grid(row=2, column=1, padx=10)


# Generate Button
generate_button = Button(root, 
                      text="GENERATE & EXPORT (.txt)", 
                      command=generate_wordlist, 
                      bg=BUTTON_COLOR,          
                      fg=FOREGROUND_COLOR,       
                      font=button_font,           
                      relief=RAISED,              
                      cursor="hand2",             
                      padx=15, 
                      pady=8)
generate_button.pack(pady=30)

# Warning Label
Label(root, 
      text="WARNING: Use this tool for ethical security testing ONLY.", 
      bg=BACKGROUND_COLOR, fg="red", font=tkfont.Font(family="Helvetica", size=8, weight="bold")).pack()


root.mainloop()
