# Network Intrusion Detection_RF

## Detección de intrusiones en la red con Random Forest

En este proyecto trabajé en crear un modelo que pueda diferenciar entre tráfico normal y tráfico sospechoso o malicioso en una red. Para ello usé técnicas de machine learning aplicadas a datos reales de tráfico, con el objetivo de entender qué patrones del tráfico indican posibles ataques. Para hacer esto trabajé con el dataset **UNSW-NB15**.  

El objetivo principal era entrenar un **Random Forest** y al mismo tiempo entender qué variables del tráfico resultan realmente importantes para detectar comportamientos sospechosos.

---

## Sobre el dataset

**Fuente:** UNSW-NB15  

- Contenido original: 49 columnas con información habitual del tráfico de red, como puertos, cantidad de bytes enviados, protocolos, servicios utilizados, flags, entre otros.  
- Tras convertir variables categóricas a números, el dataset creció a **204 columnas**.

---

## Preprocesamiento realizado

- Conversión de `attack_cat` → `attack_cat_num`: como la variable describía el tipo de ataque en texto, tuve que transformarla en números para que el modelo lo pudiera entender.  

- Eliminación de IPs (`srcip`, `dstip`): las direcciones IP no aportaban valor real al modelo; aparte, causaban problemas al escalar datos, así que pensé que lo mejor sería eliminar esas dos columnas.  

- Después me di cuenta de que la variable `attack_cat_num` estaba demasiado relacionada con el target (label) y eso hacía que el modelo aprendiera demasiado fácil, así que quitarla permitió una evaluación más honesta.  

Para entrenar el modelo utilicé **Random Forest**, que es bastante fiable para este tipo de problemas de clasificación. Antes de entrenarlo normalicé los datos usando **StandardScaler** y dividí el dataset en un **80% para entrenamiento** y un **20% para pruebas**.

---

## Resultados

El modelo consiguió un **accuracy del 0.9968**, lo cual es demasiado alto.  

Según la matriz de confusión:  

- Detectó correctamente **50,672 conexiones normales**.  
- Detectó correctamente **4,307 ataques**, y cometió muy pocos errores en general.  

### Data Leakage

Al principio, el modelo me estaba dando un accuracy de **1.0**, algo que me pareció demasiado bueno para ser real. Después de investigar, descubrí que había **data leakage**, es decir, variables que filtraban información directa sobre la etiqueta. Para corregirlo eliminé esas columnas que revelaban el label, y como resultado el desempeño del modelo bajó a **0.9968**, un valor mucho más creíble y honesto.

### IP como features

Las IPs no solo aportaban información útil, sino que complicaban el preprocesamiento; quitarlas mejoró el modelo.

### Accuracy alto no significa un modelo perfecto

Tener casi un 100% en un entorno controlado no significa que en un entorno real funcionaría.

---

## Conclusión del Proyecto

El Random Forest permitió identificar qué características del tráfico son más importantes para el modelo. Gracias a esto es posible entender qué patrones en el tráfico de la red influyen realmente en la detección de intrusiones, como por ejemplo:

- Valores anómalos en **TTL**  
- Cambios en estados de conexión  
- Variaciones en los volúmenes de datos enviados o recibidos  

Esta información no solo ayuda a interpretar cómo el modelo toma decisiones, sino que también resulta muy útil para mejorar sistemas de seguridad reales, ya que permite saber qué valores se deben monitorear con más atención.

---

## Qué he aprendido

- **Importancia del preprocesamiento:** No es suficiente con entrenar un modelo; elegir y limpiar bien los datos es clave para tener unos resultados fiables.  

- **Interpretación realista de métricas:** Un accuracy alto no garantiza un modelo perfecto; es importante analizar otras métricas como **precision, recall** y la **matriz de confusión**.  

- **Valor de la visualización:** Las gráficas de importancia de features son muy importantes para entender cómo funciona el modelo y cómo podemos mejorar su fiabilidad.
