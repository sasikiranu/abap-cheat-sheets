<a name="top"></a>

# Selection Screens and Classic Lists

- [Selection Screens and Classic Lists](#selection-screens-and-classic-lists)
  - [Introduction](#introduction)
    - [Selection Screens](#selection-screens)
    - [Classic Lists](#classic-lists)
  - [ABAP Statements for Selection Screens](#abap-statements-for-selection-screens)
    - [PARAMETERS](#parameters)
    - [SELECT-OPTIONS](#select-options)
    - [SELECTION-SCREEN](#selection-screen)
      - [Variants of the SELECTION-SCREEN Statement](#variants-of-the-selection-screen-statement)
    - [Calling Selection Screens](#calling-selection-screens)
  - [Excursion: SUBMIT Statements](#excursion-submit-statements)
  - [ABAP Statements for Classic Lists](#abap-statements-for-classic-lists)
    - [Creating Lists](#creating-lists)
    - [Reading and Modifying in Lists](#reading-and-modifying-in-lists)
  - [Event Blocks](#event-blocks)
  - [Excursion: SAP List Viewer (ALV)](#excursion-sap-list-viewer-alv)
    - [Getting Started](#getting-started)
    - [Optional Layout Settings and Functionality](#optional-layout-settings-and-functionality)
  - [More Information](#more-information)
  - [Executable Examples](#executable-examples)


## Introduction

> **💡 Note**<br>
>  The content of this cheat sheet and the executable examples are only relevant to [classic ABAP](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenclassic_abap_glosry.htm).

[Selection screens](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen_glosry.htm) and [classic lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenclassic_list_glosry.htm) are among the classic ABAP user interfaces. They are integrated into the ABAP language itself, which means that special ABAP statements are available to create and handle them.

This cheat sheet provides a high-level overview of selection screens and classic lists, focusing on a selection of related statements, supported by executable examples to explore the syntax in action. It includes an excursion into the SAP List Viewer (ALV).

For more detailed information and syntax options, see the topics [Selection Screens](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen.htm) and [Classic Lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_dynpro_list.htm) in the ABAP Keyword Documentation.

> **💡 Note**<br>
> - Although they are considered outdated for application programs, you will still find classic ABAP UIs frequently in classic ABAP.
> - Classic ABAP UIs cannot be created in [ABAP Cloud](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_cloud_glosry.htm).
> - This cheat sheet ...
>   - is not intended to encourage you to start creating classic ABAP UIs for programming new applications.  
>   - does not cover all facets, techniques, and syntax options (there's a rich variety of keywords and additions) in great detail. 
>   - is intended to cover a selection of related syntax that you may encounter in older ABAP code. If you need more information, always consult the ABAP Keyword Documentation.
> - Links to the ABAP Keyword Documentation in this cheat sheet refer to the documentation for [Standard ABAP](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenstandard_abap_glosry.htm) (latest version).

<p align="right"><a href="#top">⬆️ back to top</a></p>

### Selection Screens
- Are special [dynpros](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abendynpro_glosry.htm) in [executable programs](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenexecutable_program_glosry.htm) (*"reports"*; they're also possible in [function groups](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenfunction_group_glosry.htm) and [module pools](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenmodul_pool_glosry.htm), but the focus here is on executable programs).
- Used for data entry in an executable program, i.e. they allow users to ...
  - enter parameters (for entering single values).
  - provide selection criteria (complex selection options for value lists and value ranges) that can be used to supply an executable program with values to work with.
- Can be created using special ABAP statements in the global declaration part of executable programs 
- Are processed by the ABAP runtime framework, which triggers [selection screen events](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen_event_glosry.htm) ([see further down](#event-blocks)).
- When you create a selection screen and run an ABAP program, the dynpros are generated automatically. These generated dynpros cannot be edited directly.
- Further ways in which selections screens are different from *regular* dynpros:
  - Unlike dynpros, you can create selection screens without using the [screen painter](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenscreen_painter_glosry.htm) tool. The [ABAP Editor](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_editor_glosry.htm) is the only tool you use to define a selection screen (using ABAP statements).
  - They have a more limited set of features, for example, the layout of selection screens is more restricted. However, selection screens offer various types of input fields, which can also be expressed as checkboxes or radio buttons, and which can be grouped and labeled.
  - Modification options are limited. For example, you can only use some predefined function codes.
  - You cannot define [dialog modules](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abendialog_module_glosry.htm) in the ABAP program.
- There are two types of selection screens: 
  - [Standalone selection screens](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenstand-alone_sel_screen_glosry.htm)
    - Defined between the statements `SELECTION-SCREEN BEGIN OF SCREEN` and `SELECTION-SCREEN END OF SCREEN`
    - Can be defined in all programs that can contain dynpros
  - [Standard selection screens](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenstandard_selscreen_glosry.htm)    
    - Each executable program (and only there) contains a standard selection screen with the dynpro number 1000. Note: If you create a standalone selection screen, you cannot use dynpro number 1000.
    - The screen elements on the standard selection screen are defined by all `PARAMETERS`, `SELECT-OPTIONS`, and `SELECTION-SCREEN` statements that are defined outside of the statements mentioned above for creating standalone selection screens.
  
<p align="right"><a href="#top">⬆️ back to top</a></p>

### Classic Lists
- Used to output data in a structured and formatted way.
- This output can be ...
  - created in the so-called [list buffer](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenlist_buffer_glosry.htm), a memory area for storing [screen lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenscreen_list_glosry.htm), i.e. a list that is displayed on a predefined list dynpro (list dynpro that is accessed implicitly when a program is executed; note: this dynpro is not part of the executed program). 
  - sent to the [SAP spool system](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abensap_spool_system_glosry.htm) as a spool list (i.e. a list that is not stored as a screen list but is intended for printing or archiving; the focus of this cheat sheet is on screen lists).
- Typical flow of executable programs that include selection screens and content to be displayed on the list dynpro:
  - Programs are executed using the *Execute* button (F8).
  - Selection screens are displayed, users usually make entries for whatever purpose, which are further processed in the program.  
  - Output result is written to the list buffer and finally displayed (on a list dynpro; note: the list processing is automatically started when running executable programs). 
  - The list can be further implemented to respond to user interaction, such as clicking a line in the list.  
- More modern alternatives for classic lists are available, such as the classes of the SAP List Viewer (ALV), for example `CL_SALV_TABLE`.

> **💡 Note**<br>
> The program is grouped into so-called [event blocks](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenevent_block_glosry.htm), which can contain implementations for various events (e.g. a [selection screen event](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen_event_glosry.htm) when a user selects a radio button, or a [list event](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenlist_event_glosry.htm) when a user double-clicks a line in the list). These event blocks are introduced by special ABAP statements. When a particular event is triggered, the corresponding event block is called and the appropriate code can be implemented there to react to the user action. See more information [here](#event-blocks).

<p align="right"><a href="#top">⬆️ back to top</a></p>

## ABAP Statements for Selection Screens

Selection screens can be created by using special ABAP statements in the global declaration part of executable programs: 
- [`PARAMETERS`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapparameters.htm)
- [`SELECT-OPTIONS`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapselect-options.htm)
- [`SELECTION-SCREEN`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapselection-screen.htm)

### PARAMETERS
- Creates ...
  - a program-global variable (similar to `DATA` statements) 
  - an input field on the selection screen automatically, i.e. the content of the program-global variable can be changed via the input field.
- The parameter name can be up to eight characters long.
- Multiple additions are available to define the appearance (e.g. `AS CHECKBOX`) or influence the user interface (e.g. `OBLIGATORY`).

The following table includes code snippets with a selection of available syntax options:

> **💡 Note**<br>
> - Various combinations of multiple additions are possible with `PARAMETERS` statement, and some are not. See the ABAP Keyword Documentation for details.
> - To try out the code examples, you can create a demo executable program, copy and paste the code snippets, and run the programs using F8. They are designed to output content using classic lists.
> - The code snippets anticipate topics outlined further down, for example, event blocks and `WRITE` statements.

<table>

<tr>
<td> Subject/Additions </td> <td> Notes </td> <td> Code Snippet </td>
</tr>

<tr>
<td> 

Type specification options<br><br>
Additions `TYPE` and `LIKE`

 </td>

 <td> 

- You can specify data types for parameters similar to `DATA` statements. 
- This includes the `TYPE` and `LIKE` additions. Types requiring a length specification use the `TYPE ... LENGTH ...` additions.
- Regarding the type `c`, there are syntax variants to specify the length and the type itself (such as leaving out the explicit type or length specification; length specification in parentheses). 
- Note that without the `LOWER CASE` addition, inserted character-like values are transformed to uppercase.
- For type `p`, you can specify the `DECIMALS` addition.

 </td>

<td> 

``` abap
PROGRAM.

"TYPE addition
"The example uses a built-in ABAP type
"Note the LOWER CASE addition further down.
PARAMETERS p_str TYPE string.

"LIKE addition
"Referring to an existing data object
DATA some_number TYPE i.
PARAMETERS p_num LIKE some_number.

"TYPE ... LENGTH for types that require a length specification
"such as c, n, p, and x
PARAMETERS p_clen10 TYPE c LENGTH 10.
PARAMETERS p_nlen5 TYPE n LENGTH 5.

"Length specification variants
"The length can be specified in parentheses, however, specifying LENGTH
"explicitly is recommended for better readability.
PARAMETERS p_lenpar(3) TYPE c.
"No length specified means LENGTH 1 by default
PARAMETERS p_wo_len TYPE c.
"No explicit type specification means TYPE c by default
PARAMETERS p_wo_c_a(20).
PARAMETERS p_wo_c_b LENGTH 20.
"No explicit type and length specification means TYPE c LENGTH 1 by default
PARAMETERS p_woclen.

START-OF-SELECTION.

  WRITE / `Inserted values (note that inserted characters are capitalized):`.
  SKIP.
  WRITE / |p_str: "{ p_str }"|.
  WRITE / |p_num: "{ p_num }"|.
  WRITE / |p_clen10: "{ p_clen10 }"|.
  WRITE / |p_nlen5: "{ p_nlen5 }"|.
  WRITE / |p_lenpar: "{ p_lenpar }"|.
  WRITE / |p_wo_len: "{ p_wo_len }"|.
  WRITE / |p_wo_c_a: "{ p_wo_c_a }"|.
  WRITE / |p_wo_c_b: "{ p_wo_c_b }"|.
  WRITE / |p_woclen: "{ p_woclen }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Parameters using ABAP Dicitionary input helps

 </td>

 <td> 

- When referring to data types from the ABAP Dictionary, all screen-relvant properties of that type are adopted.
- If the referred type includes input help functionality, a callable field help is automatically created.
- The input helps inlcude search helps, check tables, fixed values and calendar/clock helps. Find more information [here](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_dynpros_value_help_auto.htm).
- Note that there is no automatic value check. For that, use the `VALUE CHECK` addition.
- The code snippet includes input helps from a DDIC structure of an ABAP Keyword Documentation example.

 </td>

<td> 

``` abap
PROGRAM.

"carrier1 is based on DDIC data element demof4de, which has the search help
"demo_f4_de assigned
PARAMETERS p_carr1 TYPE demof4help-carrier1.

"carrier2 is assigned to check table scarr, which is assigned to the
"search help h_scarr
PARAMETERS p_carr2 TYPE demof4help-carrier2.

"DDIC data element that has the search help s_carrier_id assigned
PARAMETERS p_carr3 TYPE s_carr_id.

"connid is assigned to the search help demo_f4_field
PARAMETERS p_connid TYPE demof4help-connid.

"Component of demo database table that specifies a foreign key
PARAMETERS p_plane TYPE sflight-planetype.

"Fixed values as input help
PARAMETERS p_num TYPE demo_numbers.

"ABAP Dictionary types such as DATS and TIMS have
"a predefined input help (calendar and clock).
PARAMETERS p_dats TYPE dats.
PARAMETERS p_tims TYPE tims.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |p_carr1: "{ p_carr1 }"|.
  WRITE / |p_carr2: "{ p_carr2 }"|.
  WRITE / |p_carr3: "{ p_carr3 }"|.
  WRITE / |p_connid: "{ p_connid }"|.
  WRITE / |p_plane: "{ p_plane }"|.
  WRITE / |p_num: "{ p_num }"|.
  WRITE / |p_dats: "{ p_dats }"|.
  WRITE / |p_tims: "{ p_tims }"|.
``` 

 </td>
</tr>


<tr>
<td> 

Dynamically specifying the type<br><br>
Addition `LIKE (...)`

 </td>

 <td> 

- The dynamic specification works with `LIKE` followed by a data object name in parentheses.
- As name, a character-like data object that contains the name of a structure component in the ABAP Dictionary in uppercase letters.
- The parameter is then of type `c` length 132, however, the properties such as length and input help are taken from the type specified in parentheses.

 </td>

<td> 

``` abap
PROGRAM.

"In the example, the currently loaded text pool does not contain a selection text 
"for the parameters. Here, the output field displays the field label from the
"ABAP Dictionary.
DATA some_dobj TYPE c LENGTH 30.
PARAMETERS p_dyn1 LIKE (some_dobj).

DATA comp TYPE c LENGTH 20.
PARAMETERS p_dyn2 LIKE (comp).

"Parameter on the screen with type c length 132
"Here, no selection text is created for the example, and no field
"label field can be referred to. The output field displays the text 
"'Dynamic Parameter'.
DATA another_dobj like some_dobj.
PARAMETERS p_dyn3 LIKE (another_dobj).

"Text field literal, however, its content is not evaluated.
"Also here, the parameter on the screen is of type
"c length 132.
PARAMETERS p_dyn4 LIKE ('SPFLI-CARRID').

INITIALIZATION.
  some_dobj = 'SPFLI-CARRID'.
  comp = 'SFLIGHT-PLANETYPE'.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |p_dyn1: "{ p_dyn1 }"|.
  WRITE / |p_dyn2: "{ p_dyn2 }"|.
  WRITE / |p_dyn3: "{ p_dyn3 }"|.
  WRITE / |p_dyn4: "{ p_dyn4 }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Specifying value options<br><br>
Additions `DEFAULT`, `LOWER CASE`, `MATCHCODE OBJECT`, `MEMORY ID`, `VALUE CHECK`

 </td>

 <td> 

- `DEFAULT`: Defines a start value (can also be a data object instead of a literal)
- `LOWER CASE`: Prevents the effect of capitalizing the entry made when the content is transported to the data object
- `MATCHCODE OBJECT`: Links an input field with a DDIC search help
- `MEMORY ID`: Links an input field with an SPA/GPA parameter in the user memory, i.e. data objects in the user memory accessible by ABAP programs. When calling the selection screen, the input field receives the SPA/GPA parameter value if the `PARAMETERS`'s data object is initial after processing of the `AT SELECTION-SCREEN OUTPUT` event block. The SPA/GPA parameters are set using [`SET PARAMETER`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapset_parameter.htm) and read using [`GET PARAMETER`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapget_parameter.htm) statements. See the names of the parameters in the `TPARA` database table.
- `VALUE CHECK`: Checks input against fixed values defined for the domain of a data type. It can only be used for DDIC data types. Checks are also available for data types being components of foreign key tables. It is recommended to specify the `VALUE CHECK` addition with the `OBLIGATORY` addition as the check is also applied in case of empty input fields.

 </td>

<td> 

``` abap
PROGRAM.

*&---------------------------------------------------------------------*
*& DEFAULT addition
*&---------------------------------------------------------------------*

PARAMETERS p_deflti TYPE i DEFAULT 12345.
PARAMETERS p_deflts TYPE string DEFAULT `This is some demo text`.

*&---------------------------------------------------------------------*
*& LOWER CASE addition
*&---------------------------------------------------------------------*

PARAMETERS p_upper TYPE string DEFAULT `Hello World`.
PARAMETERS p_lower TYPE string DEFAULT `Hello World` LOWER CASE.

*&---------------------------------------------------------------------*
*& MATCHCODE OBJECT addition
*&---------------------------------------------------------------------*

"The following example uses a search help of an ABAP Keyword Documentation example.
PARAMETERS p_carr1 TYPE s_carr_id MATCHCODE OBJECT demo_f4_de.
"Another predelivered demo search help.
PARAMETERS p_carr2 TYPE c LENGTH 3 MATCHCODE OBJECT s_carrier_id.
"Non-existent search help (triggers a message in the status line)
PARAMETERS p_nope TYPE s_carr_id MATCHCODE OBJECT demo_f4_de###.

*&---------------------------------------------------------------------*
*& MEMORY ID addition
*&---------------------------------------------------------------------*

"The example use the memory id 'rid' that stores the name of the program
"processed last. The demo explicitly sets the value to a dummy program
"name in the AT SELECTION-SCREEN OUTPUT event block using a SET PARAMETER ID
"statement.
PARAMETERS p_prog TYPE sy-repid MEMORY ID rid.

*&---------------------------------------------------------------------*
*& VALUE CHECK addition
*&---------------------------------------------------------------------*

"The example selection parameters are specified with reference to a
"demo database table field for which the check table scarr is specified.
"So, you can only enter values that are also contained in scarr.
"When running the example program, you can ...
"- make entries that are not contained in the list and execute the program 
"  to demonstrate that the value check is applied.
"- leave the input for pvalchk1 empty to demonstrate that the value check is 
"  also applied on empty fields. Therefore, also specifying the OBLIGATORY 
"  addition is recommended.
PARAMETERS pvalchk1 TYPE spfli-carrid VALUE CHECK.
PARAMETERS pvalchk2 TYPE spfli-carrid VALUE CHECK OBLIGATORY.

AT SELECTION-SCREEN OUTPUT.
  SET PARAMETER ID 'RID' FIELD 'SOME_TEST_REPORT'.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |p_deflti: "{ p_deflti }"|.
  WRITE / |p_deflts: "{ p_deflts }"|.
  WRITE / |p_upper: "{ p_upper }"|.
  WRITE / |p_lower: "{ p_lower }"|.
  WRITE / |p_carr1: "{ p_carr1 }"|.
  WRITE / |p_nope: "{ p_nope }"|.
  WRITE / |p_prog: "{ p_prog }"|.
  WRITE / |pvalchk1: "{ pvalchk1 }"|.
  WRITE / |pvalchk2: "{ pvalchk2 }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Specifying screen options<br><br>
Additions `OBLIGATORY`, `NO-DISPLAY`, `VISIBLE LENGTH`, `AS CHECKBOX`, `RADIOBUTTON GROUP`, `AS LISTBOX VISIBLE LENGTH`

 </td>

 <td> 

- `OBLIGATORY`: Declares the input field as a required field. If there is no entry, the program cannot proceed when choosing *Execute*. A message will be displayed.
- `NO-DISPLAY`: Hides the input field on the selection screen. A value can be supplied when calling the program with `SUBMIT` and the `WITH` addition. Note that with the `NO-DISPLAY` addition, the parameter can have any data types except for reference/enumerated types, unlike in the other additions which require flat types (except type `string`).
- `VISIBLE LENGTH`: Defines the visible length of the field. If you specify the parameter with type `c` and length 10, and you specify the visible length as 3, the input field size is reduced accordingly, but you can still insert 10 characters.
- `AS CHECKBOX`: Displays an input field as checkbox. Type `c` and length 1 is expected, but the explicit length specification is not allowed. The checkbox is selected if the value has the value `X` or `x`.
- `RADIOBUTTON GROUP`: Defines a radio button group for parameters. Note:
  - The group name can have a maximum of four characters.
  - Regarding the data type, the same applies as for `AS CHECKBOX`.
  - Only one parameter can be defined with the `DEFAULT` addition.
  - If `DEFAULT` is not specified, the first parameter of the group is set to the value `X`.
- `AS LISTBOX VISIBLE LENGTH`: Creates a dropdown list box. You can use the function module `VRM_SET_VALUES` by passing a suitable list at the 
`AT SELECTION-SCREEN OUTPUT` or `AT SELECTION-SCREEN ON VALUE-REQUEST` events. You may want to specify the `OBLIGATORY` addition here, too, as the check is also applied to empty fields.

> **💡 Note**<br>
> The `USER-COMMAND` addition can be specified together with `AS CHECKBOX`, `RADIOBUTTON GROUP` and `AS LISTBOX VISIBLE LENGTH` to assign a function code to the selection parameter.



 </td>

<td> 

``` abap
PROGRAM.

*&---------------------------------------------------------------------*
*& OBLIGATORY addition
*&---------------------------------------------------------------------*

PARAMETERS p_oblgty TYPE c LENGTH 10 OBLIGATORY.

*&---------------------------------------------------------------------*
*& NO-DISPLAY addition
*&---------------------------------------------------------------------*

PARAMETERS p_hidden TYPE c LENGTH 10 NO-DISPLAY.
"With NO-DISPLAY, also complex types (except reference types) can be
"specified.
PARAMETERS p_tabtyp TYPE string_table NO-DISPLAY.

*&---------------------------------------------------------------------*
*& VISIBLE LENGTH addition
*&---------------------------------------------------------------------*

PARAMETERS p_vislen TYPE c LENGTH 10 VISIBLE LENGTH 3.

*&---------------------------------------------------------------------*
*& AS CHECKBOX addition
*&---------------------------------------------------------------------*

"Implicit type c
PARAMETERS p_chkbx1 AS CHECKBOX.
"Explicit type specification (but no explicit length)
PARAMETERS p_chkbx2 TYPE c AS CHECKBOX.

*&---------------------------------------------------------------------*
*& RADIOBUTTON GROUP addition
*&---------------------------------------------------------------------*

"The following example uses a chained statement.
PARAMETERS: p_rb1a RADIOBUTTON GROUP rbg1,
            p_rb1b TYPE c RADIOBUTTON GROUP rbg1,       "Explicit type specification
            p_rb1c RADIOBUTTON GROUP rbg1 DEFAULT 'X'.  "Select this radiobutton by default

"Separate specifications, no DEFAULT specification
"Here, the first radio button is selected by default.
PARAMETERS p_rb2a RADIOBUTTON GROUP rbg2.
PARAMETERS p_rb2b RADIOBUTTON GROUP rbg2.
PARAMETERS p_rb2c RADIOBUTTON GROUP rbg2.
PARAMETERS p_rb2d RADIOBUTTON GROUP rbg2.

*&---------------------------------------------------------------------*
*& AS LISTBOX VISIBLE LENGTH
*&---------------------------------------------------------------------*

"See the implementation in the AT SELECTION-SCREEN OUTPUT event block
PARAMETERS plistbox TYPE i AS LISTBOX VISIBLE LENGTH 10 OBLIGATORY.

AT SELECTION-SCREEN OUTPUT.

  "The following implementation is relevant for the AS LISTBOX VISIBLE LENGTH
  "addition. Note:
  "- The function module VRM_SET_VALUES is used.
  "- Column TEXT: What is displayed in the list box.
  "- Colum KEY: When a line is selected, the KEY value is added to the parameter
  "- In the example, the numbers 1 - 10 are added as values.
  CALL FUNCTION 'VRM_SET_VALUES'
    EXPORTING
      id     = CONV vrm_id( 'PLISTBOX' )
      values = VALUE vrm_values( FOR i = 1 UNTIL i > 10 ( key = i text = |Value { i }| ) ).

  "Filling the string table for the NO-DISPLAY example statement.
  p_tabtyp = VALUE #( ( `Some` ) ( `demo` ) ( `strings` ) ).

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |p_oblgty: "{ p_oblgty }"|.
  WRITE / |p_hidden: "{ p_hidden }"|.

  LOOP AT p_tabtyp into data(wa).
    WRITE / |p_tabtyp (line { sy-tabix }): "{ wa }"|.
  endloop.

  IF p_chkbx1 IS INITIAL.
    WRITE / `Checkbox p_chkbx1 was not selected.`.
  ELSE.
    WRITE / `Checkbox p_chkbx1 was selected.`.
  ENDIF.

  IF p_chkbx2 IS INITIAL.
    WRITE / `Checkbox p_chkbx2 was not selected.`.
  ELSE.
    WRITE / `Checkbox p_chkbx2 was selected.`.
  ENDIF.

  WRITE / |Radio button rbg1 selection: "{ COND #( WHEN p_rb1a IS NOT INITIAL THEN 'p_rb1a' WHEN p_rb1b IS NOT INITIAL THEN 'p_rb1b' ELSE 'p_rb1c' ) }"| .
  WRITE / |Radio button rbg2 selection: "{ COND #( WHEN p_rb2a IS NOT INITIAL THEN 'p_rb2a' WHEN p_rb2b IS NOT INITIAL THEN 'p_rb2b' WHEN p_rb2c IS NOT INITIAL THEN 'p_rb2c' ELSE 'p_rb2d' ) }"| .

  WRITE / |p_list: "{ plistbox }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Assigning function codes to selection parameters<br><br>
Addition `USER-COMMAND`

 </td>

 <td> 

- You can assign function codes to selection parameters using the `USER-COMMAND` addition.
- The addition is supported by `PARAMETERS` statements specifying the `AS CHECKBOX`, `RADIOBUTTON GROUP` and `AS LISTBOX VISIBLE LENGTH` additions.
- As a prerequisite, you must specify a [`TABLES`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abaptables.htm) statement to declare an [interface work area](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abeninterface_work_area_glosry.htm) of the `sscrfields` DDIC structure.
- On user selection (e.g. a checkbox or radio button is selected), the `AT SELECTION-SCREEN` event is raised, and the function code is passed to the `ucomm` component of `sscrfields`. For evaluation purposes, evaluate `sscrfields-ucomm` instead of `sy-ucomm` as it is not guaranteed that the latter is always passed the correct value during selection screen processing.

 </td>

<td> 

``` abap
PROGRAM.

"Declaring interface work area
TABLES sscrfields.

*&---------------------------------------------------------------------*
*& AS CHECKBOX USER-COMMAND
*&---------------------------------------------------------------------*

PARAMETERS p_chkbox AS CHECKBOX USER-COMMAND box_cmd.

*&---------------------------------------------------------------------*
*& RADIOBUTTON GROUP group USER-COMMAND
*&---------------------------------------------------------------------*

PARAMETERS: p_rb1 RADIOBUTTON GROUP rbg USER-COMMAND rb_cmd,
            p_rb2 RADIOBUTTON GROUP rbg,
            p_rb3 RADIOBUTTON GROUP rbg.

*&---------------------------------------------------------------------*
*& AS LISTBOX VISIBLE LENGTH ... USER-COMMAND
*&---------------------------------------------------------------------*

PARAMETERS plistbox TYPE i AS LISTBOX VISIBLE LENGTH 10 USER-COMMAND list_cmd.

AT SELECTION-SCREEN OUTPUT.

  CALL FUNCTION 'VRM_SET_VALUES'
    EXPORTING
      id     = CONV vrm_id( 'PLISTBOX' )
      values = VALUE vrm_values( FOR i = 1 UNTIL i > 10 ( key = i text = |Value { i }| ) ).

AT SELECTION-SCREEN.

  CASE sscrfields-ucomm.
    WHEN 'BOX_CMD'.
      MESSAGE |Hallo { sy-uname }. You selected a checkbox.| TYPE 'I'.
    WHEN 'RB_CMD'.
      MESSAGE |Hallo { sy-uname }. You selected a radio button.| TYPE 'I'.
    WHEN 'LIST_CMD'.
      MESSAGE |Hallo { sy-uname }. You selected an item from the list box.| TYPE 'I'.
  ENDCASE.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.

  IF p_chkbox IS INITIAL.
    WRITE / `Checkbox p_chkbox was not selected.`.
  ELSE.
    WRITE / `Checkbox p_chkbox was selected.`.
  ENDIF.

  WRITE / |Radio button rbg selection: "{ COND #( WHEN p_rb1 IS NOT INITIAL THEN 'p_rb1' WHEN p_rb2 IS NOT INITIAL THEN 'p_rb2' ELSE 'p_rb3' ) }"| .

  WRITE / |p_list: "{ plistbox }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Assigning a screen element of a selection screen to a modification group<br><br>
Addition `MODIF ID`

 </td>

 <td> 

- The `PARAMETERS` statement can be specified with the `MODIF ID` addition to assign a screen element of a selection screen to a modification group, which represents a group of multiple screen elements of a dynpro having a three-character ID. This ID is used to modify the display properties of all those elements specifying the ID before they are actually displayed. For that purpose, you can use the `MODIFY SCREEN` statement. The ID is assigned to the `screen-group1` component that can be evaluated. 
- Find more information [here](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenmodification_group_glosry.htm).

 </td>

<td> 

``` abap
PROGRAM.

TABLES sscrfields.

PARAMETERS showhide AS CHECKBOX USER-COMMAND box_cmd1.

PARAMETERS p_input1 TYPE c LENGTH 20.
PARAMETERS p_input2 TYPE c LENGTH 20 MODIF ID id1.
PARAMETERS p_input3 TYPE c LENGTH 20.
PARAMETERS p_input4 TYPE c LENGTH 20 MODIF ID id1.

PARAMETERS colored AS CHECKBOX USER-COMMAND box_cmd2.

PARAMETERS p_input5 TYPE c LENGTH 20 DEFAULT 'abcdefg'.
PARAMETERS p_input6 TYPE c LENGTH 20 MODIF ID id2 DEFAULT 'hijklm'.
PARAMETERS p_input7 TYPE c LENGTH 20 MODIF ID id2 DEFAULT 'nopqrs'.
PARAMETERS p_input8 TYPE c LENGTH 20 DEFAULT 'tuvwxyz'.

AT SELECTION-SCREEN.

  CASE sscrfields-ucomm.
    WHEN 'BOX_CMD1'.
      MESSAGE |Hallo { sy-uname }. You selected the SHOWHIDE checkbox. Screen elements are about to be shown or hidden.| TYPE 'I'.
    WHEN 'BOX_CMD2'.
      MESSAGE |Hallo { sy-uname }. You selected the COLORED checkbox. The colors of output fields are about to be intensified or reverted.| TYPE 'I'.
  ENDCASE.

AT SELECTION-SCREEN OUTPUT.
  LOOP AT SCREEN INTO DATA(screen_wa).
    IF showhide = 'X' AND screen_wa-group1 = 'ID1'.
      screen_wa-active = '0'.
    ENDIF.
    IF colored = 'X' AND screen_wa-group1 = 'ID2'.
      screen_wa-intensified = '1'.
    ENDIF.
    MODIFY SCREEN FROM screen_wa.
  ENDLOOP.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |p_input1: "{ p_input1 }"|.
  WRITE / |p_input2: "{ p_input2 }"|.
  WRITE / |p_input3: "{ p_input3 }"|.
  WRITE / |p_input4: "{ p_input4 }"|.
  WRITE / |p_input5: "{ p_input5 }"|.
  WRITE / |p_input6: "{ p_input6 }"|.
  WRITE / |p_input7: "{ p_input7 }"|.
  WRITE / |p_input8: "{ p_input8 }"|.
``` 

 </td>
</tr>

<tr>
<td> 

Replacing the technical name displayed on the screen 


 </td>

 <td> 

- The parameter name is a technical name. You probably do not want to display technical names on the user interface. You can define proper names ([text elements](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abentext_element_glosry.htm)) in the ABAP Editor, e.g. for translation purposes.
- For the example code snippet, proceed as follows: 
  - In ADT, make a right-click inside your demo report you copied the code to. Choose *Open Others -> Text Elements*.
  - At the bottom of the opened window, select the *Selection Texts* tab.
  - Insert `PARAM=Enter some text` and activate. Leave the *number* parameter unchanged. 
  - You can also use `SELECTION-SCREEN` statements such as the `... COMMENT ...` statement, as outlined below, and assign a text value to the screen comment.
  - When you execute the program, you will see the technical names replaced by *Enter some text* and *Enter a number*.

 </td>

<td> 

``` abap
PROGRAM.

PARAMETERS param type string.

SELECTION-SCREEN BEGIN OF LINE.
  SELECTION-SCREEN COMMENT (33) txt.
  PARAMETERS number type i.
SELECTION-SCREEN END OF LINE.

INITIALIZATION.

  txt = 'Enter a number'.

START-OF-SELECTION.

  WRITE / `Inserted values:`.
  SKIP.
  WRITE / |param: "{ param }"|.
  WRITE / |number: "{ number }"|.
``` 

 </td>
</tr>

</table>


<p align="right"><a href="#top">⬆️ back to top</a></p>

### SELECT-OPTIONS

- Declares selection criteria for a data object
- Unlike the `PARAMETERS` statement, which specifies a single value for a variable, the `SELECT-OPTIONS` statement allows you to specify complex criteria, such as an value range or a list of single values, to include or exclude values, that can be evaluated.
- The selection criteria are assigned to a selection table (see [ranges table](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenranges_table_glosry.htm) and the type `TYPE RANGE OF`).
  - Such a table type contains four columns - `LOW`, `HIGH`, `OPTION`, `SIGN` - for determining range conditions. Each line of such a table represents a condition. 
  - Typically, the content of the selection table can be evaluated in `SELECT` statements using the `IN` operator in the `WHERE` clause.
  - If the selection table is empty, all lines are respected.

Example:

```abap
DATA wa TYPE some_tab.

SELECT-OPTIONS sel FOR some_tab-some_comp.

SELECT *
       FROM some_tab
       WHERE some_comp IN @sel
       INTO @wa.
  ...
ENDSELECT.

"Multiple additions and combinations are possible.

"The following statement creates two input fields that can be 
"used to specify selection criteria on the selection screen. 
"In addition, the Multiple Selection button is available to 
"further specify the selection criteria, e.g. what to include, 
"exclude, etc.. As in all examples, the variable is an internal 
"table of type TYPE RANGE OF whose contents can be evaluated.
"The FOR addition is followed by an already declared data object.
SELECT-OPTIONS a FOR some_dobj.
"As mentioned above, NO-DISPLAY hides.
SELECT-OPTIONS b FOR some_dobj NO-DISPLAY.
"NO-EXTENSION: The Multiple Selection button is not created on the 
"selection screen
SELECT-OPTIONS c FOR some_dobj NO-EXTENSION.
"NO INTERVALS: Only one input field. Intervals can still be selected 
"using the Multiple Selection button.
SELECT-OPTIONS d FOR some_dobj NO INTERVALS.
"DEFAULT ... TO ...: Providing start values for the columns in the 
"first line of the selection table (low and high values)
SELECT-OPTIONS e FOR some_dobj DEFAULT 3 TO 10.
"DEFAULT ... OPTION ... SIGN ...: Providing further start values.
"See details further down.
SELECT-OPTIONS f FOR some_dobj DEFAULT 4 TO 8 OPTION NB SIGN I.
```

> **💡 Note**<br>
> Notes on the `OPTION` and `SIGN` additions: 
>  - `OPTION` 
>    - Start value for the `option` column for the condition in the form of comparison operators
>    - `TO` not specified: `EQ` (equal), `NE` (not equals), `GE` (greater than or equals), `GT` (greater than), `LE` (less than or equals), `LT` (less than), `CP` (conforms to pattern), or `NP` (does not conform to pattern) must be specified
>    - `TO` specified: Either `BT` (between) or `NB` (not between) must be specified
>    - `OPTION` not specified: `EQ`/`BT` is used
>    - For `CP` and `NP`, wildcard characters `*` or `+` must be specified.
>  - `SIGN` 
>    - Start value for the `sign` column 
>    - Either `I` or `E` must be specified to include/exclude the result of the condition 
>    - `SIGN` not specified: `I` by default
> - For historical reasons, the selection table is a table with [header line](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenheader_line_glosry.htm). Therefore, if you want to address the table content (beyond the use in a `SELECT ... WHERE ... IN ...` statement), use the syntax `a[]`.


<p align="right"><a href="#top">⬆️ back to top</a></p>


### SELECTION-SCREEN

- `SELECTION-SCREEN` statements can be used to create and modify the layout of standalone selection screens.
- Note: The standard selection screen for executable programs is created automatically. That is, each executable program contains a standard selection screen with dynpro number 1000.
- Selection screens can be created as regular dynpros or as subscreen dynpros.

```abap
"-------------------------- Pattern --------------------------

SELECTION-SCREEN BEGIN OF SCREEN ...

  ...
  "Here go all PARAMETERS, SELECT-OPTIONS, and SELECTION-SCREEN statements 
  "to define the screen elements of the standalone selection screen.  

SELECTION-SCREEN END OF SCREEN ...


"-------------------------- Regular dynpros --------------------------

"Creating a selection screen as regular dynpro
"Note that the dynpro number must be unique in the program. Do not use 1000.
SELECTION-SCREEN BEGIN OF SCREEN 9000.
  ...
SELECTION-SCREEN END OF SCREEN 9000.

"Note: The following additions can also be combined.
"TITLE: Defining a title (e.g. the name of a text symbol) for the title bar 
SELECTION-SCREEN BEGIN OF SCREEN 9001 TITLE some_title.
  ...
SELECTION-SCREEN END OF SCREEN 9001.

"AS WINDOW: Displaying the selection screen in a modal dialog box
SELECTION-SCREEN BEGIN OF SCREEN 9002 AS WINDOW.
  ...
SELECTION-SCREEN END OF SCREEN 9002.

"-------------------------- Subscreen dynpros --------------------------

"Can be included in other dynpros or selection screens, or in subscreen areas 
"or tab pages. But they cannot be called explicitly.
SELECTION-SCREEN BEGIN OF SCREEN 9003 AS SUBSCREEN.
...
SELECTION-SCREEN END OF SCREEN 9003.

"Note: There are more additions available regarding display adjustment, among others.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

#### Variants of the SELECTION-SCREEN Statement

- The variants do not create selection screens, but are used to change the layout of selection screens, create additional screen elements, and so on.
- Example: The `PARAMETERS` and `SELECT-OPTIONS` statements create input fields on an individual line. Using the variants of the `SELECTION-SCREEN` statement, you can arrange the layout differently.
- However, the selection screen cannot be generated if there are conflicts with existing screen elements.
  - See the effect with the following snippet. Run a program with the code snippet and comment in the third line. The program terminates. The text to be created (in the same line as the first text) interferes with the first text. However, the fourth line would be possible because there is no overlap.
    ```abap
    PARAMETERS pa TYPE string.
    SELECTION-SCREEN COMMENT /5(10) t1.
    "SELECTION-SCREEN COMMENT 10(20) t2.
    "SELECTION-SCREEN COMMENT 20(20) t3.
    ```

Syntax examples:

```abap
"Note: More optional additions are available for the the following statements.
"For the details, check the ABAP Keyword Documentation.

"Adding blank lines
SELECTION-SCREEN SKIP.   "1 blank line by default
SELECTION-SCREEN SKIP 5. "5 blank lines

"Creating a horizontal line
"Line accross the whole screen below the lines that are already filled
SELECTION-SCREEN ULINE.     
"/: Line in a new line, 5: position, (10): length
SELECTION-SCREEN ULINE /5(10).  
"Note: The following statement commented out can only be specified 
"inside a BEGIN/END OF LINE statement. Outside, specifying a position
"is required.
"SELECTION-SCREEN ULINE (10).      

"Providing text content on the selection screen (e.g. using a text symbol)
"Also here, note the use outside of BEGIN/END OF LINE regarding the additions.
"The optional FOR FIELD addition is, among others, used in the context of 
"input/field help.
SELECTION-SCREEN COMMENT /1(20) txt.

"Creating a pushbutton
"The pushbutton is assigned a function code. 
"Note: To enable it, include the statement 'TABLES sscrfields.' in the code. 
"When the button is clicked, the event AT SELECTION-SCREEN is raised and the 
"function code ('com' in the example) is passed to the 'ucomm' component in the 
"interface work area 'sscrfields' which can be evaluated and reacted upon 
"accordingly. See the executable example.
SELECTION-SCREEN PUSHBUTTON /5(10) pb USER-COMMAND com.

"Defining a new line with multiple elements
SELECTION-SCREEN BEGIN OF LINE.  
  "The following statement with the POSITION addition is only possible here.
  "It specifies the output position of a screen element.
  SELECTION-SCREEN POSITION 10.  
  ...
SELECTION-SCREEN END OF LINE.

"Chained statements come in handy
"Watch out that there are no positioning conflicts with other screen elements.
SELECTION-SCREEN: BEGIN OF LINE,
                  PUSHBUTTON 2(5) button USER-COMMAND fc,
                  POSITION 10.     "The statement specifies the position in which the 
                                   "next element (PARAMETERS) is put. 
PARAMETERS para TYPE c LENGTH 10.
SELECTION-SCREEN: COMMENT 25(10) txt,
                  END OF LINE.

"Creating blocks
"More additions available, e.g. WITH FRAME (to draw a frame around a block) 
"and TITLE (to define a block title).
SELECTION-SCREEN BEGIN OF BLOCK bl.
  ...
SELECTION-SCREEN END OF BLOCK bl.

"Creating tabbed blocks (other optional additions are available)
SELECTION-SCREEN BEGIN OF TABBED BLOCK tabbl FOR 5 LINES. "Specifies number of lines covered
  ...
  "In such a TABBED BLOCK statement, only TAB statements are allowed to integrate
  "subscreen dynpros. 
  "USER-COMMAND addition specifies a function code. When a tab is selected by users,
  "the function code can be evaluated using the the component ucomm of the structure 
  "'sscrfields' after the AT SELECTION-SCREEN event. See the executable example.
  SELECTION-SCREEN TAB (10) tab USER-COMMAND cmd.
...
SELECTION-SCREEN END OF BLOCK tabbl.

"MODIF ID addition for some of the previous statements (ULINE, COMMENT,
"PUSHBUTTON, TAB, and also for PARAMETERS and SELECT-OPTIONS)
"An identifier can be specified to assign a screen element to a modification group. 
"This identifier is assigned to the component 'group1' of the SCREEN structure.
"Using MODIFY SCREEN statements, the elements can be modified before displaying
"in the AT SELECTION-SCREEN OUTPUT event block. 
"In the following example, a radio button group consists of 4 radio buttons.
"Two of them have the addition MODIF ID.
SELECTION-SCREEN BEGIN OF BLOCK bl WITH FRAME.
PARAMETERS: pa1 RADIOBUTTON GROUP gr MODIF ID mbl,
            pa2 RADIOBUTTON GROUP gr MODIF ID mbl,
            pa3 RADIOBUTTON GROUP gr,
            pa4 RADIOBUTTON GROUP gr.
SELECTION-SCREEN END OF BLOCK bl.

"Suppose you want to modify the elements before displaying, e.g. make the 
"two radio buttons with the identifier invisible. In the AT SELECTION-SCREEN 
"OUTPUT event block, you might implement it as follows.
...
AT SELECTION-SCREEN OUTPUT.
  LOOP AT SCREEN INTO DATA(wa).
     IF wa-group1 = 'MBL'.
      wa-invisible = '1'.
      MODIFY SCREEN FROM wa.
    ENDIF.
  ENDLOOP.
...  

"Adding pushbuttons in the application toolbar
"There are five inactive pushbuttons to which the function codes FC01, FC02 up to FC05  
"are assigned. The ... FUNCTION KEY ... statement activates the pushbuttons for 
"the specified codes. 
"Note: To enable it, include the statement 'TABLES sscrfields.' in the code. 
"When the button is clicked, the event AT SELECTION-SCREEN is raised and the 
"function code is passed to the 'ucomm' component in the interface 
"work area 'sscrfields' which can be evaluated and reacted upon accordingly, e.g. 
"in a CASE control structure. 
"In the following example, three pushbuttons are added to the toolbar. To 
"provide text for the buttons, assign values to the component functxt_0n (while
"n stands for the numbers 1 - 5). Otherwise, there won't be any button text.
TABLES sscrfields.
PARAMETERS pa TYPE c LENGTH 10.
SELECTION-SCREEN: FUNCTION KEY 1,  "Stands for FC01
                  FUNCTION KEY 2,  "FC02 
                  FUNCTION KEY 3.  "FC03

INITIALIZATION.
  sscrfields-functxt_01 = 'Button 1'.
  sscrfields-functxt_02 = 'Button 2'.
  sscrfields-functxt_03 = 'Button 3'.

AT SELECTION-SCREEN.
  CASE sscrfields-ucomm.
    WHEN 'FC01'.      
      ...
    WHEN 'FC02'.
      ...      
    WHEN 'FC03'.      
    ...
  ENDCASE.
...
```

`INCLUDE` addition for reusing already created elements of the same program:

```abap
"Note: More optional additions are available. 

SELECTION-SCREEN BEGIN OF SCREEN ...
...
SELECTION-SCREEN INCLUDE PARAMETERS param.
SELECTION-SCREEN INCLUDE SELECT-OPTIONS sel.
SELECTION-SCREEN INCLUDE COMMENT (10) txt. 
SELECTION-SCREEN INCLUDE PUSHBUTTON (10) rb.   
SELECTION-SCREEN INCLUDE BLOCKS blck.
...
SELECTION-SCREEN END OF SCREEN ...
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

### Calling Selection Screens

- There are several ways of how to call selection screens (see [here](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen_call.htm)), among them [`CALL SELECTION-SCREEN`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapcall_selection_screen.htm) statements. 
- These statements call selection screens by specifying the dynpro number. 
- Any selection screen of the main program (and only from there; including 1000) can be called.

```abap
CALL SELECTION-SCREEN 9876.

"STARTING AT/ENDING AT additions for creating a modal dialog box
"The numbers determine how it should be displayed. 
"STARTING AT: upper left corner (column and line); ENDING AT: bottom right 
"corner (if not specified, it is set automatically)
CALL SELECTION-SCREEN 9345 STARTING AT 10 10.
CALL SELECTION-SCREEN 9345 STARTING AT a b ENDING AT c d.

"You can also predefine selection criteria for a selection screen using 
"the addition USING SELECTION-SET.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## Excursion: SUBMIT Statements
- [`SUBMIT`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapsubmit.htm) statements call executable programs and, therefore, selection screens.
- In general, every executable program is started implicitly with `SUBMIT`.
- Selection screens can be considered a parameter interface if the program containing them is executed using `SUBMIT`.

```abap
"The calling program is ended
SUBMIT some_program.     "Direct specification of the program name

"Dynamic call
"Note the security risks and consequences a program submitted from outside can cause;
"you may use the class CL_ABAP_DYN_PRG to tackle security risks
SUBMIT ('SOME_PROGRAM'). "Character-like data object in a pair of parentheses

"AND RETURN: The calling program is interrupted
SUBMIT some_program AND RETURN.

"Selection screen-related options for the SUBMIT statement
"Specifies a particular selection screen to be called (but it's not displayed)
SUBMIT some_program USING SELECTION-SCREEN 9123. 

"Specifies that the selection screen to be called is displayed
SUBMIT some_program VIA SELECTION-SCREEN.
SUBMIT some_program USING SELECTION-SCREEN 9123 VIA SELECTION-SCREEN.

"You can pass values to the selection screen. There are a several options.
"Check the ABAP Keyword Documentation. The following examples cover a selection.

"In the followig example, it is assumed that the called program only has 1 
"parameter with name par, type i. The addition USING SELECTION-SCREEN is 
"omitted, i.e. the standard selection screen is called (but you can also call 
"the dynpro 1000 explicitly). 
SUBMIT some_program WITH par = 1. "Specifying EQ is also possible

"Displaying the selection screen, only interrupt the current program
SUBMIT some_program VIA SELECTION-SCREEN WITH par = 1 AND RETURN.

"You can also pass multiple values, e.g. consider a program having multiple
"PARAMETERS statements (you may also have multiple WITH additions), or SELECT-OPTIONS
"statements. 
SUBMIT some_program VIA SELECTION-SCREEN WITH SELECTION-TABLE tab.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## ABAP Statements for Classic Lists

### Creating Lists

- Lists consist of consecutive list lines that are filled one after the other using the statement [`WRITE`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapwrite-.htm).
- This way, you can output mostly flat data objects and strings/xstrings, types converted to a character-like value (i.e. no internal table content or structures directly).
- Each time a data object is output, an output length is defined, either implicitly or explicitly (see [here](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenwrite_output_length.htm)).
- The following snippets show a selection of additions to `WRITE` statements, covering positioning, creating special list elements, and formatting options.

```abap
"WRITE using an unnamed data object
WRITE 'Hello ABAP.'.
"Named data objects
DATA some_text TYPE string VALUE `Hi, `.
"Note: The text is written right after the previous text in one line.
WRITE some_text && sy-uname.
"/: Writes to a new line
WRITE / 'new line'.

"-------------------------- Positioning output --------------------------
"Specifying the output position (and output in new line)
WRITE /5 'positioned'.
"Specifying the length, no position specified means the output is
"written from the first column on
WRITE /(3) 'not displayed completely'.
"Specifying both length and position
WRITE /5(30) 'this is displayed completely'.
"Position/length only specified as numeric literals as above, then the
"addition AT can be omitted
WRITE AT /3(12) 'lorem ipsum'.
"Length specifications with * or **. In that case, the output length 
"depends on the data type of the data object. There are special rules. 
"See the ABAP Keyword Documentation for the details.
DATA dc34 TYPE decfloat34 VALUE '   1.2345    '.
WRITE /(*) dc34.
WRITE /1(**) dc34.

"--------------------- Further specification options ----------------------

"Various examples for further specification options, among them function
"calls, string expressions, method calls
DATA txt TYPE string VALUE `abap`.
"String template
WRITE / |{ txt WIDTH = 20 ALIGN = RIGHT  CASE = UPPER }|.
"The following statement uses a chained statement with colon.
WRITE: / |{ 1 + 2 }|, "String template includes an arithmetic calculation
        to_upper( txt ). "Function call
"Concatenation using &&
WRITE / `conc` && `atenated`.
"Method call (returns a random integer between 1 and 10)
"Note the * specification. The returned value is of type i. Specifying *
"for the length means the length required to output the current value
"is used. You may want to try the following code snippet by removing (*).
WRITE /(*) cl_abap_random_int=>create(
              seed = cl_abap_random=>seed( )
              min  = 1
              max  = 10 )->get_next( ).

"UNDER: Output in the position of previous output
"Note: If the output is written in the same line in which the previous output
"is displayed, this output is overwritten.
DATA abc TYPE c LENGTH 5 VALUE 'abcde'.
WRITE /5(5) abc.
WRITE / 'fghij' UNDER abc.

"NO-GAP
WRITE / 'g'.
WRITE 'ap1'.  "Output: g ap1
WRITE: / 'g' NO-GAP, 'ap2'.  "Output: gap2

"QUICKINFO: Creates a tooltip for the output
WRITE / 'Place the mouse on the following output and check the tooltip:'.
WRITE: (*) sy-uname QUICKINFO 'User name',
       '/',
       (*) sy-datum QUICKINFO 'Current date',
       '/',
       (*) sy-uzeit QUICKINFO 'Current time'.

"--------------------- Special list elements ----------------------

"INPUT: Enabling fields for input
"You can overwrite the output and further evaluate the overwritten
"content in a list event. 
WRITE / '   Enter your name here    ' INPUT.

"AS CHECKBOX
"The value (blank, X) is stored in the list buffer and can be 
"evaluated during a list event.
DATA: chck1 TYPE c LENGTH 1 VALUE 'X',
      chck2 TYPE c LENGTH 1 VALUE ' '.
WRITE: / chck1 AS CHECKBOX, 'A checkbox',
       / chck2 AS CHECKBOX, 'Another checkbox'.

"AS ICON: Outputting icons
"Check the type pool ICON for names of predefined icons.  
WRITE / icon_green_light AS ICON.
WRITE / icon_red_light AS ICON.
WRITE / icon_yellow_light AS ICON.
WRITE / icon_activate AS ICON.

"AS SYMBOL: Outputting symbols
"Check the type pool SYM for names of predefined icons.  
WRITE / sym_left_hand AS SYMBOL.
WRITE / sym_caution AS SYMBOL.

"AS LINE: Outputting corners, crosses, lines, and T sections
"Check the type pool LINE for names of predefined icons.  
WRITE: /10 line_horizontal_line AS LINE NO-GAP,
           line_space           AS LINE NO-GAP,
           line_vertical_line   AS LINE NO-GAP.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

Formatting options with `WRITE` and `FORMAT`: 

```abap
"The following examples show a selection. Various other additions are
"available that deal with currency, unit, date and time-related formatting,
"among others. See the ABAP Keyword Documentation.
WRITE /(10) 'X' RIGHT-JUSTIFIED. "'         X'
WRITE /(10) 'X' CENTERED.        "'    X     '
WRITE /(10) 'X' LEFT-JUSTIFIED.  "'X         ' 
DATA dc TYPE decfloat34 VALUE '1.2345'.
WRITE /(*) dc DECIMALS 2.        "1,23

"COLOR addition
"The commented out value stands for the value that can also
"be directly specified in the syntax (except 0) or contained in
"a data object. There are several additions with many options.
WRITE / 'COL_BACKGROUND (0)' COLOR COL_BACKGROUND. "0 (GUI dependent)
WRITE / 'COL_HEADING (1)' COLOR COL_HEADING. "1 (gray-blue)
WRITE / 'COL_HEADING (1)' COLOR 1.
WRITE / 'COL_NORMAL (2)' COLOR COL_NORMAL. "2 (light gray)
WRITE / 'COL_TOTAL (3)' COLOR COL_TOTAL. "3 (yellow)
WRITE / 'COL_KEY (4)' COLOR COL_KEY. "4 (blue-green)
WRITE / 'COL_POSITIVE (5)' COLOR COL_POSITIVE. "5 (green)
WRITE / 'COL_NEGATIVE (6)' COLOR COL_NEGATIVE. "6 (red)
WRITE / 'COL_GROUP (7)' COLOR COL_GROUP. "7 (orange)
WRITE / 'COLOR 7 OFF' COLOR 7 OFF. "default color

"Setting the intensity of the background color.
"Note that many of the following additions can specify further
"additions such as ON and OFF. See the details in the documentation.
WRITE / 'COLOR 7 INTENSIFIED OFF' COLOR 7 INTENSIFIED OFF.
"INVERSE: When ON, the foreground, i.e. the output is displayed
"in the selected color
WRITE / 'COLOR 7 INVERSE ON' COLOR 7 INVERSE ON.

"FORMAT: You can use FORMAT statements for applying settings on the following 
"output statements up to a definition with new settings or ... OFF. 
"Note: Many of the additions that are also possible for WRITE (e.g. COLOR, 
"INTENSIFIED, INVERSE ...) are possible. The following example uses the already 
"mentioned COLOR addition.
FORMAT COLOR COL_POSITIVE.
WRITE / 'ABC'.
WRITE / 'DEF'.
WRITE / 'GHI'.
FORMAT COLOR OFF.

"HOTSPOT addition: Displays the mouse pointer when hovering over an element. 
"Here, a single click has then the same effect as a double click (i.e. the
"selection of function key F2).
FORMAT HOTSPOT ON.
DO 5 TIMES.
  WRITE / sy-index.
ENDDO.
FORMAT HOTSPOT OFF.

"FRAMES addition: Defines whether the - and | characters are converted to line elements,
"producing continuous lines.
FORMAT FRAMES ON.
WRITE: / '----',
       / '|  |',
       / '----'.
FORMAT FRAMES OFF.

"RESET addition: Resetting the formatting settings
"This addition sets all formatting settings for which the corresponding addition is
"not specified in the same FORMAT statement to the state OFF (exception: FRAMES).
"In the following example, all settings are reset without specifying concrete
"additions (e.g. INVERSE OFF). Note the effects of such a statement (e.g. INTENSIFIED
"is ON by default at program start; now it is set to OFF). See the ABAP Keyword 
"Documentation for more information. 
FORMAT COLOR 7 INVERSE ON.
WRITE / 'ABC'.
FORMAT RESET.
WRITE / 'DEF'.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

More list-related ABAP keywords:

```abap
"Creating a horizontal line
ULINE.
"More additions are available e.g. for position and length
ULINE AT 5(20).

"SET BLANK LINES: Specifying if blank lines created using WRITE are displayed
"In the example, the content of a string table that contains blank lines
"is output. 
DATA(itab) = VALUE string_table( ( `a` ) ( `b` ) ( `` ) ( `` )  ( `c` ) ).

"... OFF is set by default. Here it is explicitly specified.
"In this case, the blank lines are not output.
SET BLANK LINES OFF.
LOOP AT itab INTO DATA(wa).
  WRITE / wa.
ENDLOOP.

"... ON: The blank lines are output.
SET BLANK LINES ON.
LOOP AT itab INTO wa.
  WRITE / wa.
ENDLOOP.

"Setting the list cursor explicitly (more statements are
"available such as POSITION or BACK)
"SKIP: Positions the list cursor in another line (creating blank lines)
SKIP. "Skips 1 line
SKIP 3. "Specifying the number of lines to be skipped

"SKIP TO LINE
"In the example, the list cursor is explicitly set to 
"a particular number. Here, B and C are not output (i.e. 
"overwritten with the output that follows).
WRITE / 'Text A'.
WRITE / 'Text B'.
DATA(line) = sy-linno. "The line number of the previous WRITE is retrieved.
WRITE / 'Text C'.
SKIP TO LINE line.
WRITE / 'Text D'.
WRITE / 'Text E'.

"NEW-LINE: Setting list cursor to the first position of the next line
"Additions are available to affect the scrolling behavior
WRITE / 'Next statement is NEW-LINE'.
NEW-LINE.
WRITE 'WRITE statement without /'. "Output is in new line

"HIDE: Storing the content of a flat variable together with the current list line.
"Example: a (3) is the result of b (1) + c (2). Suppose you click the line  
"where a is output. You may e.g. output then b and c (e.g. another WRITE in the 
"AT LIST-SELECTION event block), then getting the correct context of the list line. 
...
a = b + c.
WRITE / a.
HIDE: b, c.
...

"RESERVE: Creating a page break if there is not enough space left on the current list 
"page between the last output and the page end or page footer, as specified with the number.
RESERVE 3 LINES.
"Consider the 3 lines defined as block. When you use BACK after RESERVE, e.g. after a loop, you 
"can put the suqsequent output to the first line of this block of lines. See the effect in the 
"executable example.
...
BACK.
WRITE ... 
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

### Reading and Modifying in Lists

`READ LINE` statements:
- Assign the content of a line stored in the list buffer to the system field `sy-lisel`. 
- Plus, it allows other target fields to be specified.
- `sy-subrc` is set: 0 (line was read), other than 0 (line does not exist)

```abap
"More additions than those covered are available (e.g. for
"list level and list page)
"sy-lisel contains the entire line content of the specified line
READ LINE 1.
"sy-lisel contains the entire line content of the current line
READ CURRENT LINE.
"Assigning the output of individual data objects
"Here, it is the entire line content.
DATA wa TYPE c LENGTH 50.
READ CURRENT LINE LINE VALUE INTO wa.
"Here, specific field values (val1/val2) are assigned to individual data objects
DATA dobj1 TYPE c LENGTH 10.
DATA dobj2 TYPE c LENGTH 10.
READ CURRENT LINE FIELD VALUE val1 INTO dobj1 val2 INTO dobj2.
```

`MODIFY LINE` statements:
- Overwrites a line stored in the list buffer with the content of the `sy-lisel` system field 
- Plus, it allows additional modifications

```abap
"Overwrites line stored in sy-lisel (which is filled e.g. using a previous 
"READ LINE statement)
MODIFY CURRENT LINE.
MODIFY LINE 2.
"Specifying content to overwrite an existing line; the following examples
"only use the addition CURRENT LINE
"Overwrites the entire list line with the content
MODIFY CURRENT LINE LINE VALUE FROM 'hi'.
"Overwrite a specific field with specified content
MODIFY CURRENT LINE FIELD VALUE a FROM b.
"Mutliple fields specified
MODIFY CURRENT LINE FIELD VALUE a FROM b c FROM d.
"Change the format (here using COLOR)
"... of a specific field
MODIFY CURRENT LINE FIELD FORMAT a COLOR 3.
"... of the line
MODIFY CURRENT LINE LINE FORMAT COLOR 4.
"Note: Multiple additions and combinations are possible, using COLOR here
MODIFY CURRENT LINE FIELD FORMAT a COLOR 5 b COLOR 6 LINE FORMAT COLOR 7.
```

> **💡 Note**<br>
> - [Relevant sy components in the context of lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenlist_systemfields.htm)
> - You can interact with a list by double-clicking a line (or pressing F2; the default `PICK` function code raises the `AT LINE-SELECTION` event). Other function codes usually trigger the `AT USER-COMMAND` event. See the event blocks below. In your ABAP program, you can react to the user action by implementing the individual event blocks. Regarding the function code, you can use the `sy-ucomm` system field for evaluation (unlike dynpros, there's no `OK_CODE` field to be filled).

<p align="right"><a href="#top">⬆️ back to top</a></p>

## Event Blocks
- Notes
  - Introduced by an [event keyword](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenevent_keyword_glosry.htm). 
  - Ended by the next processing block (there's no closing statement).
  - Should not contain separate declarations, i.e. you should put all data object declarations in the global declaration part (except for `AT SELECTION-SCREEN ...`)
  - It is recommended that you use ABAP Objects and methods consistently for data encapsulation
  - Should not be specified more than once in a program
  - Called depending on the user action and the program state (even if not explicitly specified)
- `LOAD-OF-PROGRAM` 
  - Program constructor  
  - Called when a program is loaded into the internal session
  - Can be used to initialize the global data of a program. However, for executable programs, it is recommended that you use the event block `INITIALIZATION` for this task since the start values for parameters and selection criteria are set after `LOAD-OF-PROGRAM`.   
- Reporting events related to executable programs in particular (and occur in predefined order). 
  - `INITIALIZATION`
    - Used to initialize executable programs and provide start values (that cannot be statically expressed using `DEFAULT`)
    - Called directly after `LOAD-OF-PROGRAM` and before selection screen processing    
  - `START-OF-SELECTION`
    - Called after selection screen processing (see the following event blocks)
    - If an executable program does not contain selection screens, this block is called automatically and implicitly.
    - All statements are implicitly assigned to this block unless they are explicitly assigned to another block.
    - Although it is optional, it is recommended that you specify the statement explicitly to improve readability.
- Selection screen events 
  - Used to individually evaluate selection screen elements, e.g. a specific parameter.
  - In many cases, the events can be used for input validation (this should not wait until `START-OF-SELECTION`). If your custom input validation fails on an input, you can raise a message (e.g. a message of type `E`: `MESSAGE 'Error' TYPE 'E'.`) and allow users to edit the input again.  
  - `AT SELECTION-SCREEN OUTPUT` <br>Called by the dynpro event PBO of a selection screen after `INITIALIZATION`; can be used for modifying screen elements (e.g. using `MODIFY SCREEN`)
  - `AT SELECTION-SCREEN ON par` <br>Called when values for a parameter or selection criteria were passed to the program; note: the syntax is also available for selection criteria; an additional variant `... ON END OF ...` is available (this event block is called when the selection table is passed completely allowing to check the entire selection table content) 
  - `AT SELECTION-SCREEN ON BLOCK bl` <br>Called when all input for a block is passed to the program
  - `AT SELECTION-SCREEN ON RADIOBUTTON GROUP gr` <br>Called when the values for a radio button group were passed
  - `AT SELECTION-SCREEN ON EXIT-COMMAND` <br>Called by *Back*, *Exit*, or *Cancel* (e.g. used for cleanup tasks)
  - `AT SELECTION-SCREEN ON HELP-REQUEST | VALUE-REQUEST` <br>Relates to field (F1) and input (F4) help
  - `AT SELECTION-SCREEN.` <br>Called as last event in the selection screen processing when all input values are passed to the program
- List events 
  - Are triggered during the list creation and after certain user actions on a displayed list.
  - During list creation: 
    - `TOP-OF-PAGE` <br>For definining page headers; called when a [basic list](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenbasic_list_glosry.htm) is created and when a new page begins (`... DURING LINE-SELECTION` is called when [details lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abendetails_list_glosry.htm) are created)
    - `END-OF-PAGE` <br>For definining page footers; called when the end of a page is reached, for example, when the specified number of lines in the `LINE-COUNT` addition (e.g. `PROGRAM LINE-COUNT 20(3).`) has been reached
  - After user actions: 
    - `AT LINE-SELECTION` <br>Called when a screen list is displayed, the screen cursor is on a list line and a function is selected using the function code `PICK`; when specified, the function key F2 is enabled and, therefore, double click functionality; F2 and a double click are linked with the function code `PICK`.
    - [`AT USER-COMMAND`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapat_user-command.htm) <br>Called when a function with a user-defined function code is selected when a screen list is displayed; the function codes can be evaluated using `sy-ucomm`; you can set a list event programmatically using [`SET USER-COMMAND`](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abapset_user-command.htm)


<p align="right"><a href="#top">⬆️ back to top</a></p>

## Excursion: SAP List Viewer (ALV)

- Provides an object-oriented API for displaying and formatting lists 
- Allows you to specify layout settings and functionality
- Classes such as `CL_SALV_TABLE` encapsulate the use of the ALV grid control and simplify the integration into ABAP programs. Note: The older class `CL_GUI_ALV_GRID` should no longer be used directly for new developments.
- Other classes are available to handle hierarchical lists and tree structures. The focus of the code snippets is on simple, non-nested tables and the use of the `CL_SALV_TABLE` class.

### Getting Started
- To get started quickly, you only need to ...
  - instantiate an ALV table object using the `factory` method.
  - define the display type for the ALV output. By default, full-screen display is enabled. 
  - display the ALV output using the `display` method.

> **💡 Note**<br>
> - When working with ALV, make sure that you implement appropriate error handling. 
> - The `factory` method also has optional exporting parameters. You can use the optional `list_display` parameter to specify whether you want to display the ALV output as classic list. It is set to false by default. There are also exporting parameters to display the ALV output in containers (e.g. see the dynpro cheat sheet example). The exporting parameters are not relevant in this example. Here, the ALV output is displayed on the entire screen (however, it is also possible to display the ALV output in a dialog box).

The following code snippet shows a simple example to get you started quickly with the necessary method calls. It includes a demo internal table with content.

```abap
TYPES: BEGIN OF demo_struct,
         col1 TYPE string,
         col2 TYPE i,
         col3 TYPE c LENGTH 1,
       END OF demo_struct.

DATA itab TYPE TABLE OF demo_struct WITH EMPTY KEY.
itab = VALUE #( ( col1 = `abc` col2 = 1 col3 = 'X' )
                ( col1 = `def` col2 = 2 col3 = 'Y' )
                ( col1 = `ghi` col2 = 3 col3 = 'Z' ) ).

TRY.
    cl_salv_table=>factory( IMPORTING r_salv_table = DATA(alv)
                            CHANGING  t_table      = itab ).

    alv->display( ).
  CATCH cx_salv_msg INTO DATA(err).
    MESSAGE err->get_text( ) TYPE 'I' DISPLAY LIKE 'E'.
ENDTRY.
```

### Optional Layout Settings and Functionality

- The following commented code snippet covers a selection of the many setting and functionality options.
- For simplicity of the snippet, the code only uses the root exception class. Make sure that you implement appropriate error handling and exception classes.  
- You can copy and paste the code into your own test program to explore the ALV output and the effect of the method calls. 

> **💡 Note**<br>
> Check the comments for the custom functions. If there are errors in your test program, replace the relevant code section and enable the generic ALV functions.

```abap
TYPES: BEGIN OF demo_struct,
         col1 TYPE string,
         col2 TYPE i,
         col3 TYPE string,
         col4 TYPE icon_d,
         col5 TYPE string,
       END OF demo_struct.
DATA itab TYPE TABLE OF demo_struct WITH EMPTY KEY.
DATA alv TYPE REF TO cl_salv_table.
DATA cnt4evt TYPE i VALUE 6.

"Local class to handle events
CLASS lcl_events DEFINITION.
  PUBLIC SECTION.
    CLASS-METHODS single_click
      FOR EVENT link_click OF cl_salv_events_table
      IMPORTING row column.
    CLASS-METHODS double_click
      FOR EVENT double_click OF cl_salv_events_table
      IMPORTING row column.
    CLASS-METHODS func_click
      FOR EVENT added_function OF cl_salv_events
      IMPORTING e_salv_function.
ENDCLASS.
CLASS lcl_events IMPLEMENTATION.
  METHOD single_click.
    "Both single and double click events trigger messages.
    READ TABLE itab INDEX row REFERENCE INTO DATA(sc_ref).
    IF sy-subrc = 0.
      ASSIGN sc_ref->(column) TO FIELD-SYMBOL(<fs_sc>).
      MESSAGE `Single click event. ` &&
      |Row: { row } { COND #( WHEN column IS NOT INITIAL THEN `Column: ` && column ) } | &&
      |{ COND #( WHEN <fs_sc> IS ASSIGNED THEN `Value: ` && <fs_sc> ) }| TYPE 'I'.
    ELSE.
      MESSAGE `Single click event` TYPE 'I'.
    ENDIF.
  ENDMETHOD.
  METHOD double_click.
    READ TABLE itab INDEX row REFERENCE INTO DATA(dc_ref).
    IF sy-subrc = 0.
      ASSIGN dc_ref->(column) TO FIELD-SYMBOL(<fs_dc>).
      MESSAGE `Double click event. ` &&
      |Row: { row } { COND #( WHEN column IS NOT INITIAL THEN `Column: ` && column ) } | &&
      |{ COND #( WHEN <fs_dc> IS ASSIGNED THEN `Value: ` && <fs_dc> ) }| TYPE 'I'.
    ELSE.
      MESSAGE `Double click event` TYPE 'I'.
    ENDIF.
  ENDMETHOD.
  METHOD func_click.
    "Handling custom functions
    CASE e_salv_function.
      WHEN 'DATA'.
        MESSAGE `Custom function DATA. Do something ...` TYPE 'I'.
      WHEN 'TEST'.
        IF cnt4evt = 6.
          MESSAGE `A table row will be added, demonstrating the 'refresh' ` &&
          `method that rebuilds the output table.` TYPE 'I'.
        ENDIF.
        cnt4evt += 1.
        itab = VALUE #( BASE itab ( col1 = `fffff` col2 = cnt4evt
                                    col3 = `ttttt` col4 = icon_green_light ) ).
        "Rebuilding the output table
        alv->refresh( ).
      WHEN 'QUIT'.
        MESSAGE `Custom function QUIT. Do something ...` TYPE 'I'.
      WHEN OTHERS.
        MESSAGE `Some other function` TYPE 'I'.
    ENDCASE.
  ENDMETHOD.
ENDCLASS.
START-OF-SELECTION.
"Populating internal table that is output
itab = VALUE #( ( col1 = `aaaaa` col2 = 1 col3 = `zzzzz` col4 = icon_green_light )
                ( col1 = `bbbbb` col2 = 2 col3 = 'yyyyy' col4 = icon_green_light )
                ( col1 = `ccccc` col2 = 3 col3 = 'xxxxx' col4 = icon_green_light ) ).
TRY.
    "----------- Instantiating an ALV table object -----------
    cl_salv_table=>factory( IMPORTING r_salv_table = alv
                            CHANGING  t_table      = itab ).
    "----------- Creating a new instance with new data -----------
    "Notes on the set_data method:
    "- All objects that were referred to previously are removed.
    "- Not to be used in event handlers.
    "In the example, new lines are added to the existing internal table.
    itab = VALUE #( BASE itab ( col1 = `ddddd` col2 = 4
                                col3 = `wwwww` col4 = icon_green_light )
                              ( col1 = `ddddd` col2 = 5
                                col3 = `vvvvv` col4 = icon_green_light )
                              ( col1 = `eeeee` col2 = 6
                                col3 = `uuuuu` col4 = icon_green_light ) ).
    alv->set_data( CHANGING t_table = itab ).
    "----------- Layout-related settings -----------
    "Changing the list header title
    CAST cl_salv_display_settings(
      alv->get_display_settings( ) )->set_list_header( 'Demo Title' ).
    "There are several methods to retrieve column-specific information.
    "The following examples show a selection. You can check the variable
    "content in the debugger.
    DATA(col1obj) = alv->get_columns( )->get_column( 'COL1' ).
    DATA(col1alignment) = col1obj->get_alignment( ).
    DATA(col1type) = col1obj->get_ddic_inttype( ).
    DATA(col1colname) = col1obj->get_columnname( ).
    DATA(is_col1_visible) = col1obj->is_visible( ).
    "Settings for the column header
    "This example uses the get method, which returns all column objects of
    "the output table. The table is processed in a loop.
    "Settings covered:
    "- Specifying the column header titles (long, medium, and short
    "  column header)
    "- Specifying a tooltip for the column header
    LOOP AT alv->get_columns( )->get( ) REFERENCE INTO DATA(colref).
      "Specifying the column header titles
      "The example sets the texts based on the columname value.
      "Note: The column width is optimized further down. You may want to
      "manually adjust the width in the ALV output to see the column header
      "name change.
      colref->r_column->set_long_text( |{ colref->columnname }| ).
      colref->r_column->set_medium_text( |{ colref->columnname+2(3) }| ).
      colref->r_column->set_short_text( |{ colref->columnname+3(3) }| ).
      "Specifying a tooltip for the column headers
      colref->r_column->set_tooltip(
        |Demo tooltip { colref->columnname+3(3) }| ).
    ENDLOOP.
    "Displaying/Hiding the column headers
    "In the example, the value is set to abap_true. Set it to abap_false
    "to hide the column header.
    "Note: In this and the following examples, the settings are
    "intentionally done using separate casts for each example. You can also
    "create object reference variables, such as for 'col1obj' to avoid having
    "to specify the casts explicitly each time.
    CAST cl_salv_columns_list(
      alv->get_columns( ) )->set_headers_visible( abap_true ).
    "Setting key columns
    "Note: The key columns have a default color setting. There, you can only
    "change the color in individual cells.
    CAST cl_salv_column_table(
        alv->get_columns( )->get_column( 'COL1' ) )->set_key( abap_true ).

    "Setting column color
    "Note: The executable example includes examples of coloring entire rows and
    "specific cells.
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL2' )
      )->set_color( VALUE lvc_s_colo( col = col_positive ) ).

    "Hiding columns
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL5' )
      )->set_visible( abap_false ).

    "Setting text alignment
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL2' )
      )->set_alignment( if_salv_c_alignment=>left ).

    "Optimizing column width, i.e. automatically adjusting the column width
    "to display values completely (can also be done for individual columns)
    alv->get_columns( )->set_optimize( abap_true ).

    "Setting output width explicitly (for an individual column)
    "Note: Just to demonstrate the method call. This setting has no effect in
    "the example due to the column width optimization above, and the column is
    "hidden anyway.
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL5' )
      )->set_output_length( '100' ).

    "Setting table rows to a striped pattern
    alv->get_display_settings( )->set_striped_pattern( abap_true ).

    "Displaying/hiding horizontal and vertical grid lines (here, they're hidden)
    alv->get_display_settings( )->set_horizontal_lines( abap_false ).
    alv->get_display_settings( )->set_vertical_lines( abap_false ).

    "With the following method call, the ALV output is displayed in a dialog box.
    "Demo coordinates are provided. You can comment it in to see the effect.
*      alv->set_screen_popup( start_column = 10
*                             end_column   = 100
*                             start_line   = 4
*                             end_line     = 15 ).

    "------ Adding functionality/user interaction options to the ALV output ------
    "Specifying the selection type
    "The following specification allows you to select multiple rows/columns (a column
    "is added on the left for selections). Check the other options in the
    "if_salv_c_selection_mode interface.
    alv->get_selections( )->set_selection_mode( if_salv_c_selection_mode=>row_column ).

    "Setting the sorting
    "In the example, a particular column is sorted in descending order.
    alv->get_sorts( )->add_sort( 'COL1' )->set_sequence( if_salv_c_sort=>sort_down ).

    "Applying a filter
    "The example is implemented to exclude one entry from the demo table.
    alv->get_filters( )->add_filter( 'COL2' )->add_selopt( sign   = 'E'
                                                            option = 'EQ'
                                                            low    = '6' ).

    "Making a calculation/aggregation
    "In this example, an aggregation is added for a specific column.
    "In this case, the average value is calculated. It is displayed in a row added
    "at the bottom of the table.
    alv->get_aggregations( )->add_aggregation(
      columnname  = 'COL2'
      aggregation = if_salv_c_aggregation=>average ).

    "Generic and custom ALV functions
    "Note:
    "- By default, ALV functions (such as sorting or filtering) are not available
    "  to users. You must explicitly enable them.
    "- Depending on whether the display is full-screen (as it is here) or in a
    "  container, restrictions apply. In the first case, you can use your own GUI status
    "  to integrate custom functions.

    "This (self-contained, ready-to-use) code snippet is intended so that you can simply
    "copy & paste the code into a test program. For this purpose, and to be able to
    "demonstrate custom functions in this full-screen ALV example using a GUI status, the
    "example uses a demo GUI status that is included in another sample program (in a
    "subpackage of SALV). For your own GUI status, you can check the SALV_TABLE_STANDARD
    "status contained in the SALV_METADATA_STATUS function group, which you can copy and
    "use as a template, for example.
    "*********************************** NOTE ***********************************
    "- The GUI status used here is just reused to have a copyable and self-contained example.
    "- The GUI status of the sample program (specified for the report parameter below)
    "  contains generic and additional functions. For the additional functions, a simple
    "  implementation is included in this snippet. The GUI status should include the 
    "  functions TEST, DATA, QUIT.
    "- The implementations in the event handler class here do not match the implementations
    "  there, the button texts do not make sense for the implementations here, and so on.
    "- So even though it (the button names, icons, implementations, etc.) does not make much
    "  sense for this snippet, you should get an idea about custom functions and be able to
    "  explore the event handling by clicking on the buttons.
    "- The TEST function demonstrates adding new rows to the table and the 'refresh' method.
    "- If this particular program, the GUI status and/or the custom functions are not available
    "  in your system, or if you insert the code into your own test program, run it, and
    "  encounter problems setting the status, remove the TRY control structure that contains
    "  the set_screen_status method call, and comment in the code below to use the standard
    "  ALV functions. In this case, the custom functions cannot be checked out with this
    "  snippet.
    "- Check out the executable example of the cheat sheet that includes a GUI status.
    TRY.
        alv->set_screen_status(
          pfstatus      = 'SALV_TABLE_STANDARD'
          report        = 'SALV_TEST_REFRESH'
          set_functions = alv->c_functions_all ).

        "In this GUI status, the custom functions TEST, DATA, and QUIT are specified. You can 
        "check the results of the following method calls in the debugger.
        DATA(getfunc) = alv->get_functions( ).
        "Checking the activation status
        DATA(is_enabled) = getfunc->is_enabled( 'TEST' ).
        "Checking the visibility
        DATA(is_visible) = getfunc->is_visible( 'TEST' ).

        "Registering an event handler for the custom functions
        "The added_function event is raised. In the implementation of the event handler method,
        "you can then implement your code based on the value of the e_salv_function parameter
        "(which contains the specified function name), for example, using a CASE statement.
        "The implementation in this code snippet is different from the original program.
        SET HANDLER lcl_events=>func_click FOR alv->get_event( ).

      CATCH cx_salv_object_not_found.
        MESSAGE `GUI status error. Instead, use the set_default method, for example.`
        TYPE 'I'.
    ENDTRY.

    "If you remove the TRY control structure above, you can comment in the following
    "code to use the generic ALV functions. Use the set_all method for all generic
    "functions. You can also enable generic ALV functions individually. Check the
    "set_* methods.
    "alv->get_functions( )->set_default( abap_true ).

    "Hotspot/single click functionality
    "You can define the content of a cell as a clickable area. You can do this by
    "specifying the cell type hotspot. When a user clicks the cell content,
    "the link_click event is raised.
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL1' )
      )->set_cell_type( if_salv_c_cell_type=>hotspot ).

    "You can set icon columns and make the icon cells clickable areas. To do this,
    "use the set_icon method. As above, the cell type of the column is set to hotspot.
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL4' )
      )->set_icon( if_salv_c_bool_sap=>true ).
    CAST cl_salv_column_table( alv->get_columns( )->get_column( 'COL4' )
      )->set_cell_type( if_salv_c_cell_type=>hotspot ).

    "An event handler is registered for the clickable areas (the link_click event
    "is raised). The get_event method returns the required event object. In the
    "example, a message is displayed.
    SET HANDLER lcl_events=>single_click FOR alv->get_event( ).

    "Double click functionality
    "Registering an event handler for the double click event. In the example, a message
    "is displayed. Double-click cells in columns for which the cell type is not set to hotspot.
    SET HANDLER lcl_events=>double_click FOR alv->get_event( ).

    "Adding tooltips to the icon column cells
    alv->get_functional_settings( )->get_tooltips( )->add_tooltip(
      type    = cl_salv_tooltip=>c_type_icon
      value   = CONV lvc_value( icon_green_light )
      tooltip = `This is a tooltip for an icon` ).

    "----------- Displaying the ALV output -----------
    alv->display( ).
  CATCH cx_root INTO DATA(error).
    "For simplicity, this example uses the root exception class.
    "Always make sure that you use appropriate exception classes. Check the F2
    "information for the methods in ADT.
    MESSAGE error->get_text( ) TYPE 'I' DISPLAY LIKE 'E'.
ENDTRY.
```

<p align="right"><a href="#top">⬆️ back to top</a></p>

## More Information
- ABAP Keyword Documentation:
  - [Selection Screens](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenselection_screen.htm) 
  - [Classic Lists](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abenabap_dynpro_list.htm)
- ALV:
  - [SAP List Viewer (ALV)](https://help.sap.com/docs/SAP_NETWEAVER_731_BW_ABAP/b1c834a22d05483b8a75710743b5ff26/4ec38f8788d22b90e10000000a42189d.html?locale=en-US) on the SAP Help Portal
  - Find more demonstration examples in package `SALV` and its subpackages.

## Executable Examples

After the import of the repository, proceed as follows: 
- Find the program in ADT using the search by choosing `CTRL + SHIFT + A`.
- Enter `ZDEMO_ABAP_SELSCR_LISTS_INTRO` and open the program. This program serves as an entry point for all selection screen and list examples. Program names of the individual examples:
  - `ZDEMO_ABAP_SELSCR_PARAMETERS`: Demonstrates `PARAMETERS` statements
  - `ZDEMO_ABAP_SELSCR_SELECT_OPT`: Demonstrates `SELECT-OPTIONS` statements
  - `ZDEMO_ABAP_SELSCR_STANDALONE`: Demonstrates the creation (`SELECTION-SCREEN` statements) and calling of standalone selection screens
  - `ZDEMO_ABAP_SELSCR_STMTS_VAR`: Demonstrates variants of the `SELECTION-SCREEN` statements that do not create selection screens
  - `ZDEMO_ABAP_LISTS`: Demonstrates various ABAP statements to create and handle classic lists
  - `ZDEMO_ABAP_EVENT_BLOCKS`: Demonstrates event blocks
  - `ZDEMO_ABAP_ALV`: Demonstrates the SAP List Viewer (ALV)
- Run the program by choosing `F8`.

> **💡 Note**<br>
> - The examples in the *main* branch of the ABAP cheat sheet repository are designed to be imported into the SAP BTP ABAP Environment. For Standard ABAP, you can find examples (such as `ZDEMO_ABAP_SELSCR_LISTS_INTRO`) in the other branches of the repository. 
> - The executable examples ...
>   - do not claim to include meaningful selection screens and lists.
>   - are not intended to be role models for proper selection screen and list design.   
>   - are not intended to solve concrete programming tasks. You should always work out your own solution for each individual case.
>   - are only intended to demonstrate a selection of keywords and visualize related syntax in action on a high level.
>   - include comments in the program code. 
> - The steps to import and run the code are outlined [here](README.md#-getting-started-with-the-examples).
> - [Disclaimer](./README.md#%EF%B8%8F-disclaimer)