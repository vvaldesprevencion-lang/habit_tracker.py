# habit_tracker.py

import json
import os
from datetime import date

ARCHIVO = "habitos.json"

def cargar_habitos():
    if not os.path.exists(ARCHIVO):
        return {}
    with open(ARCHIVO, "r", encoding="utf-8") as f:
        return json.load(f)

def guardar_habitos(habitos):
    with open(ARCHIVO, "w", encoding="utf-8") as f:
        json.dump(habitos, f, indent=4, ensure_ascii=False)

def crear_habito(habitos):
    nombre = input("Nombre del hábito: ").strip()
    if nombre in habitos:
        print("Ese hábito ya existe.")
        return
    habitos[nombre] = []
    guardar_habitos(habitos)
    print("Hábito creado correctamente.")

def marcar_hoy(habitos):
    nombre = input("Nombre del hábito: ").strip()
    hoy = str(date.today())

    if nombre not in habitos:
        print("Ese hábito no existe.")
        return

    if hoy in habitos[nombre]:
        print("Hoy ya marcaste este hábito.")
        return

    habitos[nombre].append(hoy)
    guardar_habitos(habitos)
    print("Hábito marcado para hoy.")

def ver_progreso(habitos):
    for nombre, dias in habitos.items():
        total = len(dias)
        print(f"- {nombre}: {total} días cumplidos")

def menu():
    habitos = cargar_habitos()

    while True:
        print("\n--- GESTOR DE HÁBITOS ---")
        print("1. Crear hábito")
        print("2. Marcar hábito hoy")
        print("3. Ver progreso")
        print("4. Salir")

        opcion = input("Elige una opción: ")

        if opcion == "1":
            crear_habito(habitos)
        elif opcion == "2":
            marcar_hoy(habitos)
        elif opcion == "3":
            ver_progreso(habitos)
        elif opcion == "4":
            print("Hasta luego.")
            break
        else:
            print("Opción inválida.")

if __name__ == "__main__":
    menu()
