
# Sistema de Gestión de una Liga Infantil de Fútbol

## 1. Introducción

El presente proyecto tiene como finalidad desarrollar un sistema de información para la gestión y administración de una liga infantil de fútbol, mediante una aplicación web que permita organizar y consultar la información relacionada con los equipos participantes, jugadores, categorías, partidos, sedes y árbitros.

Actualmente, la organización de una liga deportiva requiere llevar un control adecuado de los equipos inscritos, sus integrantes, las categorías correspondientes y la programación de los encuentros deportivos. Cuando esta información se administra de manera manual o mediante diferentes medios, pueden presentarse problemas como duplicidad de datos, pérdida de información y dificultades para consultar los resultados de los partidos.

Por esta razón, se propone el diseño de una base de datos que permita almacenar y relacionar toda la información necesaria para el funcionamiento de la liga. El sistema contempla el registro de categorías, equipos y jugadores, así como la administración de partidos, asignación de árbitros, sedes y alineaciones de los jugadores.

Además, se plantea una arquitectura que permita acceder al sistema mediante una aplicación web y dispositivos móviles, utilizando una base de datos para centralizar la información y contemplando mecanismos de respaldo y replicación para mantener disponibles los datos.

Con este proyecto se busca facilitar la administración de la liga infantil de fútbol, mejorar la organización de los encuentros deportivos y proporcionar un medio de consulta más eficiente, confiable y ordenado para los usuarios.

---

## 2. Casos de uso (UML)

### 2.1 Actores del sistema

- **Administrador de la liga:** Responsable de administrar la información general del sistema, registrar categorías, equipos, jugadores y programar los partidos.
- **Entrenador:** Puede consultar la información de su equipo y sus jugadores, así como revisar los partidos programados y las alineaciones.
- **Árbitro:** Participa en los encuentros deportivos asignados y proporciona la información correspondiente a los resultados.
- **Usuario / Consultor:** Puede consultar información general de la liga, como equipos participantes, partidos, fechas, sedes y resultados.

### 2.2 Descripción de los casos de uso

| Caso de uso | Descripción | Actor principal |
|---|---|---|
| Gestionar categorías | Registrar, consultar, modificar y eliminar categorías de la liga. | Administrador |
| Gestionar equipos | Registrar equipos, asignar entrenadores y relacionarlos con una categoría. | Administrador |
| Gestionar jugadores | Registrar jugadores, asignarlos a equipos y administrar sus datos. | Administrador |
| Gestionar sedes | Registrar y consultar los lugares donde se realizarán los partidos. | Administrador |
| Gestionar árbitros | Registrar y consultar los árbitros disponibles para los encuentros. | Administrador |
| Programar partidos | Establecer fecha, hora, sede, equipo local y equipo visitante. | Administrador |
| Registrar alineaciones | Seleccionar los jugadores que participarán en un partido e indicar si son titulares. | Administrador / Entrenador |
| Registrar resultados | Registrar los goles del equipo local y visitante. | Administrador |
| Consultar partidos | Visualizar los encuentros programados y sus resultados. | Todos los usuarios |
| Consultar equipos y jugadores | Consultar los equipos participantes y sus integrantes. | Todos los usuarios |

### 2.3 Diagrama de casos de uso

```mermaid
flowchart TB
    A["Administrador de la liga"]
    E["Entrenador"]
    R["Árbitro"]
    U["Usuario / Consultor"]

    subgraph S["Sistema de Gestión de Liga Infantil de Fútbol"]
        UC1(["Gestionar categorías"])
        UC2(["Gestionar equipos"])
        UC3(["Gestionar jugadores"])
        UC4(["Gestionar sedes"])
        UC5(["Gestionar árbitros"])
        UC6(["Programar partidos"])
        UC7(["Registrar alineaciones"])
        UC8(["Registrar resultados"])
        UC9(["Consultar partidos"])
        UC10(["Consultar equipos y jugadores"])
    end

    A --- UC1
    A --- UC2
    A --- UC3
    A --- UC4
    A --- UC5
    A --- UC6
    A --- UC7
    A --- UC8

    E --- UC9
    E --- UC10
    E --- UC7

    R --- UC8

    U --- UC9
    U --- UC10
```

