# Bitácora

## 24-nov-2026

### Git y estructura

#### ¿Qué es un archivo `.gitkeep` y por qué lo necesita Git?

Git no puede hacer push de directorios vacíos; solo puede realizar un seguimiento de archivos: si intentas subir una carpeta que no contiene nada, aunque esta exista en tu máquina local, no se incluirá en tu branch.

Una práctica común y estandarizada para resolver este problema consiste en añadir un archivo llamado `.gitkeep` dentro de las carpetas vacías, básicamente es un recurso habitual en los proyectos de software cuando es necesario que Git realice un seguimiento de una carpeta vacía.

**Fuentes:**
- [What is .gitkeep? How to Track and Push Empty Folders in Git](https://www.freecodecamp.org/news/what-is-gitkeep/)

---

#### ¿Qué es `__init__.py`?

Para entenderlo, renombramos `__init__.py` a `__init__.py.bak` y ejecutamos:
    python -c "import cubesatplan; print(cubesatplan.__file__)"

Esto devuelve `None`. Si le devolvemos su nombre original al archivo, obtenemos la ruta completa en la que se halla `__init__py`:
    C:\Users\migue\proyectos\cubesat-mission-analysis\cubesatplan\__init__.py

Que `__file__` valga `None` indica que, sin el archivo, Python trata la carpeta como un **paquete de espacio de nombres** (*namespace package*): sigue pudiendo importarse, pero no es un paquete "regular" con un archivo que lo represente.

Si dentro del archivo restaurado escribimos:
    print("importando cubesatplan")

y ejecutamos tres veces:    
    python -c "import cubesatplan"
    python -c "import cubesatplan"
    python -c "import cubesatplan; import cubesatplan"

observamos que el mensaje aparece una única vez por ejecución, incluso con el import doble. Esto es porque Python guarda cada módulo ya cargado en el diccionario `sys.modules`, luego un `import` solo ejecuta el código del módulo la **primera vez que se importa en ese proceso**; las siguientes veces reutiliza el objeto guardado. Cada ejecución de `python -c` es un proceso nuevo con un `sys.modules` vacío, por eso el mensaje sale una vez por ejecución. Se puede comprobar con:
    python -c "import sys; import cubesatplan; print('cubesatplan' in sys.modules)"

que imprime `True`.

Por último, imaginemos que más adelante tenemos `cubesatplan/geometry.py` con una función `geodetic_to_ecef`. Hay dos formas de usarla desde fuera:

```python
from cubesatplan.geometry import geodetic_to_ecef
from cubesatplan import geodetic_to_ecef
```
Aquí `geometry` es un **módulo** (un archivo `.py` con definiciones y sentencias) y `geodetic_to_ecef` es una **función** definida en él. Al importar un módulo, sus nombres no pasan al namespace actual, solo el nombre del módulo. Con `from cubesatplan import geodetic_to_ecef`, Python busca el nombre en el namespace de `__init__.py`, así que para que la segunda forma funcione, dentro de `__init__.py` hay que escribir:

```python
    from .geometry import geodetic_to_ecef
```
El punto indica una importación **relativa**: "desde el módulo `geometry` de este mismo paquete".

**Aunque Python 3 a veces funcione sin él, es útil crearlo** pues es el lugar donde se define la "cara pública" del paquete (qué nombres se pueden importar directamente) y, si hace falta, el código de inicialización.

**Fuentes:**
- [Python Tutorial: Modules, apartado Packages](https://docs.python.org/3/tutorial/modules.html#packages)
- [Python Reference: The import system, apartado Regular packages](https://docs.python.org/3/reference/import.html#regular-packages)
- [Python Library: `sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules)

---

### M0: parámetros

#### ¿Por qué los parámetros son entradas y no constantes?

Pendiente.