# Application Tkinter Python
#### Video Demo:  https://www.youtube.com/watch?v=a6C2ZSX8x5E
#### Description: 
This is an app that helps you to quickly open apps and websites with one install, get the idea from the fact that every day when you start work you always open certain websites and applications, that's why this application makes it possible for you to install it only once and use it forever.
#### NOTE: 
1. You can use multiple urls for one keyword for best results.
2. You can not open app with name more 2 words.
3. In folder /app have a file exe for this app.
#### Overview:
1. With notebook search you can search for apps and websites and open them with keywords.
2. With notebook add keyword you can add keyword , url and type of app or website.
3. With notebook list you can see all keywords and urls.
4. With keyboard Ctrl + q you can say keyword and open app or website ( *but not working well* ).
#### Librarys:
- python
- tkinter
- speech_recognition
- webbrowser
- os
- csv
#### Source code:
###### Import librarys:
```
import speech_recognition
import pyttsx3
from tkinter import *
from tkinter.ttk import *
from tkinter.messagebox import showinfo
import webbrowser
import os
import csv
```
###### Read data from file data.csv and save to variable data.
```
data = []
with open('data.csv') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=',')
    for row in csv_reader:
        data.append(row)
```
###### Initialize the app with a width of 450 and a height of 300, and it is always in the center.
```
window = Tk()
window.title('App') 
# This code helps to disable windows from resizing
window.resizable(False, False)

window_height = 300
window_width = 450

screen_width = window.winfo_screenwidth()
screen_height = window.winfo_screenheight()

x_cordinate = int((screen_width/2) - (window_width/2))
y_cordinate = int((screen_height/2) - (window_height/2) - 100)

window.geometry("{}x{}+{}+{}".format(window_width,
                                     window_height, x_cordinate, y_cordinate))
```
###### Create notebook with 3 frame.
1. Search
2. Add Keyword
3. List
```
# create a notebook
notebook = Notebook(window)
notebook.pack(pady=10, expand=True)

# create frames
frame0 = Frame(notebook, width=400, height=280)
frame1 = Frame(notebook, width=400, height=280)
frame2 = Frame(notebook, width=400, height=280)
frame0.pack(fill='both', expand=True)
frame1.pack(fill='both', expand=True)
frame2.pack(fill='both', expand=True)

# add frames to notebook
notebook.add(frame0, text='Search')
notebook.add(frame1, text='Add Keyword')
notebook.add(frame2, text='List')
```
###### Frames 1 : Create 2 Label, 1 entry , 1 button.
- Get value from entry . Then check if the value is in the data or not.
  - Yes : Open web site or app installed.
  - No : Showing information no results found.
