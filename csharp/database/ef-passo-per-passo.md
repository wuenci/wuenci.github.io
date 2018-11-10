# Entity Framework 6.1 code first - passo per passo

## Guida per l'aggiunta di EF6 Code first in un progetto ex nuovo.

### 1) Installare EF 6 tramite Nuget
Aggiungere il Framework tramite la console di powershell integrata in Visual Studio:

```
Install-Package EntityFramework
```

### 2) Aggiungere la connection string nel file di configurazione app.config
Se installi EF 6 in una __class library__ devi aggiungere la stringa di connessione sia nella class library che nel progetto primario. Ê neccessario solomente per la fase di sviluppo, in fase di produzione è basta la Assembly primaria.   

ATTENZIONE:   __La configSections deve essere la prima cosa nel app.config__

Esempio con SQL Database Server Express:
```
  <configSections>
    ...
  </configSections>
  <connectionStrings>
    <add name="DefaultConnection" providerName="System.Data.SqlClient" connectionString="Server=.\SQLEXPRESS;Database=GigHub;Integrated Security=True;"/>
  </connectionStrings>
  <entityFramework>
    ...
  </entityFramework>
```
Esempio con LocalDB in cartella App_Data:
```
  <connectionStrings>
    <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\GigHub.mdf;Initial Catalog=GigHub;Integrated Security=True" providerName="System.Data.SqlClient" />
  </connectionStrings>
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
Nel file di configurazione che si è creato si può inserire dei dati iniziali d'esempio.   
```
        protected override void Seed(Indirizzario.Db.Models.IndirizzarioDbContext context)
        {
            //  This method will be called after migrating to the latest version.

            //  You can use the DbSet<T>.AddOrUpdate() helper extension method 
            //  to avoid creating duplicate seed data.

            context.People.AddOrUpdate(x => x.Id,
            new Person() { Id=1, FirstName = "Max", LastName = "Mustermann", Gender="Maschio" },
            new Person() { Id=2, FirstName = "Massimo", LastName = "Esempio", Gender = "Maschio" }
            );

            context.Addresses.AddOrUpdate(x => x.Id,
            new Address() { Street="via belle cose", Postcode="66452", Town="Gordolanino", State="Ticino", Country="Svizzera", PersonId=1  },
            new Address() { Street = "Nel Nucleo 50", Postcode = "69543", Town = "Bidognoasco", State = "Ticino", Country = "Svizzera", PersonId = 1 },
            new Address() { Street = "via Statale", Postcode = "65847", Town = "Milano", State = "Lombardia", Country = "Italia", PersonId = 2 }
            );

        }
```

### 7 ) Fare la prima migrazione
```
Add-Migration InitialCreate   
```

### 8 ) Creare il DB per la prima volta e poi aggiornalo
```
Update-Database
```
