#include <iostream>
#include <malloc.h>
using namespace std;

struct nodo {
    char nombre[100];
    int año;
    char genero[50];
    float recaudacion;
    struct nodo *izq;
    struct nodo *der;
};

struct nodo *raiz = NULL, *aux;

void posicionar(struct nodo *nuevaraiz) {
    if (aux->año <= nuevaraiz->año) {
        if (nuevaraiz->izq == NULL) {
            nuevaraiz->izq = aux;
        } else {
            posicionar(nuevaraiz->izq);
        }
    } else {
        if (nuevaraiz->der == NULL) {
            nuevaraiz->der = aux;
        } else {
            posicionar(nuevaraiz->der);
        }
    }
}

void registrar() {
    aux = (struct nodo *)malloc(sizeof(struct nodo));
    aux->izq = aux->der = NULL;

    cout << "Nombre de la película: ";
    cin.ignore();
    cin.getline(aux->nombre, 100);
    cout << "Año de realización: ";
    cin >> aux->año;
    cout << "Género: ";
    cin.ignore();
    cin.getline(aux->genero, 50);
    cout << "Dinero recaudado (en millones): ";
    cin >> aux->recaudacion;

    if (raiz == NULL) {
        raiz = aux;
    } else {
        posicionar(raiz);
    }

    aux = NULL;
}

// Recorridos
void inorden(struct nodo *r) {
    if (r != NULL) {
        inorden(r->izq);
        cout << r->nombre << " (" << r->año << ") - " << r->genero << " - $" << r->recaudacion << "M\n";
        inorden(r->der);
    }
}

void preorden(struct nodo *r) {
    if (r != NULL) {
        cout << r->nombre << " (" << r->año << ") - " << r->genero << " - $" << r->recaudacion << "M\n";
        preorden(r->izq);
        preorden(r->der);
    }
}

void posorden(struct nodo *r) {
    if (r != NULL) {
        posorden(r->izq);
        posorden(r->der);
        cout << r->nombre << " (" << r->año << ") - " << r->genero << " - $" << r->recaudacion << "M\n";
    }
}

// Buscar por nombre
void buscarPorNombre(struct nodo *r, const char nombreBuscado[100]) {
    if (r != NULL) {
        if (r->nombre, nombreBuscado)  {
            cout << "Película encontrada: " << r->nombre << " (" << r->año << ") - " << r->genero << " - $" << r->recaudacion << "M\n";
        }
        buscarPorNombre(r->izq, nombreBuscado);
        buscarPorNombre(r->der, nombreBuscado);
    }
}

// Mostrar por género
void mostrarPorGenero(struct nodo *r, const char generoBuscado[50]) {
    if (r != NULL) {
        if (r->genero, generoBuscado)  {
            cout << r->nombre << " (" << r->año << ") - $" << r->recaudacion << "M\n";
        }
        mostrarPorGenero(r->izq, generoBuscado);
        mostrarPorGenero(r->der, generoBuscado);
    }
}

// Recolectar nodos para ordenar por recaudación
struct nodo* peliculas[100];
int totalPeliculas = 0;

void recolectar(struct nodo *r) {
    if (r != NULL) {
        peliculas[totalPeliculas++] = r;
        recolectar(r->izq);
        recolectar(r->der);
    }
}

void mostrarFracasosTaquilleros() {
    recolectar(raiz);
    // Ordenamiento burbuja por recaudación ascendente
    for (int i = 0; i < totalPeliculas - 1; i++) {
        for (int j = i + 1; j < totalPeliculas; j++) {
            if (peliculas[i]->recaudacion > peliculas[j]->recaudacion) {
                struct nodo* temp = peliculas[i];
                peliculas[i] = peliculas[j];
                peliculas[j] = temp;
            }
        }
    }

    cout << "3 Fracasos Taquilleros:\n";
    for (int i = 0; i < 3 && i < totalPeliculas; i++) {
        cout << peliculas[i]->nombre << " (" << peliculas[i]->año << ") - " << peliculas[i]->genero << " - $" << peliculas[i]->recaudacion << "M\n";
    }

    totalPeliculas = 0; // Reiniciar para próximos usos
}

// Menú
int main() {
    int opcion;
    char nombreBuscado[100];
    char generoBuscado[50];

    do {
        cout << "\n1. Registrar película\n";
        cout << "2. Mostrar Inorden\n";
        cout << "3. Mostrar Preorden\n";
        cout << "4. Mostrar Posorden\n";
        cout << "5. Buscar película por nombre\n";
        cout << "6. Mostrar películas por género\n";
        cout << "7. Mostrar 3 fracasos taquilleros\n";
        cout << "8. Salir\n";
        cout << "Su opción es: ";
        cin >> opcion;

        switch (opcion) {
            case 1: registrar(); break;
            case 2: inorden(raiz); break;
            case 3: preorden(raiz); break;
            case 4: posorden(raiz); break;
            case 5:
                cout << "Nombre a buscar: ";
                cin.ignore();
                cin.getline(nombreBuscado, 100);
                buscarPorNombre(raiz, nombreBuscado);
                break;
            case 6:
                cout << "Género a mostrar: ";
                cin.ignore();
                cin.getline(generoBuscado, 50);
                mostrarPorGenero(raiz, generoBuscado);
                break;
            case 7: mostrarFracasosTaquilleros(); break;
        }
    } while (opcion != 8);
    return 0;
}
