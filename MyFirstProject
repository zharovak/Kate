import tkinter as tk
from tkinter import ttk
import sqlite3


import sqlite3
selected = 0 # Айди выбраной записи
# Создание базы данных и подключение к ней
conn = sqlite3.connect('employees.db')
c = conn.cursor()

# Создание таблицы "employees"
c.execute('''CREATE TABLE IF NOT EXISTS employees
             (id INTEGER PRIMARY KEY AUTOINCREMENT,
              full_name TEXT,
              phone_number TEXT,
              email TEXT,
              salary REAL)''')

# Закрытие соединения с базой данных
conn.close()

def add_employee():
    full_name = entry_full_name.get()
    phone_number = entry_phone_number.get()
    email = entry_email.get()
    salary = entry_salary.get()

    conn = sqlite3.connect('employees.db')
    c = conn.cursor()
    c.execute("INSERT INTO employees (full_name, phone_number, email, salary) VALUES (?, ?, ?, ?)",
              (full_name, phone_number, email, salary))
    conn.commit()
    conn.close()

    # Очистка полей ввода после добавления сотрудника
    entry_full_name.delete(0, tk.END)
    entry_phone_number.delete(0, tk.END)
    entry_email.delete(0, tk.END)
    entry_salary.delete(0, tk.END)

    # Обновление списка сотрудников
    refresh_employee_list()


# Функция для изменения существующего сотрудника
def update_employee():
    global selected
    full_name = entry_full_name.get()
    phone_number = entry_phone_number.get()
    email = entry_email.get()
    salary = entry_salary.get()

    conn = sqlite3.connect('employees.db')
    c = conn.cursor()

    c.execute("UPDATE employees SET full_name=?, phone_number=?, email=?, salary=? WHERE id=?",
              (full_name, phone_number, email, salary, selected))
    conn.commit()
    conn.close()

    # Очистка полей ввода после изменения сотрудника
    entry_full_name.delete(0, tk.END)
    entry_phone_number.delete(0, tk.END)
    entry_email.delete(0, tk.END)
    entry_salary.delete(0, tk.END)

    # Обновление списка сотрудников
    refresh_employee_list()


# Функция для удаления сотрудника
def delete_employee():
    global selected


    conn = sqlite3.connect('employees.db')
    c = conn.cursor()

    c.execute("DELETE FROM employees WHERE id=?", (selected,))
    print(selected)
    conn.commit()
    conn.close()

    # Очистка полей ввода после удаления сотрудника
    entry_full_name.delete(0, tk.END)
    entry_phone_number.delete(0, tk.END)
    entry_email.delete(0, tk.END)
    entry_salary.delete(0, tk.END)

    # Обновление списка сотрудников
    refresh_employee_list()


# Функция для поиска сотрудника по ФИО
def search_employee():
    full_name = entry_search.get()

    conn = sqlite3.connect('employees.db')
    c = conn.cursor()
    c.execute("SELECT * FROM employees WHERE full_name=?", (full_name,))
    result = c.fetchall()
    conn.close()

    # Очистка предыдущих результатов
    for item in treeview.get_children():
        treeview.delete(item)

    # Вывод найденных сотрудников
    for employee in result:
        treeview.insert('', 'end', values=employee)

def on_treeview_click(event):
    global selected
    item = treeview.identify('item', event.x, event.y)
    selected = treeview.item(item)['values'][0]# Выводим айди строки
# Функция для обновления списка сотрудников
def refresh_employee_list():
    # Очистка предыдущих результатов
    for item in treeview.get_children():
        treeview.delete(item)

    conn = sqlite3.connect('employees.db')
    c = conn.cursor()
    c.execute("SELECT * FROM employees")
    result = c.fetchall()
    conn.close()

    # Вывод списка сотрудников
    for employee in result:
        treeview.insert('', 'end', values=employee)


# Создание окна
window = tk.Tk()
window.title("Список сотрудников компании")

# Создание интерфеса и других эелементов
label_full_name = tk.Label(window, text="ФИО:")
label_full_name.grid(row=0, column=0, padx=5, pady=5)
entry_full_name = tk.Entry(window)
entry_full_name.grid(row=0, column=1, padx=5, pady=5)

label_phone_number = tk.Label(window, text="Номер телефона:")
label_phone_number.grid(row=1, column=0, padx=5, pady=5)
entry_phone_number = tk.Entry(window)
entry_phone_number.grid(row=1, column=1, padx=5, pady=5)

label_email = tk.Label(window, text="Адрес электронной почты:")
label_email.grid(row=2, column=0, padx=5, pady=5)
entry_email = tk.Entry(window)
entry_email.grid(row=2, column=1, padx=5, pady=5)

label_salary = tk.Label(window, text="Зарплата:")
label_salary.grid(row=3, column=0, padx=5, pady=5)
entry_salary = tk.Entry(window)
entry_salary.grid(row=3, column=1, padx=5, pady=5)

button_add = tk.Button(window, text="Добавить", command=add_employee)
button_add.grid(row=4, column=0, padx=5, pady=5)

button_update = tk.Button(window, text="Изменить", command=update_employee)
button_update.grid(row=4, column=1, padx=5, pady=5)

button_delete = tk.Button(window, text="Удалить", command=delete_employee)
button_delete.grid(row=4, column=2, padx=5, pady=5)

button_delete = tk.Button(window, text="Обновить", command=refresh_employee_list)
button_delete.grid(row=4, column=3, padx=5, pady=5)

label_search = tk.Label(window, text="Поиск по ФИО:")
label_search.grid(row=5, column=0, padx=5, pady=5)
entry_search = tk.Entry(window)
entry_search.grid(row=5, column=1, padx=5, pady=5)

button_search = tk.Button(window, text="Найти", command=search_employee)
button_search.grid(row=5, column=2, padx=5, pady=5)


treeview = ttk.Treeview(window, columns=("ID", "ФИО", "Номер телефона", "Адрес электронной почты", "Зарплата"), show="headings")
treeview.column("ID", width=30)
treeview.column("ФИО", width=150)
treeview.column("Номер телефона", width=100)
treeview.column("Адрес электронной почты", width=150)
treeview.column("Зарплата", width=70)
treeview.heading("ID", text="ID")
treeview.heading("ФИО", text="ФИО")
treeview.heading("Номер телефона", text="Номер телефона")
treeview.heading("Адрес электронной почты", text="Адрес электронной почты")
treeview.heading("Зарплата", text="Зарплата")
treeview.grid(row=6, column=0, columnspan=3, padx=5, pady=5)
# Обработчик для получения выделеннйо строки
treeview.bind('<ButtonRelease-1>', on_treeview_click)

refresh_employee_list()

window.mainloop()
