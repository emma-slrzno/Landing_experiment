# Experimento A/B en página de inicio (Landing Page)

Análisis estadístico de un experimento A/B que compara dos versiones de una landing page (**A: control** y **B: variante**) para decidir, con datos, cuál versión debe quedarse en producción. Se evalúan la tasa de conversión, el gasto de los usuarios que compran y la influencia de la fuente de tráfico y del tipo de usuario.

---

## 1. Problema o contexto de negocio

La página de inicio es la puerta de entrada de los usuarios al negocio: de su diseño depende cuántos visitantes se convierten en clientes y cuánto gastan. Se rediseñó la landing (versión B) y, antes de reemplazar la versión actual (A), el equipo necesita saber:

- ¿La nueva versión convierte más visitantes en clientes?
- ¿Los clientes que llegan por la nueva versión gastan más?
- ¿Algún canal de tráfico convierte mejor que otro?
- ¿Cambia el comportamiento entre usuarios nuevos y recurrentes?

Tomar esta decisión sin validación estadística podría llevar a implementar un cambio que no aporta valor, o a descartar uno que sí lo hace.

## 2. Objetivo del análisis

Determinar, con pruebas estadísticas apropiadas, **qué versión de la landing page es mejor** y traducir los resultados en recomendaciones de marketing. En concreto:

1. Explorar y validar la calidad de los datos.
2. Comparar el **gasto promedio** entre A y B.
3. Comparar la **tasa de conversión** entre A y B.
4. Evaluar la relación entre **fuente de tráfico** y conversión.
5. Evaluar la relación entre **tipo de usuario** y conversión.
6. Visualizar los resultados y comunicar un insight ejecutivo.

## 3. Dataset utilizado

**Archivo:** `landing_experiment.csv` · **40,000 usuarios** · 9 columnas · sin valores nulos ni `user_id` duplicados.

| Columna | Descripción |
|---------|-------------|
| `user_id` | Identificador único del usuario |
| `date` | Fecha de exposición a la página (1 al 28 de enero de 2026) |
| `landing` | Versión mostrada: A o B |
| `region` | Región geográfica (5 regiones) |
| `dispositivo` | Mobile o Desktop |
| `traffic_source` | Canal de llegada: Organic, Ads, Email, Referral |
| `user_type` | Nuevo o Recurrente |
| `converted` | 1 si el usuario convirtió, 0 si no |
| `gasto` | Monto gastado (0 si no convirtió) |

**Composición de la muestra**
- Grupos balanceados: **A = 19,982** usuarios y **B = 20,018** usuarios.
- Tráfico: Organic 45.0%, Ads 29.8%, Email 15.3%, Referral 9.9%.
- Tipo de usuario: 65.1% nuevos y 34.9% recurrentes.
- Dispositivo: 62.1% Mobile.
- Conversión global: 5,706 usuarios (14.3%); gasto medio de ~$9.33 por usuario y de ~$65 por usuario que convirtió.

## 4. Herramientas y tecnologías

- **Python** (Jupyter Notebook)
- **pandas** para manipulación y validación de datos
- **scipy.stats**: `levene`, `ttest_ind` (Welch), `chi2_contingency`
- **statsmodels**: `proportions_ztest`
- **seaborn** y **matplotlib** para visualización

## 5. Proceso realizado

1. **Carga y validación:** revisión de estructura, nulos y unicidad de `user_id`; conversión de `date` de `object` a `datetime`; verificación del rango temporal y de las categorías esperadas (A/B, regiones, dispositivos, canales y tipos de usuario).
2. **Gasto promedio (A vs B):** se comparó el gasto de los **usuarios que convirtieron** (2,512 en A y 3,194 en B). Como la prueba de Levene indicó varianzas distintas entre grupos, se aplicó la **prueba t de Welch** (`equal_var=False`).
3. **Tasa de conversión (A vs B):** **prueba Z de dos muestras para proporciones**.
4. **Fuente de tráfico vs. conversión:** **prueba chi-cuadrado de independencia** sobre la tabla de contingencia.
5. **Tipo de usuario vs. conversión:** **prueba chi-cuadrado de independencia**.
6. **Visualización:** gráficos de barras (conversiones vs. no conversiones) por fuente de tráfico y por tipo de usuario.
7. **Insight ejecutivo:** síntesis de hallazgos y recomendaciones.

Nivel de significancia utilizado en todas las pruebas: **α = 0.05**.

## 6. Principales hallazgos

### 🎯 Conversión: la versión B gana con claridad

| Versión | Usuarios | Convertidos | Tasa de conversión |
|---------|---------:|------------:|-------------------:|
| A (control) | 19,982 | 2,512 | **12.57%** |
| B (variante) | 20,018 | 3,194 | **15.96%** |

- Diferencia: **+3.39 puntos porcentuales**, equivalente a una mejora relativa de **~27%**.
- Prueba Z = −9.68, **p ≈ 3.8 × 10⁻²²** → diferencia estadísticamente significativa.

