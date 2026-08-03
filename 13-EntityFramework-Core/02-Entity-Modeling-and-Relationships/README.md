# Entity Modeling & Relationships

Status: **Not started**

## Planned coverage
- Conventions: how EF Core infers keys, table names, column types by default
- One-to-many, one-to-one, many-to-many (implicit join entity vs explicit join entity)
- Navigation properties, foreign keys, principal/dependent ends
- Owned entity types (value objects / complex types)
- Table splitting, entity splitting, table-per-hierarchy (TPH) vs table-per-type (TPT) vs table-per-concrete-type (TPC) inheritance mapping
- Keyless entities (`.HasNoKey()`) for views/raw SQL
- Composite keys
- Data Annotation attributes as the alternative to Fluent API: `[Key]`, `[Required]`, `[MinLength]`/`[MaxLength]`/`[StringLength]`, `[Table]`, `[Column]`, `[Index]`, `[ForeignKey]`, `[NotMapped]` — what each maps to in the generated schema, and why Fluent API wins for anything non-trivial (keeps mapping concerns out of the domain model)
- ER diagrams for each relationship type
