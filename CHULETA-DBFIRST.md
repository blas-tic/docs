# Ingeniería inversa de una b.d. existente utilizando dotnet CLI.

Para revertir ingeniería una base de datos existente usando .NET CLI y Entity Framework Core, siga estos pasos:

1. Instale las Herramientas CLI de EF Core: Ejecute el siguiente comando en su directorio de proyecto para instalar las herramientas CLI de EF Core:
   ```
   dotnet tool install --global dotnet-ef
   ```

2. Instale los Paquetes EF Core Necesarios: Agregue los paquetes EF Core necesarios a su proyecto. Por ejemplo, si está utilizando SQL Server, instale el proveedor de SQL Server:
   ```
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.Design
   ```

3. Ejecute el Comando de Escafolding: Use el comando `dotnet ef dbcontext scaffold` para revertir ingeniería su base de datos. Proporcione la cadena de conexión a su base de datos y el proveedor para usar. Opcionalmente, use la opción `--force` para sobrescribir cualquier clase o modelo existente en la carpeta de salida:
   ```
   dotnet ef dbcontext scaffold "Server=yourserver;Database=yourdatabase;User Id=youruser;Password=yourpassword;" Microsoft.EntityFrameworkCore.SqlServer -o Models --force
   ```

Este comando generará clases C# de modelos basadas en el esquema de base de datos y colocará en la carpeta de salida especificada.

## Ejemplo con dos tablas unidas por una relacion uno-a-muchos, incluyendo páginas blazor para explotar los modelos.

Claro, aquí hay un ejemplo con dos tablas, `Authors` y `Books`, donde los autores tienen una relación uno-a-muchos con los libros.

Primero, genere los modelos utilizando el comando `dotnet ef dbcontext scaffold` proporcionado anteriormente. Los modelos generados podrían verse así:

`Author.cs`

```csharp
public partial class Author
{
    public Author()
    {
        Books = new HashSet<Book>();
    }

    public int AuthorId { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Book> Books { get; set; }
}
```

`Book.cs`

```csharp
public partial class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public int AuthorId { get; set; }

    public virtual Author Author { get; set; }
}
```

Después de generar los modelos, puede crear una página Blazor para mostrar los datos.

`Pages/DatabaseComponent.razor`

```razor
@page "/databasecomponent"
@inject YourProject.Models.AuthorDbContext DbContext

<h3>Database Component</h3>

<h4>Authors</h4>
<table class="table">
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Books</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var author in DbContext.Authors)
        {
            <tr>
                <td>@author.AuthorId</td>
                <td>@author.Name</td>
                <td>
                    @foreach (var book in author.Books)
                    {
                        @book.Title;
                        @((author.Books.Count > 1) ? ", " : "")
                    }
                </td>
            </tr>
        }
    </tbody>
</table>

@code {
}
```

En este ejemplo, el código muestra una tabla con autores y sus libros asociados. La relación uno-a-muchos se maneja mediante la colección `Books` en el modelo `Author`.



# Chuleta de scaffolding en microsoft.learn
https://learn.microsoft.com/es-es/ef/core/managing-schemas/scaffolding/?tabs=dotnet-core-cli
