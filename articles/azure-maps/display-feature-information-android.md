---
title: Visualización de información de características en los mapas de Android | Microsoft Azure Maps
description: Aprenda a mostrar información cuando los usuarios interactúan con las características de mapas. Use Android SDK de Azure Maps para mostrar las notificaciones del sistema y otros tipos de mensajes.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 37b5ab1c144ed81d995da40b87edeaccdcad7253
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680006"
---
# <a name="display-feature-information"></a>Mostrar información de características

Los datos espaciales a menudo se representan mediante puntos, líneas y polígonos. Estos datos con frecuencia tienen información de metadatos asociada. Por ejemplo, un punto puede representar la ubicación de un restaurante y los metadatos de ese restaurante pueden ser su nombre, dirección y el tipo de comida que sirve. Estos metadatos se pueden agregar como propiedades de un elemento `Feature` GeoJSON. El siguiente código crea una característica de punto simple con una propiedad `title` cuyo valor es "Hello world!"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Create a point feature, pass in the metadata properties, and add it to the data source.
source.add(feature);
```

Consulte [Creación de un origen de datos](create-data-source-android-sdk.md) para obtener información sobre cómo crear y agregar datos al mapa.

Cuando un usuario interactúa con cualquier característica del mapa, se pueden usar eventos para reaccionar ante esas acciones. Un escenario común es mostrar un mensaje creado con las propiedades de los metadatos de una característica con la que el usuario ha interactuado. `OnFeatureClick` es el evento principal que se usa para detectar cuándo el usuario ha pulsado una característica en el mapa. También hay un evento `OnLongFeatureClick`. Al agregar el evento `OnFeatureClick` al mapa, se puede limitar a una sola capa. Para ello, solo hay que pasar el identificador de una capa. Si no se pasa ningún identificador de capa, al pulsar cualquiera de las características del mapa se activaría este evento, independientemente de la capa en la que se encuentre. El código siguiente crea una capa de símbolos que representa datos de puntos en el mapa y, después, agrega un evento `OnFeatureClick` y lo limita a esta capa de símbolos.

```java
//Create a symbol and add it to the map.
SymbolLayer layer = new SymbolLayer(source);
map.layers.add(layer);

//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Do something with the message.
}, layer.getId());    //Limit this event to the symbol layer.
```

## <a name="display-a-toast-message"></a>Presentación de mensajes de notificación del sistema

Un mensaje de notificación del sistema es una de las formas más sencillas de mostrar información al usuario y está disponible en todas las versiones de Android. No admite ningún tipo de entrada del usuario y solo se muestra durante un período breve. Si desea que el usuario sepa algo acerca del elemento que ha pulsado, un mensaje de notificación del sistema puede ser una buena opción. En el código siguiente se muestra cómo se puede utilizar un mensaje de notificación del sistema con el evento `OnFeatureClick`.

```java
//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Display a toast message with the title information.
    Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
}, layer.getId());    //Limit this event to the symbol layer.
```

![Animación de una característica que se pulsa y se muestra un mensaje de notificación del sistema](./media/display-feature-information-android/symbol-layer-click-toast-message.gif)

Además de los mensajes de notificación del sistema, hay muchas otras maneras de presentar las propiedades de los metadatos de una característica, como:

- [Widget Snackbar](https://developer.android.com/training/snackbar/showing.html): los `Snackbars` proporcionan pequeños comentarios acerca de una operación. Muestran un mensaje corto en la parte inferior de la pantalla del dispositivo móvil y en la parte inferior izquierda en dispositivos mayores. Los `Snackbars` aparecen encima de los demás elementos de la pantalla y no pueden mostrarse varios a la vez.
- [Cuadros de diálogo](https://developer.android.com/guide/topics/ui/dialogs): un cuadro de diálogo es una ventana pequeña que solicita al usuario que tome una decisión o escriba más información. Un cuadro de diálogo no llena la pantalla y normalmente se usa para eventos modales que requieren que los usuarios realicen una acción para poder continuar.
- Agregue un [fragmento](https://developer.android.com/guide/components/fragments) a la actividad actual.
- Vaya a otra actividad o vista.

## <a name="next-steps"></a>Pasos siguientes

Para agregar más datos al mapa:

> [!div class="nextstepaction"]
> [Reacción a eventos de mapa](android-map-events.md)

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
