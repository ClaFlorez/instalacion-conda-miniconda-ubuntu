# 🐍 Instalación de Anaconda/Miniconda en Ubuntu (VM VirtualBox)

Este documento resume cómo logré instalar y activar **Anaconda** en mi máquina virtual Ubuntu (VirtualBox), partiendo de un problema inicial de **falta de espacio en disco**.  
Incluye además una guía alternativa con **Miniconda**, la versión ligera de Anaconda.

---

## 1️⃣ Liberar espacio en la VM

Antes de instalar, el disco estaba lleno. Para liberar espacio hice:

```bash
# Borrar instaladores repetidos de Anaconda
rm -f ~/Anaconda3-2024.02-1-Linux-x86_64.sh*

# Limpiar cachés de APT
sudo apt-get clean
sudo apt-get autoremove -y

# Limpiar logs de systemd (dejando solo 7 días)
sudo journalctl --vacuum-time=7d

# Si usaste Docker antes, liberar espacio
sudo docker system prune -a --volumes -f

# Revisar qué carpetas ocupan más espacio
sudo du -h --max-depth=1 / | sort -hr | head -20
```

📌 Solución definitiva: ampliar el disco en VirtualBox con:

```powershell
VBoxManage modifyhd "C:\Users\H0PPR202\VirtualBox VMs\ClaudiaLinux\ClaudiaLinux.vdi" --resize 50000
```

---

## 2️⃣ Instalación de Anaconda en Ubuntu

```bash
# Descargar el instalador
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh

# Dar permisos de ejecución
chmod +x Anaconda3-2024.02-1-Linux-x86_64.sh

# Ejecutar la instalación interactiva
./Anaconda3-2024.02-1-Linux-x86_64.sh
```

---

## 3️⃣ Activar Conda

Al terminar, tuve que inicializarlo manualmente:

```bash
~/anaconda3/bin/conda init
```

Luego reinicié la shell con:

```bash
exec bash
```

✅ Resultado: en el prompt ya aparece `(base)` → significa que Conda está activo.

Verificaciones:

```bash
conda --version
# conda 24.1.2

python --version
# Python 3.11.7

which conda
# /root/anaconda3/bin/conda
```

---

## 4️⃣ Crear entornos en Conda

Es recomendable **no trabajar en el entorno base**.  
Creé uno nuevo llamado `ml-course`:

```bash
conda create -n ml-course python=3.10 -y
conda activate ml-course
```

Instalé librerías básicas:

```bash
conda install numpy pandas matplotlib scikit-learn jupyter -y
```

Prueba rápida:

```bash
python -c "import numpy as np, pandas as pd; print(np.__version__, pd.__version__)"
```

---

## 5️⃣ Estructura de carpetas de Anaconda

Dentro de `~/anaconda3` se crean carpetas como:

- **bin/** → ejecutables principales (`conda`, `python`, etc.)  
- **envs/** → aquí se guardan los entornos creados (`ml-course`, etc.)  
- **pkgs/** → caché de paquetes descargados (puede ocupar mucho espacio)  
- **lib/** → librerías de Python  
- **etc/** → configuraciones de entorno  
- **share/**, **include/**, **conda-meta/** → archivos de soporte internos  

---

## 6️⃣ Miniconda (versión ligera)

Si no quieres instalar todo Anaconda (que pesa >3 GB), puedes usar **Miniconda**:

```bash
# Descargar Miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Instalar
bash Miniconda3-latest-Linux-x86_64.sh
```

### ✅ Ventajas
- Instalador más pequeño (~400 MB).  
- Solo trae `conda` y `python`.  
- Tú decides qué paquetes instalar.  

### ⚠️ Desventajas
- No incluye Jupyter, Pandas, NumPy, etc. (hay que instalarlos manualmente).  
- Puede tardar más al preparar un entorno completo para ML/Data Science.  

Ejemplo:

```bash
conda create -n ml-course python=3.10 -y
conda activate ml-course
conda install numpy pandas matplotlib scikit-learn jupyter -y
```

---

## 🚀 Conclusión

- Liberé espacio en disco antes de instalar.  
- Instalé **Anaconda** y activé Conda exitosamente.  
- Creé un entorno `ml-course` con Python 3.10 y librerías de ML.  
- Documenté las carpetas principales de Anaconda.  
- Incluí la opción alternativa de **Miniconda** para entornos más ligeros.  

Ahora la VM está lista para proyectos de **Machine Learning y Ciencia de Datos** en Jupyter Notebook 🎉.
