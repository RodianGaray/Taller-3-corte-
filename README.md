# Taller-3-corte-
Estudiantes:Rodian Garay & Mariana Lombana
---

Este taller reúne tres ejercicios clave del tercer corte, cada uno enfocado en un área distinta pero esencial dentro del desarrollo y las operaciones modernas
--- 
## Índice
1. Humanoid Gym  
2. Algoritmo de Segmentos  
3. Kubernetes
--- 
# 1. Humanoid Gym  
Simulación y control manual de robots humanoides (NAO, Pepper, Romeo y Dancer) usando **Gym**, **PyBullet** y **QiBullet**  
---

## 📌 Descripción del Proyecto

Este repositorio contiene el desarrollo completo de un entorno de simulación basado en **humanoid-gym**, permitiendo controlar manualmente robots humanoides mediante sliders en PyBullet.  
Se implementaron ambientes funcionales para los robots:

- **NAO**
- **Pepper**
- **Romeo**
- **Dancer** (

El sistema permite:
- Crear entornos Gym (`robot-v0`)
- Visualizar simulaciones en PyBullet GUI  
- Controlar cada articulación del robot en tiempo real  
- Ejecutar pruebas automáticas (`env.step()`)    
- Cargar robots con sus URDF y recursos originales de QiBullet  

---
## 🏗️ Tecnologías y librerías usadas

| Tecnología | Uso |
|-----------|-----|
| **Python 3.8+** | Lenguaje principal |
| **Gym (OpenAI)** | Definición de entornos RL |
| **PyBullet** | Física y visualización (GUI) |
| **QiBullet** | Simulación específica de los robots NAO, Pepper y Romeo |
| **humanoid-gym** | Base de los entornos personalizados |
| **Docker** | Contenedor portable del proyecto |
| **GitHub + DockerHub** | Versionamiento y publicación |

---

## 📦 Instalación

### 1. Crear entorno virtual
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Instalar dependencias
```bash
pip install -r requirements.txt
```

### 3. Instalar humanoid-gym en modo desarrollo
```bash
pip install -e .
```

### 4. Primera ejecución con robots QiBullet  
QiBullet descargará los recursos (URDF, meshes).  
Debe aceptarse:

```
The robot meshes and URDFs will be installed…
Continue the installation (y/n)? y
```

---

## 🦾 Robots implementados

Se desarrollaron y probaron entornos para:

| Robot | Gym ID | Estado |
|--------|--------|---------|
| NAO | `nao-v0` | ✔️ Funcional |
| Pepper | `pepper-v0` | ✔️ Funcional |
| Romeo | `romeo-v0` | ✔️ Funcional |
| Dancer | `dancer-v0` | ✔️ Añadido al proyecto |

Todos soportan:
- `reset()`
- `step()`
- `action_space`
- `observation_space`
- Control manual mediante sliders PyBullet  

---

## 🎮 Ejecución de cada robot

### NAO
```python
import gym, humanoid_gym
env = gym.make('nao-v0')
env.reset()
print(env.action_space)
print(env.observation_space)
```

### Pepper
```python
env = gym.make('pepper-v0')
env.reset()
```

### Romeo
```python
env = gym.make('romeo-v0')
env.reset()
```

### Dancer
```python
env = gym.make('dancer-v0')
env.reset()
```

---

## 🕹️ Control manual con sliders

Código unificado para cualquier robot:

```python
import gym, humanoid_gym
import pybullet as p

env = gym.make("nao-v0")   # cambiar a pepper-v0, romeo-v0, dancer-v0
env.reset()

motor_ids = []
for name, low, up in zip(env.joint_names, env.lower_limits, env.upper_limits):
    motor_ids.append(p.addUserDebugParameter(name, low, up, 0))

while True:
    actions = [p.readUserDebugParameter(i) for i in motor_ids]
    env.step(actions)
    env.render()
```

---

## 🐳 Docker

### Construir imagen
```bash
docker build -t humanoid-gym .
```

### Ejecutar contenedor
```bash
docker run -it humanoid-gym
```

La imagen también está disponible en DockerHub:

```
docker pull <tu_usuario>/humanoid-gym
```

---

## 📁 Estructura del repositorio

```
humanoid-gym/
│
├── humanoid_gym/
│   ├── envs/
│   │   ├── nao_env.py
│   │   ├── pepper_env.py
│   │   ├── romeo_env.py
│   │   └── dancer_env.py   ← añadido
│   └── __init__.py
│
├── examples/
│   ├── human_control_nao.py
│   ├── human_control_pepper.py
│   ├── human_control_romeo.py
│   └── human_control_dancer.py  ← añadido
│
├── Dockerfile
├── requirements.txt
└── README.md
```

---


