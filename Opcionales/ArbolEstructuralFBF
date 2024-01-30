from collections import deque
from sys import stdin


def transform(lst):
  while ' ' in lst:
    lst.remove(' ')
  while "-" in lst:
    lst[lst.index(">") - 1] += ">"
    lst.remove(">")
  while "<" in lst:
    lst[lst.index("<")] += "->"
    lst.remove(">")


def check(fbf : deque):
  fbf_deque = fbf
  if len(fbf_deque) == 0:
    return False
  else:
    if (fbf_deque[0] == "¬"):
      fbf_deque.popleft()
      return check(fbf_deque)
    elif (fbf_deque[0] == "(" and fbf_deque[-1] == ")"):
        cont = 0
        fbf_deque.popleft()
        fbf_deque.pop()
        fbf2 = deque()
        fbf1 = deque()
        for i in range(len(fbf_deque)):
          if fbf_deque[i] == "(":
            cont += 1
          elif fbf_deque[i]  == ")":
            cont -= 1
          elif fbf_deque[i]  == "¬" and fbf_deque[i + 1] in {'->', '<->', '^', 'v'}:
            return False
          elif fbf_deque[i] in {'->', '<->', '^', 'v'} and cont == 0:
            id = i
            tmp1 = list(fbf_deque)[:id]
            fbf1 = deque(tmp1)
            tmp2 = list(fbf_deque)[id:]
            fbf2 = deque(tmp2)
            fbf2.popleft()
            break
        return check(fbf1) and check(fbf2)

    else:
      return fbf_deque[0].isalpha()

class Nodo:
    def __init__(self, valor=None):
        self.valor = valor
        self.hijos = []

def agregar_nodo(padre, valor_nuevo):
    nuevo_nodo = Nodo(valor_nuevo)
    padre.hijos.append(nuevo_nodo)
    return nuevo_nodo

def imprimir_arbol(nodo, nivel=0):
    print("  " * nivel + str(nodo.valor))
    for hijo in nodo.hijos:
        imprimir_arbol(hijo, nivel + 1)


def arb2(fbf, raiz=None):
    fbf_deque = deque(fbf)

    if raiz is None:
        if fbf_deque[0] == "(":
           cont = 0
           i = 0
           flag = True
           while i != 0 and flag:
                if fbf_deque[i] == "(":
                    cont += 1
                elif fbf_deque[i] == ")":
                    cont -= 1
                    if cont == 1:
                       raiz = Nodo(fbf_deque[i])
                       flag = False
                i += 1
        else:
            token = fbf_deque[0]
            raiz = Nodo(token)
                
    else:
        if fbf_deque[0] == "¬":
            valor = fbf_deque.popleft()
            nodo = agregar_nodo(raiz, valor)
            arb2(fbf_deque, nodo)
            return raiz
        elif fbf_deque[0] == "(" and fbf_deque[-1] == ")":
            cont = 0
            fbf_deque.popleft()
            fbf_deque.pop()
            fbf1 = deque()
            fbf2 = deque()
            nodo1 = None
            for i in range(len(fbf_deque)):
                if fbf_deque[i] == "(":
                    cont += 1
                elif fbf_deque[i] == ")":
                    cont -= 1
                elif fbf_deque[i] in {'->', '<->', '^', 'v'} and cont == 0:
                    id = i
                    tmp1 = list(fbf_deque)[:id]
                    fbf1 = deque(tmp1)
                    tmp2 = list(fbf_deque)[id:]
                    fbf2 = deque(tmp2)
                    nodo1 = agregar_nodo(raiz, fbf_deque[i])
                    fbf2.popleft()
                    arb2(fbf1, nodo1)
                    arb2(fbf2, nodo1) 
                    break
            return raiz
        else:
            nodo = agregar_nodo(raiz, fbf_deque[0])
            return raiz

def makeab(fbf):
    fbf_deque = fbf
    raiz = Nodo()
    if fbf_deque[0] == "(":
        cont = -1
        i = 0
        flag = True
        
        while i != len(fbf_deque) and flag:
            if fbf_deque[i] == "(":
                cont += 1
            elif fbf_deque[i] == ")":
                cont -= 1
            elif fbf_deque[i] in {'->', '<->', '^', 'v'} and cont == 0:
                raiz = Nodo(fbf_deque[i])
                flag = False
            i += 1
    else:
        token = fbf_deque[0]
        raiz = Nodo(token)
    return arb2(fbf, raiz)

def main():
    """ fbf = "(¬( ¬p -> q) ^ ( q v r ))"
        fbf2 = "( p v q )"
    """
    print("Ingrese su fbf, puede utilizar cualquier letra del alfabeto\nLos conectivos son los siguientes:\n^, v, ->, <>")

    line = stdin.readline().strip()
    while line != "":
        fbf_dequeCk = deque(line)
        transform(fbf_dequeCk)
        if check(fbf_dequeCk):
            print("Arbol resultante:")
            fbf_deque = deque(line)
            transform(fbf_deque)
            raiz = makeab(fbf_deque)
            imprimir_arbol(raiz.hijos[0])
        else:
           print("La formula no es una formula bien formada")
        print("Ingrese otra fbf, o enter para salir")
        line = stdin.readline().strip()


main()
