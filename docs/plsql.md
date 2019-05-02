# My PLSQL Codes

My [PLSQL Repository](https://github.com/marvinamorim/Developer/tree/master/SQL).

### PLSQL block to apex.server.process
Model of a plsql block that return an object to the call of the apex.server.process 

	declare
	c1 SYS_REFCURSOR;
	begin
	open c1 for
		select 1 "number", a "letter" from dual
		union
		select 2 "number", b "letter" from dual;
	APEX_JSON.OPEN_OBJECT;
	APEX_JSON.WRITE('db', c1);
	APEX_JSON.CLOSE_OBJECT;
	end;

This will return the object

	{"db":[{"number":1, "letter":"a"},
		   {"number":2, "letter":"b"}]}

###