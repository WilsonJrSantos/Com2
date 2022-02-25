import numpy as np
def encrypt(mensaje):
    #reemplaza los espacios del mensaje original
    mensaje = mensaje.replace(" ", "")
    # Solicitar el texto y la clave del cifrado
    C = make_key()
    # Se agregan los ceros necesarios si el mensaje no es divisible por 2
    determinar_tamaño = len(mensaje) % 2 == 0
    if not determinar_tamaño:
        mensaje += "0"
    # Rellenar la matriz con los datos solicitados mensaje
    P = crear_Matriz_enteros(mensaje) # crear una matriz de enteros a partir de una cadena
    # Calcular la lngitud del mensaje
    mensaje_len = int(len(mensaje) / 2)
    # Calcular P * C
    mensaje_encriptado = ""
    for i in range(mensaje_len):
        # se realiza el producto punto
        row_0 = P[0][i] * C[0][0] + P[1][i] * C[0][1]
        # Se determina el abecedario a-z en formato ascii
        integer = int(row_0 % 26 + 65)
        # cambia el tipo de mensaje a chr, el cual devuelve el caracter
        mensaje_encriptado += chr(integer)
        # Se repite para la segunda columna
        row_1 = P[0][i] * C[1][0] + P[1][i] * C[1][1]
        integer = int(row_1 % 26 + 65)
        mensaje_encriptado += chr(integer)
    return mensaje_encriptado

def desencriptar(mensaje_encriptado):
    # Solicite una palabra clave y obtenga una matriz de cifrado
    C = make_key()
    # matriz inversa
    determinante = C[0][0] * C[1][1] - C[0][1] * C[1][0]
    determinante = determinante % 26
    nverso_multiplicativo = buscar_inverso_multiplicativo(determinante)
    C_inverse = C
    # intercambiar a <-> d
    C_inverse[0][0], C_inverse[1][1] = C_inverse[1, 1], C_inverse[0, 0]
    # reemplazar
    C[0][1] *= -1
    C[1][0] *= -1
    for row in range(2):
        for column in range(2):
            C_inverse[row][column] *= nverso_multiplicativo
            C_inverse[row][column] = C_inverse[row][column] % 26

    P = crear_Matriz_enteros(mensaje_encriptado)
    mensaje_len = int(len(mensaje_encriptado) / 2)
    desencriptared_mensaje = ""
    for i in range(mensaje_len):
        # Dot product
        column_0 = P[0][i] * C_inverse[0][0] + P[1][i] * C_inverse[0][1]
        # Modula y agrega 65 para volver al rango A-Z en ascii
        integer = int(column_0 % 26 + 65)
        # Vuelva a cambiar al tipo chr y agregue al texto
        desencriptared_mensaje += chr(integer)
        # Repita para la segunda columna.
        column_1 = P[0][i] * C_inverse[1][0] + P[1][i] * C_inverse[1][1]
        integer = int(column_1 % 26 + 65)
        desencriptared_mensaje += chr(integer)
    if desencriptared_mensaje[-1] == "0":
        desencriptared_mensaje = desencriptared_mensaje[:-1]
    return desencriptared_mensaje

def buscar_inverso_multiplicativo(determinante):
    nverso_multiplicativo = -1
    for i in range(26):
        inverse = determinante * i
        if inverse % 26 == 1:
            nverso_multiplicativo = i
            break
    return nverso_multiplicativo


def make_key():
     # Asegúrese de que el determinante de cifrado sea relativamente primo a 26 y solo se proporcionen a/A - z/Z
    determinante = 0
    C = None
    while True:
        cipher = input("Ingrese el cifrado de 4 letras: ")
        C = crear_Matriz_enteros(cipher)
        determinante = C[0][0] * C[1][1] - C[0][1] * C[1][0]
        determinante = determinante % 26
        inverse_element = buscar_inverso_multiplicativo(determinante)
        if inverse_element == -1:
            print("determinante no es relativamente primo a 26, clave invertible")
        elif np.amax(C) > 26 and np.amin(C) < 0:
            print("Solo se aceptan caracteres a-z")
            print(np.amax(C), np.amin(C))
        else:
            break
    return C

def crear_Matriz_enteros(string):
    # Map string to a list of integers a/A <-> 0, b/B <-> 1 ... z/Z <-> 25
    integers = [chr_to_int(c) for c in string]
    length = len(integers)
    M = np.zeros((2, int(length / 2)), dtype=np.int32)
    iterator = 0
    for column in range(int(length / 2)):
        for row in range(2):
            M[row][column] = integers[iterator]
            iterator += 1
    return M

def chr_to_int(char):
    # Mayúsculas el carácter para entrar en el rango 65-90 en la tabla ascii
    char = char.upper()
    #castear chr a int y reste 65 para obtener 0-25
    integer = ord(char) - 65
    return integer

if __name__ == "__main__":
    mensaje = input("Escriba un mensaje: ")
    mensaje_encriptado = encrypt(mensaje)
    print(mensaje_encriptado)
    desencriptared_mensaje = desencriptar(mensaje_encriptado)
    print(desencriptared_mensaje)
