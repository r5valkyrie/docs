# Datatables

Datatables are CSV (Comma Separate Values) files that store tables of information.    
There will usually be a row dedicated to specifying which type of data is stored in each column.    
CSV files can be opened with software such as LibreOffice Calc or Microsoft Excel.      
Datatables can be and are loaded and parsed by the engine, to read various types of information.   
Datatables are currently READ-ONLY. They are opened and extracted with RSX.  
Mods can override Datatables and add new ones. RPAKs containing Datatables are packed using RePak and are placed in paks/Win64 and / or paks/Win64_Server for dedicated servers.  
When getting and registering Datatables, the game looks inside datatable/ (the root is the RPAK), so your modded Datatables should be placed in that directory, inside the pack folder, when creating a new RPAK.  
Pay attention to mod load order! Datatables are loaded in the order that mods are loaded. Datatables with the same name / GUID will override each other in the order that mods are loaded.  

## Datatable Functions:

```
var datatable = GetDataTable( asset datatable )  
Example:   
GetDataTable( $"datatable/survival_loot.rpak" )  
ValidateDataTable( entity player, string pathToDatatable )


GetDataTableRowCount( var datatable )
GetDataTableColumnByName( var datatable, string columnName )

GetDataTableString( var datatable, int datatableRow, int datatableColumn )
GetDataTableAsset( var datatable, int datatableRow, int datatableColumn )
GetDataTableBool( var datatable, int datatableRow, int datatableColumn )
GetDataTableInt( var datatable, int datatableRow, int datatableColumn )
GetDataTableFloat( var datatable, int datatableRow, int datatableColumn )

Iterating through a table:

var datatable = GetDataTable( asset datatable )
int rowCount = GetDataTableRowCount( var datatable )

for( int i = 0; i < rowCount; i++ )
{

    // code
    // passes through each row, i.e.: GetDataTableString( datatable, i, datatableColumn )
    // can use an additional for-loop to iterate through rows i.e.: for( int j = 0, i < columnCount; j++ ) { }

}
```

## Which Datatables Do What:

The Datatables which store loot are:

```
$"datatable/survival_loot_sdk.rpak" // survival_loot_sdk.csv or 0xBA8EBDBBD6713667 (GUID) 
$"datatable/titanfall_loot_sdk.rpak" // titanfall_loot_sdk.csv or 0xCADAC7D029E19EF (GUID)
```

The function that registers SURVIVAL Loot from the Datatables is SURVIVAL_LOOT_RegFromDataTable(), in sh_survival_loot_all.gnut, defined starting at line 410.    

Models and particle effects may need to be precached.  

The Datatable which stores most information for attachments / hop-ups is:  

$"datatable/weapon_mods_sdk.rpak"  // weapon_mods_sdk.csv or 0x62CE41C5376EA985 (GUID)

0xB96787430378078B ?? // TODO: clarify what this one does

The Datatable which determines weapon compatibility with attachments / hop-ups is:  

0x6562F1F8FA2CA288 (GUID)

The Datatable which stores visual compatiblity information for attachment / hop-up information is: 

0xE861B7C77949555A (GUID)

This table contains a row for each attachment and a column for each weapon. Inserting an "x" in a column conveys to 
the UI VM scripts that a weapon supports a certain attachment / hop-up / mod.  