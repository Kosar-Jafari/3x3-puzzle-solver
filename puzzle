import customtkinter as ctk
import tkinter as tk
import threading
from tkinter import messagebox
from os import system
system('cls')
import copy
from PIL import Image,ImageTk,ImageDraw,ImageOps

global lstLblMoves
lstLblMoves=[]
def create_final_moves(mv):
    global lstLblMoves
    for i in range(3):
        frmResult.grid_rowconfigure(i,weight=1)
    for i in range(11):
        frmResult.grid_columnconfigure(i,weight=1)
    for i in range(len(mv)):
        lstLblMoves.append(ctk.CTkLabel(frmResult,text=mv[i]))
        lstLblMoves[i].grid(row=i//11,column=i%11,sticky="nsew")
    lstLblMoves[0].configure(text_color="red",font=("Arial",20))
    global start
    lbl_Solution2.configure(text=str(start[0][0]))
    lbl_Solution3.configure(text=str(start[0][1]))
    lbl_Solution4.configure(text=str(start[0][2]))
    lbl_Solution5.configure(text=str(start[1][0]))
    lbl_Solution6.configure(text=str(start[1][1]))
    lbl_Solution7.configure(text=str(start[1][2]))
    lbl_Solution8.configure(text=str(start[2][0]))
    lbl_Solution9.configure(text=str(start[2][1]))
    lbl_Solution0.configure(text=str(start[2][2]))


def change_frame(F):
    frmAbout.place_forget()
    frmGetPuzzle.place_forget()
    frmS.place_forget()
    frm_kosar.place_forget()
    frm1.place_forget()
    frm2.place_forget()
    F.place(x=0, y=0)


def collect_data():
     return [[int(ent1.get()), int(ent2.get()), int(ent3.get())],
            [int(ent4.get()), int(ent5.get()), int(ent6.get())],
             [int(ent7.get()),int(ent8.get()), int(ent9.get())]]

def validate_data():
    ents=[
        [ent1.get(), ent2.get(), ent3.get()],
        [ent4.get(), ent5.get(), ent6.get()],
        [ent7.get(), ent8.get(), ent9.get()]
        ]
    for k in ents:
        for i in range(3):
            if not k[i].isdigit():
                messagebox.showerror("Error","Input invalid.")
                return 
    data=collect_data()
    temp=[]
    for i in data:
        for j in range(3):
            temp.append(i[j])
    if len(set(temp)) == len(temp):
        for i in temp:
            if i<0 or i>8:
                messagebox.showerror("Error","Please enter numbers from 0 to 8.")
        else:
            inv = check_inversion(temp)
            if inv==1:
                messagebox.showerror("Error","This puzzle is not solvable!! The number of inversions must be even.")
            elif inv==-1:
                messagebox.showwarning("Warning","This is the goal state! Please enter another puzzle.")
            else:
                Enter()
                change_frame(frm2)
    else:
        messagebox.showerror("Error","The puzzle has duplicate numbers.")
        
def check_inversion(LST):
    LST_inv=LST.copy()
    LST_inv.remove(0)
    inv=0
    for i in range(len(LST_inv)-1):
        for j in range(i+1, len(LST_inv)):
            if LST_inv[j]<LST_inv[i]:
                inv+=1
    if inv!=0:
        return inv%2
    else:
        return -1
    

goal=[[1,2,3],[4,5,6],[7,8,0]]
goal_positions = {
    1:(0,0), 2:(0,1), 3:(0,2),
    4:(1,0), 5:(1,1), 6:(1,2),
    7:(2,0), 8:(2,1)
}
global start
start=[[1,2,3],
       [4,6,5],
       [8,7,0],
       "",
       0,
       0]

search_count=0
global solve_found
solve_found = False
global open_list
closed_list=[]
global result
result=None
global current_state
current_state=[[0,0,0],
               [0,0,0],
               [0,0,0],
               "",
               0,
               0]

def update_entries():
    global current_state
    update(current_state)
    root.after(50, update_entries)


def check_goal(LST):
    global search_count
    search_count+=1
    global current_state
    current_state = LST
    if LST[:3] == goal:
        global solve_found
        solve_found = True
        print(f"\n\nGoal found in {search_count} tries!", LST)
        global result
        result=LST
        btnSolution.configure(state="normal")
        create_final_moves(LST[3])




def check_duplicate(LST):
    for l in closed_list:
        if l[:3]==LST[:3]:
            return True
    else:
        return False



def calculate_f(L):
    h = 0
    for r in range(3):
        for c in range(3):
            val = L[r][c]
            if val != 0:
                target_r, target_c = goal_positions[val]
                h += abs(r - target_r) + abs(c-target_c)
    L[4] = h
    L[5] = h+len(L[3])



def find_z(LST):
    tmp_lst =copy.deepcopy(LST)
    for i in range(3):
        for j in range(3):
            if tmp_lst[i][j] == 0:
                return i,j
def up2(LST):
    for i in range(3):
        for j in range(3):
            if LST[i][j] == 0:
                LST[i][j],LST[i-1][j]=LST[i-1][j],LST[i][j]
                return
def down2(LST):
    for i in range(3):
        for j in range(3):
            if LST[i][j] == 0:
                LST[i+1][j],LST[i][j]=LST[i][j],LST[i+1][j]
                return
def left2(LST):
    for i in range(3):
        for j in range(3):
            if LST[i][j] == 0:
                LST[i][j],LST[i][j-1]=LST[i][j-1],LST[i][j]
                return

def right2(LST):
    for i in range(3):
        for j in range(3):
            if LST[i][j] == 0:
                LST[i][j+1],LST[i][j]=LST[i][j],LST[i][j+1]
                return

def left(LST):
    tmp_lst = copy.deepcopy(LST)
    for i in range(3):
        for j in range(3):
            if tmp_lst[i][j] == 0:
                tmp_lst[i][j],tmp_lst[i][j-1]=tmp_lst[i][j-1],tmp_lst[i][j]
                tmp_lst[3]+='L'
                calculate_f(tmp_lst)
                print(tmp_lst)
                if not check_duplicate(tmp_lst):
                    check_goal(tmp_lst)
                    open_list.append(tmp_lst)
                return

def up(LST):
    tmp_lst = copy.deepcopy(LST)
    for i in range(3):
        for j in range(3):
            if tmp_lst[i][j] == 0:
                tmp_lst[i][j],tmp_lst[i-1][j]=tmp_lst[i-1][j],tmp_lst[i][j]
                tmp_lst[3]+='U'
                calculate_f(tmp_lst)
                print(tmp_lst)
                if not check_duplicate(tmp_lst):
                    check_goal(tmp_lst)
                    open_list.append(tmp_lst)
                return

def right(LST):
    tmp_lst = copy.deepcopy(LST)
    for i in range(3):
        for j in range(3):
            if tmp_lst[i][j] == 0:
                tmp_lst[i][j+1],tmp_lst[i][j]=tmp_lst[i][j],tmp_lst[i][j+1]
                tmp_lst[3]+='R'
                calculate_f(tmp_lst)
                print(tmp_lst)
                if not check_duplicate(tmp_lst):
                    check_goal(tmp_lst)
                    open_list.append(tmp_lst)
                return

def down(LST):
    tmp_lst = copy.deepcopy(LST)
    for i in range(3):
        for j in range(3):
            if tmp_lst[i][j] == 0:
                tmp_lst[i+1][j],tmp_lst[i][j]=tmp_lst[i][j],tmp_lst[i+1][j]
                tmp_lst[3]+='D'
                calculate_f(tmp_lst)
                print(tmp_lst)
                if not check_duplicate(tmp_lst):
                    check_goal(tmp_lst)
                    open_list.append(tmp_lst)
                return

def check_moves(LST):
    tmp_lst = copy.deepcopy(LST)[:3]
    tmp_moves=[]
    i,j = find_z(tmp_lst)
    if i>0:
        tmp_moves.append('U')
    if i<2:
        tmp_moves.append('D')
    if j>0:
        tmp_moves.append('L')
    if j<2:
        tmp_moves.append('R')
    return tmp_moves

def do_move(LST):
    move_lst = copy.deepcopy(LST)
    moves = check_moves(move_lst)
    for move in moves:
        match move:
            case 'L': left(move_lst)
            case 'D': down(move_lst)
            case 'U': up(move_lst)
            case 'R': right(move_lst)

def sort_open_list():
    open_list.sort(key=fun)

def fun(LST):
    return (LST[5],LST[4])

def Enter():
    global start
    start[0][0]=int(ent1.get())
    start[0][1]=int(ent2.get())
    start[0][2]=int(ent3.get())
    start[1][0]=int(ent4.get())
    start[1][1]=int(ent5.get())
    start[1][2]=int(ent6.get())
    start[2][0]=int(ent7.get())
    start[2][1]=int(ent8.get())
    start[2][2]=int(ent9.get())
    
    current1.configure(text=str(start[0][0]))
    current2.configure(text=str(start[0][1]))
    current3.configure(text=str(start[0][2]))
    current4.configure(text=str(start[1][0]))
    current5.configure(text=str(start[1][1]))
    current6.configure(text=str(start[1][2]))
    current7.configure(text=str(start[2][0]))
    current8.configure(text=str(start[2][1]))
    current0.configure(text=str(start[2][2]))

    global open_list
    open_list=[copy.deepcopy(start)]

def update(LST):
    lbl_solve2.configure(text=str(LST[0][0]))
    lbl_solve3.configure(text=str(LST[0][1]))
    lbl_solve4.configure(text=str(LST[0][2]))
    lbl_solve5.configure(text=str(LST[1][0]))
    lbl_solve6.configure(text=str(LST[1][1]))
    lbl_solve7.configure(text=str(LST[1][2]))
    lbl_solve8.configure(text=str(LST[2][0]))
    lbl_solve9.configure(text=str(LST[2][1]))
    lbl_solve0.configure(text=str(LST[2][2]))

def update2(LST):
    lbl_Solution2.configure(text=str(LST[0][0]))
    lbl_Solution3.configure(text=str(LST[0][1]))
    lbl_Solution4.configure(text=str(LST[0][2]))
    lbl_Solution5.configure(text=str(LST[1][0]))
    lbl_Solution6.configure(text=str(LST[1][1]))
    lbl_Solution7.configure(text=str(LST[1][2]))
    lbl_Solution8.configure(text=str(LST[2][0]))
    lbl_Solution9.configure(text=str(LST[2][1]))
    lbl_Solution0.configure(text=str(LST[2][2]))

def solve():
    global solve_found
    while not solve_found:
        do_move(open_list[0])
        closed_list.append(open_list.pop(0))
        sort_open_list()
    global result

def start_solving():
    update_entries()
    threading.Thread(target=solve,daemon=True).start()

def exit():
    x=messagebox.askyesno("exit!","are you sure?")
    if x:
        root.destroy()
    else:
        return None

def Auto_focus(event,current_entry,next_entry=None,prev_entry=None):
    if event.keysym == "BackSpace":
        current_entry.delete(0, tk.END)
        if prev_entry:
            prev_entry.focus_set()

    if len(event.char)==1 and event.char.isprintable():
        current_entry.delete(0,tk.END)
        current_entry.insert(0,event.char)
        if next_entry:
            next_entry.focus_set()
        return "break"
    
global move_counter
move_counter=0

def do_move2(LST,move):
    match move:
        case 'L': left2(LST)
        case 'D': down2(LST)
        case 'U': up2(LST)
        case 'R': right2(LST)

def move(SL,RL): #START/RESULT
    global move_counter
    global lstLblMoves
    if len(RL[3])-1>move_counter:
        do_move2(SL,RL[3][move_counter])
        update2(SL)
        lstLblMoves[move_counter].configure(text_color="green",font=("Arial",10))
        lstLblMoves[move_counter+1].configure(text_color="red",font=("Arial",20))
        move_counter+=1
    elif len(RL[3])-1==move_counter:
        do_move2(SL,RL[3][move_counter])
        update2(SL)
        lstLblMoves[move_counter].configure(text_color="green",font=("Arial",10))
        messagebox.showinfo("Congratulations","Puzzle solved.")


MAIN_WIDTH=600
MAIN_HEIGHT=600
ctk.set_appearance_mode("light")
ctk.set_default_color_theme("dark-blue")
root=ctk.CTk()
root.geometry(f"{MAIN_WIDTH}x{MAIN_HEIGHT}+100+100")
root.resizable(False,False)
root.title("solve puzzle")

frmGetPuzzle=ctk.CTkFrame(root,width=600,height=600)
frmGetPuzzle.grid_rowconfigure(0,weight=1)
frmGetPuzzle.grid_rowconfigure(1,weight=1)
frmGetPuzzle.grid_rowconfigure(2,weight=1)
frmGetPuzzle.grid_rowconfigure(3,weight=1)
frmGetPuzzle.grid_rowconfigure(4,weight=1)
frmGetPuzzle.grid_rowconfigure(5,weight=1)
frmGetPuzzle.grid_columnconfigure(0,weight=1)
frmGetPuzzle.grid_propagate(False)


lbl_01=ctk.CTkLabel(frmGetPuzzle,text="0 means empty.",text_color="#02023b",font=("Arial",16),fg_color="white")
lbl_01.grid(row=1,column=0)

lbl_02=ctk.CTkLabel(frmGetPuzzle,text="Enter a puzzle(numbers from 0 to 8 with an even number of inversions).",text_color="#02023b",font=("Arial",16),fg_color="white")
lbl_02.grid(row=0,column=0,padx=30,pady=0)

frmS=ctk.CTkFrame(root,width=600,height=600)
frmS.grid_propagate(False)
frmS.grid_rowconfigure((0,1,2,3,4,5,6,7,8,9,10),weight=1)
frmS.grid_columnconfigure((0,1,2,3,4),weight=1)

lbl_up=ctk.CTkLabel(frmS,text="U:up",text_color="black",font=("Arial",16))
lbl_up.grid(row=0,column=0,pady=10)

lbl_down=ctk.CTkLabel(frmS,text="D:down",text_color="black",font=("Arial",16))
lbl_down.grid(row=0,column=1,pady=10)

lbl_left=ctk.CTkLabel(frmS,text="L:left",text_color="black",font=("Arial",16))
lbl_left.grid(row=0,column=2,pady=10)

lbl_right=ctk.CTkLabel(frmS,text="R:right",text_color="black",font=("Arial",16))
lbl_right.grid(row=0,column=3,pady=10)

frmResult=ctk.CTkFrame(frmS,width=600,height=200)
frmResult.grid(row=2,column=0,columnspan=5,rowspan=4,padx=10,pady=10)
frmResult.grid_propagate(False)


frmSolution=ctk.CTkFrame(frmS,width=200,height=200,fg_color="#02023b")
frmSolution.grid(row=7,column=2,columnspan=1)
frmSolution.grid_rowconfigure(0,weight=1)
frmSolution.grid_rowconfigure(1,weight=1)
frmSolution.grid_rowconfigure(2,weight=1)
frmSolution.grid_columnconfigure(0,weight=1)
frmSolution.grid_columnconfigure(1,weight=1)
frmSolution.grid_columnconfigure(2,weight=1)
frmSolution.grid_propagate(False)

lbl_Solution=ctk.CTkLabel(frmSolution,text_color="white",fg_color="#02023b",font=("Arial",16),text="solving")
lbl_Solution.grid(row=0,column=0,columnspan=3,padx=10,pady=10,sticky="nsew")

lbl_Solution2=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="1",fg_color="white")
lbl_Solution2.grid(row=1,column=0,padx=10,pady=10,sticky="nsew")

