# Le basi dell'Entity Framework 6.1
Entity Framework è un set di tecnologie ADO.NET che supportano lo sviluppo di applicazioni software orientate ai dati.

Entity Framework consente agli sviluppatori di usare i dati sotto forma di proprietà e oggetti specifici di un dominio.

Con Entity Framework, gli sviluppatori possono operare a un livello superiore di astrazione quando gestiscono i dati e possono creare e gestire applicazioni orientate ai dati con meno codice rispetto alle applicazioni tradizionali.

Poiché Entity Framework è un componente di .NET Framework, le applicazioni Entity Framework possono essere eseguite su qualsiasi computer in cui è installato .NET Framework 3.5 SP1 o versione successiva. 

https://msdn.microsoft.com/it-it/library/bb399567(v=vs.110).aspx

### Attivare EF6 nel progetto di Visual Studio
Aggiungere il Framework tramite NuGet:
```
Install-Package EntityFramework
```

### Creare un dominio di modelli
```
    [Table("Rilevamenti")]
    public class Rilevamento
    {
        public int Id { get; set; }
        public DateTime Settimana { get; set; }
        public ICollection<SCARapporto> RCARapporti { get; set; }
    }
```

### Mappare un dominio di dati - il DbContext

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

### Ovveride OnModelCreate()
PluralizingTableNameConvention
```
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);
            modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
            modelBuilder.Conventions.Remove<OneToManyCascadeDeleteConvention>();
        }
```

### Attributi, virtual e campi obbligatori
Il nome della tabella si definisce tramite un attributo [Table("NomeTabella")]  
Il denominatore `virtual` si aggiunge per poter effettuare il Lazy Loading. (Carica dati in un secondo momento).

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

### Relazioni
__Esempio di una relazione 1 a 1__   
Here, you will learn to configure One-to-Zero-or-One relationships between two entities. 
We will implement a one-to-Zero-or-One relationship between the following Student and StudentAddress entities.   

```
public class Student
{
    public int StudentId { get; set; }
    public string StudentName { get; set; }

    public virtual StudentAddress Address { get; set; }
}
     
public class StudentAddress 
{
    public int StudentAddressId { get; set; }
    public string Address1 { get; set; }
    public string Address2 { get; set; }
    public string City { get; set; }
    public int Zipcode { get; set; }
    public string State { get; set; }
    public string Country { get; set; }

    public virtual Student Student { get; set; }
}
```

__Esempio di una relazione 1 a n__
Here, we will learn how to configure One-to-Many relationships between two entities (domain classes) in Entity Framework 6.x using the code-first approach. 
Let's configure a one-to-many relationship between the following Student and Grade entities where there can be many students in one grade.   

```
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public int GradeId { get; set; }
    public Grade Grade { get; set; }
}

public class Grade
{

    public int GradeId { get; set; }
    public string GradeName { get; set; }
    
    public ICollection<Student> Student { get; set; }
}
```

__Esempio di una relazione n a n__
Here, we will learn how to configure a Many-to-Many relationship between the Student and Course entity classes. Student can join multiple courses and multiple students can join one Course. 
Visit the Entity Relationship chapter to understand how EF manages one-to-one, one-to-many and many-to-many relationships between entities.   

```
public class Student
{
    public Student() 
    {
        this.Courses = new HashSet<Course>();
    }

    public int StudentId { get; set; }
    [Required]
    public string StudentName { get; set; }

    public virtual ICollection<Course> Courses { get; set; }
}
        
public class Course
{
    public Course()
    {
        this.Students = new HashSet<Student>();
    }

    public int CourseId { get; set; }
    public string CourseName { get; set; }

    public virtual ICollection<Student> Students { get; set; }
}
```


### Aggiungere il database nel SQL Server
- Aprire una console di NuGet  
- Aggiungere seguenti comandi:  
`PM> Enable-Migrations` // Aggiunge il file Configuration.cs nella cartella Migrations  
`PM> Add-Migration InitialCreate`  
`PM> Update-Database -Verbose`  

### Modificare le tabelle o i dati nel database
`PM> Add-Migration AddedConstrains`  
`PM> Update-Database`

## Creare uno script di deploy per il server di produzione
`PM> Update-Database -Script -SourceMigration InsertSampleData3 -TargetMigration InsertSampleDataNEW`

### Riferimento 
Whitepaper relativo al entity framework code first migration:  
https://msdn.microsoft.com/en-us/library/jj591621(v=vs.113).aspx
