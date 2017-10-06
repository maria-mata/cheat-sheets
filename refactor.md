# CJ's Refactor Checklist

* [ ] Remove global variables
* [ ] Extract and name anonymous functions
* [ ] Convert for loops to forEach/map/reduce/filter etc.
* [ ] Create variables anywhere with lots of brackets/dot notation
	* array[i].item[thing].how[wat]
	* var somethingDescriptive = array[i].item[thing].how[wat]
* [ ] Update string concatenation to use template strings
* [ ] Update functions to be no longer than 10 lines
	* Extract functions where needed
* [ ] Convert functions to fat arrow
* [ ] Update objects to use new object literal notation
* [ ] Move related code to separate files
