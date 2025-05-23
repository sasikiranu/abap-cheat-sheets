<a name="top"></a>

# ABAP Managed Database Procedures (AMDP)

- [ABAP Managed Database Procedures (AMDP)](#abap-managed-database-procedures-amdp)
  - [Introduction](#introduction)
  - [AMDP Classes](#amdp-classes)
  - [AMDP Methods](#amdp-methods)
  - [AMDP Procedures](#amdp-procedures)
  - [AMDP Functions](#amdp-functions)
    - [CDS Table Functions](#cds-table-functions)
  - [Using AMDP in ABAP for Cloud Development](#using-amdp-in-abap-for-cloud-development)
    - [Notes on Client Handling and Client Safety](#notes-on-client-handling-and-client-safety)
    - [Restrictions for AMDP Methods in ABAP for Cloud Development](#restrictions-for-amdp-methods-in-abap-for-cloud-development)
    - [Making AMDP Methods Client-Safe](#making-amdp-methods-client-safe)
  - [More Information](#more-information)
  - [Executable Example](#executable-example)


This cheat sheet gathers basic information on [ABAP Managed Database
Procedures
(AMDP)](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenabap_managed_db_proc_glosry.htm "Glossary Entry").
Find more details
[here](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp.htm)
in the ABAP Keyword Documentation.

## Introduction

-   AMDP are a class-based framework for managing and calling

    -   [database
        procedures](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abendatabase_procedure_glosry.htm "Glossary Entry")
        (which is a synonym for [stored
        procedures](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenstored_procedure_glosry.htm "Glossary Entry"), i.
        e. the procedures are stored in the database - the [SAP HANA
        database](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenhana_database_glosry.htm "Glossary Entry")
        in this case - and executed there)
    -   [database
        functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abendatabase_function_glosry.htm "Glossary Entry")
        (which are [SQLScript
        functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abensql_script_function_glosry.htm "Glossary Entry")
        in the SAP HANA database)

    in [AS ABAP](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenas_abap_glosry.htm "Glossary Entry").

-   "ABAP managed" enters the picture in ABAP with the option of
    implementing special [AMDP
    procedures](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_procedure_glosry.htm "Glossary Entry")
    as database procedures and [AMDP
    functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_function_glosry.htm "Glossary Entry")
    as database functions.
-   The implementations are programmed using a database-specific
    language. Currently, AMDP only supports database procedures and
    functions from the SAP HANA database. That is,
    [SQLScript](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abensql_script_glosry.htm "Glossary Entry")
    is the programming language of choice.
-   AMDP procedures and functions are part of a dedicated [AMDP
    class](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_class_glosry.htm "Glossary Entry")
    and declared and implemented as part of a method. The classes and
    methods have certain characteristics as outlined further down.
-   The AMDP framework replicates the procedure or function to the
    database system, i. e. despite the fact that the programming happens
    in an AMDP class (which is an [ABAP
    Repository](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenabap_repository_glosry.htm "Glossary Entry")
    object as other global classes, too), the (SQLScript) code is
    executed only on the (SAP HANA) database and not in AS ABAP, i. e.
    method calls are sent to the database procedure or function.

> **💡 Note**<br>
>- The use of AMDP is not recommended if the same task can be
        achieved using [ABAP
        SQL](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenabap_sql_glosry.htm "Glossary Entry").
>- AMDP classes can only be edited with the [ABAP development tools for Eclipse
        (ADT)](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenadt_glosry.htm "Glossary Entry").

<p align="right"><a href="#top">⬆️ back to top</a></p>

## AMDP Classes

-   As mentioned above, an [AMDP
    class](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_class_glosry.htm "Glossary Entry")
    is an [ABAP
    Repository](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenabap_repository_glosry.htm "Glossary Entry")
    object like other global classes.
-   However, an AMDP class includes the specification of the interface
    `IF_AMDP_MARKER_HDB` for the SAP HANA
    database (indicated by `HDB`), which is currently the
    only possible database.
-   An AMDP class can contain both (one or more) [AMDP
    methods](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_method_glosry.htm "Glossary Entry")
    and non-AMDP methods.

Example for a [declaration  part](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abendeclaration_part_glosry.htm "Glossary Entry")
of an AMDP class:

```abap
CLASS cl_some_amdp_class DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    "Specifying the interface is mandatory
    INTERFACES if_amdp_marker_hdb.
...
ENDCLASS.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## AMDP Methods

-   Can be created as [instance
    methods](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abeninstance_method_glosry.htm "Glossary Entry")
    using <code>METHODS</code> or [static
    methods](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenstatic_method_glosry.htm "Glossary Entry")
    using <code>CLASS-METHODS</code> in any visibility section.
-   Cannot be identified as AMDP methods in the declaration part of the
    class since there are no specific additions to the methods.
    Exceptions: AMDP function implementations that implement any [CDS table functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_table_function_glosry.htm "Glossary Entry")
    as shown further down and method declarations using [`AMDP
    OPTIONS`](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abapmethods_amdp_options.htm)
    that are not dealt with here.

AMDP method declarations in any visibility section like non-AMDP
methods:

```abap
...
PUBLIC SECTION.

  METHODS some_amdp_meth
    ... "Here go the parameters

PRIVATE SECTION.

  CLASS-METHODS another_amdp_meth
    ... "Here go the parameters

...
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## AMDP Procedures

Despite the fact that AMDP methods cannot be identified as such from the
[declaration
part](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abendeclaration_part_glosry.htm "Glossary Entry")
(apart from the exceptions mentioned above), the declaration part of
[AMDP
procedures](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_procedure_glosry.htm "Glossary Entry")
has special characteristics:

-   Parameters must be [passed by
    value](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenpass_by_value_glosry.htm "Glossary Entry")
    using <code>VALUE(...)</code>. [Passing by
    reference](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenpass_by_reference_glosry.htm "Glossary Entry")
    is not allowed.
-   Parameter types ...
    -   must not be generic.
    -   can only be [elementary data
        types](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenelementary_data_type_glosry.htm "Glossary Entry")
        and [table
        types](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abentable_type_glosry.htm "Glossary Entry")
        with a structured row type (and this type can only contain
        elementary data types as components).
-   Return values cannot be declared using <code>RETURNING</code>.
-   Only input parameters can be flagged as optional parameters.

Example for an AMDP procedure's declaration part:

```abap
...
PUBLIC SECTION.

  "Table type with a structured row type
  TYPES tab_type TYPE STANDARD TABLE OF dbtab WITH EMPTY KEY.

  METHODS amdp_meth
    IMPORTING VALUE(num) TYPE i,
    EXPORTING VALUE(tab) TYPE tab_type.

...
```

In contrast to the declaration part, the [implementation
part](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_procedure_method_glosry.htm "Glossary Entry")
of an AMDP method has multiple special additions for AMDP purposes
following <code>METHOD</code> and the method name:

```abap
...
METHOD amdp_meth
  BY DATABASE PROCEDURE
  FOR HDB  
  LANGUAGE SQLSCRIPT  
  OPTIONS READ-ONLY
  USING db_object.   "see comments further down
                         
"Beginning of the SQLScript code (note that it is not ABAP code although it looks similar)

  ... "Here goes SQLScript code
      "Note that an AMDP method implementation must not be empty.

"End of the SQLScript code

ENDMETHOD.
...

"Comments:
"  BY DATABASE PROCEDURE   -> Flags the AMDP method as AMDP procedure
"  FOR HDB                 -> Definess the database system where the method is to be used;
"                             currently, only HDB (SAP HANA database) is possible
"  LANGUAGE SQLSCRIPT      -> Defines the programming language of the database system
"  OPTIONS READ-ONLY       -> Specifies database-specific options
"  USING db_object.        -> Optional addition; specifies database objects;
"                             can also be AMDP procedures and functions
```

Note:

-   In [ABAP for Cloud Development](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_for_sap_cloud_glosry.htm) (i.e. the [restricted ABAP language version](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenrestricted_version_glosry.htm "Glossary Entry")
    scope), only reads are allowed. Hence, the addition `OPTIONS READ-ONLY` is mandatory. Furthermore, you must make sure
    that the database objects that are specified after `USING` are accessible.
-   Generally, in [Standard ABAP](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenstandard_abap_glosry.htm) (i.e. the [unrestricted ABAP language
    version](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenunrestricted_version_glosry.htm "Glossary Entry") scope), more syntax options are allowed for AMDP method declaration
    and implementation parts. Check the ABAP Keyword Documentation for
    more details as covered further down.

<p align="right"><a href="#top">⬆️ back to top</a></p>

## AMDP Functions

[Scalar](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_scalar_function_glosry.htm "Glossary Entry")
and [table
functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abentable_function_glosry.htm "Glossary Entry")
can be managed as [AMDP
functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_function_glosry.htm "Glossary Entry").
Such [AMDP table
functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_table_function_glosry.htm "Glossary Entry")
have, as the name implies, a tabular [return
value](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenreturn_value_glosry.htm "Glossary Entry")
whereas AMDP scalar functions have a scalar or elementary return value
(more information
[here](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp_function_methods.htm)).
Regarding AMDP table functions, there are two types available:

-   Functions that can only be accessed in other AMDP methods (i. e.
    other AMDP functions or procedures) and cannot be called directly in
    ABAP
-   Functions that implement [CDS table
    functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_table_function_glosry.htm "Glossary Entry")
    that can be accessed in ABAP SQL

Characteristics of method declaration parts of AMDP functions:

-   Similar to AMDP procedures, the methods can be declared as static or
    instance methods in any visibility section.
-   The method parameters must include a return value using
    <code>RETURNING</code> and having a tabular data type.
-   Additionally, the parameters can include elementary and tabular
    input parameters.
-   No class-based exceptions can be declared using <code>RAISING</code>.

Example for an AMDP function's declaration part:

```abap
...
PUBLIC SECTION.

  "Table type with a structured row type
  TYPES tab_type TYPE STANDARD TABLE OF dbtab WITH EMPTY KEY.

  METHODS amdp_func
    IMPORTING VALUE(num)       TYPE i,
              VALUE(some_elem) TYPE c LENGTH 3,
    RETURNING VALUE(tab)       TYPE tab_type.

...
```

The implementation part of an AMDP function is similar to the one of
AMDP procedure as shown above. The difference is the use of `BY DATABASE FUNCTION` instead of `BY DATABASE PROCEDURE`:

```abap
...
METHOD amdp_func
  BY DATABASE FUNCTION    
  FOR HDB
  LANGUAGE SQLSCRIPT
  OPTIONS READ-ONLY
  USING db_object.

"Beginning of the SQLScript code (note that it is not ABAP code although it looks similar)

  ... "Here goes SQLScript code;
      "AMDP table function to be called by other AMDP methods only

"End of the SQLScript code

ENDMETHOD.

"Comment:
"  BY DATABASE FUNCTION    -> Flags the AMDP method as AMDP function
...
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

### CDS Table Functions

-   Each [CDS table
    function](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_table_function_glosry.htm "Glossary Entry")
    is linked with an AMDP function in which it is implemented using
    SQLScript.
-   Can be used as data sources of ABAP SQL read statements.
-   Characteristics for method declaration and implementation parts
    regarding AMDP functions for CDS table functions:
    -   Method can only be declared as a static method in the public visibility section of
        an AMDP class using `CLASS-METHODS`.
    -   For the declaration, there is a special form with the addition
        `FOR TABLE FUNCTION`.
    -   The [parameter
        interface](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenparameter_interface_glosry.htm "Glossary Entry")
        is not specified. Instead, the input parameters are determined
        by the [input
        parameters](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abeninput_parameter_glosry.htm "Glossary Entry")
        of the CDS table function (i. e. the names and data types -
        which are always elementary - specified there are used). As the
        return value, a standard table with an empty key is generated
        based on the structured row type including the components as
        specified in the CDS table function.

Example for an AMDP functions declaration implementing a CDS table
function:

```abap
...
PUBLIC SECTION.

  CLASS-METHODS:
    table_func FOR TABLE FUNCTION some_ddl_source.

...
```

Notes on the [CDS
DDL](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_ddl_glosry.htm "Glossary Entry")
source of a CDS table function:

-   You have defined (and activated) a CDS DDL source, i. e. a [CDS
    entity](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_entity_glosry.htm "Glossary Entry"),
    with the notation `DEFINE TABLE FUNCTION`
-   You can specify optional input parameters using `... WITH PARAMETERS parameter1, parameter2, ...`
-   You must specify an element list using `... RETURNS { element1; element2; ...; } ...`. The elements determine the
    components of the structured data type represented by a CDS table
    function.
-   You have specified the `IMPLEMENTED BY METHOD` addition
    followed by a fully qualified method name in the form of
    `amdp_class=>amdp_method` using the names of the AMDP
    class and method.
-   More information
    [here](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abencds_table_functions.htm).

The CDS DDL source might look like this:

```
//Here go annotations.
define table function some_ddl_source
  returns
  {
    client : abap.clnt;
    field1 : abap.char(5);
    field2 : abap.char(5);
  }
  implemented by method amdp_class=>amdp_method;
```

You can then use the CDS table function as source for a
`SELECT` statement, for example: `SELECT * FROM some_ddl_source INTO ...`.

<p align="right"><a href="#top">⬆️ back to top</a></p>


## Using AMDP in ABAP for Cloud Development

### Notes on Client Handling and Client Safety

> **🌰 In brief**<br>
> - ABAP SQL features implicit client handling.
> - You cannot disable this feature in ABAP for Cloud Development, as you can in [Standard ABAP](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenstandard_abap_glosry.htm). You can only access your own client in ABAP for Cloud Development.
> - Native SQL, unlike ABAP SQL, does not feature implicit client handling. AMDP, which uses Native SQL, can be used in ABAP for Cloud Development. It is essential to ensure AMDP accesses only client-safe repository objects, meaning it retrieves data solely from client-dependent sources providing data of one client, i.e. the current client. Cross-client access must be avoided. Client-independent data sources are implicitly client-safe.
> - Dedicated additions and annotations in the context of AMDP ensure client safety.

**ABAP SQL**
- An SAP system can have multiple clients, each distinguished by a unique client ID.
- Each client holds specific data. When you log in, you select a client and can only access its data. For example, if you log into client 000, you access only client 000's data.
- A client column manages client-dependent data. If a database table has a client column, it contains client-dependent data. There are also client-independent data sources accessed typically by system programs.
- ABAP SQL features implicit client handling, automatically managing client selection. When you execute an ABAP SQL statement, it uses the current client without needing separate specification. The `USING CLIENT` addition to ABAP SQL statements can alter this behavior by disabling implicit client handling, however, this addition is not available in ABAP for Cloud Development.

**Native SQL/AMDP**
- Unlike ABAP SQL, Native SQL requires you to explicitly pass the client as there is no implicit handling.
- Native SQL is sent directly to the database. 
- AMDP, which uses Native SQL, does not feature implicit client handling either.
- Although AMDP is allowed in ABAP for Cloud Development, Native SQL cannot access client-dependent data.
- When using AMDP in ABAP for Cloud Development, you must ensure access only to the current client. Client safety is essential.
- AMDP methods in ABAP for Cloud Development must be client-safe. This means SQLScript code should access data only within your client. It is required to only use artifacts that restrict access to a single client or are client-independent.
- Therefore, all objects in the `USING` list of AMDP methods must be client-safe, including CDS table functions implemented as AMDP methods.
- Additions and annotations are available, ensuring client safety by restricting access to data of the current client only.

<p align="right"><a href="#top">⬆️ back to top</a></p>

### Restrictions for AMDP Methods in ABAP for Cloud Development 

- The AMDP procedure implementation must be restricted to reads. The relevant additions are `AMDP OPTIONS READ-ONLY` in AMDP method declarations and `OPTIONS READ-ONLY` in AMDP method implementations.
- AMDP method implementations can only use SQLScript code.
- The data sources accessed must be released for ABAP for Cloud Development.
- AMDP methods must be client-safe.

<p align="right"><a href="#top">⬆️ back to top</a></p>

### Making AMDP Methods Client-Safe

To ensure client safety, the following prerequisites must be met. 

- **AMDP method declarations** must use one of the following additions and specify particular objects in the **`USING` list**:

    - `CDS SESSION CLIENT DEPENDENT`
      - Declares the AMDP method as client-dependent when it uses at least one client-dependent object.
      - Ensures client safety by specifying client-safe objects in the `USING` list that access data from the current client only.
      - Data sources that can be specified in the `USING` list:
        - Client-dependent CDS view entities filtered by the HANA session variable `CDS_CLIENT` (CDS session variable `$session.client`) 
        - Client-dependent DDIC database tables 
          - Note: AMDP methods with the addition `CDS SESSION CLIENT DEPENDENT` handle client safety automatically. A database view, that wraps the database table, is generated implicitly to access and select data for the client specified in the HANA session variable `CDS_CLIENT`, ensuring the data source is client-safe.          
        - Client-safe CDS table functions 
        - Other client-dependent or client-independent AMDP methods with the AMDP option `CDS SESSION CLIENT DEPENDENT` or `CLIENT INDEPENDENT` AMDP methods
        - Client-independent CDS objects and DDIC database tables        

    - `CLIENT INDEPENDENT`
      - Declares the AMDP method as client-independent when it uses only client-independent objects.
      - Client-independent data sources can be specified in the `USING` list: Client-independent CDS view entities, DDIC database tables, client-safe CDS table functions, AMDP methods (specifying `CLIENT INDEPENDENT`)


- **CDS table functions** must have one of these annotations for client safety:  
  - `@ClientHandling.type: CLIENT_DEPENDENT` (should be used together with the  `@ClientHandling.algorithm: #SESSION_VARIABLE` annotation)  
  - `@ClientHandling.type: #CLIENT_INDEPENDENT`  
  - Parameters should not be marked with `@Environment.systemField: #CLIENT`  
  - Note: The annotation `@ClientHandling.clientSafe` ensures client-dependent CDS table functions are client-safe by enforcing client-safety checks. In ABAP for Cloud Development, specifying this annotation is unnecessary as the checks are automatically enabled.

 - **SQLScript code** in implementations of AMDP methods
    - Must not access database objects not managed by ABAP or not included in the `USING` list.
    - Must not call built-in HANA functions that include a client ID parameter.
    - The client parameter from AMDP method signatures should be removed.  If removal is not possible, for example, due to compatibility reasons, ensure the client is excluded from the SQLScript code to prevent interference from external callers.

> **💡 Note**<br>
> - Using pre-delivered repository objects in ABAP for Cloud Development:
>   - Use only pre-delivered AMDP methods with a [C4 contract](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/ABENC4_CONTRACT_GLOSRY.html).
>   - Create and use your own wrapper CDS view entities for pre-delivered CDS view entities with a [C1 contract](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/ABENC1_CONTRACT_GLOSRY.html).
> - More information: 
>   - The table in [this topic](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/ABENAMDP_CALL_MATRICES.html) shows an overview of specification options regarding client safety.
>   - [Client Handling in CDS Table Functions](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/ABENCDS_FUNC_CLIENT_HANDLING.html)

<p align="right"><a href="#top">⬆️ back to top</a></p>

## More Information 

- AMDP in ABAP for Cloud Development: [ABAP Keyword Documentation (ABAP for Cloud Development)](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm?file=abenamdp.htm)
- AMDP in Standard ABAP: [ABAP Keyword Documentation (Standard ABAP)](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenamdp.htm)
    > **✔️ Hint**<br>
    > Checking if AMDP is supported in classic ABAP: 
    >    ```abap
    >    IF NOT cl_abap_dbfeatures=>use_features(
    >        EXPORTING requested_features =
    >        VALUE #( ( cl_abap_dbfeatures=>call_amdp_method ) ) ).
    >
    >      "Result: Current database system does not support AMDP procedures
    >    RETURN.
    >    ENDIF.
    >    ```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## Executable Example

[zcl_demo_abap_amdp](./src/zcl_demo_abap_amdp.clas.abap)

> **💡 Note**<br>
> - The executable example covers the following topics:
>   - AMDP procedures, calling AMDP procedures from SQLScript
>   - AMDP table functions for AMDP methods
>   - AMDP table functions for CDS table functions
> - The AMDP example for ABAP for Cloud Development is designed differently compared to the AMDP example for Standard ABAP. Instead of using demo database tables, CDS view entities are used in the `USING` list. Additionally, the client handling is adjusted for the AMDP methods by including appropriate additions in the AMDP method declaration part.
> - The steps to import and run the code are outlined [here](README.md#-getting-started-with-the-examples).
> - [Disclaimer](./README.md#%EF%B8%8F-disclaimer)