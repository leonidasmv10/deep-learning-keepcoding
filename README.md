# Proyecto Deep Learning – Clasificación de Lesiones Dermatológicas

## 🎯 Objetivo General

Desarrollar un sistema de clasificación automática de lesiones de piel usando el dataset **HAM10000**, combinando **datos tabulares clínicos** y **datos visuales (imágenes dermatoscópicas)** mediante redes neuronales profundas. Se busca comparar diversas arquitecturas para determinar cuál ofrece mejor generalización, equidad y rendimiento en la predicción.

## 📦 Dataset

- Imágenes RGB de 28x28 reescaladas a 96x96
- Datos tabulares: edad, localización, tipo de lesión, etc.
- Etiquetas en 7 clases:
  `['akiec', 'bcc', 'bkl', 'df', 'mel', 'nv', 'vasc']`

Particiones estratificadas `train / val / test`.

## 🧪 Modelos implementados

### 1. Modelo Tabular (Hito 1)

- Red neuronal densa (`MLP`) con hiperparámetros optimizados (Hyperopt)
- Uso de `class_weight` para mitigar el desbalance

**Resultados:**
- Accuracy: `0.3186`
- F1 macro: `0.19`

### 2. CNN desde cero

- Arquitectura CNN con 3 bloques `Conv2D + MaxPooling` + `Dropout`

**Resultados:**
- Accuracy: `0.72`
- F1 macro: `0.35`

### 3. MobileNetV2 (Transfer Learning)

- MobileNetV2 congelada (`include_top=False`)
- `ImageDataGenerator` + `class_weight`

**Resultados:**
- Accuracy: `0.60`
- F1 macro: `0.37`

### 4. Modelo Combinado (Late Fusion)

- CNN para imágenes + MLP para datos tabulares
- Fusión tardía (`Concatenate`) + capas densas finales

**Resultados:**
- Accuracy: `0.7637`
- F1 macro: `0.4698`

### 5. Modelo Early Fusion

- Imagen aplanada + datos tabulares directamente concatenados (Early Fusion)
- Red MLP unificada

**Resultados:**
- Accuracy: `0.6687`
- F1 macro: `0.1145`

## 🔁 Comparativa Final

| Modelo             | Estrategia     | Accuracy | F1 Macro | Técnicas clave              |
|--------------------|----------------|----------|----------|-----------------------------|
| Tabular            | Solo tabular   | 0.32     | 0.19     | Hyperopt, class_weight      |
| CNN desde cero     | Imagen (2D)    | 0.72     | 0.35     | Dropout, EarlyStopping      |
| MobileNetV2        | TL + imagen    | 0.60     | 0.37     | Data Aug., class_weight     |
| Combinado (Late)   | Late Fusion    | 0.76     | 0.47     | Fusión de entradas, balance |
| Combinado (Early)  | Early Fusion   | 0.67     | 0.11     | Simple concatenación        |

## 📌 Conclusión del proyecto

- La fusión tardía fue la estrategia más efectiva, logrando alto rendimiento y equilibrio entre clases.
- La fusión temprana no fue adecuada por la pérdida de estructura espacial en imágenes.
- La combinación de datos clínicos e imágenes mejora claramente el rendimiento respecto a modelos aislados.