---

## 3. Modelo entidad-relación

### 3.1 Descripción de las entidades

El modelo entidad-relación está compuesto por siete entidades principales que permiten representar la información necesaria para administrar la liga infantil de fútbol.

**1. Categoría**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_categoria | INT | PK |
| nombre | VARCHAR | |
| edades | VARCHAR | |

**2. Equipo**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_equipo | INT | PK |
| nombre | VARCHAR | |
| tecnico | VARCHAR | |
| categoria_id | INT | FK |

**3. Jugadores**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_jugador | INT | PK |
| nombre | VARCHAR | |
| edad | INT | |
| numero | INT | |
| posicion | VARCHAR | |
| id_equipo | INT | FK |

**4. Sede**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_sede | INT | PK |
| nombre | VARCHAR | |
| direccion | VARCHAR | |

**5. Árbitro**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_arbitro | INT | PK |
| nombre | VARCHAR | |
| licencia | VARCHAR | |

**6. Alineación**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_alineacion | INT | PK |
| id_partido | INT | FK |
| id_jugador | INT | FK |
| titular | BOOLEAN | |

**7. Partido**

| Atributo | Tipo de dato | Clave |
|---|---|---|
| id_partido | INT | PK |
| fecha | DATE | |
| hora | TIME | |
| id_sede | INT | FK |
| gol_visitante | INT | |
| id_arbitro | INT | FK |
| gol_local | INT | |
| equipo_local | INT | FK |
| equipo_visitante | INT | FK |

### 3.2 Diagrama entidad-relación

```mermaid
erDiagram

    CATEGORIA ||--o{ EQUIPO : clasifica
    EQUIPO ||--o{ JUGADORES : integra

    EQUIPO ||--o{ PARTIDO : "juega como local"
    EQUIPO ||--o{ PARTIDO : "juega como visitante"

    SEDE ||--o{ PARTIDO : alberga
    ARBITRO ||--o{ PARTIDO : dirige

    PARTIDO ||--o{ ALINEACION : contiene
    JUGADORES ||--o{ ALINEACION : participa

    CATEGORIA {
        int id_categoria PK
        string nombre
        string edades
    }

    EQUIPO {
        int id_equipo PK
        string nombre
        string tecnico
        int categoria_id FK
    }

    JUGADORES {
        int id_jugador PK
        string nombre
        int edad
        int numero
        string posicion
        int id_equipo FK
    }

    SEDE {
        int id_sede PK
        string nombre
        string direccion
    }

    ARBITRO {
        int id_arbitro PK
        string nombre
        string licencia
    }

    ALINEACION {
        int id_alineacion PK
        int id_partido FK
        int id_jugador FK
        boolean titular
    }

    PARTIDO {
        int id_partido PK
        date fecha
        time hora
        int id_sede FK
        int gol_visitante
        int id_arbitro FK
        int gol_local
        int equipo_local FK
        int equipo_visitante FK
    }
```

### 3.3 Descripción de las relaciones

- **Categoría - Equipo (1:N):** Una categoría puede tener varios equipos registrados, pero cada equipo pertenece a una sola categoría.
- **Equipo - Jugadores (1:N):** Un equipo puede estar integrado por varios jugadores, pero cada jugador pertenece a un equipo.
- **Equipo - Partido como local (1:N):** Un equipo puede participar en diferentes partidos como equipo local.
- **Equipo - Partido como visitante (1:N):** Un equipo puede participar en diferentes partidos como equipo visitante.
- **Sede - Partido (1:N):** Una sede puede albergar diferentes partidos, pero cada partido se realiza en una sola sede.
- **Árbitro - Partido (1:N):** Un árbitro puede dirigir varios partidos, pero cada partido tiene asignado un árbitro.
- **Partido - Alineación (1:N):** Un partido puede tener varias alineaciones correspondientes a los jugadores participantes.
- **Jugador - Alineación (1:N):** Un jugador puede aparecer en diferentes alineaciones a lo largo de distintos partidos.
