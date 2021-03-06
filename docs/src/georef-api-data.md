# Modelo de datos para Georef API

Los archivos de datos de Georef consisten de seis (6) archivos en formato JSON, los cuales contienen provincias, departamentos, municipios, localidades, calles e intersecciones de calles.

## ETL
Los datos utilizados por Georef API son obtenidos a través de un proceso de ETL. El código del mismo se encuentra en el repositorio GitHub [georef-ar-etl](https://github.com/datosgobar/georef-ar-etl), así también como su [guía de instalación y uso](https://github.com/datosgobar/georef-ar-etl/blob/master/docs/install.md).

## Fuentes
Los orígenes de los datos procesados en el ETL son:

### Unidades Territoriales
- Recursos: `/provincias`, `/departamentos`, `/municipios`, `/ubicacion`
- Fuente: **Instituto Geográfico Nacional (IGN)**
- Enlace: [Datos Abiertos - Unidades Territoriales](http://datos.gob.ar/dataset/ign-unidades-territoriales)

### BAHRA
- Recursos: `/localidades`
- Fuente: **Base de Asentamientos Humanos de la República Argentina (BAHRA)**
- Enlace: [BAHRA - Descargas](http://www.bahra.gob.ar/)

### Vías de Circulación
- Recursos: `/calles`, `/direcciones`
- Fuente: **Instituto Nacional de Estadística y Censos de la República Argentina (INDEC)**
- Enlace: [Portal de geoservicios de INDEC](https://geoservicios.indec.gov.ar/nomenclador-vias-de-circulacion/?contenido=descargas)

## Archivos
A continuación se detallan, a través de ejemplos, los esquemas de los archivos para las entidades utilizadas. Notar que el campo `version` se utiliza al momento de indexar para determinar si los datos son compatibles con la versión de la API siendo utilizada; la versión detallada en este documento es la `9.0.0`.

### Provincias
El archivo de datos de provincias debe tener formato JSON. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de entidades
		{
			"id": "90", // ID de provincia
			"nombre": "Tucumán", // Nombre de provincia,
			"nombre_completo": "Provincia de Tucumán", // Nombre completo
			"iso_id": "AR-T", // Identificador ISO 3166-2
			"iso_nombre": "Tucumán", // Nombre ISO
			"categoria": "Provincia", // Tipo de entidad
			"centroide": {
				"lat": -26.9478, // Latitud de centroide
				"lon": -65.36475 // Longitud de centroide
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "MultiPolygon",
				"coordinates": [[[[-58.4549, -34.5351], [-58.4545, -34.5353], ...]]]
			},
			"fuente": "IGN" // Fuente del dato
		},
		{ ... },
	]
}
```

### Departamentos
El archivo de datos de departamentos debe tener formato JSON. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de entidades
		{
			"id": "06427", // ID del departamento
			"nombre": "La Matanza", // Nombre del departamento
			"nombre_completo": "Partido de la Matanza", // Nombre completo
			"categoria": "Partido", // Tipo de entidad
			"centroide": {
				"lat": -34.770165, // Latitud de centroide
				"lon": -58.625449  // Longitud de centroide
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "MultiPolygon",
				"coordinates": [[[[-58.4549, -34.5351], [-58.4545, -34.5353], ...]]]
			},
			"provincia": { // Provincia que contiene al departamento
				"id": "06",
				"nombre": "Buenos Aires",
				"interseccion": "0.0412936" // Porcentaje del área de la provincia que ocupa el depto.
			},
			"fuente": "ARBA - Gerencia de Servicios Catastrales" // Fuente del dato
		},
		{ ... },
	]
}
```

### Municipios
El archivo de datos de municipios debe tener formato JSON. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de entidades
		{
			"id": "060105", // ID del municipio
			"nombre": "Bolívar", // Nombre del municipio
			"nombre_completo": "Municipio Bolívar", // Nombre completo
			"categoria": "Municipio", // Tipo de entidad
			"centroide": {
				"lat": -36.298222, // Latitud de centroide
				"lon": -61.149648  // Longitud de centroide
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "MultiPolygon",
				"coordinates": [[[[-58.4453, -34.4324], [-58.6463, -34.6841], ...]]]
			},
			"provincia": {  // Provincia que contiene al municipio
				"id": "06",
				"nombre": "Buenos Aires",
				"interseccion": "0.0100845" // Porcentaje del área de la provincia que ocupa el municipio
			},
			"fuente": "ARBA - Gerencia de Servicios Catastrales" // Fuente del dato
		},
		{ ... },
	]
}
```

### Localidades
El archivo de datos de localidades debe tener formato JSON. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de entidades
		{
			"id": "06189080000", // ID de la localidad
			"nombre": "San Roman", // Nombre de la localidad
			"categoria": "Localidad simple (LS)", // Tipo de asentamiento BAHRA
			"centroide": {
				"lat": -38.741555, // Latitud de centroide
				"lon": -61.537720  // Longitud de centroide
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "MultiPoint",
				"coordinates": [[-61.5377, -38.7415], ...]
			},
			"municipio": { // Municipio que contiene a la localidad
				"id": "060189", // Puede ser nulo
				"nombre": "Coronel Dorrego" // Puede ser nulo
			},
			"departamento": { // Departamento que contiene a la localidad
				"id": "06189",
				"nombre": "Coronel Dorrego"
			},
			"provincia": {  // Provincia que contiene a la localidad
				"id": "06",
				"nombre": "Buenos Aires"
			},
			"fuente": "INDEC" // Fuente del dato
		},
		{ ... },
	]
}
```

