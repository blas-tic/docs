# CREAR UN PROYECTO BLAZOR:

## BLAZOR WEBAPP

	dotnet new blazor -o BlazorApp
 
## BLAZOR WEBASSEMBLY
	dotnet new blazorwasm -o BlazorApp

# Proyecto WEB API de DOTNET 9
	dotnet new webapi -o NombreProyecto -controllers -uld -f x.x

		-uld (sólo si vas a usar autenticación para que cree las migraciones para sqlserver en vez de sqlite que es el defecto)
		-f (8.0, 9.0, 7.0) por defecto la última que tengas instalada

# Proyecto WEBAPP con interacción WEBASSEMBLY
	dotnet new blazor -o BlazorSignalRApp -int WebAssembly -ai False

# Proyecto WEBAPP con interacción SERVER Ee Interactivity Location = Per page/component
	dotnet new blazor -o BlazorServerCRUD -int Server -ai False
	
# Añadir un Proyecto LIBRERÍA A un Proyecto EXISTENTE
	dotnet new classlib -o BlazorWASMCrud.Shared

# Crear una solución vacía
	Con el directorio ya creado:

 	`dotnet new sln`   --> crea una solución vacía con el mismo nombre del directorio

  	Sin tener aun directorio
   	`dotnet new sln --output MySolution`  --> crea el directorio MySolution y en él una solución vacía con el mismo nombre del directorio
    
# Añadir un proyecto a una Solución Existente. En el directorio donde está el .sln:
	dotnet sln add BlazorWASMCrud.Shared
	
# Proyecto WEBAPP con IDENTITY, con INTERACCION SERVER e Interactivity Location = Per page/component
	dotnet new blazor -o BlazorIdentity -int Server -au Individual -ai False
	dotnet new blazor -o BlazorIdentity -int Server -au Individual -ai False -uld 
		// uld significa usar LocalDB que como es parte de SQLSERVER puede que construya mejor las migrations para usar SQLSERVER,
		// pq por defecto los genera para SQLite      
	
# COMPILAR Y CONSTRUIR LA APLICACION
dotnet build<br>
https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-build
		
# LIMPIAR obj Y bin
dotnet clean

# Generar un certificado autofirmado para usar https en desarrollo
dotnet dev-certs<br>
	https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-dev-certs
	
# EJECUTAR LA APLICACION:

dotnet watch

## EJECUTARLA POR https:
	dotnet watch -lp https
	
# Buscar los nombres de las plantillas en  nuget.org
dotnet new search <TEMPLATE_NAME>	

# AÑADIR una herramienta

```console
dotnet tool install <PACKAGE_NAME> -g|--global
```

```console
dotnet tool install --global dotnet-ef
dotnet tool update --global dotnet-ef
```

