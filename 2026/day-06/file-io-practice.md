***************************************************************TASK************************************************************************************************************************


touch notes.txt

Created a file named notes.txt using the touch command, as mentioned in the task

echo "This is for the Testing Purpose " > notes.txt

This command adds the text This for the Testing Purpose to notes.txt. If the file already contains data, it will be overwritten

cat notes.txt

cat notes.txt is used to display the contents of the file notes.txt

echo "Second line for the Testing Purpose " >> notes.txt

This command adds the text Second line for the Testing Purpose to the end of notes.txt

echo "Third line for the Testing Purpose" | tee -a notes.txt

This command appends the text to notes.txt and also displays it on the terminal

<img width="1365" height="643" alt="image" src="https://github.com/user-attachments/assets/a8fd4759-b860-4114-ab23-1feee59f46e2" />

head -n 2 notes.txt

This command displays the first 2 lines of notes.txt

tail -n 2 notes.txt

This command displays the last 2 lines of notes.txt

<img width="1365" height="633" alt="image" src="https://github.com/user-attachments/assets/81b95725-cb41-46d9-9a14-118bfd9166e6" />

