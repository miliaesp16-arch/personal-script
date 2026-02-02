import os
import shutil
from pathlib import Path

# change this to wherever your messy files are
folder_to_organize = Path.home() / "Downloads"

# keywords in filenames → where they should go
file_rules = {
    "School": ["syllabus", "assignment", "lecture", "notes", "exam", "quiz", 
               "hofstra", "psych", "neuro", "cog sci", "criminology"],
    
    "Cover Letters": ["cover letter", "cover_letter", "coverletter"],
    
    "Job Applications": ["resume", "cv", "application", "job", "position"],
    
    "Internship - Boston": ["boston", "ivanov", "bu ", "eeg", "plamen"],
    
    "Internship - Hofstra": ["sarah", "eating disorder", "ed research", 
                             "manuscript", "hofstra research"],
}

def get_destination(filename):
    """check filename against rules to find where it belongs"""
    name_lower = filename.lower()
    
    for folder_name, keywords in file_rules.items():
        for keyword in keywords:
            if keyword in name_lower:
                return folder_name
    return None

def organize_files(folder):
    moved_count = 0
    
    for file in folder.iterdir():
        if file.is_file() and not file.name.startswith('.'):
            destination_name = get_destination(file.name)
            
            if destination_name:
                destination_folder = folder / destination_name
                destination_folder.mkdir(exist_ok=True)
                shutil.move(str(file), str(destination_folder / file.name))
                print(f"Moved: {file.name} → {destination_name}/")
                moved_count += 1
    
    print(f"\nDone! Moved {moved_count} files.")

if __name__ == "__main__":
    organize_files(folder_to_organize)
