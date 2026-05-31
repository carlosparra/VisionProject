# Avance 4.31 - Sistema de Re-identificación Visual de Perros

Un sistema completo de identificación visual de perros para casos de mascotas perdidas/encontradas, desarrollado por el Equipo 31.

## 👥 Equipo

- **A00194173** Adriana González Ugalde
- **A01123424** José Alberto Herrera Bernal  
- **A00534649** Carlos Alberto Parra Arredondo

**Asesores:**
- Dra. Grettel Barceló Alonso
- Dr. Luis Eduardo Falcón Morales
- Dra. María de la Paz Rico Fernández

## 📖 Descripción del Proyecto

Este proyecto evolucionó desde un enfoque inicial de clasificación de razas caninas hacia un **sistema de re-identificación visual** diseñado específicamente para ayudar en la búsqueda de perros perdidos y encontrados. Utiliza técnicas avanzadas de visión computacional y embeddings visuales para encontrar coincidencias visuales entre perros reportados.

## 🔄 Evolución del Proyecto

### V1 - Clasificación de Raza (Original)
- **Objetivo:** Identificar la raza de un perro
- **Pregunta:** ¿Qué raza parece ser este perro?
- **Limitación:** La raza no identifica al individuo

### V1 Ajustada - Mejora Visual
- **Objetivo:** Mejorar la calidad visual del dataset
- **Mejoras:** Curaduría, detección YOLO, crops optimizados
- **Herramientas:** OpenCV, CLAHE, ajustes de contraste/brillo

### V2 - Re-identificación Visual (Cambio Fundamental)
- **Objetivo:** Re-identificación visual individual
- **Pregunta:** ¿Este perro se parece a uno ya reportado?
- **Método:** YOLO + EfficientNet + similitud coseno

### V2 Optimizada - Versión Final Recomendada ⭐
- **Objetivo:** Máxima cobertura y precisión
- **Mejora clave:** Detección optimizada con mayor cobertura

## 📊 Resultados y Métricas

### Configuración Final Recomendada

```python
YOLO_MODEL = "yolo26s.pt"
CONF_THRESHOLD = 0.15
CROP_MARGIN = 0.35
EMBEDDING_BACKBONE = "EfficientNetB0"
SIMILARITY = "cosine"
TOP_K = 10
```

### Comparativa de Versiones

| Métrica | V2 Base | V2 Optimizada | Mejora |
|---------|---------|---------------|--------|
| **Top-5 Same Dog Accuracy** | 95.69% | **97.09%** | +1.40% |
| Top-1 Same Dog Accuracy | 92.18% | 93.20% | +1.02% |
| Top-10 Same Dog Accuracy | 95.96% | 98.06% | +2.10% |
| Cobertura de detección | 93.07% | **95.08%** | +2.01% |
| Falsos negativos (Top-5) | 16 | **12** | -25% |

### Detección y Procesamiento

| Componente | V2 Base | V2 Optimizada | Ganancia |
|------------|---------|---------------|----------|
| Imágenes evaluadas | 20,580 | 20,580 | - |
| Perros detectados | 19,153 | **19,568** | +415 |
| Tasa de detección | 93.07% | **95.08%** | +2.01% |
| Crops válidos | 19,153 | **19,568** | +415 |
| Embeddings generados | 19,153 | **19,568** | +415 |

### Curaduría del Dataset

Se procesaron **20,580 imágenes** de **120 razas** diferentes:

| Calidad Original | Cantidad | Calidad Post-V2 | Cantidad |
|------------------|----------|-----------------|----------|
| Excellent | 8,071 | Excellent | 8,168 |
| Good | 7,216 | Good | 9,264 |
| Needs Review | 4,111 | Needs Review | 2,735 |
| Acceptable | 1,182 | Acceptable | 413 |

**Mejoras visuales conseguidas:**
- Nitidez: 2,455 → 4,375 (+78%)
- Brillo: 115.3 → 128.9 (+13.6)
- Reducción de imágenes oscuras: 451 → 2 (-99.6%)

## 🔬 Comparación de Modelos

Se evaluaron tres arquitecturas de EfficientNet:

| Modelo | Top-5 Accuracy | Separación | Decisión |
|--------|----------------|------------|----------|
| **EfficientNetB0** | **95.69%** | 0.4574 | ✅ **Recomendado** |
| EfficientNetV2B0 | 95.42% | 0.4811 | ❌ No mejora Top-5 |
| EfficientNetV2B1 | 95.15% | 0.4951 | ❌ Peor Top-5 |

**EfficientNetB0** fue seleccionado porque maximiza la métrica más importante: **Top-5 Same Dog Accuracy**.

## 📂 Estructura del Proyecto

