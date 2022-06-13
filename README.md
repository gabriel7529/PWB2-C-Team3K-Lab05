<div align="center">
<table>
<theader>
<tr>
<td><img src="https://github.com/elopezqu/Lab2_Team3K/blob/main/epis.png" alt="EPIS" style="width:50%; height:auto"/></td>
<th>
<span style="font-weight:bold;">UNIVERSIDAD NACIONAL DE SAN AGUSTIN</span><br />
<span style="font-weight:bold;">FACULTAD DE INGENIERÍA DE PRODUCCIÓN Y SERVICIOS</span><br />
<span style="font-weight:bold;">ESCUELA PROFESIONAL DE INGENIERÍA DE SISTEMAS</span>
</th>
<td><img src="https://github.com/elopezqu/Lab2_Team3K/blob/main/abet.png" alt="ABET" style="width:50%; height:auto"/></td>
</tr>
</theader>
<tbody>
<tr><td colspan="3"><span style="font-weight:bold;">Formato</span>: Guía de Práctica de Laboratorio</td></tr>
<tr><td><span style="font-weight:bold;">Aprobación</span>:  2022/03/01</td><td><span style="font-weight:bold;">Código</span>: GUIA-PRLD-001</td><td><span style="font-weight:bold;">Página</span>: 1</td></tr>
</tbody>
</table>
</div>
<div align="center">
<h3>INFORME DE LABORATORIO</h3>
</div>
<table>
<theader>
<tr><th colspan="6" bgcolor="red">INFORMACIÓN BÁSICA</th></tr>
</theader>
<tbody>
<tr><td>ASIGNATURA:</td><td colspan="5">Programación Web 2</td></tr>
<tr><td>TÍTULO DE LA PRACTICA:</td><td colspan="5">Django</td></tr>
<tr><td>NÚMERO DE PRÁCTICA:</td><td>Practica de Laboratorio 05</td><td>AÑO LECTIVO:</td><td>2022 A</td><td>NRO. SEMESTRE:</td><td>III</td></tr>
<tr><td>FECHA DE PRESENTACIÓN:</td><td>12/05/2022</td><td>HORA DE PRESENTACIÓN:</td><td colspan="3">11:30 p.m.</td></tr>
<tr><td>INTEGRANTES:</td><td colspan="3">- Edson Joel López Quispe<br>- Gabriel Steven Machicao Quispe<br>- Fernando Coyla Alvarez</td><td>NOTA:</td><td>...</td></tr>
<tr><td>DOCENTE:</td><td colspan="5">Richart Smith Escobedo Quispe - rescobedoq@unsa.edu.pe</td></tr>
</tbody>
</table>
<table>
<theader>
<tr><th>SOLUCIÓN Y RESULTADOS</th></tr>
</theader>
<tbody>
<tr><td>I. SOLUCIÓN DE EJERCICIOS/PROBLEMAS:Para la creación de nuestra página web se tuvo que recurrir a los siguientes pasos:<br> - Primeramentes se tuvo que crear el proyecto con ayuda del Django, para que nos ofrezca los recursos para iniciar con nuestra página, además de configurarla a nuestro idioma, la zona donde noes encontramos y también creamos la base de datos<br><img src='imagenes/inicioDjango.png' alt='inicio'><br> 2. Luego se crea un modelo en el cuál se pueda poner los objetos que tendrá nuestro post, en el cual se vera el titulo, el autor, el dia creado y publico, aparte del texto que va contener<br>
  <em>Código del modelo del blog</em>
  <pre>
  from django.conf import settings
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)
    def publish(self):
        self.published_date = timezone.now()
        self.save()
    def __str__(self):
        return self.title
  </pre>
  3. Luego tuvimos crear al super-usuario, para que pueda manipular y crear los post en la base de datos.<br>
  <img src= 'imagen/baseDatos' src='GUI de la pagina donde contiene los post'><br>
  4. Una vez tenida la base datos y su modelo, se inicia con la modifcación con el diseño de la página, la cual la vamos a hacer modificaciones en el codigo de Django, manipulando los accesos para redirrecionarlos a una función que va ejecutar la página web que deseamos.<br><br>
  <em>Código de rutas de url</em>
  <pre>
  from django.contrib import admin
from django.urls import path, include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
  </pre>
  
  <em>Código que renderizará la página</em><br>
  <pre>
  def post_list(request):
    return render(request, 'blog/post_list.html', {})
  </pre>
  
  5. Se crea una carpeta la cuál se ubicara los archivos html que se van renderizar las páginas dependiendo de la ruta<br>
  <em>Ruta de ubicación </em><br>
  <pre>
  blog
└───templates
    └───blog
  </pre><br>
  <em> Django shell </em><br>
  - En esta parte entramos a la consola de Django en la cual podremos crear Post, asimismo podemos usar filtros para buscar nuestros post creados.<br>
  
  <em> Datos dinámicos en plantillas </em><br>
  - Luego de realizar los post, aqui modificamos el archivo blog/views.py para publicar los post que se haya crreado
  - Despues en el archivo blog/templates/blog/post_list.html agregamos {% for %} y {% endfor %} para mostrar el contenido de nuestros post
   <br><pre>
{% for post in posts %}
        publicado: {{ post.published_date }}
        {{ post.title }}
        {{ post.text|linebreaksbr }}    
        {% endfor %}