lbl_Solution3=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="2",fg_color="white")
lbl_Solution3.grid(row=1,column=1,padx=10,pady=10,sticky="nsew")

lbl_Solution4=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="3",fg_color="white")
lbl_Solution4.grid(row=1,column=2,padx=10,pady=10,sticky="nsew")

lbl_Solution5=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="4",fg_color="white")
lbl_Solution5.grid(row=2,column=0,padx=10,pady=10, sticky="nsew")

lbl_Solution6=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="5",fg_color="white")
lbl_Solution6.grid(row=2,column=1,padx=10,pady=10,sticky="nsew")

lbl_Solution7=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="6",fg_color="white")
lbl_Solution7.grid(row=2,column=2,padx=10,pady=10,sticky="nsew")

lbl_Solution8=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="7",fg_color="white")
lbl_Solution8.grid(row=3,column=0,padx=10,pady=10,sticky="nsew")

lbl_Solution9=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="8",fg_color="white")
lbl_Solution9.grid(row=3,column=1,padx=10,pady=10,sticky="nsew")

lbl_Solution0=ctk.CTkLabel(frmSolution,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_Solution0.grid(row=3,column=2,padx=10,pady=10,sticky="nsew")

btn_exit=ctk.CTkButton(frmS,text="exit",text_color="white",fg_color="#f51dc3",font=("Arial",16),command=exit)
btn_exit.grid(row=10,column=4)


btn_move=ctk.CTkButton(frmS,text="move",text_color="white",fg_color="#f51dc3",font=("Arial",16),command=lambda:move(start,result))
btn_move.grid(row=9,column=2)


frmAbout=ctk.CTkFrame(root,width=600,height=600)
frmAbout=ctk.CTkFrame(root,width=600,height=600)
frmAbout.grid_rowconfigure(0,weight=1)
frmAbout.grid_rowconfigure(1,weight=1)
frmAbout.grid_rowconfigure(2,weight=1)
frmAbout.grid_rowconfigure(3,weight=1)
frmAbout.grid_rowconfigure(4,weight=1)
frmAbout.grid_rowconfigure(5,weight=1)
frmAbout.grid_rowconfigure(6,weight=1)
frmAbout.grid_rowconfigure(7,weight=1)
frmAbout.grid_rowconfigure(8,weight=1)
frmAbout.grid_rowconfigure(9,weight=1)
frmAbout.grid_rowconfigure(10,weight=1)
frmAbout.grid_rowconfigure(11,weight=1)

frmAbout.grid_columnconfigure(0,weight=1)
frmAbout.grid_propagate(False)
lbl_about=ctk.CTkLabel(frmAbout,text="This is a 3×3 puzzle solver written in Python.",text_color="black",font=("Arial",16))
lbl_about.grid(row=0,column=0)

lbl_about2=ctk.CTkLabel(frmAbout,text="The user enters a puzzle and the program checks that",text_color="black",font=("Arial",16))
lbl_about2.grid(row=1,column=0)

lbl_about3=ctk.CTkLabel(frmAbout,text="the numbers are valid and contain no duplicates.",text_color="black",font=("Arial",16))
lbl_about3.grid(row=2,column=0)


lbl_about5=ctk.CTkLabel(frmAbout,text="If the input is invalid,it asks the user to try again.",text_color="black",font=("Arial",16))
lbl_about5.grid(row=3,column=0)

lbl_about6=ctk.CTkLabel(frmAbout,text="If it's valid, the program solves the puzzle and",text_color="black",font=("Arial",16))
lbl_about6.grid(row=4,column=0)

lbl_about7=ctk.CTkLabel(frmAbout,text="displays the solution.",text_color="black",font=("Arial",16))
lbl_about7.grid(row=5,column=0)

lbl_about7=ctk.CTkLabel(frmAbout,text="Note: 0 represents the empty space.",text_color="black",font=("Arial",16))
lbl_about7.grid(row=6,column=0)

lbl_about8=ctk.CTkLabel(frmAbout,text="Kosar Jafari Ebrahimzadeh",text_color="black",font=("Arial",24))
lbl_about8.grid(row=8,column=0)

btn_back=ctk.CTkButton(frmAbout,text="back",text_color="white",fg_color="#f51dc3",font=("Arial",17),command=lambda:change_frame(frm_kosar))
btn_back.grid(row=9,column=0)

frm_kosar=ctk.CTkFrame(root,width=600,height=600)
frm_kosar.grid_propagate(False)
frm_kosar.rowconfigure(0,weight=1)
frm_kosar.rowconfigure(1,weight=1)
frm_kosar.rowconfigure(2,weight=1)
frm_kosar.rowconfigure(3,weight=1)
frm_kosar.rowconfigure(4,weight=1)
frm_kosar.rowconfigure(5,weight=1)
frm_kosar.columnconfigure(0,weight=1)
frm_kosar.columnconfigure(1,weight=1)

frm_info=ctk.CTkFrame(frm_kosar)
frm_info.grid(row=0,column=1,sticky="nsew")
frm_info.rowconfigure(0,weight=1)
frm_info.rowconfigure(1,weight=1)
frm_info.rowconfigure(2,weight=1)
frm_info.rowconfigure(3,weight=1)
frm_info.rowconfigure(4,weight=1)
frm_info.columnconfigure(0,weight=1)

img = Image.open("kosarp.png") 
img = img.resize((200,200)) 

mask = Image.new("L",(200,200),0)
draw = ImageDraw.Draw(mask)
draw.ellipse((0,0,200,200),fill=255)
img.putalpha(mask) 
photo = ImageTk.PhotoImage(img)
lbl_a = ctk.CTkLabel(frm_kosar,image=photo,text="")
lbl_a.grid(row=0,column=0)

lbl_kosar=ctk.CTkLabel(frm_info,text="Kosar Jafari Ebrahimzadeh",text_color="black",font=("Arial",16))
lbl_kosar.grid(row=0,column=0,padx=10,pady=10)

lbl_year=ctk.CTkLabel(frm_info,text="18 years old",text_color="black",font=("Arial",16))
lbl_year.grid(row=1,column=0)

lbl_year=ctk.CTkLabel(frm_info,text="Tabriz,Iran",text_color="black",font=("Arial",16))
lbl_year.grid(row=2,column=0)

lbl_en=ctk.CTkLabel(frm_info,text="Aspiring software engineer",text_color="black",font=("Arial",16))
lbl_en.grid(row=3,column=0)

lbl_pr=ctk.CTkLabel(frm_info,text="project:3x3 puzzle",text_color="black",font=("Arial",16))
lbl_pr.grid(row=4,column=0)

btn_about=ctk.CTkButton(frm_kosar,text="about",text_color="white",fg_color="#f51dc3",font=("Arial",17),command=lambda:change_frame(frmAbout))
btn_about.grid(row=3,column=0)

btn_start=ctk.CTkButton(frm_kosar,text="start",fg_color="#f51dc3",text_color="white",font=("Arial",17),command=lambda:change_frame(frmGetPuzzle))
btn_start.grid(row=3,column=1)


frm1=ctk.CTkFrame(frmGetPuzzle,width=200,height=200,fg_color="#02023b")
frm1.grid_columnconfigure(0,weight=1)
frm1.grid_rowconfigure(0,weight=1)
frm1.grid_propagate(False)
frm1.grid(row=2,column=0)


frm1.grid_rowconfigure(0,weight=1)
frm1.grid_rowconfigure(1,weight=1)
frm1.grid_rowconfigure(2,weight=1)
frm1.grid_rowconfigure(3,weight=1)

frm1.grid_columnconfigure(0,weight=1)
frm1.grid_columnconfigure(1,weight=1)
frm1.grid_columnconfigure(2,weight=1)

frm2=ctk.CTkFrame(root,width=600,height=600)
frm2.grid_propagate(False)
frm2.grid_rowconfigure(0,weight=1)
frm2.grid_rowconfigure(1,weight=1)
frm2.grid_rowconfigure(2,weight=1)
frm2.grid_rowconfigure(3,weight=1)
frm2.grid_rowconfigure(4,weight=1)
frm2.grid_columnconfigure(0,weight=1)
frm2.grid_columnconfigure(1,weight=1)
frm2.grid_columnconfigure(2,weight=1)


# img1 = Image.open("r2.png") 
# img1 = img1.resize((200,200)) 
# photo1 = ImageTk.PhotoImage(img1)
# lbl_a1 = ctk.CTkLabel(frm2,image=photo1,text="")
# lbl_a1.grid(row=2,column=0)


# img2 = Image.open("r1.png") 
# img2 = img2.resize((200,200)) 
# photo2 = ImageTk.PhotoImage(img2)
# lbl_a2 = ctk.CTkLabel(frm2,image=photo2,text="")
# lbl_a2.grid(row=2,column=2)

frm3=ctk.CTkFrame(frm2,width=200,height=200,fg_color="#02023b")
frm3.grid(row=0,column=0,padx=10,pady=10)
frm3.grid_propagate(False)
frm3.grid_rowconfigure(0,weight=1)
frm3.grid_rowconfigure(1,weight=1)
frm3.grid_rowconfigure(2,weight=1)
frm3.grid_rowconfigure(3,weight=1)
frm3.grid_columnconfigure(0,weight=1)
frm3.grid_columnconfigure(1,weight=1)
frm3.grid_columnconfigure(2,weight=1)


frm4=ctk.CTkFrame(frm2,width=200,height=200,fg_color="#02023b")
frm4.grid(row=0,column=2,padx=10,pady=10)
frm4.grid_propagate(False)
frm4.grid_rowconfigure(0,weight=1)
frm4.grid_rowconfigure(1,weight=1)
frm4.grid_rowconfigure(2,weight=1)
frm4.grid_rowconfigure(3,weight=1)
frm4.grid_columnconfigure(0,weight=1)
frm4.grid_columnconfigure(1,weight=1)
frm4.grid_columnconfigure(2,weight=1)


frm5=ctk.CTkFrame(frm2,width=200,height=200,fg_color="#02023b")
frm5.grid(row=2,column=1,padx=10,pady=10)
frm5.grid_propagate(False)
frm5.grid_rowconfigure(0,weight=1)
frm5.grid_rowconfigure(1,weight=1)
frm5.grid_rowconfigure(2,weight=1)
frm5.grid_rowconfigure(3,weight=1)
frm5.grid_columnconfigure(0,weight=1)
frm5.grid_columnconfigure(1,weight=1)
frm5.grid_columnconfigure(2,weight=1)

lbl_solve1=ctk.CTkLabel(frm5,text_color="white",fg_color="#02023b",font=("Arial",16),text="solving")
lbl_solve1.grid(row=0,column=0,columnspan=3,padx=10,pady=10,sticky="nsew")

lbl_solve2=ctk.CTkLabel(frm5,text_color="black",fg_color="white",font=("Arial",16),text="")
lbl_solve2.grid(row=1,column=0,padx=10,pady=10,sticky="nsew")

lbl_solve3=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve3.grid(row=1,column=1,padx=10,pady=10,sticky="nsew")

lbl_solve4=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve4.grid(row=1,column=2,padx=10,pady=10,sticky="nsew")

lbl_solve5=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve5.grid(row=2,column=0,padx=10,pady=10, sticky="nsew")

lbl_solve6=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve6.grid(row=2,column=1,padx=10,pady=10,sticky="nsew")

lbl_solve7=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve7.grid(row=2,column=2,padx=10,pady=10,sticky="nsew")

lbl_solve8=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve8.grid(row=3,column=0,padx=10,pady=10,sticky="nsew")

lbl_solve9=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve9.grid(row=3,column=1,padx=10,pady=10,sticky="nsew")

lbl_solve0=ctk.CTkLabel(frm5,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl_solve0.grid(row=3,column=2,padx=10,pady=10,sticky="nsew")

lbl_c=ctk.CTkLabel(frm3,fg_color="#02023b",text_color="white",font=("Arial",16),text="current puzzle")
lbl_c.grid(row=0,column=0,padx=10,pady=10,sticky="nsew",columnspan=3)

current1=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="1",fg_color="white")
current1.grid(row=1,column=0,padx=10,pady=10,sticky="nsew")

current2=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="2",fg_color="white")
current2.grid(row=1,column=1,padx=10,pady=10,sticky="nsew")