### 💰 Gasto: los clientes de B gastan más

- Levene: p ≈ 6.9 × 10⁻⁸ → las varianzas son distintas, por lo que se usó la prueba t de Welch.
- Prueba t = −9.48, **p ≈ 3.6 × 10⁻²¹** → el gasto promedio de los usuarios convertidos en **B es significativamente mayor** que en A.

En conjunto, B no solo convierte a más usuarios sino que cada cliente aporta más valor.

### 🌐 Fuente de tráfico: relación significativa, pero de efecto pequeño

| Canal | Usuarios | Convertidos | Tasa de conversión |
|-------|---------:|------------:|-------------------:|
| Email | 6,123 | 918 | **14.99%** |
| Ads | 11,935 | 1,759 | 14.74% |
| Referral | 3,955 | 549 | 13.88% |
| Organic | 17,987 | 2,480 | 13.79% |

- Chi-cuadrado = 8.66, gl = 3, **p = 0.034** → existe asociación estadísticamente significativa entre el canal y la conversión.
- Email y Ads convierten mejor; Organic aporta el mayor volumen absoluto de conversiones, pero con la tasa más baja. El rango entre canales es de apenas ~1.2 puntos porcentuales.

### 👤 Tipo de usuario: sin diferencia

| Tipo | Usuarios | Convertidos | Tasa de conversión |
|------|---------:|------------:|-------------------:|
| Nuevo | 26,033 | 3,738 | 14.36% |
| Recurrente | 13,967 | 1,968 | 14.09% |

- Chi-cuadrado = 0.51, **p = 0.474** → no hay evidencia de que el tipo de usuario influya en la probabilidad de convertir.

## 7. Recomendaciones e impacto para el negocio

1. **Implementar la versión B como landing principal.** Mejora la conversión en ~27% relativo y aumenta el gasto por cliente. Ambos efectos son significativos y apuntan en la misma dirección, por lo que el impacto en ingresos es doble: más compradores y mayor valor por compra.
2. **Priorizar Email y Ads en las campañas de conversión directa.** Son los canales con mayor tasa de conversión. Antes de reasignar presupuesto, conviene cruzar estos datos con el costo por canal para confirmar cuál tiene mejor retorno.
3. **Dar un tratamiento distinto al tráfico Organic y Referral.** Generan volumen pero convierten menos; vale la pena probar contenido informativo, captación de correos con incentivo y flujos de nurturing para llevarlos después a Email.
4. **No segmentar la estrategia de la landing por tipo de usuario.** Nuevos y recurrentes convierten de forma prácticamente idéntica, así que no hay evidencia para diseñar experiencias separadas.
5. **Monitorear tras el lanzamiento.** Dar seguimiento a conversión y gasto por cliente durante las primeras semanas para confirmar que el efecto se sostiene fuera del experimento.

## 8. Limitaciones y puntos a considerar

- **El gasto se comparó solo entre usuarios que convirtieron.** Esto mide el valor por cliente, no el ingreso por visitante (que combina conversión y gasto). Comparar el ingreso por usuario expuesto, incluyendo ceros, daría una lectura más directa del impacto económico total.
- **No se cuantifica el efecto del gasto.** El notebook reporta significancia, pero no las medias por grupo ni un intervalo de confianza; sin ellos no se puede estimar cuánto más gasta B.
- **Efecto pequeño en el canal de tráfico.** La significancia (p = 0.034) es moderada y las diferencias entre canales son de ~1 punto porcentual; conviene tratarlas como una tendencia, no como una diferencia contundente.
- **No se probó la interacción landing × segmento.** La afirmación de que ambos tipos de usuario se beneficiaron por igual de la versión B no está respaldada por una prueba: solo se comparó `user_type` contra conversión de forma global. Lo mismo aplica a canal, región y dispositivo.
- **No se verificó la aleatorización por covariables.** El reparto A/B por volumen es balanceado, pero no se revisó si región, dispositivo, canal y tipo de usuario están distribuidos de forma similar entre A y B.
- **Duración del experimento.** Fueron 28 días; no se evaluó un posible efecto de novedad ni la estacionalidad semanal.
- **Sin datos de costos.** Las conclusiones sobre "rentabilidad" por canal requieren información de inversión que este dataset no incluye.

## 9. Cómo reproducir el análisis

```bash
pip install pandas scipy statsmodels seaborn matplotlib jupyter
jupyter notebook Landing_Experiment.ipynb
```

El notebook carga el archivo desde `/datasets/landing_experiment.csv`; si lo ejecutas en local, ajusta la ruta a donde tengas guardado el CSV.

## 10. Estructura del proyecto

```
├── Landing_Experiment.ipynb   # Análisis completo
├── landing_experiment.csv     # Datos del experimento
└── README.md
```
# Contacto

- LinkedIn: https://www.linkedin.com/in/emma-solorzano-hernandez-jauregui-200301345/
- Perfil de Tableau Public: https://public.tableau.com/app/profile/emma.solorzano7415/vizzes
