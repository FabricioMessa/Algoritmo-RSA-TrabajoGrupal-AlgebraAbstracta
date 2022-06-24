# Algoritmo-RSA-TrabajoGrupal-AlgebraAbstracta
Integrantes del grupo
- Royer Diosdado Carcausto Choquehuanca
- Fabricio Arian Messa Mandujano

        import math
        import random

        aleatorio = random.SystemRandom() #aquí se generan números aleatorios

        def prueba(n, a): #función con los valores "n" y "a" siguiendo la fórmula -> "a" elevado a "(n-1)" es congruente con "1 módulo n"
            exp = n - 1 #es la potencia
            while not exp & 1: #mientras el exponente sea par
                exp >>= 1 #manipulamos bits rápida, esto es lo mismo que exp //= 2
            if pow(a, exp, n) == 1: #aquí verificamos el módulo con ayuda de la librería math si es igual a 1 para que retorne verdadero
                return True
            while exp < n - 1:
                if pow(a, exp, n) == n - 1: #algoritmo para verificar que uno sea divisible
                    return True
                exp <<= 1 #desplazamos bit
            return False

        def Miller_Rabin(n, k = 40): #ponemos 40 como ejemplo ya que es la cantidad de valores de n
            for i in range(k): #recorremos hasta "k" valores
                a = aleatorio.randrange(2, n - 1) #a = a un número aleatorio con el rango
                if not prueba(n, a): #hacemos la verificación
                    return False
            return True

        def gen_primo(bits):
            while True:
                #todo esto garantiza que "a" es impar moviendo bits
                a = (aleatorio.randrange(1 << bits - 1, 1 << bits) >> 1) + 1
                if Miller_Rabin(a): #hacemos test de primalidad
                    return a #devolvemos el número aleatorio 

        def euclides(a, b): #hacemos euclides con recusividad
            if b == 0:
                return a
            return euclides(b, a % b)

        print("Generar numero p primo aleatorio de k/2 bits")
        #eleccion = int(input("Elija: ")) #quite el michi del inicio para poder una cantidad X de bits
        eleccion = 64
        #mitad = eleccion / 2 en esta parte quisimos poner k/2 bits pero no pudimos :C
        #print("Numero p aleatorio de " + str(mitad) + " bits")
        print("Valor de p: ")
        print(gen_primo(eleccion))
        print()
        #=============================================================================
        print("Generar numero q primo aleatorio de k/2 bits")
        #eleccion2 = int(input("Elija: ")) #quite el michi para poder poner una cantidad X de bits
        eleccion2 = 64
        #mitad2 = eleccion2 / 2
        #print("Numero q aleatorio de " + str(mitad2) + " bits"
        print("Valor de q: ")
        print(gen_primo(eleccion2))
        print()
        #=============================================================================
        #Calculamos n = p * q
        n = eleccion * eleccion2 #multiplicamos p y q
        print("Generamos valor de n")
        print(n)
        print()
        #Calcular la función Euler de n
        def PHI(ene):
            r = 0
            for i in range(ene):
                d = euclides(i, n) #utilizamos la función euclides anteriormente creada
                if d == 1:
                    r = r + 1
            return r
        print("Funcion Euler de n es: ")
        print(PHI(n))
        #=============================================================================
        #generamos un número entre [2 y n - 1]
        def generar_E(minimo, maximo):
            r = aleatorio.randrange(minimo, maximo) #aquí se genera un número aleatorio entre 2 y n - 1 para hallar "e"
            return r

        print("Generamos el valor de e: ")
        e = generar_E(2, n - 1)
        print(e)
        print()
        #============================================================================
        #usamos el algoritmo extendido de Euclides 
        def extendido_euclides(a, b):
            x1, x2 = 1, 0
            y1, y2 = 0, 1
            while b:
                q, r = divmod(a, b)
                x2, x1 = x1 - q * x2, x2
                y2, y1 = y1 - q * y2, y2
                a, b = b, r
            return a, x1, y1 #retorna "n", "d", "e"

        print("Hallamos d con algoritmo extendido de euclides: ")
        d = extendido_euclides(e, n)
        print(d)
        print()