current3=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="3",fg_color="white")
current3.grid(row=1,column=2,padx=10,pady=10,sticky="nsew")

current4=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="4",fg_color="white")
current4.grid(row=2,column=0,padx=10,pady=10, sticky="nsew")

current5=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="5",fg_color="white")
current5.grid(row=2,column=1,padx=10,pady=10,sticky="nsew")

current6=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="6",fg_color="white")
current6.grid(row=2,column=2,padx=10,pady=10,sticky="nsew")

current7=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="7",fg_color="white")
current7.grid(row=3,column=0,padx=10,pady=10,sticky="nsew")

current8=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="8",fg_color="white")
current8.grid(row=3,column=1,padx=10,pady=10,sticky="nsew")

current0=ctk.CTkLabel(frm3,text_color="black",font=("Arial",16),text="",fg_color="white")
current0.grid(row=3,column=2,padx=10,pady=10,sticky="nsew")


lbl_t=ctk.CTkLabel(frm4,fg_color="#02023b",text_color="white",font=("Arial",16),text="target")
lbl_t.grid(row=0,column=0,padx=10,pady=10,sticky="nsew",columnspan=3)

lbl1=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="1",fg_color="white")
lbl1.grid(row=1,column=0,padx=10,pady=10,sticky="nsew")

lbl2=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="2",fg_color="white")
lbl2.grid(row=1,column=1,padx=10,pady=10,sticky="nsew")

