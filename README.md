# xlsx-extract -- extracts data from XLSX files with a low memory footprint


xlsx-files can get pretty large, so nodejs & full featured xlsx-modules often reach memory limits or just use more than is needed for that task.

(--max-old-space-size & --stack_size can't help you all the time either)

hence these magnificent features:

- filestreams are piped & xml is parsed with sax parser `node-expat`
- get rows each by callback, all in a array or write them to a .tsv file
- empty lines at the end of the file are ignored

## Install

    npm install xlsx-extract

or from source:

    git clone git://github.com/ffalt/xlsx-extract.git
	cd xlsx-extract
	npm link

#Convenience API

```javascript

	var XLSX = require('xlsx-extract');

	//dump by row
	new XLSX().extract('path/to/file.xlsx', {sheetNr:1})
		.on('row', function (row) {
			console.log(row);
		})
		.on('error', function (err) {
			console.error(err);
		})
		.on('end', function (err) {
			console.log('eof');
		});

	//dump by row in tsv-format
	new XLSX().extract('path/to/file.xlsx', {sheetNr:1, format:'tsv'})
		.on('row', function (row) {
			console.log(row); //row a tsv line
		})
		.on('error', function (err) {
			console.error(err);
		})
		.on('end', function (err) {
			console.log('eof');
		});

	//convert to tsv-file
	new XLSX().convert('path/to/file.xlsx', 'path/to/destfile.tsv')
		.on('error', function (err) {
			console.error(err);
		})
		.on('end', function () {
			console.log('written');
		})

	//convert to json-file
	new XLSX().convert('path/to/file.xlsx', 'path/to/destfile.json')
		.on('error', function (err) {
			console.error(err);
		})
		.on('end', function () {
			console.log('written');
		})


	demo_options = {
        sheetnr: 1, // default 1 - the number of the sheet starting on 1
        ignore_header: 0,  // default 0 - the number of header lines to ignore
        include_trailing_empty_rows: false, // default false - include empty rows at the end or not
		include_empty_rows: false,  // default false - include empty rows in the middle/at start
        raw_values: false,   // default false - do not try to apply cell nr formats
        date1904: false,    // default false - use date 1904 conversion
        tsv_float_comma: false  // default false - use "," als decimal point for floats
     };


```

#TODO

- better error handling
- more testing