```
# frame0
# create a label
label_search = Label(frame0, text="Search").place(x=40,
                                                  y=60)
# create a label
label_talk = Label(frame0, text="Ctrl + q to talk", font=("Helvetica", 11)).place(x=5,
                                                                                  y=200)
# create a entry
search_text = StringVar()
search = Entry(frame0,
               width=30, textvariable=search_text)
search.focus()
search.place(x=110,
             y=60)
# create a button


def search(event=None):
    if search_text.get() == "":
        showinfo("Error", "Please fill in the search field")
    else:
        check = 0
        search = search_text.get().lower()
        for i in data:
            if i[0] == search and i[2] == '1':
                webbrowser.open(i[1])
                search_text.set("")
                check = 1
            if i[0] == search and i[2] == '2':
                nameApp = "start {}:".format(i[1])
                os.system(nameApp)
                search_text.set("")
                check = 1
        if check == 0:
            showinfo("Error", "No result found")
            search_text.set("")


text = Text(window, height=1, width=25)
text.place(x=5,
           y=260)
#text['state'] = 'disabled'
text.insert('1.0', 'Welcome to the app')

search_button = Button(frame0,
                       text="Search", command=search)
search_button.bind("<Return>", search)
search_button.place(x=150, y=120)
```
###### Frame 2 : Create 3 label , 2 entry , 1 radiobutton , 1 button.
- Get value from entry and radio button . Then save it to variable data.
```
# frame1
# the label for keyword
label_keyword = Label(frame1,
                      text="Keyword").place(x=40,
                                            y=60)
# the label for url

label_url = Label(frame1,
                  text='URL | Name').place(x=40,
                                           y=100)
# the label for type
label_type = Label(frame1,
                   text="Type").place(x=40,
                                      y=140)

keyword_text = StringVar()
keyword = Entry(frame1,
                width=30, textvariable=keyword_text).place(x=110,
                                                           y=60)
url_text = StringVar()
url = Entry(frame1,
            width=30, textvariable=url_text).place(x=110,
                                                   y=100)

# the radio button for type
type_text = StringVar()
Type = Radiobutton(frame1, text="Web", variable=type_text,
                   value=1).place(x=110, y=140)
Type = Radiobutton(frame1, text="App", variable=type_text,
                   value=2).place(x=180, y=140)


def submit(event=None):
    # print(type_text.get(), keyword_text.get(), url_text.get())
    if (keyword_text.get() == "" or url_text.get() == "" or type_text.get() == ""):
        showinfo("Error", "Please fill in all the fields")
    else:
        # add the data to the list
        data.append([keyword_text.get(), url_text.get(), type_text.get()])
        url_text.set("")
        type_text.set("")
    insertDataToTree(data)


# the button for submit
submit_button = Button(frame1,
                       text="Submit", command=submit)

submit_button.bind("<Return>", submit)
submit_button.place(x=110, y=180)
```
###### Frame 3 : Create 1 Treeview with 3 colums (keyword,url,type), 1 button
- Treeview : Show data
- Button : Delete item you select
```
# fame 2
# define columns
columns = ('keyword', 'url', 'Type')

tree = Treeview(frame2, columns=columns, show='headings', height=9)
# define headings
tree.column("# 1", anchor=CENTER, stretch=NO, width=100)
tree.column("# 2", anchor=CENTER, stretch=NO, width=275)
tree.column("# 3", anchor=CENTER, stretch=NO, width=75)

tree.heading('keyword', text='Keyword',)
tree.heading('url', text='URL | Name')
tree.heading('Type', text='Type')
# generate sample data
# contacts = []


def insertDataToTree(data):
    for i in tree.get_children():
        tree.delete(i)
    for n in data:
        if n[2] == '1':
            tree.insert('', 'end', values=(n[0], n[1], 'web'))
        else:
            tree.insert('', 'end', values=(n[0], n[1], 'app'))


insertDataToTree(data)


def item_selected(event):
    for selected_item in tree.selection():
        item = tree.item(selected_item)
        record = item['values']
        print(record)
        # show a message
        # showinfo(title='Information', message=','.join(record))


tree.bind('<<TreeviewSelect>>', item_selected)

tree.grid(row=0, column=0, sticky='nsew')

# add a scrollbar
scrollbar = Scrollbar(frame2, orient=VERTICAL, command=tree.yview)
tree.configure(yscroll=scrollbar.set)
scrollbar.grid(row=0, column=1, sticky='ns')

# function for delete


def delete():
    for selected_item in tree.selection():
        item = tree.item(selected_item)
        record = item['values']
        # tree.delete(selected_item)
        for i in data:
            if i[1] == record[1] and i[0] == record[0]:
                data.remove(i)
    insertDataToTree(data)
    print(data)


# the button for delete
delete_button = Button(frame2,
                       text="Delete", command=delete).place(x=300,
                                                            y=220)


```
###### Besides
1. You can talk keyword with keybroad Ctrl + q ( but library *speech_recognition* not working well ).
- If it can hear : Open web site or app installed.
- if it can't hear : Show info
```
def talk(event=None):
    print("Listening...")
    rotbot_ear = speech_recognition.Recognizer()
    robot_mouth = pyttsx3.init()
    with speech_recognition.Microphone() as mic:
        #print("Robot : I'm Listening")
        audio = rotbot_ear.listen(mic)
    try:
        you = rotbot_ear.recognize_google(audio, language="en")
    except:
        you = 'None'
    if you != 'None':
        text.delete(1.0, END)
        text.insert('1.0', "You : " + you)
        for i in data:
            you = you.lower()
            text.insert('1.0', "")
            if i[0] == you and i[2] == '1':
                webbrowser.open(i[1])
            if i[0] == you and i[2] == '2':
                nameApp = "start {}:".format(i[1])
                os.system(nameApp)
    else:
        #text.delete(1.0, END)
        #text.insert('1.0', "Say again")
        showinfo("Error", "I can't understand what you said")
    robot_mouth.runAndWait()
```
2. Text show you word heared
```
text = Text(window, height=1, width=25)
text.place(x=5,
           y=260)
```
###### Loop of app.
```
window.mainloop()
```
##### Final : Open file to write operation data from data variable.
```
with open('data.csv', 'w', newline='') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(data)
```