lbl3=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="3",fg_color="white")
lbl3.grid(row=1,column=2,padx=10,pady=10,sticky="nsew")

lbl4=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="4",fg_color="white")
lbl4.grid(row=2,column=0,padx=10,pady=10, sticky="nsew")

lbl5=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="5",fg_color="white")
lbl5.grid(row=2,column=1,padx=10,pady=10,sticky="nsew")

lbl6=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="6",fg_color="white")
lbl6.grid(row=2,column=2,padx=10,pady=10,sticky="nsew")

lbl7=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="7",fg_color="white")
lbl7.grid(row=3,column=0,padx=10,pady=10,sticky="nsew")

lbl8=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="8",fg_color="white")
lbl8.grid(row=3,column=1,padx=10,pady=10,sticky="nsew")

lbl9=ctk.CTkLabel(frm4,text_color="black",font=("Arial",16),text="",fg_color="white")
lbl9.grid(row=3,column=2,padx=10,pady=10,sticky="nsew")


ent1=tk.Entry(frm1,font=("Arial",16),justify="center")
ent1.grid(row=0,column=0,padx=10,pady=10,sticky="nsew")
ent1.bind("<Key>",lambda e: Auto_focus(e,ent1,ent2))

ent2=tk.Entry(frm1,font=("Arial",16),justify="center")
ent2.grid(row=0,column=1,padx=10,pady=10,sticky="nsew")
ent2.bind("<Key>",lambda e: Auto_focus(e,ent2,ent3,ent1))

