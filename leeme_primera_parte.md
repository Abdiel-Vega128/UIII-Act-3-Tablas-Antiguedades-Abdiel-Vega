

-----

## 📂 Estructura de Carpetas y Archivos

Aquí está la organización de los directorios y archivos de su proyecto Django:

```
UIII_Antiguedades_0395/
├── .venv/                         # 4. Entorno virtual
├── backend_Antiguedades/          # 8. Proyecto Django principal
│   ├── __init__.py
│   ├── settings.py                # 25. Configuración de la aplicación
│   ├── urls.py                    # 26. Enrutamiento principal
│   └── wsgi.py
├── app_Antiguedades/              # 11. Aplicación Django
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py                   # 27. Registro de modelos
│   ├── models.py                  # 12. Modelos de datos (Proveedor, Cliente, Pieza...)
│   ├── views.py                   # 14. Lógica de las vistas
│   ├── urls.py                    # 24. Rutas de la aplicación
│   └── templates/                 # 15. Carpeta de plantillas
│       ├── base.html              # 16, 17. Plantilla base con Bootstrap
│       ├── header.html            # 16. Cabecera (opcional, puede ir en base.html)
│       ├── navbar.html            # 16, 18. Barra de navegación
│       ├── footer.html            # 16, 19. Pie de página fijo
│       ├── inicio.html            # 16, 20. Página de inicio
│       └── proveedor/             # 21. Subcarpeta para plantillas de Proveedor
│           ├── agregar_proveedor.html     # 22. Formulario de adición
│           ├── ver_proveedores.html       # 22. Listado con botones
│           ├── actualizar_proveedor.html  # 22. Formulario de edición
│           └── borrar_proveedor.html      # 22. Confirmación de borrado
└── manage.py                      # Administrador de Django
```

-----

## ⚙️ Procedimientos de Configuración (Pasos 1-11)

A continuación, se detalla el procedimiento solicitado:

### 1\. Crear carpeta del Proyecto

Utilice su sistema operativo para crear la carpeta:

```bash
# En el Explorador de Archivos (Windows) o Finder (macOS):
# Cree una nueva carpeta llamada UIII_Antiguedades_0395

# Alternativamente, en la terminal:
mkdir UIII_Antiguedades_0395
cd UIII_Antiguedades_0395
```

### 2\. Abrir VS Code sobre la carpeta

Si está en la terminal (dentro de la carpeta `UIII_Antiguedades_0395`):

```bash
code .
```

Si está usando la interfaz gráfica de VS Code:

  * Vaya a **Archivo** \> **Abrir carpeta...**
  * Seleccione la carpeta `UIII_Antiguedades_0395`.

### 3\. Abrir terminal en VS Code

  * Vaya a **Terminal** en el menú superior.
  * Seleccione **Nueva terminal** (o use el atajo de teclado: `Ctrl + Shift + Ñ` o $\text{Ctrl} + \text{`}$ en Windows/Linux, $\text{Cmd} + \text{`}$ en macOS).

### 4\. Crear carpeta de entorno virtual `.venv`

Ejecute el siguiente comando en la terminal de VS Code:

```bash
python -m venv .venv
```

### 5\. Activar el entorno virtual

  * **Windows (PowerShell):**
    ```bash
    .venv\Scripts\Activate.ps1
    ```
  * **Windows (CMD):**
    ```bash
    .venv\Scripts\activate.bat
    ```
  * **macOS/Linux:**
    ```bash
    source .venv/bin/activate
    ```
    Verá `(.venv)` al inicio de su *prompt* de terminal.

### 6\. Activar intérprete de Python

Si el entorno virtual está activo (paso 5), el intérprete de Python ya está activo dentro de ese entorno.

### 7\. Instalar Django

Con el entorno virtual activo:

```bash
pip install django
```

### 8\. Crear proyecto `backend_Antiguedades` (sin duplicar carpeta)

