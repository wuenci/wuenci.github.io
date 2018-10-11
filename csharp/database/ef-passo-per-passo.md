# Entity Framework 6.1 code first - passo per passo

## Guida per l'aggiunta di EF6 Code first in un progetto ex nuovo.

### 1) Installare EF 6 tramite Nuget
Aggiungere il Framework tramite la console di powershell integrata in Visual Studio:

```
Install-Package EntityFramework
```

### 2) Aggiungere la connection string nel file di configurazione app.config
Se installi EF 6 in una __class library__ devi aggiungere la stringa di connessione sia nella class library che nel progetto primario. Ê neccessario solomente per la fase di sviluppo, in fase di produzione è basta la Assembly primaria.   
__La configSections deve essere la prima cosa nel app.config__
```
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false"/>
  </configSections>
  <connectionStrings>
    <add name="IndirizzarioConnString" providerName="System.Data.SqlClient" connectionString="Server=.\SQLEXPRESS;Database=Indirizzario;Integrated Security=True;"/>
  </connectionStrings>
  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework"/>
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer"/>
    </providers>
  </entityFramework>
```

### 3) Creare i modelli
Esempio di due tabelle 1 a n
```
    [Table("Rapporti")]  // Nome della Tabella nel database
    public class Rapporto
    {
        public int Id { get; set; }

        public String Eml { get; set; }

        [Required]
        [StringLength(150)]
        public String Soggetto { get; set; }

        [StringLength(150)]
        public String PDFurl { get; set; }

        [MaxLength(150)]
        [Required]
        public String CollaboratoreNome { get; set; }

        [Required]
        public DateTime DataInvio { get; set; }

        [Required(AllowEmptyStrings = true)]
        public DateTime DataRisposta { get; set; }

        [Required]
        public int Stato { get; set; }

        [Required]
        public virtual Periodo Periodo { get; set; }

        [Required]
        public int PeriodoId { get; set; }

        [Required]
        public virtual Categoria Categoria { get; set; }

        [Required]
        public int CategoriaId { get; set; }
    }

    [Table("Categorie")]
    public class Categoria
    {
        public int Id { get; set; }

        [Index(IsUnique = true)]
        [StringLength(4)]
        public String Numero { get; set; }

        [Index(IsUnique = true)]
        [StringLength(20)]
        public String Nome { get; set; }

        public String CartellaFs { get; set; }

        public virtual ICollection<Rapporto> Rapporti { get; set; }
    }
```
### 4) Creare il DBContext con il Model
Creare il DB Context nella cartella Models

```
    public class EFFullDemoDBContext : DbContext // Erredita da DBContext
    {
        public EFFullDemoDBContext()
            : base("name=EFFullDemo") // Nome uguale alla connection string nel web.config
        {
        }
        public virtual DbSet<Rilevamento> Rlevamenti { get; set; }
    }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
        modelBuilder.Conventions.Remove<OneToManyCascadeDeleteConvention>();
    }
```

### 5 ) Enable Migratione
Impostare le migrazioni
```
Enable-Migrations` // Aggiunge il file Configuration.cs nella cartella Migrations 
```

### 6 ) Aggiungere i seed iniziali
```
TODO: SEED INIZIALE
```

### 7 ) Fare la prima migrazione
```
Add-Migration InitialCreate   
```

### 8 ) Creare il DB per la prima volta e poi aggiornalo
```
Update-Database
```
