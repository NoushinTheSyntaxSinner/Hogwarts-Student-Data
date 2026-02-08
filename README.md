# Hogwarts-Student-Data
This project will store, collect, delete and edit the data of different Hogwarts' students in tkinter. This one's for all potter heads who love coding. 🪄
import sqlite3
from tkinter import *
root = Tk()
root.geometry('800x900') #opens the tab in this size
root.title("Hogwarts school for witchcraft and wizadry") #title of the tab
root.configure(bg="azure") #sets background colour

def RS():
    con = sqlite3.connect("MyDB_Noushin.db") #the SQLite database name
    cursor = con.cursor()
    cursor.execute("SELECT * FROM Student") #Student is table name
    records = cursor.fetchall()
    print_record = " "
    for record in records:
        print_record += str(record[0]) + " " + str(record[1]) + " " + str(record[2]) + "\n"
    lblrecordshow = Label(text=print_record)
    lblrecordshow.grid(row=201, column=0, columnspan=2)

def RD():
    con = sqlite3.connect("MyDB_Noushin.db")
    cursor = con.cursor()
    cursor.execute("DELETE FROM Student WHERE StudentId ="+ txtId.get())
    con.commit()
    message="Record deletenti successfully (●'◡'●)"
    con.close()
    txtId.delete(0,END)

    lblmessageshow = Label(text=message, bg="lightblue")
    lblmessageshow.grid(row = 121, column=0,columnspan=2)

    RS()

def RSa():
    con = sqlite3.connect("MyDB_Noushin.db")
    cursor= con.cursor()
    cursor.execute("INSERT INTO Student(Name, Wandcore) VALUES(:txt_name, :txt_wandcore)",
                    {
                    'txt_name': txtname.get(),
                    'txt_wandcore': txtwandcore.get()
                    })
    con.commit()
    print("Record savius successfully (∩^o^)⊃━☆")
    con.close()
 

 

def ER():
    global editor
    editor = Tk()
    editor.geometry('500x500')
    editor.title('Editiarmenti for dark bugs')

    global txtname_edit
    global txtwandcore_edit

    lbls1_edit = Label(editor)
    lbls1_edit.grid(row= 1, column = 0)
    lbls2_edit = Label(editor)
    lbls2_edit.grid(row = 2, column = 0)
               
    lblname_edit = Label(editor, text = 'Name')
    lblname_edit.grid(row= 10, column = 0)
    txtname_edit = Entry(editor, width = 35)
    txtname_edit.grid(row = 11, column = 1)

    lblwand_edit = Label(editor, text = 'House owl spell')
    lblwand_edit.grid(row=13 , column = 0 )
    txtwand_edit = Entry(editor, width = 40)
    txtwand_edit.grid(row = 14, column = 1)

    lbls10_edit = Label(editor)
    lbls10_edit.grid(row= 15, column = 0)

    con = sqlite3.connect("MyDB_Noushin.db")
    cursor = con.cursor()
    cursor.execute("SELECT * FROM Student WHERE studentId =" + txtId.get())

    record = cursor.fetchall()

    for data in record:
        txtname_edit.insert(0, data[1])
        txtwand_edit.insert(0, data[2])

    btnUpdate = Button(editor, text = 'Updatohomora', command = RU )
    btnUpdate.grid(row = 110, column = 2)

def RU():
    con = sqlite3.connect("MyDB_Noushin.db")
    cursor = con.cursor()
    cursor.execute("""
                    UPDATE Student SET
                    Name = :name,
                    Email = :email
                    WHERE studentId = :id""",
                    {
                        'name' : txtname_edit.get(),
                        'wandcore' : txtwandcore_edit.get(),
                        'id' : txtId.get()
                    }
                   )
    con.commit()
    con.close()
    editor.destroy()

lbls1 = Label(root, bg="azure")
lbls1.grid(row= 1, column = 0)
lbls2 = Label(root, bg="azure")
lbls2.grid(row = 2, column = 0)
           
lblname = Label(root, text = 'Name', fg = "black")
lblname.grid(row= 10, column = 0)
txtname = Entry(root, width = 35)
txtname.grid(row = 11, column = 1)

lblwandcore = Label(root, text = 'House owl spell', fg = "black")
lblwandcore.grid(row=13 , column = 0 )
txtwandcore = Entry(root, width = 40)
txtwandcore.grid(row = 14, column = 1)

lbls10 = Label(root, bg="lightblue")
lbls10.grid(row= 15, column = 0)

lblId = Label(root, text = 'Id(delte/edit)', fg = "black")
lblId.grid(row=112, column = 0)
txtId = Entry(root)
txtId.grid(row=113, column=1)

lbls11 = Label(root, bg="lightblue")
lbls11.grid(row= 125, column = 0)

btnSave = Button(root, text = 'Savius', command = RSa)
btnSave.grid(row = 130, column = 0)

btnEdit = Button(root, text = 'Editiarmus', command = ER)
btnEdit.grid(row = 130, column = 1)

btnShow = Button(root, text = 'Show', command = RS)
btnShow.grid(row = 131, column = 0)

btnDelete = Button(root, text = 'Obliviate', command=RD)
btnDelete.grid(row = 131, column = 1)

lbls13 = Label(root, bg="lightblue")
lbls13.grid(row = 150, column = 0)


lbls23 = Label(root, bg="lightblue")
lbls23.grid(row = 151, column = 0)


root.mainloop()
