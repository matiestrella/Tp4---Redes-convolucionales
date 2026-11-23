# TP4 - Redes convolucionales para clasificar lesiones de piel (ISIC)

Clasificación de imágenes dermatoscópicas del dataset ISIC (9 clases, 878 imágenes) con una MLP de referencia y redes convolucionales. El notebook [`04_CNN_Skin_ISIC.ipynb`](04_CNN_Skin_ISIC.ipynb) está **ejecutado con todos sus outputs** e incluye el EDA, la limpieza de duplicados, el split estratificado, la comparación MLP vs. CNN, los experimentos de arquitectura y de data augmentation, la evaluación final en test y el análisis de resultados.

## Resultado principal

| Modelo | Parámetros | Test accuracy | Test F1 macro |
|---|---|---|---|
| CNN final (3 bloques, 32 px, flips+rot90) | 156 057 | 0.698 ± 0.037 | 0.693 ± 0.036 |
| MLP 64 px | 6 358 793 | 0.504 ± 0.053 | 0.487 ± 0.057 |

Media ± desvío de 5 semillas, sobre 169 imágenes de test (error estándar ≈ ±0.035).

## Cómo reproducirlo

El dataset **no está incluido** en este repositorio. Se descarga del repositorio de la cátedra: [2245-RN-ITBA/skin-dataset-classification](https://github.com/2245-RN-ITBA/skin-dataset-classification) (carpeta `data/Split_smol`).

### Opción A: Google Colab (como se ejecutó)

1. Abrir el notebook en Colab (*Archivo → Abrir notebook → GitHub* y pegar la URL de este repositorio).
2. *Entorno de ejecución → Cambiar tipo de entorno de ejecución → **GPU T4***.
3. *Entorno de ejecución → **Ejecutar todo***. La primera celda descarga el dataset a `/content/data/Split_smol`.

### Opción B: local

```bash
git clone <URL de este repositorio>
cd Tp4---Redes-convolucionales
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# descargar el dataset en data/Split_smol
git clone --depth 1 https://github.com/2245-RN-ITBA/skin-dataset-classification.git /tmp/skin
mkdir -p data && cp -r /tmp/skin/data/Split_smol data/

jupyter notebook 04_CNN_Skin_ISIC.ipynb
```

En local hay que **saltear la primera celda** (es específica de Colab: hace `%cd /content`) y ejecutar el resto en orden. Sin GPU corre en CPU; los tiempos serán mayores que los reportados.

La estructura esperada es:

```
data/Split_smol/
├── train/<clase>/*.jpg
└── val/<clase>/*.jpg
```

## Entorno de la corrida entregada

- Google Colab, GPU T4, PyTorch 2.14.1 (CUDA 13.0).
- **No se modificó ninguna constante** de la sección 0. Semilla del split: `SEED_SPLIT = 42`.
- En GPU el entrenamiento no es completamente determinista (cuDNN). Al volver a ejecutar, los números pueden variar en el orden de ±0.02 de accuracy, dentro del ruido entre semillas que se reporta.
