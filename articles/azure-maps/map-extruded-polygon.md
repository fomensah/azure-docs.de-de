---
title: Hinzufügen einer Polygonextrusionsebene zu einer Karte | Microsoft Azure Maps
description: Erfahren Sie, wie Sie dem Microsoft Azure Maps Web SDK eine Polygonextrusionsebene hinzufügen.
author: philmea
ms.author: philmea
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7405098bd4924333aafcd1c285eb2f37bb1d4f75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334542"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Hinzufügen einer Polygonextrusionsebene zur Karte

In diesem Artikel erfahren Sie, wie Sie mithilfe der Polygonextrusionsebene Bereiche von `Polygon`- und `MultiPolygon`-Featuregeometrien als extrudierte Formen darstellen. Das Azure Maps-Web-SDK unterstützt das Rendering von Kreisgeometrien gemäß der Definition im [erweiterten GeoJSON-Schema](extend-geojson.md#circle). Diese Kreise können beim Rendern auf der Karte in Polygone umgewandelt werden. Alle Featuregeometrien können ganz einfach aktualisiert werden, wenn sie in der Klasse [atlas.Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) umschlossen sind.

## <a name="use-a-polygon-extrusion-layer"></a>Verwenden einer Polygonextrusionsebene

Verbinden Sie die [Polygonextrusionsebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) mit einer Datenquelle. Laden Sie sie anschließend auf die Karte. Die Polygonextrusionsebene stellt die Bereiche eines `Polygon`- und `MultiPolygon`-Features als extrudierte Formen dar. Die Eigenschaften `height` und `base` der Polygonextrusionsebene definieren den Basisabstand vom Boden und die Höhe der extrudierten Form in **Metern**. Der folgende Code zeigt, wie Sie ein Polygon erstellen, dieses einer Datenquelle hinzufügen und dann mit einer Klasse für die Polygonextrusionsebene rendern.

> [!Note]
> Der in der Polygonextrusionsebene definierte Wert `base` sollte kleiner oder gleich dem Wert von `height` sein.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Extrudiertes Polygon" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>Extruded polygon</a> (Extrudiertes Polygon) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-polygons"></a>Hinzufügen von datengesteuerten Polygonen

Eine Choroplethenkarte kann mithilfe der Polygonextrusionsebene gerendert werden. Legen Sie die Eigenschaften `height` und `fillColor` der Extrusionsebene auf die Messung der statistischen Variablen in den Featuregeometrien `Polygon` und `MultiPolygon` fest. Das folgende Codebeispiel zeigt eine extrudierte Choroplethenkarte der USA basierend auf der Messung der Bevölkerungsdichte nach Bundesstaat.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Extrudierte Choroplethenkarte" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/eYYYNox'>Extruded choropleth map</a> (Extrudierte Choroplethenkarte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Hinzufügen eines Kreises zur Karte

Azure Maps verwendet eine erweiterte Version des GeoJSON-Schemas, das eine Definition für Kreise bereitstellt, wie [hier](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle) beschrieben. Ein extrudierter Kreis kann auf der Karte gerendert werden, indem ein `point`-Feature erstellt wird, das eine `subType`-Eigenschaft dem Wert `Circle` und eine `Radius`-Eigenschaft mit einer Zahl, die den Radius in **Metern** darstellt, aufweist. Beispiel:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Das Azure Maps-Web-SDK wandelt diese `Point`-Features im Hintergrund in `Polygon`-Features um. Diese `Point`-Features können auf der Karte auf der Polygonextrusionsebene gerendert werden, wie im folgenden Codebeispiel gezeigt.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drohnenluftraum-Polygon" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>Drone airspace polygon</a> (Drohnenluftraum-Polygon) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Anpassen einer Polygonextrusionsebene

Die Polygonextrusionsebene hat verschiedene Formatierungsoptionen. Mit dem folgenden Tool können Sie diese ausprobieren.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Polygonextrusionsebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Zusätzliche Ressourcen:

> [!div class="nextstepaction"]
> [Azure Maps-Erweiterung der Maps GeoJSON-Spezifikation](extend-geojson.md#circle)
