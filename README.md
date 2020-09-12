# csvs

".csvs" files (the plural of csv) are a csv variation with table delimiters (in addition to row and column delimiters).

The physical file contains many logical tables. Each table has a name, a relative path, and the data itself.

The concepts in csvs should mirror those in csvz. Unless stated otherwise here, assume csvz specifications apply to `csvs` - with minor changes where needed. 

Changes from csvz to csvs:

- instead of files being contained in a zip file (or folder) they are contained in a single file.
- a `csvs-unpack` tool (imagined at the moment) can easily/unambiguously unpack the csvs into a folder, which could then be zipped, creating a csvz file.
- unzip can easily unpack the csvz into a folder, which a `csvs-pack` tool (imaginary at the moment) can then be unambiguously pack into a csvs file.

A csvs file has this format:

todo: define this in EBNF or whatever the kids are using these days.


	table-delimiter

	file-name
	
	table-delimiter
	
	relative-path
	
	table-delimiter
	
	field data  
	
	[field delimiter field data  ]   (0 or more)


	
		[row delimiter]
		
		field data  
		
		[field delimiter field data  ]   (0 or more)
		
	^^ (0 or more)
	
	

# `csvs-0`

	To comply with `csvs-0` file:
	
		must start with a table delimiter, then a table name, then table delimiter, then a relative path, then a table delimiter, then regular csv data
		
		Optionally can include more tables after that. Each table requires those 3 segments (name,path,content)
		
		The file may or may not end with a table delimiter. (i.e. it is a separator, not a terminator)
		
		If there is a table delimiter at the end with nothing after it, then that's the end (it doesn't mean "plus a null name/path file with no content)
		
		If there is a table delimiter at the end with a string after it, no more table delimiters and then EOF... then:
			that does not mean: plus a table with a name but no path and no content.
			it means "plus a comment" which is that final content.
		
			(Same for "a table delimiter at the end with a string after it, then table-delimiter then EOF.)
			
			
			(Same for "a table delimiter at the end with a string after it, then table-delimiter then "whitespace" EOF.)
			
		
		If there is a table delimiter at the end with a non-whitespace string after it, then a table delimiter, a non-whitespace string and then an end of file:
		
			that does not mean: plus a table with a name and path but no content.
			
			it means: plus a table with a name, in the root folder, and with content from that final string.
			
			(Same for "a table delimiter at the end with a non-whitespace string after it, then table-delimiter, a non-white-space string (then optionally a table delimiter and optional whitespace string) EOF.)
			
			
(TODO: Sorry for repetition in these sections, i haven't had time to write them shorter. Once I reboot the EBNF in my mind, I can describe this far simpler)
			
			
			
		
		
In general:

		files can be packed in any order	
		
Duplicate file/path -- 

   currently undefined how this can be handled.
	 
Possibilities:
	 
		- subsequent segments with duplicate path/filename -- should be discarded
		- subsequent segments with duplicate path/filename -- should win over previous version
		- subsequent segments with duplicate path/filename -- should render entire file invalid (NO, doesn't meet postel's law)
		- subsequent segments with duplicate path/filename -- subsequent files should be unpacked as path/filename[ID].csv where [ID] is for disambiguation. (And should they be treated a slogical continuations of previous file? hard to say.)
		
SHOULD names include ".csv" on the end?

 - no.
 
 it's not needed.
 
 TODO: the csvs-unpack tool may choose to add ".csv" to the end or it may not. to be confirmed. 

	
In particular:

is there a preferred order for files?

-if included `_meta/csv.csv` is highest priority
-if included `_meta/tables.csv` is next highest priority
-if included `_meta/tables/*.csv` is next highest priority
-if included `_meta/columns.csv` is next highest priority
-if included `_meta/columns/*.csv` is next highest priority
-if included `_meta/relations.csv` is next highest priority
-if included `_meta/relations/*.csv` is next highest priority
-other `_meta/` files to be described later.




