# CREAR UN PROYECTO BLAZOR:

## BLAZOR WEBAPP

	dotnet new blazor -o BlazorApp
	## BLAZOR WEBASSEMBLY
	dotnet new blazorwasm -o BlazorApp

# PROYECTO WEB API DE DOTNET 9
`	dotnet new webapi -o NombreProyecto -controllers -uld -f x.x`

		-uld (sólo si vas a usar autenticación para que cree las migraciones para sqlserver en vez de sqlite que es el defecto)
		-f (8.0, 9.0, 7.0) por defecto la última que tengas instalada

# PROYECTO WEBAPP CON INTERACCION WEBASSEMBLY
	dotnet new blazor -o BlazorSignalRApp -int WebAssembly -ai False

# PROYECTO WEBAPP CON INTERACCION SERVER E Interactivity Location = Per page/component
	dotnet new blazor -o BlazorServerCRUD -int Server -ai False
	
# AÑADIR UN PROYECTO LIBRERÍA A UN PROYECTO EXISTENTE
	dotnet new classlib -o BlazorWASMCrud.Shared
	
# AÑADIR UN PROYECTO A UNA SOLUCION EXISTENTE. EN EL DIRECTORIO DONDE ESTÁ EL .sln:
	dotnet sln add BlazorWASMCrud.Shared
	
# PROYECTO WEBAPP CON IDENTITY, CON INTERACCION SERVER E Interactivity Location = Per page/component
	dotnet new blazor -o BlazorIdentity -int Server -au Individual -ai False
	dotnet new blazor -o BlazorIdentity -int Server -au Individual -ai False -uld 
		// uld significa usar LocalDB que como es parte de SQLSERVER puede que construya mejor las migrations para usar SQLSERVER,
		// pq por defecto los genera para SQLite      
		==== COMPROBADO: con uld genera codigo de migración válido para SQLSERVER =====
	
# COMPILAR Y CONSTRUIR LA APLICACION
dotnet build
	https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-build
		
# LIMPIAR obj Y bin
dotnet clean

# GENERAR UN CERTIFICADO AUTOFIRMADO PARA UTILIZAR https EN DESARROLLO
dotnet dev-certs
	https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-dev-certs
	
# EJECUTAR LA APLICACION
dotnet watch
	## EJECUTARLA POR https:
	dotnet watch -lp https
	
# BUSCAR LOS NOMBRES DE LAS PLANTILLAS EN nuget.org
dotnet new search <TEMPLATE_NAME>	

# AÑADIR UNA HERRAMIENTA
dotnet tool install <PACKAGE_NAME> -g|--global
dotnet tool install --global dotnet-ef
dotnet tool update --global dotnet-ef

# CHULETA SOBRE LAS PLANTILLAS BLAZOR
https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-sdk-templates#blazor

# AÑADIR UN PAQUETE NUGET
dotnet add package <NOMBRE-PAQUETE>
	https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-add-package	
	dotnet add package Microsoft.EntityFrameworkCore --version x.x.x
	dotnet add package Microsoft.EntityFrameworkCore.Tools --version x.x.x // necesario si quieres usar migrations en un CodeFirst approach
	dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version x.x.x
	dotnet add package Microsoft.EntityFrameworkCore.Design --version x.x.x // necesario para scaffolding
	dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version x.x.x
	dotnet add package System.Net.Http.Json --version x.x.x
	
# LISTAR LOS PAQUETES QUE CONTIENE LA SOLUCION
dotnet list package	
# ELIMINAR UN PAQUETE
dotnet remove [<PROJECT>] package <PACKAGE_NAME>
	https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-remove-package

# CADENA DE CONEXION A SQLSERVER EN PODMAN ---NO FUNCIONA--
"Server=localhost,1460;Database=VideoGameDB;User Id=sa;Password=Passw0rd;Trusted_Connection=true;Encrypt=false "
	#OJO: Trusted_Connection=true; IMPLICA WINDOWS E IDENTIFICACION KERBEROS. EN LINUX NO FUNCIONA, Y NO PUEDE USARSE CON USER/PASS
	
# CADENA DE CONEXION A SQLSERVER EN PODMAN ===FUNCIONA===
"Server=localhost,1460;Database=VideoGameDB;User Id=sa;Password=Passw0rd;Encrypt=false "

# CADENA DE CONEXION A SQLSERVER DIRECTAMENTE INSTALADO EN UBUNTU ===FUNCIONA===
"Server=localhost;Database=BlazorPurchaseOrders;User Id=sa;Password=tr#P3zoide;Encrypt=false"

# CADENA DE CONEXION A SQLITE
"DefaultConnection": "Data Source=database.db"
	
# CREAR MIGRACIONES
dotnet ef migrations add Initial

# EJECUTAR MIGRACIONES
dotnet ef database update
	
	
# CREAR UN COMPONENTE RAZOR
dotnet new razorcomponent -n Todo -o Components/Pages

# BLAZOR SECURITY
	# CREAR UN PROYECTO CON AUTENTICACIÓN
	dotnet new blazor -int Server -au Individual -o AcademiaWebapp
	# DOCUMENTACION SOBRE IDENTITY Y BLAZOR SECURITY
	https://learn.microsoft.com/es-es/aspnet/core/blazor/security/?view=aspnetcore-9.0&tabs=net-cli
	

