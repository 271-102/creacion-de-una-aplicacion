# creacion-de-una-aplicacion
casandra
import tkinter as tk
from tkinter import ttk, messagebox
from tkcalendar import DateEntry

# Crear ventana principal
ventana = tk.Tk()
ventana.title("Agenda Personal")
ventana.geometry("700x400")

# ---------- Frame para TreeView ----------
frame_lista = tk.Frame(ventana)
frame_lista.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)

# TreeView para mostrar eventos
columnas = ("Fecha", "Hora", "Descripción")
tree = ttk.Treeview(frame_lista, columns=columnas, show="headings")
for col in columnas:
    tree.heading(col, text=col)
    tree.column(col, width=150)
tree.pack(fill=tk.BOTH, expand=True)

# ---------- Frame para Entrada de Datos ----------
frame_entrada = tk.Frame(ventana)
frame_entrada.pack(pady=10)

tk.Label(frame_entrada, text="Fecha:").grid(row=0, column=0, padx=5, pady=5)
calendario = DateEntry(frame_entrada, width=12, background="darkblue", foreground="white", borderwidth=2)
calendario.grid(row=0, column=1, padx=5, pady=5)

tk.Label(frame_entrada, text="Hora:").grid(row=0, column=2, padx=5, pady=5)
entrada_hora = tk.Entry(frame_entrada, width=10)
entrada_hora.grid(row=0, column=3, padx=5, pady=5)

tk.Label(frame_entrada, text="Descripción:").grid(row=0, column=4, padx=5, pady=5)
entrada_desc = tk.Entry(frame_entrada, width=25)
entrada_desc.grid(row=0, column=5, padx=5, pady=5)

# ---------- Funciones ----------
def agregar_evento():
    fecha = calendario.get()
    hora = entrada_hora.get()
    desc = entrada_desc.get()
    
    if fecha and hora and desc:
        tree.insert("", "end", values=(fecha, hora, desc))
        entrada_hora.delete(0, tk.END)
        entrada_desc.delete(0, tk.END)
    else:
        messagebox.showwarning("Atención", "Debe completar todos los campos.")

def eliminar_evento():
    seleccionado = tree.selection()
    if not seleccionado:
        messagebox.showwarning("Atención", "Seleccione un evento para eliminar.")
        return
    
    confirmar = messagebox.askyesno("Confirmar", "¿Está seguro de eliminar el evento seleccionado?")
    if confirmar:
        for item in seleccionado:
            tree.delete(item)

# ---------- Frame para Botones ----------
frame_botones = tk.Frame(ventana)
frame_botones.pack(pady=10)

btn_agregar = tk.Button(frame_botones, text="Agregar Evento", command=agregar_evento, bg="lightgreen")
btn_agregar.grid(row=0, column=0, padx=10)

btn_eliminar = tk.Button(frame_botones, text="Eliminar Evento Seleccionado", command=eliminar_evento, bg="tomato")
btn_eliminar.grid(row=0, column=1, padx=10)

btn_salir = tk.Button(frame_botones, text="Salir", command=ventana.destroy, bg="lightgray")
btn_salir.grid(row=0, column=2, padx=10)

ventana.mainloop()
