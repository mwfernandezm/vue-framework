# integrantes del grupo

Los integrantes que participaron en el desarrollo de la app de inventarios para la empresa fictcia F&Fson:

Fernandez Mamani Marin Wenceslao

Flores Medrano Victor Hugo

Gonzales Heredia Alejandro

Lavayen Pinto Gabriel Alfredo

# inventarios

La app de inventarios tiene 2 tablas y fue desarrollado con el framework VUE

La tabla AREAS tiene los siguientes campos:

"id": 1,
"nombre": "finanzas",
"encargado": "Juan Linares Pozo",
"cantFuncionario": 4,
"active": true

La tabla ACTIVOS tiene los siguientes campos:

"id": 1,
"tipo": "computadoras",
"marca": "HP",
"modelo": "pavillon 360",
"estado": "nuevo",
"areaId": 1

La app inventarios permite agregar, quitar, editar y mostrar las AREAS de una empresa aplicando busquedas y filtros.
La app inventarios permite agregar, quitar, editar y mostrar los ACTIVOS que cada AREA aplicando busquedas y filtros.
con una busqueda y filtros

La app inventarios corre sobre el puerto 8080

\inventarios\npm run serve
http://localhost:8080/

La base de datos esta corriendo sobre el puerto 3000

\inventarios\backend> json-server --watch db.json
Resources
http://localhost:3000/areas
http://localhost:3000/activos
Home
http://localhost:3000

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
