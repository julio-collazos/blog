---
title: 🗺️GeoJSON y la regulación en productos libres de deforestación (EUDR)
published: 2024-10-15
updated: 2025-10-05
description: 'Entender las especificaciones técnicas de la EUDR en cuanto a información geospacial.'
image: '../../assets/images/eudr_publication.png'
tags: [EUDR, GeoJSON, Data Structure, Deforestation]
category: 'Blog'
lang: es
draft: false 
---


> **EUDR GeoJSON documento descriptivo versión 1.5:** <a href='https://circabc.europa.eu/ui/group/34861680-e799-4d7c-bbad-da83c45da458/library/adebc1da-4e60-4479-83dd-ee344607c6f9/details'>Fuente</a>

El 29 de junio de 2023, la **Regulación en Productos Libres de Deforestación (EUDR)** entró en vigor en la Unión Europea (UE). Esta norma afecta a empresas que importan o comercializan productos como ganado 🐄, cacao 🍫, café ☕, palma de aceite 🌴, caucho 🌳, soja 🌱 y madera 🪵, o derivados como cuero, chocolate o muebles. Su objetivo es garantizar que estos productos no provengan de áreas deforestadas o degradadas después del **31 de diciembre de 2020**.

Para cumplir con la EUDR, las empresas deben enviar información geográfica precisa sobre los lugares de producción en un formato llamado **GeoJSON**. Si no estás familiarizado con sistemas de información geográfica (GIS), no te preocupes: esta guía te explicará todo paso a paso.

Para más información oficial, consulta: [Regulación EUDR](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX:32023R1115).

---

## 1. ¿Qué es un archivo GeoJSON? 🌍📝

Piensa en un archivo **GeoJSON** como una forma de dibujar en un mapa digital usando texto. Es una manera sencilla de describir ubicaciones (como un punto donde está una finca) o formas (como el contorno de un predio) para que una computadora pueda entenderlas y mostrarlas.

GeoJSON está basado en **JSON**, un formato de texto común que organiza información de forma estructurada. No necesitas ser un experto en tecnología para usarlo: puedes abrirlo y editarlo con cualquier editor de texto, como el Bloc de Notas, aunque normalmente se usa en aplicaciones de mapas.

### 1.1. ¿Qué incluye un archivo GeoJSON?

Un archivo GeoJSON tiene tres partes principales:

1. **Geometrías**: La forma o ubicación que quieres describir:
   - **Punto (Point)**: Una sola ubicación, como un marcador en Google Maps.
   - **Polígono (Polygon)**: Una forma cerrada, como el borde de un terreno.
   - **Líneas (LineString)**: Una forma abierta, que normalmente representa elementos lineales como vías, drenajes, entre otros elementos.
   - También existen colecciones como **MultiPoint** (varios puntos) y **MultiPolygon** (varios polígonos).

2. **Propiedades**: Datos extra sobre la geometría, como el nombre del lugar o del productor.

3. **Coordenadas**: Usan latitud y longitud en grados decimales (sistema WGS84), igual que en tu celular o GPS.

### 1.2. Ejemplo básico de GeoJSON