### Calles
El archivo de datos de calles debe tener formato JSON. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de vías de circulación
		{
			"nomenclatura": "LARREA, Comuna 3, Ciudad Autónoma de Buenos Aires", // Nomenclatura: 'nombre, departamento, provincia'
			"id": "0202101007345", // ID de la vía de circulación
			"nombre": "LARREA", // Nombre de vía de circulación
			"categoria": "CALLE", // Tipo de vía de circulación
			"altura": {
				"inicio": {
					"derecha": 1, // Número inicial de altura (lado derecho)
					"izquierda": 2, // Número inicial de altura (lado izquierdo)
				},
				"fin": {
					"derecha": 799, // Número final de altura (lado derecho)
					"izquierda": 800, // Número final de altura (lado izquierdo)
				}
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "MultiLineString",
				"coordinates": [[[-58.52815846522327, -34.611800397637424], ...]]
			},
			"departamento": { // Departamento
				"nombre": "Comuna 3",
				"id": "02021"
			},
			"provincia": { // Provincia
				"nombre": "Ciudad Autónoma de Buenos Aires",
				"id": "02"
			},
			"fuente": "INDEC" // Fuente del dato
		},
		{ ... },
	]
}
```

### Intersecciones de Calles
El archivo de datos de intersecciones debe tener formato JSON, y no debe tener intersecciones repetidas. Dadas las calles con ID X y ID Z, solo debe estar presente la intersección X-Z o Z-X. Su esquema de datos debe ser el siguiente:
```
{
	"timestamp": "1532435389", // Timestamp de creación
	"fecha_creacion": "2018-07-24 12:29:49.813835+00:00", // Fecha de creación
	"version": "9.0.0", // Versión de archivo
	"datos": [ // Lista de intersecciones
		{
			"id": "0207001002300-0207001007975", // ID de la calle A, ID de la calle B
			"calle_a": {
				"id": "0207001002300", // ID de la calle A
				"nombre": "BOSTON", // Nombre de la calle A
				"departamento": { // Departamento de la calle A
					"id": "02070",
					"nombre": "Comuna 10"
				},
				"provincia": { // Provincia de la calle A
					"id": "02",
					"nombre": "Ciudad Autónoma de Buenos Aires"
				},
				"categoria": "CALLE", // Tipo de la calle A
				"fuente": "INDEC" // Fuente del dato
			},
			"calle_b": {
				"id": "0207001007975", // ID de la calle B
				"nombre": "MARCOS SASTRE", // Nombre de la calle B
				"departamento": { // Departamento de la calle B
					"id": "02070",
					"nombre": "Comuna 10"
				},
				"provincia": { // Provincia de la calle B
					"id": "02",
					"nombre": "Ciudad Autónoma de Buenos Aires"
				},
				"categoria": "CALLE", // Tipo de la calle B
				"fuente": "INDEC" // Fuente del dato
			},
			"geometria": { // Geometría en formato GeoJSON
				"type": "Point",
				"coordinates": [
					-58.5077676091915,
					-34.6150993860767
				]
			}
		}
		{ ... },
	]
}
```
