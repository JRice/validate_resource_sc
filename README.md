. given an input of a resource dir...
. mkdir resource_dir/converted_csv if not exists
. get the next int for a csv in that dir (1 if none)
. read in our list of URIs https://raw.githubusercontent.com/EOL/eol_terms/main/resources/terms.yml
. read in our list of possible XML Meta fields https://raw.githubusercontent.com/EOL/harvester/main/db/data/meta_analyzed.json
. parse meta.xml from resource_dir
. for each format
  . set a list of fields
  . return an exception if the number of fields does not match the file
  . if header line(s)
    . for each column and field
      . return an exception if the column doesn't match the field
  . create a collection of errors
  . create a converted_csv output file with the next available int
    . for each row of the input file
      . for each field
        . store an error if the field is blank and isn't allowed to be
        . store an error if the field is a non-integer and isn't allowed to be
        . store an error if the field is a URI and isn't in our list
        . remember the id, if it is one (and there are several possible types)
        . store an error if the field is a PK and it's already been used
        . add the field to the row
      . write the row to the output file
  . for each format
    . for each field
      . store an error if the field is a PK and we haven't seen it (and there are several types)
. if there were errors
  . write an error log in the resource_dir
  . return an exception pointing to the error log
. return success