//De esta manera mostramos todos nuestros Post con su contenido
   </pre>
  
<em> Agregando CSS </em><br>
  - Aqui en la carpeta "blog/static/css" agregamos contenido CSS para dar estilo a nuestra pagina, aqui resulta importante agregar en "blog/templates/blog/post_list.html" 
  <br><pre>
  //la linea de codigo
  {% load static %}
  //Ademas agregamos lo siguiente para que reconzca nuestro archivo CSS
    \<link rel="stylesheet" href="{% static 'css/blog.css' %}"\>
  </pre><br>
  <em> Lanzamiento de la primera página de mi blog </em> <br><br>-Posteriormente para mejorar el dinamismo de las paginas html, se hace uso de plantillas, que sirve para reutilizar partes del HTML de diferentes paginas web, las paginas web usadas son: <li>base.html</li><li>post_detail.html</li><li>post_edit.html</li><li>post_list.html</li>Cada una de estas paginas debe tener su URL que se modifica en <em>urls.py</em>, para poder editar las publicaciones se necesita un formulario, asi que se crea el mismo en la carpeta blog con el nombre de <em>forms.py</em>. Ya por último agregar las vistas para cada uan de las páginas, en el archivo<em>views.py</em> y lanzar el servidor
  </td></tr>
<tr><td>II. SOLUCIÓN DEL CUESTIONARIO:<br><strong><em>1. ¿Cuál es un estándar de codificación para Python?</em></strong><br><p>En el caso de Python usa una codificación de PEP 8, donde esta convención trae consigo una guía para escribir código de Python, esta basado en la guía realizado por Guido y algunas ediciones de la guía de estilo de Barry. La guia esta en constante cambio; el link de este estandar es:  https://legacy.python.org/dev/peps/pep-0008/#introduction.</p><strong><em>2. ¿Qué diferencias existen entre EasyInstall, pip, y PyPM?</em></strong><p>easy_install esta actualmente en desuso y fue reemplazado por pip, ya que brindaba mejores funciones en cuanto a las instalaciones de paquetes y la ayuda de PEP 438, que este tuvo varias mejorar con repecto a easy_install, por otro lado PyPM es un administrador d paquetes de Python, donde esta permite administrar los proyectos, que a diferencia de pip este soloque es un instalador de paquetes y los administra.</p><strong><em> 3.  ¿Qué otros tipos de archivos se deberían agregar a este archivo?</em></strong><p>A parte de lo mencionado también se pude considerar tambipen la base de datos, el cual es db.squite3, además de de los .eggs/ y los archivos pip-log.txt que sirve para los paquetes instalados y son efímeros en cuanto al desarrollo del código.</p><strong><em>4. Utilice python manage.py shell para agregar objetos. ¿Qué archivos se modificaron al agregar más objetos?</em></strong><p>...</p></td></tr>
  <tr><td>III. CONCLUSIONES:<br><p>Este laboratorio nos mostró cómo poder crear un Blog de una manera más optimizada, utilizando tecnologías como Django, este Framework, basado en python permite crear paginas web, gracias a su diseño: modelo-vista-controlador, que permite ahorrar tiempo. Lo aprendido en este Lab será de mucha ayuda para posteriores trabajos que vayamos hacer con este Framework.</p></td><</tr>
</tbody>
</table>

<table>
<theader>
  <tr><td>RETROALIMENTACIÓN GENERAL</td><br><tr>
</theader>
<tbody>
  <tr><td><p>...</p></td></tr>
</tbody>
</table>

<table>
<theader>
<tr><td>REFERENCIAS Y BIBLIOGRAFÍA</td><tr>
</theader>
<tbody>
<tr><td>[1] ¿Qué es Django? · HonKit. (s. f.). Choose a language · HonKit. https://tutorial.djangogirls.org/es/django/<br>
[2] User Guide - pip documentation v22.2.dev0. (s. f.). pip documentation v22.1.2. https://pip.pypa.io/en/latest/user_guide/<br>
[3] Django documentation | Django documentation | Django. (s. f.). Django documentation | Django documentation | Django. https://docs.djangoproject.com/en/4.0/<br>
[4] The Python Tutorial — Python 3.10.5 documentation. (s. f.). 3.10.5 Documentation. https://docs.python.org/3/tutorial/<br>
[5] Tutorial Django Parte 3: Uso de modelos - Aprende sobre desarrollo web | MDN. (s. f.). MDN Web Docs. https://developer.mozilla.org/es/docs/Learn/Server-side/Django/Models<br>
[6] Installing Packages — Python Packaging User Guide. (s. f.). Python Packaging User Guide — Python Packaging User Guide. https://packaging.python.org/en/latest/tutorials/installing-packages/<br></tr>
</tbody>
</table>