# AGREGAR IDENTITY A UN PROYECTO QUE SE CREÓ INICIALMENTE SIN ELLO
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore

# CURSO YOUTUBE DE PATRICK GOD SOBRE BLAZOR IDENTITY (NET8)
https://www.youtube.com/watch?v=tNzSuwV62Lw


# CURSO YOUTUBE 5 HORAS SOBRE BLAZOR CON TUTORIAL
https://www.youtube.com/watch?v=RBVIclt4sOo&t=263s

# CÓDIGO CON EJEMPLOS
https://github.com/bigboybamo/BlazorCRUDApp

========================================================================================
# SCAFFOLDING  (base de datos previa existe y lo que quieres es crear los modelos y el dbcontext)
# Requisitos previos:
	# el controlador para la b.d. en cuestion: Microsoft.EntityFrameworkCore.SqlServer
	# el diseñador: Microsoft.EntityFrameworkCore.Design
========================================================================================

dotnet ef dbcontext scaffold Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models

dotnet ef dbcontext scaffold [host, userid, pass, encrypt, etc.] [driver] 
	--context-dir Data
	--output-dir Models

PROBADO:
dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models

	ESTO FUNCIONA:
	dotnet new blazor -o PruebaManyToMany
	  cd PruebaManyToMany/
	  code .
	  dotnet add package Microsoft.EntityFrameworkCore
	  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
	  dotnet add package Microsoft.EntityFrameworkCore.Design
	  dotnet ef dbcontext scaffold "Server=localhost;Database=testdb;User Id=sa;Password=tr#P3zoide;Encrypt=false" Microsoft.EntityFrameworkCore.SqlServer --context-dir Data --output-dir Models
	PERO: NO ME GUSTAN LOS MODELOS QUE HA CREADO... SEGUIR ESTUDIANDO A VER QUÉ ENCUENTRO
	
##########################################################################################################################################################	
# OTRO TIPO DE SCAFFOLDING: CREAR PÁGINAS RAZOR PARA CRUD A PARTIR DE LOS MODELOS:
# REQUISITO
			dotnet tool install -g dotnet-aspnet-codegenerator
			https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=9.0.0&arch=x64&rid=ubuntu.24.04-x64&os=linuxmint.22

		dotnet aspnet-codegenerator blazor CRUD -dbProvider sqlite -dc BlazorWebAppMovies.Data.BlazorWebAppMoviesContext -m Movie -outDir Components/Pages

			SIN PROBAR AUN. A VER CÓMO SE LAS APAÑA CON LAS RELACIONES
		dotnet aspnet-codegenerator blazor CRUD -dbProvider sqlserver -dc PruebaManyToMany.Data.TestdbContext -m Post -outDir Components/Pages

######## NO FUNCIONA #####################################################################################################################################	
		Da un error de incompatibilidad entre dotnet 8 y dotnet9 que parece que no pueden convivir en linuxmint
##########################################################################################################################################################	

# PROYECTO BLAZOR WASM CRUD
	Según el vídeo de Patrick God: https://www.youtube.com/watch?v=AKiGGtBj1go
	Crea un tercer proyecto .Shared que en realidad es una biblioteca normal (creo: se podría elegir también biblioteca Blazor, pero no sé cuál es la diferencia)
	## OJO ## el proyecto biblioteca se crea con --> dotnet new classlib -o BlazorWASMCrud.Shared
			  pero además hay que ejecutar, en el directorio donde esté el .sln --> dotnet sln add BlazorWASMCrud.Shared
			  y además, hay que añadir la referencia al nuevo proyecto en el .csproj del principal, en este caso BlazorWASMCrud.csproj
				--> <ProjectReference Include="..\BlazorWASMCrud.Shared\BlazorWASMCrud.Shared.csproj" /> 
			  porque si no, no lo reconoce y no encuentra las clases ni nada... no funciona y te lo marca en rojo
	En ese proyecto mete un directorio Entities, con una clase para el modelo
	Crea las migraciones y las aplica con database update. Todo OK
	
	La división server/client/shared es un maremagnum que me parece innecesario y que es muy fácil equivocarse y bloquear el proyecto... creo que me quedo
	con InteractiveServer
	
	minuto 34:16
	
	
# TUTORIAL BLAZOR & MAUI DE JUAN ZULOAGA
		Este tío es un profesor de FP de Colombia que graba sus clases presenciales
		y las cuelga en Youtube. Explica bastante bien, aunque son videos largos
		Incluye también documentos de Google Docs con la teoría y el paso-a-paso en sus 
		playlists.
https://youtube.com/playlist?list=PLuEZQoW9bRnRHpzGBYKWfW01AFB_-Qgn1&si=Jfo_YBWgjVxSVlNM
# ANÁLOGO PERO PARA UN TALLER DE VEHÍCULOS
https://www.youtube.com/playlist?list=PLuEZQoW9bRnSXVHOk8WLQVGX3VBTKQKj7	
# LO MISMO PARA UNA GESTIÓN DE PEDIDOS CON BLAZOR Y .NET 7
https://www.youtube.com/playlist?list=PLuEZQoW9bRnRBThyGs208ZMrCYBRTvIg2