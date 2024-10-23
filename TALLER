import RPi.GPIO as IO
import time

# Deshabilitar advertencias y definir modo de pines
IO.setwarnings(False)
IO.setmode(IO.BOARD)

# Definir pines de entrada y salida
right = 22  # Banda derecha
left = 23   # Banda izquierda

# Definir pines de los sensores
S1 = 21  # Sensor S1
S2 = 19  # Sensor S2
S3 = 15  # Sensor S3
S4 = 13  # Sensor S4

# Configuración de los pines de salida y entrada
IO.setup(right, IO.OUT, initial=IO.LOW)
IO.setup(left, IO.OUT, initial=IO.LOW)

# Configuración de los sensores como entradas
IO.setup(S1, IO.IN)
IO.setup(S2, IO.IN)
IO.setup(S3, IO.IN)
IO.setup(S4, IO.IN)

def move_band(direction):
    """Función para mover la banda en la dirección dada"""
    if direction == 'right':
        IO.output(right, IO.HIGH)
        IO.output(left, IO.LOW)
    elif direction == 'left':
        IO.output(right, IO.LOW)
        IO.output(left, IO.HIGH)

def stop_band():
    """Función para detener la banda"""
    IO.output(right, IO.LOW)
    IO.output(left, IO.LOW)

def get_sensor_position():
    """Función para obtener el sensor activo (la posición de la pieza)"""
    if IO.input(S1) == IO.HIGH:
        return 1
    elif IO.input(S2) == IO.HIGH:
        return 2
    elif IO.input(S3) == IO.HIGH:
        return 3
    elif IO.input(S4) == IO.HIGH:
        return 4
    return None

# Programa principal
def main():
    # Solicitar la cantidad de secuencias
    num_sequences = int(input("Ingrese el número de secuencias que desea: "))
    
    points = []
    times = []
    for i in range(num_sequences):
        point = int(input(f"Ingrese el punto destino {i + 1} (1-4): "))
        stay_time = float(input(f"Ingrese el tiempo de permanencia en punto {i + 1} (segundos): "))
        points.append(point)
        times.append(stay_time)
    
    print("Secuencia programada lista. Presione # para iniciar.")
    
    # Esperar a que el usuario presione # para iniciar la secuencia
    while input() != '#':
        pass
    
    # Obtener la posición inicial de la pieza
    initial_position = get_sensor_position()
    
    if initial_position is None:
        print("No se detecta la pieza en ningún sensor.")
        return

    print(f"Pieza detectada en el sensor {initial_position}")
    
    # Ejecutar la secuencia de movimiento
    for point, stay_time in zip(points, times):
        if point < initial_position:
            print(f"Moviendo a la izquierda hacia el punto {point}")
            move_band('left')
        elif point > initial_position:
            print(f"Moviendo a la derecha hacia el punto {point}")
            move_band('right')
        
        # Esperar hasta llegar al punto de destino
        while get_sensor_position() != point:
            time.sleep(0.1)
        
        stop_band()  # Detener la banda al llegar
        print(f"En el punto {point}, esperando {stay_time} segundos.")
        time.sleep(stay_time)  # Esperar el tiempo programado
    
    # Regresar al punto inicial
    if initial_position < point:
        print(f"Regresando a la izquierda al punto inicial {initial_position}")
        move_band('left')
    elif initial_position > point:
        print(f"Regresando a la derecha al punto inicial {initial_position}")
        move_band('right')
    
    while get_sensor_position() != initial_position:
        time.sleep(0.1)
    
    stop_band()  # Detener la banda al regresar
    print("Secuencia finalizada. Reiniciando el programa.")

# Ejecutar el programa
if __name__ == "__main__":
    main()
