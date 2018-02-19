# Basics di Entity Framework 6.1
Entity Framework è un set di tecnologie ADO.NET che supportano lo sviluppo di applicazioni software orientate ai dati.

Entity Framework consente agli sviluppatori di usare i dati sotto forma di proprietà e oggetti specifici di un dominio.

Con Entity Framework, gli sviluppatori possono operare a un livello superiore di astrazione quando gestiscono i dati e possono creare e gestire applicazioni orientate ai dati con meno codice rispetto alle applicazioni tradizionali.

Poiché Entity Framework è un componente di .NET Framework, le applicazioni Entity Framework possono essere eseguite su qualsiasi computer in cui è installato .NET Framework 3.5 SP1 o versione successiva. 

https://msdn.microsoft.com/it-it/library/bb399567(v=vs.110).aspx

## Attivare il Framework nel Progetto di Visual Studio
- Aggiungere il Framework tramite NuGet:
```
Install-Package EntityFramework
```

## Creare un dominio di modelli - le properties
```
    [Table("Rilevamenti")]
    public class Rilevamento
    {
        public int Id { get; set; }
        public DateTime Settimana { get; set; }
        public ICollection<SCARapporto> RCARapporti { get; set; }
    }
```

## Mappare un dominio di dati - il DbContext

```
    public class EFFullDemoDBContext : DbContext // Erredita da DBContext
    {
        public EFFullDemoDBContext()
            : base("name=EFFullDemo") // Nome uguale alla connection string nel web.config
        {
        }
        public virtual DbSet<Rilevamento> Rlevamenti { get; set; }
    }
```

## Ovveride OnModelCreate()
- PluralizingTableNameConvention
```
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);
            modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
            modelBuilder.Conventions.Remove<OneToManyCascadeDeleteConvention>();
        }
```

## Attributi, virtual e campi obbligatori
- Il nome della tabella si definisce tramite un attributo [Table("NomeTabella")]  
- Il denominatore `virtual` si aggiunge per poter effettuare il Lazy Loading. (Carica dati in un secondo momento).

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
```
```
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

## Aggiungere il Database nel SQL Server
- Aprire una console di NuGet  
- Aggiungere seguenti comandi:  
`PM> Enable-Migrations` // Aggiunge il file Configuration.cs nella cartella Migrations  
`PM> Add-Migration InitialCreate`  
`PM> Update-Database -Verbose`  

## Modificare le tabelle o i dati nel database
`PM> Add-Migration AddedConstrains`  
`PM> Update-Database`

## Creare uno script di Deploy per il server di produzione
`PM> Update-Database -Script -SourceMigration InsertSampleData3 -TargetMigration InsertSampleDataNEW`

## Riferimento 
Whitepaper relativo al entity framework code first migration:  
https://msdn.microsoft.com/en-us/library/jj591621(v=vs.113).aspx