Utilice el punto final (`.`) para crear el proyecto en el directorio actual, evitando una carpeta anidada.

```bash
django-admin startproject backend_Antiguedades .
```

### 9\. Ejecutar servidor en el puerto 8036

```bash
python manage.py runserver 8036
```

### 10\. Copiar y pegar el link en el navegador

El servidor Django mostrará la URL en la terminal, que generalmente es:
**`http://127.0.0.1:8036/`**

  * Copie esta dirección y péguela en su navegador.
  * Debería ver la página de bienvenida de Django ("The install worked successfully\! Congratulations\!").

### 11\. Crear aplicación `app_Antiguedades`

Detenga el servidor (`Ctrl + C`) y cree la aplicación:

```bash
python manage.py startapp app_Antiguedades
```

-----

## 💾 Procedimientos de Código y Configuración (Pasos 12-33)

### 12\. Modelo `models.py` (Ya proporcionado)

El código del modelo ya está incluido en su solicitud.

> **Ubicación:** `app_Antiguedades/models.py`

### 12.5 Realizar las migraciones

Ejecute en la terminal:

```bash
python manage.py makemigrations
python manage.py migrate
```

### 13\. y 14. Vistas en `views.py`

Cree las funciones en `app_Antiguedades/views.py`.

```python
# app_Antiguedades/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Proveedor
from django.http import HttpResponse

def inicio_antiguedades(request):
    """Página de inicio."""
    return render(request, 'inicio.html')

def agregar_proveedor(request):
    """Maneja la adición de un nuevo proveedor."""
    if request.method == 'POST':
        # Asumiendo que los campos coinciden con el modelo
        nombre_empresa = request.POST.get('nombre_empresa')
        contacto = request.POST.get('contacto')
        email = request.POST.get('email')
        telefono = request.POST.get('telefono')
        direccion = request.POST.get('direccion')
        especialidad = request.POST.get('especialidad')
        años_experiencia = request.POST.get('años_experiencia')
        
        # Crear y guardar el nuevo proveedor
        Proveedor.objects.create(
            nombre_empresa=nombre_empresa,
            contacto=contacto,
            email=email,
            telefono=telefono,
            direccion=direccion,
            especialidad=especialidad,
            años_experiencia=años_experiencia
        )
        # Redirigir a la lista de proveedores
        return redirect('ver_proveedores')
    
    return render(request, 'proveedor/agregar_proveedor.html')

def ver_proveedores(request):
    """Muestra la lista de todos los proveedores."""
    proveedores = Proveedor.objects.all()
    context = {'proveedores': proveedores}
    return render(request, 'proveedor/ver_proveedores.html', context)

def actualizar_proveedor(request, pk):
    """Muestra el formulario para actualizar un proveedor."""
    proveedor = get_object_or_404(Proveedor, pk=pk)
    context = {'proveedor': proveedor}
    return render(request, 'proveedor/actualizar_proveedor.html', context)

def realizar_actualizacion_proveedor(request, pk):
    """Procesa la actualización de un proveedor."""
    proveedor = get_object_or_404(Proveedor, pk=pk)
    if request.method == 'POST':
        proveedor.nombre_empresa = request.POST.get('nombre_empresa')
        proveedor.contacto = request.POST.get('contacto')
        proveedor.email = request.POST.get('email')
        proveedor.telefono = request.POST.get('telefono')
        proveedor.direccion = request.POST.get('direccion')
        proveedor.especialidad = request.POST.get('especialidad')
        proveedor.años_experiencia = request.POST.get('años_experiencia')
        proveedor.save()
        return redirect('ver_proveedores')
    return redirect('actualizar_proveedor', pk=pk) # Redirigir a la vista de formulario si no es POST

def borrar_proveedor(request, pk):
    """Muestra la confirmación para borrar un proveedor."""
    proveedor = get_object_or_404(Proveedor, pk=pk)
    if request.method == 'POST':
        # Borrado confirmado
        proveedor.delete()
        return redirect('ver_proveedores')
    
    # Vista de confirmación
    context = {'proveedor': proveedor}
    return render(request, 'proveedor/borrar_proveedor.html', context)

```

