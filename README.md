import tkinter as tk
from tkinter import simpledialog, messagebox

class TravelMastersGUI:
    def __init__(self, master):
        self.master = master
        master.title("Aplicativo de Gerenciamento de Tarefas TravelMasters")

        self.travel_masters = []

        self.label = tk.Label(master, text="Escolha uma opção:")
        self.label.pack()

        self.add_button = tk.Button(master, text="Adicionar Tarefa", command=self.add_task)
        self.add_button.pack()

        self.list_button = tk.Button(master, text="Listar Tarefas", command=self.list_tasks)
        self.list_button.pack()

        self.mark_completed_button = tk.Button(master, text="Marcar Tarefa como Concluída", command=self.mark_task_completed)
        self.mark_completed_button.pack()

        self.text_widget = tk.Text(master, height=10, width=40)
        self.text_widget.pack()

        self.quit_button = tk.Button(master, text="Sair", command=master.quit)
        self.quit_button.pack()

    def add_task(self):
        description = simpledialog.askstring("Adicionar Tarefa", "Informe a descrição da tarefa:")
        if description:
            task = {"description": description, "completed": False, "repetitions": 2}
            self.travel_masters.append(task)
            self.show_success_message("Sucesso", "Tarefa adicionada com sucesso.")
            self.list_tasks()

    def list_tasks(self):
        self.text_widget.delete(1.0, tk.END)  # Limpa o conteúdo atual no widget Text
        for i, task in enumerate(self.travel_masters, start=1):
            task_info = f"{i}. {task['description']} - {'Concluída' if task['completed'] else 'Pendente'} - Repetições Restantes: {task['repetitions']}\n"
            self.text_widget.insert(tk.END, task_info)

    def mark_task_completed(self):
        self.list_tasks()
        task_index = simpledialog.askinteger("Marcar Tarefa como Concluída", "Informe o número da tarefa concluída:")
        if task_index is not None and 1 <= task_index <= len(self.travel_masters):
            task = self.travel_masters[task_index - 1]
            if task['repetitions'] > 0:
                task['repetitions'] -= 1
                task['completed'] = True
                self.show_success_message("Sucesso", f"Tarefa {task_index} marcada como concluída. Repetições Restantes: {task['repetitions']}")
            else:
                self.show_success_message("Sucesso", f"Tarefa {task_index} concluída, mas não há mais repetições.")
            self.list_tasks()
        else:
            messagebox.showerror("Erro", "Número de tarefa inválido.")

    def show_success_message(self, title, message):
        messagebox.showinfo(title, message)

def main():
    root = tk.Tk()
    app = TravelMastersGUI(root)
    root.mainloop()

if __name__ == "__main__":
    main()
