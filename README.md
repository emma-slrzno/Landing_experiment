# Experimento A/B en página de inicio (Landing Page)

## Objetivo

Evaluar un experimento A/B realizado sobre una página de inicio (landing page) con dos versiones, **A y B**, para determinar qué versión genera mejores resultados de negocio y **recomendar cuál implementar**, respaldado por pruebas estadísticas. Pensado para los equipos de **producto, marketing y crecimiento**.

**Preguntas que responde el dashboard:**
- ¿Qué versión de la página (A o B) genera mayor gasto promedio por usuario convertido?
- ¿Qué versión tiene una tasa de conversión más alta?
- ¿La fuente de tráfico (Ads, Email, Organic, Referral) está asociada con la probabilidad de conversión?
- ¿El tipo de usuario (Nuevo vs. Recurrente) influye en la conversión?

## Datos

- **Fuente:** `landing_experiment.csv`
- **Periodo:** 1 al 28 de enero de 2026 (28 días de experimento)
- **Tamaño:** 40,000 registros (usuarios únicos), sin valores nulos
- **Variables principales:** `user_id`, `date`, `landing` (A/B), `region`, `dispositivo`, `traffic_source`, `user_type` (Nuevo/Recurrente), `converted`, `gasto`

## Herramientas

- Python (pandas, seaborn, matplotlib)
- Pruebas estadísticas: prueba de Levene (homogeneidad de varianzas), t-test de Welch, Z-test de dos proporciones, Chi-cuadrado de independencia

## Contenido del dashboard

- **Comparación de gasto promedio:** distribución del gasto entre usuarios convertidos de la página A vs. B
- **Comparación de tasa de conversión:** conversión total y por versión (A vs. B)
- **Conversión por fuente de tráfico:** conteo y proporción de usuarios convertidos por canal (Ads, Email, Organic, Referral)
- **Conversión por tipo de usuario:** conteo y proporción de conversión entre usuarios Nuevos y Recurrentes
- **Filtros interactivos:** versión de landing (A/B), región, dispositivo, fuente de tráfico, tipo de usuario
- **Indicadores clave (KPI):** tasa de conversión por versión, gasto promedio por usuario convertido, p-values de cada prueba estadística

## Principales conclusiones

- **La Página B es la clara ganadora en conversión:** tasa de conversión de **15.96% (3,194 / 20,018)** vs. **12.57% (2,512 / 19,982)** de la Página A, una mejora relativa del **+27%**. La diferencia es estadísticamente significativa (Z-test de proporciones: **Z = -9.68, p < 0.001**).
- **La Página B también genera mayor gasto promedio** entre los usuarios que convirtieron. La prueba de Levene confirmó varianzas distintas entre grupos (p < 0.001), y el t-test de Welch mostró una diferencia significativa (**t = -9.48, p < 0.001**): el gasto de la Página A es significativamente menor que el de la Página B.
- **La fuente de tráfico sí está asociada con la conversión** (Chi-cuadrado: **χ² = 8.66, p = 0.034**, 3 grados de libertad). Email destaca como el canal con mejor tasa de conversión relativa, mientras que Organic aporta el mayor volumen absoluto de usuarios convertidos (pero también de no convertidos).
- **El tipo de usuario NO influye en la conversión.** Usuarios Nuevos convierten al **14.36% (3,738 / 26,033)** y Recurrentes al **14.09% (1,968 / 13,967)**, una diferencia no significativa (Chi-cuadrado: **χ² = 0.51, p = 0.474**).
- **Distribución del tráfico:** Organic (~45%) es la fuente dominante, seguida de Ads (~30%), Email (~15%) y Referral (~10%, el canal menos usado).

## Aprendizajes

- Selección y aplicación correcta de pruebas estadísticas según el tipo de variable: **t-test de Welch** (con prueba previa de Levene) para comparar medias de gasto, **Z-test de proporciones** para comparar tasas de conversión, y **Chi-cuadrado de independencia** para asociar variables categóricas con conversión.
- Interpretación de resultados estadísticos en términos de **decisión de negocio** (rechazar o no rechazar H₀) y su implicación práctica, evitando confundir significancia estadística con relevancia de negocio.
- Uso de visualizaciones (`countplot` con `hue`) para reforzar y comunicar visualmente los resultados de las pruebas estadísticas.
- Construcción de un **insight ejecutivo** que traduce hallazgos técnicos en recomendaciones accionables: priorizar la Página B, reasignar presupuesto hacia Ads y Email, y diseñar una estrategia de nurturing distinta para tráfico orgánico y de referidos.


## Contacto

- LinkedIn: (https://www.linkedin.com/in/emma-solorzano-hernandez-jauregui-200301345/)
