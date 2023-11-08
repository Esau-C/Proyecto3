# Proyecto 3 de algoritmos
## Nombre: Edgar Esaú Contreras Garcia
## Carnet: 7690-23-6131
## Carrera: Ingenieria en sistemas

Acontinuacion se le presentara en lenguaje python el proyecto que consistia en una aplicacion que pueda leer archivos de texto, modificarlos y guardarlos mas unas ideas que podiamos colocar, adentro de la aplicacion viene el boton para acceder al manual de usuario y la licencia con la que se trabajo

### Acontinuacion el codigo

```python
import tkinter as tk
from tkinter import filedialog
from tkinter import font
import webbrowser

# Función para abrir el cuadro de diálogo de selección de fuentes
def seleccionar_fuente():
    selected_font = font.Font(family=font_family_var.get(), size=font_size_var.get())
    area_de_texto.config(font=selected_font)

# Función para abrir el menú de cambio de fuente
def abrir_menu_cambio_fuente():
    ventana_fuente = tk.Toplevel(ventana)
    ventana_fuente.title("Cambio de Fuente")
    ventana_fuente.geometry("300x150")

    # Crear una lista de familias de fuente disponibles
    familias_fuentes = font.families()

    # Crear un cuadro de selección de fuente
    lbl_fuente = tk.Label(ventana_fuente, text="Selecciona la Fuente:")
    lbl_fuente.pack()

    fuente_var = tk.StringVar(value=font_family_var.get())
    fuente_menu = tk.OptionMenu(ventana_fuente, fuente_var, *familias_fuentes)
    fuente_menu.pack()

    # Crear un cuadro de selección de tamaño de fuente
    lbl_tamano = tk.Label(ventana_fuente, text="Selecciona el Tamaño de Fuente:")
    lbl_tamano.pack()

    tamano_var = tk.IntVar(value=font_size_var.get())
    tamano_menu = tk.Spinbox(ventana_fuente, from_=8, to=72, textvariable=tamano_var)
    tamano_menu.pack()

    # Botón para aplicar los cambios
    btn_aplicar = tk.Button(ventana_fuente, text="Aplicar", command=lambda: aplicar_cambios_fuente(fuente_var, tamano_var))
    btn_aplicar.pack()

# Función para aplicar los cambios de fuente y tamaño de letra
def aplicar_cambios_fuente(fuente_var, tamano_var):
    font_family_var.set(fuente_var.get())
    font_size_var.set(tamano_var.get())
    seleccionar_fuente()

def nuevo_archivo(event=None):
    area_de_texto.delete(1.0, tk.END)

def abrir_archivo(event=None):
    global ruta_archivo
    tipos_de_archivo = [("Archivos de texto", "*.txt"),
                       ("Archivos de Python", "*.py"),
                       ("Archivos de C++", "*.cpp"),
                       ("Todos los archivos", "*.*")]
    ruta_archivo = filedialog.askopenfilename(defaultextension=".txt", filetypes=tipos_de_archivo)

    if ruta_archivo:
        try:
            with open(ruta_archivo, "rb") as file:
                contenido = file.read()
                # Intenta decodificar el contenido con UTF-8
                try:
                    texto = contenido.decode("utf-8")
                except UnicodeDecodeError:
                    # Si la decodificación falla, usa una codificación alternativa
                    texto = contenido.decode("iso-8859-1")  
                area_de_texto.delete(1.0, tk.END)
                area_de_texto.insert(tk.INSERT, texto)
                actualizar_contador_caracteres()
        except FileNotFoundError:
            # Manejo de errores si el archivo no existe
            print("El archivo no se encuentra.")
        except:
            # Manejo de otros errores
            print("Error al abrir el archivo")

def guardar_archivo(event=None):
    global ruta_archivo
    if ruta_archivo:
        try:
            with open(ruta_archivo, "w", encoding="utf-8") as file:
                file.write(area_de_texto.get(1.0, tk.END))
        except:
            print("No se pudo guardar el archivo")

def guardar_como(event=None):
    nueva_ruta = filedialog.asksaveasfilename(defaultextension=".txt",
                                              filetypes=[("Archivos de texto", "*.txt"),
                                                         ("Archivos de python", "*py"),
                                                         ("Archivos de C++", "*.cpp"),
                                                         ("Todos los archivos", "*.*")])
    if nueva_ruta:
        with open(nueva_ruta, "w", encoding="utf-8") as file:
            file.write(area_de_texto.get(1.0, tk.END))
    
    print(nueva_ruta)

def copiar(event=None):
    area_de_texto.event_generate("<<Copy>>")

def pegar(event=None):
    area_de_texto.event_generate("<<Paste>>")

def cortar(event=None):
    area_de_texto.event_generate("<<Cut>>")

# Función para mostrar el manual de usuario
def mostrar_manual_usuario(event=None):
    ventana_manual = tk.Toplevel(ventana)
    ventana_manual.title("Manual de Usuario")
    ventana_manual.geometry("400x200")

    # Crea un label para mostrar el enlace
    label_enlace = tk.Label(ventana_manual, text="Click para ver el Manual de Usuario", fg="blue", cursor="hand2")
    label_enlace.pack()

    # Función para abrir el enlace cuando se hace clic en el label
    def abrir_enlace(event):
        webbrowser.open_new("https://drive.google.com/file/d/14Rh5CLvCVD5M75WShBPpCvFh4KoEkOxx/view?usp=sharing")  # Reemplaza con la URL real del manual

    # Vincula la función de abrir enlace al label
    label_enlace.bind("<Button-1>", abrir_enlace)

# Función para mostrar la licencia
def mostrar_licencia(event=None):
    ventana_licencia = tk.Toplevel(ventana)
    ventana_licencia.title("Licencia")
    texto_licencia = tk.Text(ventana_licencia)
    texto_licencia.pack(fill=tk.BOTH, expand=True)
    contenido_licencia = """
    Este software está protegido por derechos de autor.
    Creado en 07/11/2023.
    Autor: Esaú Contreras

    Términos y condiciones:
    1. Ser parte de la UMG
    2. Entender que el programa es con fines educativos
    3. No dibulgar el codigo

    Contacto: econtrerasg.7@miumg.edu.gt
    """
    texto_licencia.insert(tk.END, contenido_licencia)

# Función para habilitar o deshabilitar el modo "Noche"
def activar_modo_noche():
    if modo_noche_var.get() == 1:
        # Modificar colores para el modo "Noche"
        area_de_texto.config(bg="black", fg="white", insertbackground="white")  # Cambia el color del cursor a blanco
    else:
        # Restaurar colores originales
        area_de_texto.config(bg="white", fg="black", insertbackground="black")  # Cambia el color del cursor a negro

# Función para actualizar el contador de caracteres
def actualizar_contador_caracteres(event=None):
    contenido = area_de_texto.get(1.0, tk.END)
    num_caracteres = len(contenido) - 1  # Restar 1 para excluir el carácter de nueva línea al final
    contador_caracteres.config(text=f"Caracteres: {num_caracteres}")

ventana = tk.Tk()
ventana.title("Lector de archivos")
ventana.geometry("800x600")
ruta_archivo = ""

menu = tk.Menu(ventana)
ventana.config(menu=menu)

archivo = tk.Menu(menu)
menu.add_cascade(label="Archivo", menu=archivo)

edicion = tk.Menu(menu)
menu.add_cascade(label="Edición", menu=edicion)

ayuda = tk.Menu(menu)
menu.add_cascade(label="Ayuda", menu=ayuda)

# Crear una variable de control para la familia de fuente
font_family_var = tk.StringVar(value="Arial")
# Crear una variable de control para el tamaño de fuente
font_size_var = tk.IntVar(value=12)

# Variable de control para el modo "Noche"
modo_noche_var = tk.IntVar(value=0)

area_de_texto = tk.Text(ventana)
area_de_texto.pack(fill=tk.BOTH, expand=True)

archivo.add_command(label="Nuevo", command=nuevo_archivo, accelerator="Ctrl+N")
archivo.add_command(label="Abrir", command=abrir_archivo, accelerator="Ctrl+O")
archivo.add_command(label="Guardar", command=guardar_archivo, accelerator="")
archivo.add_command(label="Guardar como", command=guardar_como, accelerator="")

edicion.add_command(label="Copiar", command=copiar, accelerator="")
edicion.add_command(label="Pegar", command=pegar, accelerator="")
edicion.add_command(label="Cortar", command=cortar, accelerator="")

# Agregar una opción en el menú de "Edición" para abrir el menú de cambio de fuente
edicion.add_command(label="Cambio de Fuente", command=abrir_menu_cambio_fuente, accelerator="")

ayuda.add_command(label="Manual de usuario", command=mostrar_manual_usuario, accelerator="")
ayuda.add_command(label="Licencia", command=mostrar_licencia)

# Agregar una opción en el menú de "Ver" para activar/desactivar el modo "Noche"
ver = tk.Menu(menu)
menu.add_cascade(label="Ver", menu=ver)
ver.add_checkbutton(label="Modo Noche", variable=modo_noche_var, command=activar_modo_noche, accelerator="")

# Crear un Label para mostrar el contador de caracteres
contador_caracteres = tk.Label(ventana, text="Caracteres: 0")
contador_caracteres.pack(side=tk.LEFT, anchor=tk.W)

ventana.bind_all("<Control-n>", nuevo_archivo)
ventana.bind_all("<Control-o>", abrir_archivo)
ventana.bind_all("<Control-s>", guardar_archivo)
ventana.bind_all("<Control-Shift-s>", guardar_como)
ventana.bind_all("<Control-c>", copiar)
ventana.bind_all("<Control-v>", pegar)
ventana.bind_all("<Control-x>", cortar)
ventana.bind_all("<F1>", mostrar_manual_usuario)
ventana.bind_all("<Alt-n>", activar_modo_noche)

# Actualizar el contador de caracteres cuando se modifica el texto
area_de_texto.bind("<KeyRelease>", actualizar_contador_caracteres)

ventana.mainloop()

```
