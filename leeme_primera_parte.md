1–11: Procedimientos básicos (crear carpeta, abrir VS Code, virtualenv, intérprete, instalar Django, crear proyecto/app, ejecutar servidor)
1. Crear la carpeta del proyecto

Desde tu explorador o desde terminal:

# en la carpeta donde quieras crear el proyecto:
mkdir UIII_Antiguedades_0395
cd UIII_Antiguedades_0395

2. Abrir VS Code sobre la carpeta

En la carpeta UIII_Antiguedades_0395:

code .


(O abre VS Code y selecciona File → Open Folder... y elige UIII_Antiguedades_0395.)

3. Abrir terminal en VS Code

Menú: Terminal → New Terminal

Atajo: `Ctrl + `` (la tecla de tilde invertida, junto al 1).

4. Crear carpeta de entorno virtual .venv desde terminal

Recomendado crear .venv dentro del proyecto para que VSCode lo detecte:

# Windows (PowerShell)
python -m venv .venv

# Windows (cmd)
py -m venv .venv

# macOS / Linux
python3 -m venv .venv

5. Activar el entorno virtual
# PowerShell (Windows)
.venv\Scripts\Activate.ps1

# Si da error de ejecución en PowerShell (política), en consola:
# Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
# luego volver a ejecutar la activación.

# CMD (Windows)
.venv\Scripts\activate.bat

# macOS / Linux (bash/zsh)
source .venv/bin/activate


Al activarlo verás (.venv) al inicio del prompt.

6. Activar intérprete de Python en VS Code

Ctrl+Shift+P → Python: Select Interpreter → elegir la ruta que apunte a UIII_Antiguedades_0395/.venv/...

Confirma que la barra inferior muestra el intérprete .venv.

7. Instalar Django

Con el entorno activo:

pip install django


(Para versiones fijas: pip install "django>=4.2,<5.0" si quieres asegurar compatibilidad.)

8. Crear proyecto Django backend_Antiguedades (sin duplicar carpeta)

Desde la carpeta UIII_Antiguedades_0395:

django-admin startproject backend_Antiguedades .


Importante: el . final crea el proyecto en la carpeta actual evitando duplicar (no uses startproject backend_Antiguedades sin . si ya estás dentro de la carpeta deseada).

Esto crea manage.py y la carpeta backend_Antiguedades/ con settings.py, urls.py, etc.

9. Ejecutar servidor en puerto 8036

Con entorno activo y desde la raíz del proyecto (donde está manage.py):

python manage.py runserver 8036


Visita http://127.0.0.1:8036/ en el navegador (ver paso 10).

10. Copiar y pegar el link en el navegador

Abre tu navegador y pega http://127.0.0.1:8036/ (ó http://localhost:8036/).

11. Crear aplicación app_Antiguedades

Desde la raíz del proyecto:

python manage.py startapp app_Antiguedades

12: Ya tienes el models.py (lo pegaste).

(Has incluido Proveedor, Cliente, PiezaAntiguedad y modelos intermedios. Perfecto.)

12.5 Migraciones (procedimiento)
python manage.py makemigrations
python manage.py migrate

13–14: Trabajamos primero con el MODELO: PROVEEDOR y vistas en views.py

A continuación te doy el contenido listo para pegar en app_Antiguedades/views.py. Estas son vistas basadas en funciones (sin forms.py) usando HTML en templates.

# app_Antiguedades/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.urls import reverse
from .models import Proveedor
from django.http import HttpResponse

# Página de inicio del módulo/tienda
def inicio_antiguedades(request):
    contexto = {
        'titulo': 'Sistema de Administración Tienda de Antigüedades'
    }
    return render(request, 'inicio.html', contexto)

# Agregar proveedor (GET muestra formulario, POST guarda)
def agregar_proveedor(request):
    if request.method == 'POST':
        nombre_empresa = request.POST.get('nombre_empresa')
        contacto = request.POST.get('contacto')
        email = request.POST.get('email')
        telefono = request.POST.get('telefono')
        direccion = request.POST.get('direccion')
        especialidad = request.POST.get('especialidad')
        años_experiencia = request.POST.get('años_experiencia') or 1

        proveedor = Proveedor.objects.create(
            nombre_empresa=nombre_empresa,
            contacto=contacto,
            email=email,
            telefono=telefono,
            direccion=direccion,
            especialidad=especialidad,
            años_experiencia=int(años_experiencia)
        )
        return redirect('ver_proveedores')
    return render(request, 'proveedor/agregar_proveedor.html')

# Vista para listar proveedores
def ver_proveedores(request):
    proveedores = Proveedor.objects.all()
    return render(request, 'proveedor/ver_proveedores.html', {'proveedores': proveedores})

# Mostrar formulario para actualizar (GET) y guardarlo (POST)
def actualizar_proveedor(request, proveedor_id):
    proveedor = get_object_or_404(Proveedor, pk=proveedor_id)
    if request.method == 'POST':
        proveedor.nombre_empresa = request.POST.get('nombre_empresa')
        proveedor.contacto = request.POST.get('contacto')
        proveedor.email = request.POST.get('email')
        proveedor.telefono = request.POST.get('telefono')
        proveedor.direccion = request.POST.get('direccion')
        proveedor.especialidad = request.POST.get('especialidad')
        proveedor.años_experiencia = int(request.POST.get('años_experiencia') or proveedor.años_experiencia)
        proveedor.save()
        return redirect('ver_proveedores')
    return render(request, 'proveedor/actualizar_proveedor.html', {'proveedor': proveedor})

# Página de confirmación y eliminación
def borrar_proveedor(request, proveedor_id):
    proveedor = get_object_or_404(Proveedor, pk=proveedor_id)
    if request.method == 'POST':
        proveedor.delete()
        return redirect('ver_proveedores')
    return render(request, 'proveedor/borrar_proveedor.html', {'proveedor': proveedor})


Nombres de vistas (para urls.py) que usaré abajo: inicio_antiguedades, agregar_proveedor, ver_proveedores, actualizar_proveedor, borrar_proveedor.

15–22: Estructura de templates y archivos a crear

Crea la carpeta templates dentro de app_Antiguedades y dentro de ella proveedor/.

Estructura:

app_Antiguedades/
  templates/
    base.html
    header.html
    navbar.html
    footer.html
    inicio.html
    proveedor/
      agregar_proveedor.html
      ver_proveedores.html
      actualizar_proveedor.html
      borrar_proveedor.html


A continuación te doy contenido mínimo y funcional para cada archivo.

templates/base.html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>{% block title %}Tienda de Antigüedades{% endblock %}</title>

  <!-- Bootstrap CSS (CDN) -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body { background-color: #f7f9fb; color:#2c3e50; }
    .soft-card { border-radius: 12px; box-shadow: 0 4px 12px rgba(20,20,20,0.06); background: #ffffff; }
    footer.fixed-bottom { background: #f1f5f9; padding: 10px 0; }
  </style>
  {% block head %}{% endblock %}
</head>
<body>
  {% include 'navbar.html' %}
  <main class="container my-4">
    {% block content %}{% endblock %}
  </main>
  {% include 'footer.html' %}

  <!-- Bootstrap JS (Popper + Bundle) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
  {% block scripts %}{% endblock %}
</body>
</html>

templates/navbar.html
<nav class="navbar navbar-expand-lg navbar-light" style="background:#ffffff;">
  <div class="container">
    <a class="navbar-brand d-flex align-items-center" href="{% url 'inicio_antiguedades' %}">
      <!-- ícono simple usando emoji o svg -->
      <span style="font-size:1.4rem; margin-right:.5rem;">🕰️</span>
      Sistema de Administración Tienda de Antigüedades
    </a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio_antiguedades' %}">Inicio</a></li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#"> 
            🧾 Proveedores
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_proveedor' %}">Agregar</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_proveedores' %}">Ver</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#"> 🧑‍🤝‍🧑 Clientes</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar</a></li>
            <li><a class="dropdown-item" href="#">Ver</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#"> 🖼️ Piezas</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar</a></li>
            <li><a class="dropdown-item" href="#">Ver</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>


Observación: pediste íconos en secciones principales (usé emojis para simplicidad). Si quieres SVG o iconos externos, los añadimos.

templates/footer.html
<footer class="fixed-bottom text-center">
  <div class="container">
    <small>© {{ now.year }} — Creado por Ing. Abdiel Vega, Cbtis 128</small>
  </div>
</footer>


now se puede pasar desde contexto o usar plantilla {{ now|date:"Y" }} si usas el contexto django.template.context_processors.request y now. Si quieres simple: reemplaza {{ now.year }} por 2025 o usa {% now "Y" %}.

Usa:

<small>© {% now "Y" %} — Creado por Ing. Abdiel Vega, Cbtis 128</small>

templates/inicio.html
{% extends 'base.html' %}
{% block title %}Inicio - Tienda de Antigüedades{% endblock %}
{% block content %}
<div class="row g-3">
  <div class="col-md-8">
    <div class="p-4 soft-card">
      <h1>Bienvenido al Sistema de Administración</h1>
      <p>Gestiona proveedores, clientes y piezas de antigüedad. Diseño con colores suaves y componentes fáciles de usar.</p>
    </div>
  </div>
  <div class="col-md-4">
    <div class="soft-card p-3">
      <img src="https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?w=900&q=80" alt="Tienda de antigüedades" class="img-fluid rounded">
      <p class="mt-2 small">Imagen de colección/tienda</p>
    </div>
  </div>
</div>
{% endblock %}


La imagen viene de Unsplash (URL pública). Si no quieres depender de internet, reemplaza por una imagen propia/estática.

Plantillas en templates/proveedor/
agregar_proveedor.html
{% extends 'base.html' %}
{% block title %}Agregar Proveedor{% endblock %}
{% block content %}
<div class="soft-card p-4">
  <h3>Agregar Proveedor</h3>
  <form method="post">
    {% csrf_token %}
    <div class="mb-3">
      <label class="form-label">Nombre de la empresa</label>
      <input class="form-control" name="nombre_empresa" required>
    </div>
    <div class="mb-3">
      <label class="form-label">Persona de contacto</label>
      <input class="form-control" name="contacto" required>
    </div>
    <div class="mb-3">
      <label class="form-label">Correo electrónico</label>
      <input class="form-control" type="email" name="email" required>
    </div>
    <div class="mb-3">
      <label class="form-label">Teléfono</label>
      <input class="form-control" name="telefono">
    </div>
    <div class="mb-3">
      <label class="form-label">Dirección</label>
      <textarea class="form-control" name="direccion"></textarea>
    </div>
    <div class="mb-3">
      <label class="form-label">Especialidad</label>
      <input class="form-control" name="especialidad">
    </div>
    <div class="mb-3">
      <label class="form-label">Años de experiencia</label>
      <input class="form-control" name="años_experiencia" type="number" min="1" value="1">
    </div>
    <button class="btn btn-primary">Guardar</button>
    <a class="btn btn-secondary" href="{% url 'ver_proveedores' %}">Cancelar</a>
  </form>
</div>
{% endblock %}

ver_proveedores.html
{% extends 'base.html' %}
{% block title %}Proveedores{% endblock %}
{% block content %}
<div class="soft-card p-3">
  <div class="d-flex justify-content-between align-items-center mb-3">
    <h3>Proveedores</h3>
    <a class="btn btn-success" href="{% url 'agregar_proveedor' %}">+ Agregar proveedor</a>
  </div>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>Empresa</th><th>Contacto</th><th>Email</th><th>Teléfono</th><th>Especialidad</th><th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      {% for p in proveedores %}
      <tr>
        <td>{{ p.nombre_empresa }}</td>
        <td>{{ p.contacto }}</td>
        <td>{{ p.email }}</td>
        <td>{{ p.telefono }}</td>
        <td>{{ p.especialidad }}</td>
        <td>
          <a class="btn btn-sm btn-info" href="{% url 'actualizar_proveedor' p.id %}">Editar</a>
          <a class="btn btn-sm btn-danger" href="{% url 'borrar_proveedor' p.id %}">Borrar</a>
        </td>
      </tr>
      {% empty %}
      <tr><td colspan="6">No hay proveedores registrados.</td></tr>
      {% endfor %}
    </tbody>
  </table>
</div>
{% endblock %}

actualizar_proveedor.html
{% extends 'base.html' %}
{% block title %}Actualizar Proveedor{% endblock %}
{% block content %}
<div class="soft-card p-4">
  <h3>Actualizar Proveedor</h3>
  <form method="post">
    {% csrf_token %}
    <div class="mb-3"><label class="form-label">Nombre de la empresa</label>
      <input class="form-control" name="nombre_empresa" value="{{ proveedor.nombre_empresa }}"></div>
    <div class="mb-3"><label class="form-label">Persona de contacto</label>
      <input class="form-control" name="contacto" value="{{ proveedor.contacto }}"></div>
    <div class="mb-3"><label class="form-label">Correo electrónico</label>
      <input class="form-control" name="email" value="{{ proveedor.email }}"></div>
    <div class="mb-3"><label class="form-label">Teléfono</label>
      <input class="form-control" name="telefono" value="{{ proveedor.telefono }}"></div>
    <div class="mb-3"><label class="form-label">Dirección</label>
      <textarea class="form-control" name="direccion">{{ proveedor.direccion }}</textarea></div>
    <div class="mb-3"><label class="form-label">Especialidad</label>
      <input class="form-control" name="especialidad" value="{{ proveedor.especialidad }}"></div>
    <div class="mb-3"><label class="form-label">Años de experiencia</label>
      <input class="form-control" name="años_experiencia" value="{{ proveedor.años_experiencia }}" type="number" min="1"></div>
    <button class="btn btn-primary">Actualizar</button>
    <a class="btn btn-secondary" href="{% url 'ver_proveedores' %}">Cancelar</a>
  </form>
</div>
{% endblock %}

borrar_proveedor.html
{% extends 'base.html' %}
{% block title %}Borrar Proveedor{% endblock %}
{% block content %}
<div class="soft-card p-4">
  <h3>Eliminar Proveedor</h3>
  <p>¿Estás seguro que deseas eliminar al proveedor <strong>{{ proveedor.nombre_empresa }}</strong>?</p>
  <form method="post">
    {% csrf_token %}
    <button class="btn btn-danger">Sí, eliminar</button>
    <a class="btn btn-secondary" href="{% url 'ver_proveedores' %}">Cancelar</a>
  </form>
</div>
{% endblock %}

24–26: urls.py de la app y del proyecto, y agregar app en settings.py
app_Antiguedades/urls.py (crear)
# app_Antiguedades/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_antiguedades, name='inicio_antiguedades'),
    path('proveedores/agregar/', views.agregar_proveedor, name='agregar_proveedor'),
    path('proveedores/', views.ver_proveedores, name='ver_proveedores'),
    path('proveedores/actualizar/<int:proveedor_id>/', views.actualizar_proveedor, name='actualizar_proveedor'),
    path('proveedores/borrar/<int:proveedor_id>/', views.borrar_proveedor, name='borrar_proveedor'),
]

backend_Antiguedades/urls.py (modificar para incluir la app)
# backend_Antiguedades/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Antiguedades.urls')),  # rutas de la app en la raíz
]

Agregar app_Antiguedades en settings.py

En backend_Antiguedades/settings.py, dentro de INSTALLED_APPS agrega:

INSTALLED_APPS = [
    # apps por defecto...
    'django.contrib.staticfiles',
    # ...
    'app_Antiguedades',
]


También asegúrate de que TEMPLATES tenga 'APP_DIRS': True, (por defecto sí) para que Django busque templates/ dentro de apps.

27: Registrar modelos en admin.py y migraciones
app_Antiguedades/admin.py
from django.contrib import admin
from .models import Proveedor, Cliente, PiezaAntiguedad, ClienteProveedor, CompraPieza

@admin.register(Proveedor)
class ProveedorAdmin(admin.ModelAdmin):
    list_display = ('nombre_empresa', 'contacto', 'email', 'telefono', 'especialidad', 'años_experiencia')
    search_fields = ('nombre_empresa', 'contacto', 'email')

# Registrar (aunque por ahora solo usarás Proveedor en la UI).
admin.site.register(Cliente)
admin.site.register(PiezaAntiguedad)
admin.site.register(ClienteProveedor)
admin.site.register(CompraPieza)


Luego:

python manage.py makemigrations
python manage.py migrate


Si quieres crear superusuario:

python manage.py createsuperuser

28: Por ahora trabajar sólo con Proveedor

En las vistas y templates dejamos Cliente y PiezaAntiguedad pendientes (no las implementamos en la interfaz). Ya están en models.py y en admin.py.

29: Colores suaves en diseño

Ya incluí paleta suave en base.html y soft-card con sombra y fondo blanco sobre #f7f9fb. Puedes ajustar CSS en base.html o crear static/css/styles.css y añadirlo.

30: No validar entradas de datos

Tal como pediste, los formularios no realizan validaciones extras del lado servidor (más allá de requisitos mínimos HTML). Si más adelante quieres validar, podemos agregar.

31: Crear toda la estructura de carpetas y archivos antes de comenzar

Resumen comando para crear carpetas vacías (ejecutar desde raíz del proyecto):

mkdir -p app_Antiguedades/templates/proveedor
# Crear archivos vacíos (Linux/mac)
touch app_Antiguedades/templates/base.html app_Antiguedades/templates/navbar.html app_Antiguedades/templates/footer.html app_Antiguedades/templates/inicio.html
touch app_Antiguedades/templates/proveedor/agregar_proveedor.html app_Antiguedades/templates/proveedor/ver_proveedores.html app_Antiguedades/templates/proveedor/actualizar_proveedor.html app_Antiguedades/templates/proveedor/borrar_proveedor.html


En Windows PowerShell usa New-Item -Path . -ItemType File.

32: Proyecto totalmente funcional

Con los archivos y código que te proporcioné, el CRUD de Proveedor funcionará (crear, ver, actualizar, borrar). Admin panel también registrado.

33: Ejecutar servidor en puerto 8042 (final)

Si quieres iniciar el servidor en el puerto final que indicas:

python manage.py runserver 8042


Visita http://127.0.0.1:8042/.

(Recuerda que en el paso 9 también pediste puerto 8036 — puedes arrancar en cualquiera de los dos puertos según necesites.)

Resumen de archivos clave (lista rápida)

UIII_Antiguedades_0395/

.venv/

manage.py

backend_Antiguedades/ (settings.py, urls.py, ...)

app_Antiguedades/

models.py (ya dado)

views.py (contenido de arriba)

urls.py (contenido de arriba)

admin.py (contenido de arriba)

templates/ (base.html, navbar.html, footer.html, inicio.html)

proveedor/ (4 htmls: agregar, ver, actualizar, borrar)