### 15\. Crear la carpeta `templates` (Hecho en la estructura inicial)

> **Ubicación:** `app_Antiguedades/templates/`

### 16\. y 21. Crear archivos HTML y subcarpeta `proveedor` (Hecho en la estructura inicial)

> **Ubicación:** `app_Antiguedades/templates/`
> **Ubicación:** `app_Antiguedades/templates/proveedor/`

### 17\. `base.html` (Incluir Bootstrap)

```html
{# app_Antiguedades/templates/base.html #}
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Tienda de Antigüedades{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">

    <style>
        /* Estilos suaves y modernos, colores claros */
        body {
            background-color: #f8f9fa; /* Gris muy claro */
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        .main-content {
            flex: 1; /* El contenido principal ocupa el espacio restante */
            padding-bottom: 70px; /* Espacio para el footer fijo */
        }
        .footer {
            background-color: #e9ecef; /* Gris más claro para el footer */
            color: #495057; /* Texto gris oscuro */
            padding: 10px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
            text-align: center;
            z-index: 1000;
        }
        .navbar-custom {
            background-color: #adb5bd; /* Tono de gris suave */
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %}

    <div class="container mt-4 main-content">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

### 18\. `navbar.html`

```html
{# app_Antiguedades/templates/navbar.html #}
<nav class="navbar navbar-expand-lg navbar-dark navbar-custom">
    <div class="container-fluid">
        <a class="navbar-brand text-dark" href="{% url 'inicio_antiguedades' %}">
            <i class="fas fa-hammer me-2"></i> Sistema de Administración Tienda de Antigüedades
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link active text-dark" aria-current="page" href="{% url 'inicio_antiguedades' %}">
                        <i class="fas fa-home me-1"></i> Inicio
                    </a>
                </li>
                
                {# Menú Proveedores #}
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-truck me-1"></i> Proveedores
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_proveedor' %}">Agregar</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_proveedores' %}">Ver</a></li>
                        {# Las rutas de Actualizar y Borrar se manejan desde la tabla de Ver #}
                    </ul>
                </li>
                
                {# Menú Clientes (Pendiente) #}
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-users me-1"></i> Clientes
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar</a></li>
                        <li><a class="dropdown-item" href="#">Ver</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar</a></li>
                        <li><a class="dropdown-item" href="#">Borrar</a></li>
                    </ul>
                </li>

                {# Menú Piezas de Antigüedad (Pendiente) #}
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-gem me-1"></i> Piezas de Antigüedad
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar</a></li>
                        <li><a class="dropdown-item" href="#">Ver</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar</a></li>
                        <li><a class="dropdown-item" href="#">Borrar</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

### 19\. `footer.html`

```html
{# app_Antiguedades/templates/footer.html #}
<footer class="footer">
    <div class="container">
        <p class="mb-1">
            &copy; {% now "Y" %} Derechos de Autor. | Fecha del Sistema: {% now "d/m/Y H:i" %}
        </p>
        <p class="mb-0">
            Creado por Ing. Abdiel Vega, Cbtis 128
        </p>
    </div>
</footer>
```

### 20\. `inicio.html`

```html
{# app_Antiguedades/templates/inicio.html #}
{% extends "base.html" %}

{% block title %}Inicio - Tienda de Antigüedades{% endblock %}

{% block content %}
<div class="text-center py-5">
    <h1 class="display-4 mb-4">Bienvenido al Sistema de Administración de Antigüedades</h1>
    <p class="lead">Este sistema permite gestionar eficientemente los proveedores, clientes y las valiosas piezas de nuestra colección.</p>
    
    <div class="row mt-5">
        <div class="col-md-6 offset-md-3">
            
            <p class="mt-3 text-muted">Explora la historia y administra tu inventario con facilidad.</p>
        </div>
    </div>
    
    <div class="row mt-5">
        <div class="col-md-4">
            <div class="card shadow-sm">
                <div class="card-body">
                    <h5 class="card-title text-success"><i class="fas fa-truck"></i> Proveedores</h5>
                    <p class="card-text">Gestiona quién nos suministra estas joyas del pasado.</p>
                    <a href="{% url 'ver_proveedores' %}" class="btn btn-outline-success">Ir a Proveedores</a>
                </div>
            </div>
        </div>
        <div class="col-md-4">
            <div class="card shadow-sm">
                <div class="card-body">
                    <h5 class="card-title text-info"><i class="fas fa-users"></i> Clientes</h5>
                    <p class="card-text">Administra los coleccionistas y compradores de nuestras piezas.</p>
                    <a href="#" class="btn btn-outline-info disabled">Ir a Clientes (Pendiente)</a>
                </div>
            </div>
        </div>
        <div class="col-md-4">
            <div class="card shadow-sm">
                <div class="card-body">
                    <h5 class="card-title text-warning"><i class="fas fa-gem"></i> Piezas</h5>
                    <p class="card-text">Inventario detallado de cada antigüedad.</p>
                    <a href="#" class="btn btn-outline-warning disabled">Ir a Piezas (Pendiente)</a>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

### 22\. Archivos HTML de Proveedor

#### `agregar_proveedor.html`

```html
{# app_Antiguedades/templates/proveedor/agregar_proveedor.html #}
{% extends "base.html" %}

{% block title %}Agregar Proveedor{% endblock %}

{% block content %}
<h2 class="mb-4">Agregar Nuevo Proveedor</h2>

<div class="card shadow-lg p-3 mb-5 bg-body rounded">
    <div class="card-body">
        <form method="post" action="{% url 'agregar_proveedor' %}">
            {% csrf_token %}
            
            <div class="row g-3">
                <div class="col-md-6">
                    <label for="id_nombre_empresa" class="form-label">Nombre de la empresa:</label>
                    <input type="text" class="form-control" id="id_nombre_empresa" name="nombre_empresa" required>
                </div>
                <div class="col-md-6">
                    <label for="id_contacto" class="form-label">Persona de contacto:</label>
                    <input type="text" class="form-control" id="id_contacto" name="contacto" required>
                </div>
                <div class="col-md-6">
                    <label for="id_email" class="form-label">Correo electrónico:</label>
                    <input type="email" class="form-control" id="id_email" name="email" required>
                </div>
                <div class="col-md-6">
                    <label for="id_telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="id_telefono" name="telefono">
                </div>
                <div class="col-12">
                    <label for="id_direccion" class="form-label">Dirección:</label>
                    <textarea class="form-control" id="id_direccion" name="direccion" rows="3" required></textarea>
                </div>
                <div class="col-md-6">
                    <label for="id_especialidad" class="form-label">Especialidad:</label>
                    <input type="text" class="form-control" id="id_especialidad" name="especialidad" required>
                </div>
                <div class="col-md-6">
                    <label for="id_años_experiencia" class="form-label">Años de experiencia:</label>
                    <input type="number" class="form-control" id="id_años_experiencia" name="años_experiencia" min="1" required>
                </div>
            </div>

            <div class="d-flex justify-content-end mt-4">
                <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary me-2">Cancelar</a>
                <button type="submit" class="btn btn-primary">Guardar Proveedor</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

#### `ver_proveedores.html`

```html
{# app_Antiguedades/templates/proveedor/ver_proveedores.html #}
{% extends "base.html" %}

{% block title %}Ver Proveedores{% endblock %}

{% block content %}
<h2 class="mb-4">Lista de Proveedores</h2>

<div class="d-flex justify-content-end mb-3">
    <a href="{% url 'agregar_proveedor' %}" class="btn btn-success"><i class="fas fa-plus me-1"></i> Agregar Proveedor</a>
</div>

{% if proveedores %}
<div class="table-responsive">
    <table class="table table-hover table-striped shadow-sm">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Empresa</th>
                <th>Contacto</th>
                <th>Email</th>
                <th>Especialidad</th>
                <th>Años Exp.</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for p in proveedores %}
            <tr>
                <td>{{ p.id }}</td>
                <td>{{ p.nombre_empresa }}</td>
                <td>{{ p.contacto }}</td>
                <td>{{ p.email }}</td>
                <td>{{ p.especialidad }}</td>
                <td>{{ p.años_experiencia }}</td>
                <td class="text-nowrap">
                    <a href="{% url 'actualizar_proveedor' p.id %}" class="btn btn-sm btn-info" title="Ver/Editar"><i class="fas fa-edit"></i></a>
                    <a href="{% url 'borrar_proveedor' p.id %}" class="btn btn-sm btn-danger" title="Borrar"><i class="fas fa-trash-alt"></i></a>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</div>
{% else %}
<div class="alert alert-warning" role="alert">
    No hay proveedores registrados.
</div>
{% endif %}
{% endblock %}
```

#### `actualizar_proveedor.html`

```html
{# app_Antiguedades/templates/proveedor/actualizar_proveedor.html #}
{% extends "base.html" %}

{% block title %}Actualizar Proveedor{% endblock %}

{% block content %}
<h2 class="mb-4">Actualizar Proveedor: **{{ proveedor.nombre_empresa }}**</h2>

<div class="card shadow-lg p-3 mb-5 bg-body rounded">
    <div class="card-body">
        <form method="post" action="{% url 'realizar_actualizacion_proveedor' proveedor.id %}">
            {% csrf_token %}
            
            <div class="row g-3">
                <div class="col-md-6">
                    <label for="id_nombre_empresa" class="form-label">Nombre de la empresa:</label>
                    <input type="text" class="form-control" id="id_nombre_empresa" name="nombre_empresa" value="{{ proveedor.nombre_empresa }}" required>
                </div>
                <div class="col-md-6">
                    <label for="id_contacto" class="form-label">Persona de contacto:</label>
                    <input type="text" class="form-control" id="id_contacto" name="contacto" value="{{ proveedor.contacto }}" required>
                </div>
                <div class="col-md-6">
                    <label for="id_email" class="form-label">Correo electrónico:</label>
                    <input type="email" class="form-control" id="id_email" name="email" value="{{ proveedor.email }}" required>
                </div>
                <div class="col-md-6">
                    <label for="id_telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="id_telefono" name="telefono" value="{{ proveedor.telefono|default_if_none:'' }}">
                </div>
                <div class="col-12">
                    <label for="id_direccion" class="form-label">Dirección:</label>
                    <textarea class="form-control" id="id_direccion" name="direccion" rows="3" required>{{ proveedor.direccion }}</textarea>
                </div>
                <div class="col-md-6">
                    <label for="id_especialidad" class="form-label">Especialidad:</label>
                    <input type="text" class="form-control" id="id_especialidad" name="especialidad" value="{{ proveedor.especialidad }}" required>
                </div>
                <div class="col-md-6">
                    <label for="id_años_experiencia" class="form-label">Años de experiencia:</label>
                    <input type="number" class="form-control" id="id_años_experiencia" name="años_experiencia" min="1" value="{{ proveedor.años_experiencia }}" required>
                </div>
            </div>

            <div class="d-flex justify-content-end mt-4">
                <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary me-2">Cancelar</a>
                <button type="submit" class="btn btn-primary">Actualizar Proveedor</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

#### `borrar_proveedor.html`

```html
{# app_Antiguedades/templates/proveedor/borrar_proveedor.html #}
{% extends "base.html" %}

{% block title %}Borrar Proveedor{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card border-danger shadow-lg">
            <div class="card-header bg-danger text-white">
                <h4 class="mb-0"><i class="fas fa-exclamation-triangle me-2"></i> Confirmar Eliminación</h4>
            </div>
            <div class="card-body">
                <p class="card-text fs-5">
                    ¿Está **seguro** de que desea eliminar el proveedor:
                    <br>
                    <strong>{{ proveedor.nombre_empresa }}</strong> (Contacto: {{ proveedor.contacto }})?
                </p>
                <p class="text-muted">Esta acción es irreversible.</p>
                
                <form method="post" action="{% url 'borrar_proveedor' proveedor.id %}">
                    {% csrf_token %}
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-4">
                        <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary">Cancelar</a>
                        <button type="submit" class="btn btn-danger"><i class="fas fa-trash-alt me-1"></i> Confirmar Eliminación</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

### 23\. No utilizar `forms.py`

(Cumplido: Se usan formularios HTML directamente con el objeto `request.POST` en `views.py`).

### 24\. Crear `urls.py` en `app_Antiguedades`

> **Ubicación:** `app_Antiguedades/urls.py`

```python
# app_Antiguedades/urls.py
from django.urls import path
from . import views

urlpatterns = [
    # General
    path('', views.inicio_antiguedades, name='inicio_antiguedades'),

    # CRUD Proveedores
    path('proveedores/', views.ver_proveedores, name='ver_proveedores'),
    path('proveedores/agregar/', views.agregar_proveedor, name='agregar_proveedor'),
    path('proveedores/actualizar/<int:pk>/', views.actualizar_proveedor, name='actualizar_proveedor'),
    path('proveedores/actualizar/submit/<int:pk>/', views.realizar_actualizacion_proveedor, name='realizar_actualizacion_proveedor'),
    path('proveedores/borrar/<int:pk>/', views.borrar_proveedor, name='borrar_proveedor'),
]
```

### 25\. Agregar `app_Antiguedades` en `settings.py`

> **Ubicación:** `backend_Antiguedades/settings.py`

Asegúrese de agregar el nombre de la aplicación a la lista `INSTALLED_APPS`:

```python
# backend_Antiguedades/settings.py

# ...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # NUESTRA APLICACIÓN
    'app_Antiguedades', 
]
# ...
```

### 26\. Configurar el `urls.py` principal

> **Ubicación:** `backend_Antiguedades/urls.py`

```python
# backend_Antiguedades/urls.py
from django.contrib import admin
from django.urls import path, include # Importar include

urlpatterns = [
    path('admin/', admin.site.urls),
    # Enlazar la aplicación principal
    path('', include('app_Antiguedades.urls')), 
]
```

### 27\. Registrar los modelos en `admin.py` y volver a realizar migraciones

> **Ubicación:** `app_Antiguedades/admin.py`

```python
# app_Antiguedades/admin.py
from django.contrib import admin
from .models import Proveedor, Cliente, PiezaAntiguedad, ClienteProveedor, CompraPieza

# Registrar todos los modelos, aunque se trabaje solo con Proveedor por ahora
admin.site.register(Proveedor)
admin.site.register(Cliente)
admin.site.register(PiezaAntiguedad)
admin.site.register(ClienteProveedor)
admin.site.register(CompraPieza)
```

**Volver a ejecutar las migraciones** (necesario después de modificar `admin.py` y para asegurar todas las tablas):

```bash
python manage.py makemigrations
python manage.py migrate
```

*Opcional: Si no ha creado un superusuario, cree uno para acceder al admin:*

```bash
python manage.py createsuperuser
```

### 28., 29., 30., 31., 32. (Cumplidos con el código y estructura proporcionada)

### 33\. Ejecutar el servidor en el puerto 8042

Finalmente, ejecute el servidor en el puerto solicitado:

```bash
python manage.py runserver 8042
```

El link de acceso al proyecto funcional es: **`http://127.0.0.1:8042/`**

-----

¿Le gustaría que le muestre el contenido del archivo `base.html` o `navbar.html` con la implementación de **Bootstrap y los estilos suaves** para verificar el diseño?