Para hacerte a una idea de como se representa la información geográfica dentro de un mapa, a continuación se muestra un mapa interactivo con las coordenadas de 2 ciudades en Indonesia (para ver el código del mapa puedes ir al siguiente enlace en [Observable](https://observablehq.com/d/43d64c5d5e261afd)).

<iframe width="100%" height="576" frameborder="0"
  src="https://observablehq.com/embed/43d64c5d5e261afd?cells=viewof+map"></iframe>

Ya que has explorado un poco como interactuan los datos geográficos dentro de un mapa, ahora trata de imaginar que quieres marcar la ubicación una finca o un lote en un mapa. Ese tipo de representación tendría la siguiente estructura en formato GeoJSON:

```json
{
  "type": "Feature",
  "properties": {
    "nombre": "Finca Ejemplo"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [-3.703790, 40.416775]  // Longitud y latitud (ejemplo: Madrid)
  }
}
```

- **"type": "Feature"**: Dice que estamos describiendo algo en el mapa.
- **"properties"**: Aquí va información adicional, como el nombre.
- **"geometry"**: Indica que es un punto y da sus coordenadas.

Si quieres delimitar un lote como un polígono, se vería así:

```json
{
  "type": "Feature",
  "properties": {
    "nombre": "Campo Ejemplo"
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [-49.004616, -22.734322],
        [-49.004675, -22.734318],
        [-49.006830, -22.735260],
        [-49.004616, -22.734322]  // El primero y último punto son iguales para cerrar
      ]
    ]
  }
}
```

---
## 2. ¿Qué pide la EUDR en términos de información geográfica? 🌳⚠️

La EUDR exige que las empresas demuestren que sus productos no vienen de áreas deforestadas o degradadas desde el 31 de diciembre de 2020. Para eso, necesitan enviar la **ubicación exacta** de donde se produjeron las materias primas, usando archivos GeoJSON.

### 2.1. Requisitos clave

- **Ubicaciones precisas**: Debes dar las coordenadas de los lugares de producción (fincas, lotes, etc.).
- **Formato GeoJSON**: Es el único formato aceptado por el Sistema de Información EUDR hasta el momento.
- **Verificación**: Las entidades competentes usarán esta información para comprobar si hubo deforestación o degradación en esas áreas.

### 2.2. Términos importantes

- **Deforestación**: Cuando un bosque se convierte en terreno agrícola (por acción humana o natural).
- **Degradación forestal**: Cambios en la estructura del bosque, como pasar de bosque natural a plantación.
- **Bosque (según la FAO)**: Área de al menos 0.5 hectáreas, con árboles de 5 metros o más y una cobertura del 10% o superior.

---
## 3. Requisitos técnicos de GeoJSON para la EUDR 📝🏞️

Para que tu archivo GeoJSON sea válido en la EUDR, debe seguir estas reglas:

1. **Sistema de coordenadas**: Usa **WGS84 (EPSG:4326)**, con latitud y longitud en grados decimales.
   - Ejemplo: [-49.004616, -22.734322].

2. **Tipos de geometrías permitidas**:
   - **Point**: Para un solo lugar (como una finca pequeña).
   - **MultiPoint**: Varios puntos juntos.
   - **Polygon**: Para áreas delimitadas (como un campo).
   - **MultiPolygon**: Varias áreas separadas.
   - **Nota**: Las líneas (**LineString**) no están permitidas.

3. **Propiedades opcionales** (pero recomendadas):
   - `"ProducerName"`: Nombre del productor.
   - `"ProducerCountry"`: Código del país (ejemplo: "BR" para Brasil).
   - `"ProductionPlace"`: Nombre del lugar de producción.
   - `"Area"`: Tamaño en hectáreas (solo para puntos; si no lo pones, se asumen 4 ha).

4. **Reglas importantes**:
   - Los polígonos deben estar **cerrados** (el primer y último punto iguales).
   - No se permiten polígonos con agujeros ni líneas que se crucen.
   - Las coordenadas deben ser únicas (evita redondear demasiado).

## 4. Ejemplo práctico para la EUDR

Aquí tienes un archivo GeoJSON para declarar un campo en Brasil:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "ProductionPlace": "Finca Brasil",
        "ProducerCountry": "BR",
        "ProducerName": "Juan Pérez",
        "Area": 10.5
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [-49.004616, -22.734322],
            [-49.004675, -22.734318],
            [-49.006830, -22.735260],
            [-49.004616, -22.734322]
          ]
        ]
      }
    }
  ]
}
```

- **"FeatureCollection"**: Permite incluir varias geometrías (aunque aquí solo hay una).
- **"Polygon"**: Define el contorno del campo con cuatro puntos.

Como ejemplo ilustrativo se muestra un mapa interactivo con polígonos de producción de Palma de Aceite en Colombia siguiendo los requerimientos minimos de la EUDR. 

<iframe width="100%" height="576" frameborder="0"
  src="https://observablehq.com/embed/b7f63b57427a202f?cells=viewof+map"></iframe>

> En este ejemplo los campos son alterados al momento de visualizarlos pero la estructura del archivo GeoJSON es fiel a los requerimientos técnicos establecidos, para más información vista el código del ejemplo construido en [Observable](https://observablehq.com/d/b7f63b57427a202f)

---
## 5. Tipos de archivos GeoJSON para la EUDR

Hay dos formas de enviar datos GeoJSON según cómo organices la información:

1. **Nivel de Productor (Tipo I)**:
   - Para un solo productor con una o más geometrías.
   - Útil si envías datos por API o la interfaz de usuario (UI).

2. **Nivel de Mercancía (Tipo II)**:
   - Para varios productores en un solo archivo.
   - Solo se usa en la UI y agrupa datos por `"ProducerName"` y `"ProducerCountry"`.

**Consejo**: Siempre incluye `"ProductionPlace"` para que sea más fácil identificar cada ubicación.

---
## 6. Errores comunes y cómo evitarlos 🚫

Estos son problemas típicos y cómo solucionarlos:

1. **Polígonos no cerrados**:
   - **Problema**: El primer y último punto no son iguales.
   - **Solución**: Revisa que la lista de coordenadas termine donde empezó.

2. **Coordenadas duplicadas**:
   - **Problema**: Al limitarse a 6 decimales, dos coordenadas muy cercanas pueden redondearse y quedar idénticas.
   - **Solución**: Ajusta los vértices para que no queden demasiado juntos.

3. **Líneas cruzadas o formas inválidas**:
   - **Problema**: Polígonos con intersecciones o formas imposibles (ej. figura en ocho).
   - **Solución**: Simplifica la geometría o divídela en polígonos separados.

4. **Polígonos con agujeros internos**:
   - **Problema**: El sistema **no reconoce agujeros dentro de un polígono**.
   - **Solución**: Representa los huecos creando polígonos separados que rodeen la zona.

5. **Exceso de detalle en líneas rectas**:
   - **Problema**: Añadir demasiados puntos a segmentos que en realidad son rectos genera archivos muy pesados.
   - **Solución**: Usa solo los vértices necesarios (dos puntos son suficientes para una línea recta).

6. **Archivos demasiado grandes**:
   - **Problema**: El sistema no acepta archivos GeoJSON mayores a 25 MB.
   - **Solución**:
     - Simplifica los polígonos reduciendo vértices innecesarios.
     - Usa **puntos (Point)** en lugar de polígonos para parcelas menores a 4 hectáreas.
     - Divide la información en varios archivos DDS y luego fusiónalos en el sistema.

7. **Errores en propiedades**:
   - **Problema**: Uso de nombres mal escritos o en minúscula (`"productionplace"` en vez de `"ProductionPlace"`) o valores mal tipados (`"Area": "3"` en lugar de `"Area": 3`).
   - **Solución**: Revisa bien mayúsculas/minúsculas y asegúrate de que los números no estén entre comillas.

8. **Problemas de formato y validez**:
   - **Problema**: Archivos protegidos con contraseña, en otro formato (PDF, TXT), o con coordenadas fuera de rango válido (latitud -90/90, longitud -180/180).
   - **Solución**: Asegúrate de que tu archivo sea un `.geojson` válido en EPSG:4326 y revisa las coordenadas antes de subirlo.

**Truco**: Antes de enviar, prueba tu archivo en [GeoJSON.io](https://geojson.io/) o en cualquier visor GIS. Si no se dibuja correctamente allí, tampoco funcionará en el sistema de la EUDR.

---
## 7. Recursos para aprender más 📚🛠️

- [Especificación GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946)
- [Documentación EUDR](https://ec.europa.eu/environment/forests/deforestation-due-diligence-registry_en)
- [Guía de coordenadas](https://medium.com/@jccollazosave/geographic-or-projected-b367ace046e1)

---
## 8. Conclusión

Crear un archivo GeoJSON para la EUDR no es tan complicado como parece. Con esta guía, puedes entender qué es GeoJSON, qué se espera bajo la regulación y cómo preparar tus datos geográficos, incluso si no tienes experiencia en GIS. La clave está en ser preciso y seguir las indicaciones técnicas. ¡Anímate a probarlo y cumple con la EUDR sin estrés!