# Chuleta sobre las plantillas BLAZOR
[Enlace](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-sdk-templates#blazor)

# Añadir un paquete Nuget
dotnet add package [NOMBRE-PAQUETE]<br>
	[Enlace](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-add-package)

 ```console
 	dotnet add package Microsoft.EntityFrameworkCore --version x.x.x
	dotnet add package Microsoft.EntityFrameworkCore.Tools --version x.x.x // necesario si quieres usar migrations en un CodeFirst approach
	dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version x.x.x
	dotnet add package Microsoft.EntityFrameworkCore.Design --version x.x.x // necesario para scaffolding
	dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version x.x.x
	dotnet add package System.Net.Http.Json --version x.x.x
```
# Listar los paquetes que contiene la solución
`dotnet list package`	

# Eliminar un paquete
`dotnet remove [PROJECT] package <PACKAGE_NAME>`<br>
	[Enlace](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-remove-package)

# Cadena de conexión a SQLSERVER en PODMAN **NO FUNCIONA**
`"Server=localhost,1460;Database=VideoGameDB;User Id=sa;Password=Passw0rd;Trusted_Connection=true;Encrypt=false"`<br>
	**OJO: Trusted_Connection=true; IMPLICA WINDOWS E IDENTIFICACION KERBEROS. EN LINUX NO FUNCIONA, Y NO PUEDE USARSE CON USER/PASS**
	
# Cadena de conexión a SQLSERVER en PODMAN === _**FUNCIONA**_ ===
`"Server=localhost,1460;Database=VideoGameDB;User Id=sa;Password=Passw0rd;Encrypt=false"`

# Cadena de conexión a SQLSERVER directamente instalado en Ubuntu === _**FUNCIONA**_ ===
`"Server=localhost;Database=BlazorPurchaseOrders;User Id=sa;Password=tr#P3zoide;Encrypt=false"`

# Cadena de conexión a SQLITE
`"DefaultConnection": "Data Source=database.db"`
	
# Crear Migraciones
`dotnet ef migrations add Initial`

# Ejecutar Migraciones en la b.d.
`dotnet ef database update`
	
	
# Crear un componente Razor
`dotnet new razorcomponent -n Todo -o Components/Pages`

# BLAZOR SECURITY

## Crear un proyecto con autenticación

	`dotnet new blazor -int Server -au Individual -o AcademiaWebapp`

## Documentación sobre identity y Blazor Security

[Enlace](https://learn.microsoft.com/es-es/aspnet/core/blazor/security/?view=aspnetcore-9.0&tabs=net-cli)
	

## Agregar IDENTITY a un proyecto que se creó inicialmente sin ello

`dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore`

----

## CURSO YOUTUBE DE PATRICK GOD SOBRE BLAZOR IDENTITY (NET8)
https://www.youtube.com/watch?v=tNzSuwV62Lw


# CURSO YOUTUBE 5 HORAS SOBRE BLAZOR CON TUTORIAL
https://www.youtube.com/watch?v=RBVIclt4sOo&t=263s

# CÓDIGO CON EJEMPLOS
https://github.com/bigboybamo/BlazorCRUDApp

---
# SCAFFOLDING  (base de datos previa existe y lo que quieres es crear los modelos y el dbcontext)
## Requisitos previos:
* el controlador para la b.d. en cuestion: Microsoft.EntityFrameworkCore.SqlServer
* el diseñador: Microsoft.EntityFrameworkCore.Design

---

`dotnet ef dbcontext scaffold Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer`

`dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models`

`dotnet ef dbcontext scaffold [host, userid, pass, encrypt, etc.] [driver] 
	--context-dir Data
	--output-dir Models`

***PROBADO:***

`dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models`

**ESTO FUNCIONA:**

```console
	dotnet new blazor -o PruebaManyToMany
	  cd PruebaManyToMany/
	  code .
	  dotnet add package Microsoft.EntityFrameworkCore
	  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
	  dotnet add package Microsoft.EntityFrameworkCore.Design
	  dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models
```
**PERO:** NO ME GUSTAN LOS MODELOS QUE HA CREADO... SEGUIR ESTUDIANDO A VER QUÉ ENCUENTRO
	
---

# Otro tipo de  SCAFFOLDING: Crear páginas Razor para CRUD a partir de los modelos:
## REQUISITO

`dotnet tool install -g dotnet-aspnet-codegenerator`<br>
[Enlace](https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=9.0.0&arch=x64&rid=ubuntu.24.04-x64&os=linuxmint.22)


```console
dotnet aspnet-codegenerator blazor CRUD -dbProvider sqlite -dc BlazorWebAppMovies.Data.BlazorWebAppMoviesContext -m Movie -outDir Components/Pages
```

**Esto no funciona en Linux**: pide la dotnet 8 y yo tengo la 9. Aparentemente no son compatibles

---

# PROYECTO BLAZOR WASM CRUD

	Según el vídeo de Patrick God: https://www.youtube.com/watch?v=AKiGGtBj1go

Crea un tercer proyecto .Shared que en realidad es una biblioteca normal (creo: se podría elegir también biblioteca Blazor, pero no sé cuál es la diferencia)

**OJO** el proyecto biblioteca se crea con --> `dotnet new classlib -o BlazorWASMCrud.Shared` <br>
pero además hay que ejecutar, en el directorio donde esté el .sln --> `dotnet sln add BlazorWASMCrud.Shared` <br>
y además, hay que añadir la referencia al nuevo proyecto en el .csproj del principal, en este caso `BlazorWASMCrud.csproj`

```csharp
<ProjectReference Include="..\BlazorWASMCrud.Shared\BlazorWASMCrud.Shared.csproj" />
``` 
porque si no, no lo reconoce y no encuentra las clases ni nada... no funciona y te lo marca en rojo.

En ese proyecto mete un directorio Entities, con una clase para el modelo.
Crea las migraciones y las aplica con database update. Todo OK

>  La división server/client/shared es un maremagnum que me parece innecesario y que es muy fácil equivocarse y bloquear el proyecto... creo que me quedo	con InteractiveServer
	
# TUTORIAL BLAZOR & MAUI DE JUAN ZULOAGA

Este tío es un profesor de FP de Colombia que graba sus clases presenciales
y las cuelga en Youtube. Explica bastante bien, aunque son videos largos
Incluye también documentos de Google Docs con la teoría y el paso-a-paso en sus 
playlists.

[Ver en Youtube](https://youtube.com/playlist?list=PLuEZQoW9bRnRHpzGBYKWfW01AFB_-Qgn1&si=Jfo_YBWgjVxSVlNM)

## Análogo pero para un taller de vehículos
[Ver en Youtube](https://www.youtube.com/playlist?list=PLuEZQoW9bRnSXVHOk8WLQVGX3VBTKQKj7)	

## Lo mismo para una gestión de pedidos con blazor y .net 7
[Ver en Youtube](https://www.youtube.com/playlist?list=PLuEZQoW9bRnRBThyGs208ZMrCYBRTvIg2)
