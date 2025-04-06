# Resultados y Discusión del Proyecto de Clasificación de Lesiones Cutáneas

Este estudio compara el rendimiento de diferentes arquitecturas de redes neuronales aplicadas a un conjunto de datos con imágenes de lesiones dermatológicas y metadatos clínicos, como edad, sexo y localización.

---

# Link Google Colab

https://colab.research.google.com/drive/1g4Glragp0HndYLempEkTD31iDCrr4y9W?usp=sharing

---

## Comparativa General de Modelos

| Modelo                          | Precisión Total | F1-score Macro |
|--------------------------------|------------------|----------------|
| Modelo Tabular (Pesos)   | 0.2881           | 0.17           |
| CNN desde 0 (Pesos)        | 0.6893           | 0.53           |
| MobileNetV2 (Hyperopt + Transfer Learning + Pesos) | 0.5556       | 0.38           |
| Late Fusion (Imagen + Tabular) | 0.6287           | 0.33           |
| Early Fusion (Imagen + Tabular) | 0.2409          | 0.19           |

---

## Descripción de los Modelos

### Modelo Tabular (Hito 1)
- Utiliza únicamente variables clínicas: edad, sexo, localización.
- Técnicas utilizadas:
  - Balanceo de clases con `class_weight`.
- **Desempeño:** muy limitado. No logra capturar patrones discriminativos suficientes.  
- 🔻 *Conclusión:* insuficiente información en variables tabulares.

---

### CNN desde cero (Hito 2)
- Red convolucional personalizada, entrenada desde cero.
- Técnicas utilizadas:
  - Arquitectura CNN profunda + `Dropout` + `BatchNormalization`.
  - Balanceo de clases (`class_weight`).
- **Desempeño:** sobresaliente para una red entrenada desde cero.
- 🔼 *Conclusión:* demuestra que la arquitectura CNN aprende bien sobre imágenes, incluso sin preentrenamiento.

---

### MobileNetV2 (Transfer Learning)
- Uso de red preentrenada sobre ImageNet, con ajuste de capas superiores.
- Técnicas utilizadas:
  - `Transfer Learning`, `class_weight`.
- **Desempeño:** mejora respecto al modelo tabular, pero no supera al CNN desde cero.
- 🔁 *Conclusión:* el conocimiento previo de MobileNetV2 no se adapta perfectamente al dominio médico.

---

### Late Fusion (Hito 3)
- Combinación de dos ramas: CNN + red para datos tabulares.
- Fusiona salidas densas de ambas arquitecturas antes del clasificador.
- Técnicas utilizadas:
  - `class_weight` + fusión tardía.
- **Desempeño:** muy competitivo, integrando ambas fuentes de datos.
- ✅ *Conclusión:* la combinación mejora los resultados respecto a usar solo imágenes.

---

### Early Fusion (Hito 3)
- Imágenes y datos tabulares fusionados antes del proceso convolucional.
- Entrenamiento sobre imagen "aplanada" + datos tabulares.
- Técnicas utilizadas:
  - `class_weight` + fusión temprana.
- **Desempeño:** el peor de todos. La información de imagen no se aprovecha correctamente.
- ❌ *Conclusión:* esta estrategia distorsiona la estructura espacial de la imagen.

---

## Conclusiòn

- El mejor rendimiento lo alcanza la red **CNN desde cero**, seguida por la estrategia de **Late Fusion**.
- El modelo tabular y Early Fusion son los que peores resultados ofrecen.
- El uso de MobileNetV2 aporta mejoras, pero **no garantiza mejor desempeño** en este contexto médico específico.
- El uso de técnicas como `class_weight` y `hyperopt` fue crucial para compensar la desbalanceada distribución de clases.

---