ent3=tk.Entry(frm1,font=("Arial",16),justify="center")
ent3.grid(row=0,column=2,padx=10,pady=10,sticky="nsew")
ent3.bind("<Key>",lambda e: Auto_focus(e,ent3,ent4,ent2))

ent4=tk.Entry(frm1,font=("Arial",16),justify="center")
ent4.grid(row=1,column=0,padx=10,pady=10, sticky="nsew")
ent4.bind("<Key>",lambda e: Auto_focus(e,ent4,ent5,ent3))

ent5=tk.Entry(frm1,font=("Arial",16),justify="center")
ent5.grid(row=1,column=1,padx=10,pady=10,sticky="nsew")
ent5.bind("<Key>",lambda e: Auto_focus(e,ent5,ent6,ent4))

ent6=tk.Entry(frm1,font=("Arial",16),justify="center")
ent6.grid(row=1,column=2,padx=10,pady=10,sticky="nsew")
ent6.bind("<Key>",lambda e: Auto_focus(e,ent6,ent7,ent5))

ent7=tk.Entry(frm1,font=("Arial",16),justify="center")
ent7.grid(row=2,column=0,padx=10,pady=10,sticky="nsew")
ent7.bind("<Key>",lambda e: Auto_focus(e,ent7,ent8,ent6))

ent8=tk.Entry(frm1,font=("Arial",16),justify="center")
ent8.grid(row=2,column=1,padx=10,pady=10,sticky="nsew")
ent8.bind("<Key>",lambda e: Auto_focus(e,ent8,ent9,ent7))

ent9=tk.Entry(frm1,font=("Arial",16),justify="center")
ent9.grid(row=2,column=2,padx=10,pady=10,sticky="nsew")
ent9.bind("<Key>",lambda e: Auto_focus(e,ent9,ent9,ent8))


btn_solve=ctk.CTkButton(frmGetPuzzle,text="next",text_color="white",fg_color="#f51dc3",font=("Arial",16),command=validate_data)
btn_solve.grid(row=4,column=0)

btn_solve2=ctk.CTkButton(frm2,text="solve",fg_color="#f51dc3",text_color="white",font=("Arial",16),command=start_solving)
btn_solve2.grid(row=4,column=1)

btnSolution=ctk.CTkButton(frm2,text="solution",text_color="white",fg_color="#f51dc3",font=("Arial",16),command=lambda:change_frame(frmS),state="disabled")
btnSolution.grid(row=4,column=2)


change_frame(frm_kosar)

root.mainloop()
