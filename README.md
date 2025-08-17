# web-scraping-inmobiliaria

# demo_web_scraping_inmobiliaria_pro.py

import pandas as pd
from openpyxl import load_workbook
from openpyxl.styles import Font, PatternFill
from openpyxl.chart import BarChart, Reference

# Simulamos datos de propiedades
propiedades = [
    {"Título": "Depto 2 ambientes en Palermo", "Precio": 120000, "Ubicación": "Palermo, CABA", "Link": "https://ejemplo.com/prop1"},
    {"Título": "Casa 3 habitaciones en Recoleta", "Precio": 250000, "Ubicación": "Recoleta, CABA", "Link": "https://ejemplo.com/prop2"},
    {"Título": "Monoambiente en Belgrano", "Precio": 80000, "Ubicación": "Belgrano, CABA", "Link": "https://ejemplo.com/prop3"},
    {"Título": "Depto 1 ambiente en San Telmo", "Precio": 95000, "Ubicación": "San Telmo, CABA", "Link": "https://ejemplo.com/prop4"},
]

# Convertimos a DataFrame
df = pd.DataFrame(propiedades)

# Guardamos en Excel
archivo_excel = "Reporte_Propiedades_Pro.xlsx"
df.to_excel(archivo_excel, index=False)

# Abrimos el archivo con openpyxl para aplicar formato
wb = load_workbook(archivo_excel)
ws = wb.active

# Formato: encabezados
for cell in ws[1]:
    cell.font = Font(bold=True, color="FFFFFF")
    cell.fill = PatternFill("solid", fgColor="1abc9c")

# Agregar gráfico de precios
chart = BarChart()
chart.title = "Precio de Propiedades"
chart.x_axis.title = "Propiedad"
chart.y_axis.title = "Precio USD"

data = Reference(ws, min_col=2, min_row=1, max_row=len(df)+1)  # Columna Precio
cats = Reference(ws, min_col=1, min_row=2, max_row=len(df)+1)  # Títulos
chart.add_data(data, titles_from_data=True)
chart.set_categories(cats)
ws.add_chart(chart, "F2")

# Guardamos cambios
wb.save(archivo_excel)

print("¡Reporte profesional generado! Revisa 'Reporte_Propiedades_Pro.xlsx'")