```
Avance4.31/
├── V1_ajustada_pipeline_original/          # Pipeline inicial con mejoras
│   ├── 01_curaduria_y_calidad_de_imagenes_V1_ajustada.ipynb
│   ├── 02_deteccion_y_clasificacion_de_razas_V1_ajustada.ipynb
│   ├── 03_Embeddings_EfficientNet_V1_ajustada.ipynb
│   ├── 04_Comparacion_embeddings_V1_ajustada.ipynb
│   └── 05_Team_Test_End_to_End_Modelo_Embeddings_V1_ajustada.ipynb
├── V2_nuevo_pipeline/                       # Cambio a re-identificación
│   ├── 00_Setup_Proyecto_Integrador_V2.ipynb
│   ├── 01_Image_Quality_Curation.ipynb
│   ├── 02_Dog_Detection_Cropping_YOLO.ipynb
│   ├── 03_Dog_Visual_Embeddings.ipynb
│   ├── 04_Vector_Search_Cosine_Similarity.ipynb
│   ├── 05_Lost_Found_End_to_End_Search.ipynb
│   └── 06_Dog_ReIdentification_Evaluation_15_25.ipynb
├── V2_optimizado_pipeline_v2/              # ⭐ Pipeline final recomendado
│   ├── 01_Image_Quality_Curation.ipynb
│   ├── 02B_Dog_Detection_Cropping_YOLO_Optimized.ipynb
│   ├── 03B_Dog_Visual_Embeddings_Optimized.ipynb
│   ├── 04B_Vector_Search_Cosine_Similarity_Optimized.ipynb
│   ├── 05B_Lost_Found_End_to_End_Search_Optimized.ipynb
│   └── 06B_Dog_ReIdentification_Evaluation_Optimized.ipynb
├── comparacion_modelos_y_parametros/       # Evaluaciones adicionales
└── 07_Resumen_de_Experimieto_variantes_y_seleccion_de_modelo_pipeline.ipynb
```

## 🎯 Pipeline Final Recomendado (V2 Optimizada)

### Flujo de Procesamiento

```
01. Image Quality Curation
↓
02B. Dog Detection & Cropping (Optimized)
↓
03B. Visual Embeddings Generation (Optimized)
↓
04B. Vector Search & Cosine Similarity (Optimized)
↓
05B. End-to-End Lost/Found Search (Optimized)
↓
06B. Re-identification Evaluation (Optimized)
```

### Componentes Técnicos

1. **Curaduría de Imágenes:** CLAHE + contraste + nitidez → 224×224 px
2. **Detección:** YOLO v26s con confidence 0.15 y margin 0.35
3. **Embeddings:** EfficientNetB0 pre-entrenado → vectores 1280D
4. **Búsqueda:** Similitud coseno + Top-K retrieval
5. **Evaluación:** Métricas de re-identificación

## 🔍 Métricas de Evaluación

- **Top-5 Same Dog Accuracy:** 97.09% (métrica principal)
- **Top-1 Same Dog Accuracy:** 93.20%
- **Top-10 Same Dog Accuracy:** 98.06%
- **Separación media:** 0.4552 (mismo vs diferente perro)
- **Falsos negativos Top-5:** 12 casos
- **Falsos positivos Top-1:** 28 casos

## 🎨 Técnicas de Mejora Visual

### Pipeline de Curaduría V2
- **CLAHE:** Equalización adaptativa de histograma
- **Contraste/Brillo:** Ajustes moderados (α=1.08, β=4)
- **Saturación:** Mejora ligera (factor=1.05)
- **Nitidez:** Unsharp mask (amount=1.35)
- **Normalización:** Resize con padding gris 224×224

### Resultados de Mejora
- **+78% nitidez promedio**
- **99.6% reducción de imágenes oscuras**
- **84% reducción de casos de bajo contraste**
- **0 archivos corruptos** después del procesamiento

## 🚀 Próximos Pasos Recomendados

1. **Agrupación por identidad:** Consolidar resultados por `dog_id`
2. **Multi-embedding:** Múltiples fotos por perro individual
3. **Calibración de umbrales:** Match fuerte/posible/débil
4. **Metadata contextual:** Ubicación, fecha, descripción
5. **Modelos especializados:** DINOv2, CLIP, redes Siamese
6. **Demo web/app:** Interfaz para flujo lost/found

## 🏆 Logros Principales

1. **Cambio de paradigma exitoso:** De clasificación de raza a re-identificación visual
2. **Optimización significativa:** +2.01% cobertura, +1.40% Top-5 accuracy
3. **Pipeline robusto:** 0 errores en 20,580 imágenes procesadas
4. **Selección técnica fundamentada:** EfficientNetB0 vs modelos más modernos
5. **Documentación completa:** Proceso experimental transparente y reproducible

## 📈 Impacto

El sistema desarrollado puede ayudar significativamente en la búsqueda de mascotas perdidas al:

- **Automatizar la búsqueda visual** en bases de datos de reportes
- **Reducir tiempo de búsqueda manual** mostrando candidatos relevantes
- **Mejorar tasas de reunión** entre mascotas y familias
- **Proporcionar herramienta técnica** para refugios y organizaciones

---

**Estado del Proyecto:** ✅ Completado - V2 Optimizada recomendada para implementación

**Última actualización:** Mayo 2026