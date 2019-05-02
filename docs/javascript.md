# My Javascript Codes

My [Javascript Repository](https://github.com/marvinamorim/Developer/tree/master/JavaScript).

## Apex Loading
Object to add/remove loading from apex.

	let isLoading=false;
	let load = {
		on: function() {
			if (isLoading===true){
				console.log("Loading already on.");
			}
			else setTimeout(function(){
				$wP = apex.widget.waitPopup();
				isLoading = true;
			},100);
		},
		off: function() {
			if (isLoading===true) {
				setTimeout(function(){
					$wP.remove();
					isLoading = false;
				},100);
			} else {
				console.log("Loading not on.")
			}
		}
	}

## Number Pad
Prototype to give leading 0's to a number

	Number.prototype.pad = function(size) {
	    var s = String(this);
	    while (s.length < (size || 2)) {s = "0" + s;}
	    return s;
	}

## apex.server.process
How to use an apex.server.process to call an Ajax Callback Process from the server, with exemple of it's parameters.

	let num = 1,
		str = "teste",
		arr = [1,2,3,4],
		pageitems: "#P1_ITEM1, #P1_ITEM2, #P3_ITEM3";
	apex.server.process('AJAXCALLBACK_NAME', {pageItems: pageitems, x01:num, x02:str, f01:arr}, {dataType:"text"})
	.then(function(data) {
	    console.log(data);
	});

#### Things to know
AJAXCALLBACK_NAME<br>
	<code>Name of the PLSQL process created that will return a value to the 'data' variable.</code><br>
pageItems<br>
	<code>String with the Apex items that will be submited to the process called. Each item must have the '#' before it's name, and for multiple items, separate them with a comma.</code><br>
x01<br>
	<code>You can pass up to 10 variables as x01..x10 to the callback.</code><br>
f01<br>
	<code>You can pass up to 10 array of variables as f01..f20 to the callback.</code><br>
dataType<br>
	<code>Type of variable returned by the callback. Remove this option to recieve an object.</code>

## Array Comparsion
Check if the value of every index is equal between arrays.

	function isArrEqual(arr1, arr2) {
		for (let i=0;i<arr1.length;i++) {
			if (arr1[i]!=arr2[i]) {
				return false
	        }
		}
		return true
	}

## Print HTML Element
Print the HTML element with givin ID.

	function Print(el) {
	    let html='<html><body>';
	    html+= document.getElementById(el).innerHTML;
	    html+='</body></html>';
	    var printWin = window.open();
	    printWin.document.write(html);
	    printWin.document.close();
	    printWin.focus();
	    printWin.print();
	    printWin.close();
	}

## Promise
A simple example about how to use a promise.

	function NewPromise(value) { 
	    return new Promise ((resolve, reject)=> {
	        //Do the process here, and use 'resolve' to return the promise, or reject to fail the promise
	        if (value==1) {
	            resolve(true);
	        } else {
	            reject(false);
	        }
	    }); 
	}

##### The use of a promise:
	NewPromise(5)
	.then(function(res){
	    console.log(res)
	}).catch(function(err){
	    console.log(err)
	});


## Download CSV
Uses an array of objects to download a CSV file formated with some options.

##### - Function to Convert the array to string
This function uses the argument passed to the download function to generate the string.
The argument is an object that have the following keys:

*	filename: String. The name of the file to be download. This string need to have the extension to be download. Ex: ```'report.csv'```, ```'report.txt'```.
*	arr: Array of objects. The array of objects to be converted. Ex: ```[{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6},{"a":7,"b":8,"c":9}]```.
*	header: ```True``` or ```False```. If the header of the columns should be included in the file or not.
*	colDelimiter: String. The separator of the values. If no value is given, the standard comma characater ```','``` will be used to separate. Ex: ```','```, ```';'```.
*	lineDelimiter: String. The separator of the lines. If no value is given, the standard new lines character ```'\n'``` will be used. Ex: ```'\n'```, ```'\r'```.

##### - Download Function
Call the functions ```downloadCSV(object)``` with an object that have the value for the keys described above.

	function downloadCSV(args) {
	  let data, filename, link, argsConvert;
	  filename = args.filename != undefined ? args.filename : 'file.csv';
	  argsConvert = {
	    name: filename,
	    data: args.arr,
	    header: args.header,
	    colDelimiter: args.colDelimiter != undefined ? args.colDelimiter : ',',
	    lineDelimiter: args.lineDelimiter != undefined ? args.lineDelimiter : '\n'
	  }

	  let csv = convertArrayOfObjectsToCSV(argsConvert);
	  if (csv == null) return;
	  if (!csv.match(/^data:text\/csv/i)) {
	    csv = 'data:text/csv;charset=utf-8,' + csv;
	  }

	  data = encodeURI(csv);
	  link = document.createElement('a');
	  link.setAttribute('href', data);
	  link.setAttribute('download', filename);
	  link.click();
	}

	function convertArrayOfObjectsToCSV(args) {
	  let result, ctr, keys, data;
	  data = args.data || null;
	  if (data == null || !data.length) {
	      return null;
	  }
	  keys = Object.keys(data[0]);
	  result = '';
	  if (args.header) {
	    result += keys.join(args.colDelimiter);
	    result += args.lineDelimiter;
	  }
	  
	  data.forEach(function(item) {
	    ctr = 0;
	    keys.forEach(function(key) {
	      if (ctr > 0) result += args.colDelimiter;
	      result += item[key];
	      ctr++;
	    });
	    result += args.lineDelimiter;
	  });
	  return result;
	}

Usage example:

	let argsDownload = {
		filename: 'report.csv',
		arr: [{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6},{"a":7,"b":8,"c":9}],
		header: true,
		colDelimiter: ';'
	};
	downloadCSV(argsDownload);

Calling this function with those arguments will download the file 'report.csv', with the content:<br>
```
a;b;c
1;2;3
4;5;6
7;8;9
```

	
