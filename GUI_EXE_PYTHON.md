# Crear un `.exe` con interfaz gráfica para el verificador

Sí, **se puede** convertir tu script a un `.exe` con una interfaz gráfica bonita.

## Enfoque recomendado

1. **Separar lógica y UI**
   - Mantén funciones como carga de datos, extracción de equipos y cálculo de estadísticas en un módulo.
   - Crea una capa visual aparte (por ejemplo con `customtkinter`).

2. **Framework de interfaz**
   - Opción simple y elegante: `customtkinter`.
   - Si quieres algo más avanzado: `PySide6` o `PyQt6`.

3. **Empaquetado a `.exe`**
   - Usa `pyinstaller` para generar un ejecutable de Windows.

## Estructura sugerida

```text
proyecto/
  app_core.py          # lógica (Google Sheets, cálculos)
  app_gui.py           # interfaz gráfica
  requirements.txt
```

## Dependencias

```bash
pip install pandas customtkinter pyinstaller
```

## Ejemplo mínimo de interfaz (`app_gui.py`)

```python
import customtkinter as ctk

ctk.set_appearance_mode("dark")
ctk.set_default_color_theme("blue")

class App(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Verificador de Equipos")
        self.geometry("900x600")

        self.label = ctk.CTkLabel(self, text="Verificador de acierto", font=("Segoe UI", 24, "bold"))
        self.label.pack(pady=20)

        self.home_entry = ctk.CTkEntry(self, placeholder_text="Equipo local")
        self.home_entry.pack(pady=8, padx=20, fill="x")

        self.away_entry = ctk.CTkEntry(self, placeholder_text="Equipo visitante")
        self.away_entry.pack(pady=8, padx=20, fill="x")

        self.run_btn = ctk.CTkButton(self, text="Analizar")
        self.run_btn.pack(pady=14)

        self.output = ctk.CTkTextbox(self)
        self.output.pack(padx=20, pady=10, fill="both", expand=True)

if __name__ == "__main__":
    App().mainloop()
```

## Crear el `.exe`

```bash
pyinstaller --noconfirm --onefile --windowed --name VerificadorEquipos app_gui.py
```

El `.exe` quedará en:

```text
dist/VerificadorEquipos.exe
```

## Recomendaciones para que se vea “bonito y estético”

- Usa una paleta consistente (1 color principal + neutros).
- Añade tipografía clara (`Segoe UI`, `Inter`, `Roboto`).
- Muestra métricas en tarjetas: acierto, yield, partidos analizados.
- Usa icono propio con `--icon app.ico`.
- Evita consola negra con `--windowed`.

## Limitaciones importantes

- El `.exe` generado en Windows funciona en Windows.
- Si compilas en Linux/macOS, no obtendrás `.exe` de Windows de forma nativa.
- Para distribución profesional, firma digital del ejecutable (opcional, recomendado).

## Flujo ideal de implementación

1. Extraer funciones del script actual a `app_core.py`.
2. Crear UI con `customtkinter` en `app_gui.py`.
3. Probar localmente.
4. Empaquetar con PyInstaller.
5. Ajustar icono, nombre y versión.

---

Si quieres, en el siguiente paso te puedo dejar una versión completa de `app_core.py` + `app_gui.py` ya conectada a tu Google Sheet, lista para compilar a `.exe`.
