# Backup de resolução de dúvidas/dicas


~~~java
// Remover dois ultimos números de uma string
String nome = 1213.0;
nome.substring(0,nome.length()-2)
//Saída 1213
~~~
### Criar Enums a partir da descrição
~~~java
@Getter
@AllArgsConstructor
public enum TipoEdificioEnum {

    CASA("Casa"),
    APARTAMENTO("Apartamento");

    private String descricao;

    public static TipoEdificioEnum valueOfDescricao(String descricao) throws ValidationException {
        for(TipoEdificioEnum ts : values()){
            if(ts.getDescricao().equalsIgnoreCase(descricao))
                return ts;
        }
        throw new ValidationException("Valor invalido para o campo Tipo de edificio");
    }

}
~~~
# SQL
### Procurar campos com valor  
-- Crie a função a seguir
~~~sql
CREATE OR REPLACE FUNCTION search_columns(
    needle text,
    haystack_tables name[] default '{}',
    haystack_schema name[] default '{}'
)
RETURNS table(schemaname text, tablename text, columnname text, rowctid text)
AS $$
begin
  FOR schemaname,tablename,columnname IN
      SELECT c.table_schema,c.table_name,c.column_name
      FROM information_schema.columns c
        JOIN information_schema.tables t ON
          (t.table_name=c.table_name AND t.table_schema=c.table_schema)
        JOIN information_schema.table_privileges p ON
          (t.table_name=p.table_name AND t.table_schema=p.table_schema
              AND p.privilege_type='SELECT')
        JOIN information_schema.schemata s ON
          (s.schema_name=t.table_schema)
      WHERE (c.table_name=ANY(haystack_tables) OR haystack_tables='{}')
        AND (c.table_schema=ANY(haystack_schema) OR haystack_schema='{}')
        AND t.table_type='BASE TABLE'
  LOOP
    FOR rowctid IN
      EXECUTE format('SELECT ctid FROM %I.%I WHERE cast(%I as text)=%L',
       schemaname,
       tablename,
       columnname,
       needle
      )
    LOOP
      -- uncomment next line to get some progress report
      -- RAISE NOTICE 'hit in %.%', schemaname, tablename;
      RETURN NEXT;
    END LOOP;
 END LOOP;
END;
$$ language plpgsql;

-- Chame a função em outra aba, passando o campo que deseja procurar
select * from search_columns('String a ser procurada')
~~~

~~~sql
-- Dropa a coluna disponibilidade da tabela esporte
ALTER TABLE esporte DROP COLUMN  disponibilidade;

~~~

~~~sql
-- Atualiza coluna em uma tabela
update tabela set coluna = 'novo valor' where coluna = 'valor antigo';

~~~

~~~
-- Apagar registro de uma tabela
DELETE FROM documento WHERE uuid = 'ac78926d-043d-42c1-8aed-4628f76e2b61'
~~~

~~~java
// Pesquisar dentro de entidades compostas
    private static<T> Specification<T> pesquisarComLikeNumeroContrato(String nomeColuna,Object value ){
        return (root, query, builder) ->{
            return builder.like(builder.lower(root.get("numeroContrato").get(nomeColuna).get(nomeColuna)), "%" + value.toString() + "%");
        };
    }
~~~

### Pesquisar
~~~java
// Pesquisar dentro de uma lista com criteria
Predicate codigo = root.join("nomeDaLista").get("outraEntidade").get("nomeCampo").in(parametroDesejado);
~~~

# Regex
## Validar String com apenas letras A-Z
~~~java
nome.matches("^([a-zA-Z]+\s)*[a-zA-Z]+$")
~~~
## Validar String com apenas números
~~~java
nome.matches("^\\d+$")
~~~

~~~ JAVA
// Iginorar campos na integracao com outra api
@JsonIgnoreProperties(ignoreUnknown = true)
~~~
